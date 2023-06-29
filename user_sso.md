# SSO
支持基于SAML 2.0和OIDC的SSO（Single Sign On，单点登录），也称为身份联合登录。本文为您介绍企业如何使用自有的身份系统实现与UCloud 的SSO。

### 基本概念
* 身份提供商（IdP）：一个包含有关外部身份提供商元数据的IAM实体，身份提供商可以提供身份管理服务。
* 企业本地IdP：Microsoft Active Directory Federation Service （AD FS）、Shibboleth等。
* Cloud IdP：UCloud 应用身份服务、Azure AD、Google Workspace、Okta、OneLogin等。
* 服务提供商（SP）：利用IdP的身份管理功能，为用户提供具体服务的应用，SP会使用IdP提供的用户信息。
* 安全断言标记语言（SAML 2.0）：实现企业级用户身份认证的标准协议，它是SP和IdP之间实现沟通的技术实现方式之一。SAML 2.0已经是目前实现企业级SSO的一种事实标准。
* SAML断言（SAML assertion）：SAML协议中用来描述认证请求和认证响应的核心元素。例如：用户的具体属性就包含在认证响应的断言里。
* 信赖（Trust）：建立在SP和IdP之间的互信机制，通常由公钥和私钥来实现。SP通过可信的方式获取IdP的SAML元数据，元数据中包含IdP签发SAML断言的签名验证公钥，SP则使用公钥来验证断言的完整性。
* 验证指纹：为了防止颁发者URL被恶意劫持或篡改，您需要配置外部IdP的HTTPS CA证书生成的验证指纹。UCloud会辅助您自动计算该验证指纹，但是建议您在本地自己计算一次（例如：使用OpenSSL计算指纹），与UCloud计算的指纹进行对比。如果对比发现不同，则说明该颁发者URL可能已经受到攻击，请您务必再次确认，并填写正确的指纹。
* 临时身份凭证：STS（Security Token Service）是UCloud 提供的一种临时访问权限管理服务，通过STS获取可以自定义时效和访问权限的临时身份凭证（STS Token）。

### UCloud SP的SAML配置
####  操作步骤
打开访问控制——SSO管理，此功能默认为关闭，此时UCloud用户可以使用密码登录，所有SSO设置不生效。
![image](https://github.com/UCloudDoc-Team/uproject/assets/107971405/b8aa3fb3-dc2d-493c-be97-fe92d1b3870d)

如果选择开启此功能，此时子用户密码登录方式将会被关闭，统一跳转到企业IdP登录服务进行身份认证。如果再次关闭，用户密码登录方式自动恢复。  
元数据文档：单击上传文件，上传企业IdP提供的元数据文档。  
说明：元数据文档由企业IdP提供，一般为XML格式，包含IdP的登录服务地址以及X.509公钥证书（用于验证IdP所颁发的SAML断言的有效性）。  
单击开启，上传元数据文件。

![image](https://github.com/UCloudDoc-Team/uproject/assets/107971405/cfbfb947-a20e-4a57-ab98-7a2402ec3644)



说明：该功能只对UCloud账号下的所有IAM子用户生效，不会影响UCloud主账号的登录。

####  后续步骤
完成SAML配置后，创建与企业IdP相匹配的IAM用户，用户管理-邀请子用户，填写用户名，无需邮箱，系统会为该子用户生成虚拟邮箱，邀请成功后即激活状态。
<img width="882" alt="image" src="https://github.com/UCloudDoc-Team/uproject/assets/107971405/73191284-5c07-473c-8d8c-004eab740d19">  
注：关闭SSO功能后，需要将开启SSO功能期间创建的子用户开启控制台访问能力。便于关闭SSO功能后子账号可以使用账号邮箱登录等功能。
<img width="633" alt="image" src="https://github.com/UCloudDoc-Team/uproject/assets/107971405/1b974345-7f1e-42a5-b2d2-5a0f70e9b501">

<img width="1147" alt="image" src="https://github.com/UCloudDoc-Team/uproject/assets/107971405/a04cd603-83ac-4d94-8fb4-d83094112500">


##### 企业IdP的SAML配置
从UCloud获取SAML服务提供商元数据URL。  
![image](https://github.com/UCloudDoc-Team/uproject/assets/107971405/85a0c150-bf44-4729-b0f2-36c8203c0e60)
在企业IdP中创建一个SAML SP，并根据实际情况选择下面任意一种方式配置UCloud 为信赖方。
直接使用步骤1所述的UCloud元数据URL进行配置。
如果您的IdP不支持URL配置，您可以通过步骤1所述URL下载元数据文件并上传至您的IdP。
如果您的IdP不支持元数据文件上传，则需要手动配置以下参数：
* Entity ID：下载的元数据XML中，md:EntityDescriptor元素的entityID属性值。
* ACS URL：下载的元数据XML中，md:AssertionConsumerService元素的Location属性值。

### 用户SSO的SAML响应
在基于SAML 2.0的SSO流程中，当企业用户在IdP登录后，IdP将根据SAML 2.0 HTTP-POST绑定的要求生成包含SAML断言的认证响应，并由浏览器（或程序）自动转发给UCloud。这个SAML断言会被用来确认用户登录状态并从中解析出登录的主体。因此，断言中必须包含UCloud要求的元素，否则登录用户的身份将无法被确认，导致SSO失败。

#### SAML响应
请确保您的IdP向UCloud发出符合如下要求的SAML响应，每一个元素都必须要有，否则SSO将会失败。
```
<saml2p:Response>
    <saml2:Issuer>...</saml2:Issuer>
    <ds:Signature>
        ...
    </ds:Signature>
    <saml2p:Status>
        ...
    </saml2p:Status>
    <saml2:Assertion>
        <saml2:Issuer>...</saml2:Issuer>
        <saml2:Subject>
            <saml2:NameID>${NameID}</saml2:NameID>
            <saml2:SubjectConfirmation>
                ...
            </saml2:SubjectConfirmation>
        </saml2:Subject>
        <saml2:Conditions>
            <saml2:AudienceRestriction>
                <saml2:Audience>${Audience}</saml2:Audience>
            </saml2:AudienceRestriction>
        </saml2:Conditions>
        <saml2:AuthnStatement>
            ...
        </saml2:AuthnStatement>
    </saml2:Assertion>
</saml2p:Response> 
 ```
#### SAML断言中的元素说明
* SAML 2.0协议的通用元素

|  元素   | 说明                                                                                                                                                                                                                                                                                                     |
|  ----  |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Issuer  | Issuer的值必须与您在UCloud用户SSO设置中上传的元数据文件中的EntityID匹配。                                                                                                                                                                                                                                                       |
| Signature  | UCloud要求SAML断言必须被签名以确保没有篡改，Signature及其包含的元素必须包含签名值、签名算法等信息。                                                                                                                                                                                                                                            |
|  Subject  | Subject必须包含以下元素：有且仅有一个NameID元素，是UCloud账号下的某个IAM子用户用户的身份标识。详情请参见本文下面所述的NameID元素和NameID示例。有且仅有一个SubjectConfirmation元素，其中包含一个SubjectConfirmationData元素。SubjectConfirmationData必须有以下两个属性：NotOnOrAfter：规定SAML断言的有效期。Recipient：UCloud通过检查该元素的值来确保UCloud是该断言的目标接收方，其取值必须为https://signin.ucloud.cn/saml/SSO  。 
| Conditions  | 在Conditions元素中，必须包含一个AudienceRestriction元素，其中可包含一至多个Audience元素，但必须有一个Audience元素的取值为 https://signin.ucloud.cn/${CompanyID}/saml/SSO ，${CompanyID}为UCloud公司ID。                                                                                                                                           |

以下是一个Subject元素的示例：
```
<Subject>
  <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">Alice</NameID>        
  <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">   
    <SubjectConfirmationData NotOnOrAfter="2019-01-01T00:01:00.000Z" Recipient="https://signin.ucloud.cn/saml/SSO"/>    
  </SubjectConfirmation>
</Subject>|
```

以下是一个Conditions元素的示例：
```
<Conditions>
  <AudienceRestriction>
    <Audience>https://signin.ucloud.cn/${CompanyID}/saml/SSO</Audience>
  </AudienceRestriction>
</Conditions>
```

* NameID元素

UCloud需要通过CompanyID+用户名来定位一个IAM子用户，所以要求企业IdP生成的SAML断言包含子用户的用户名。UCloud通过解析SAML断言中的NameID元素，来匹配IAM子用户的用户名从而实现用户SSO。

因此，在配置IdP颁发的SAML断言时，需要将对应于IAM子用户用户名的字段映射为SAML断言中的NameID元素。

