# 跨账号API访问


当其他UCloud账号向您申请通过API访问资源权限时，您可以通过【角色】+【STS】功能为其分配权限。其访问范围 (如：项目范围、产品范围等) 由您赋予的角色所决定。

## 适用场景
您可以通过跨账号访问角色生成的临时安全密钥使您的其它UCloud账号扮UCloud已有账号的角色，在权限范围内管理资源。

## 前提条件
拥有多个UCloud主账号。本文档中假设拥有两个UCloud账号 A 即授信方、B 即访问方，假设 B 账号想管理账号 A 下的资源。


## 操作步骤
1. 授信方创建角色
   - 使用A的主账号登录控制台，进入【访问控制】->【角色管理】
   - 创建自定义角色
   - 点击对应角色详情，为角色添加可操作的功能以及资源范围权限
 ![](/images/policy/角色添加权限.png)


2. 授信方在角色的信任策略内添加访问方

   - 点击对应角色的“详情”按钮，选择“信任策略”，点击“编辑策略”
   - 在“Principal“处输入“ucs:iam::xxxxxxx:root”，xxxxxxx为访问方的账号即Company ID（可在控制台上点击头像查看Company ID）
![](/images/policy/角色添加信任策略.png)


3. 使用方创建子用户并添加可调用STS生成临时安全密钥的权限

   - 使用B的主账号登录控制台，进入【访问控制】->【用户管理】，邀请子用户
   - 点击对应子用户的添加权限按钮
   - 搜索“STSCreateOnlyAccess”策略，进行添加
![](/images/policy/子账号添加sts权限.png)


4. 使用方通过子账号调用STS服务AssumeRole接口生成临时安全密钥
> 注解：STS获取扮演角色的临时身份凭证 API说明文档可查看：https://docs.ucloud.cn/api/sts-api/assume_role。

调用接口获取到以下三个信息，进行步骤5：
   - SecurityToken：安全令牌
   - AccessKeyId：密钥ID
   - AccessKeySecret：密钥Secret
![](/images/policy/sts接口说明.png)
  

5. 使用步骤4生成的临时安全密钥扮演账号 A 授权的角色，在角色的权限范围内可以通过以下方式中的任意一种，调用您需要的接口管理UCloud资源：

   - CloudShell 云命令行
   - 多语言 OpenSDK / Go / Python /

CloudShell示例
```
ucloud api  --Action AssumeRole  --RoleUrn ucs:iam::xxxx:role/test  --RoleSessionName test-session
```



  
SDK示例
```
package main

import (
	"fmt"
	"os"

	"github.com/ucloud/ucloud-sdk-go/services/sts"
	"github.com/ucloud/ucloud-sdk-go/services/uhost"
	"github.com/ucloud/ucloud-sdk-go/ucloud"
	"github.com/ucloud/ucloud-sdk-go/ucloud/auth"
	"github.com/ucloud/ucloud-sdk-go/ucloud/log"
	"github.com/ucloud/ucloud-sdk-go/ucloud/request"
)

// loadConfig load ucloud config and credential
func loadConfig() (*ucloud.Config, *auth.Credential) {
	cfg := ucloud.NewConfig()
	cfg.LogLevel = log.DebugLevel

	credential := auth.NewCredential()
	credential.PrivateKey = os.Getenv("UCLOUD_PRIVATE_KEY")
	credential.PublicKey = os.Getenv("UCLOUD_PUBLIC_KEY")

	log.Info("setup clients ...")

	return &cfg, &credential
}

func main() {
	// get sts credential
	cfg, credential := loadConfig()
	stsClient := sts.NewClient(cfg, credential)
	var assumeRoleRequest sts.AssumeRoleRequest
	assumeRoleRequest.RoleUrn = ucloud.String("ucs:iam::xxxx:role/test")
	assumeRoleRequest.RoleSessionName = ucloud.String("test-session")
	assumeRoleResponse, err := stsClient.AssumeRole(&assumeRoleRequest)
	if err != nil {
		panic(err)
	}
	fmt.Println(assumeRoleResponse.Credentials.AccessKeyId)
	// the token from AssumeRole has an expiration time
	fmt.Println(assumeRoleResponse.Credentials.Expiration)
	cred := &auth.Credential{
		PublicKey:     assumeRoleResponse.Credentials.AccessKeyId,
		PrivateKey:    assumeRoleResponse.Credentials.AccessKeySecret,
		SecurityToken: assumeRoleResponse.Credentials.SecurityToken,
	}
	// invoke uhost client with the sts credential
	uhostClient := uhost.NewClient(cfg, cred)
	cfg.ProjectId = os.Getenv("UCLOUD_PROJECT_ID")
	cfg.Region = "cn-bj2"
	req := &uhost.DescribeUHostInstanceRequest{}
	req.SetEncoder(request.NewJSONEncoder(cfg, cred))
	resp, err := uhostClient.DescribeUHostInstance(&uhost.DescribeUHostInstanceRequest{})
	if err != nil {
		log.Errorf("error: %v", err)
	} else {
		log.Infof("response: %+v", resp)
	}
}
```



