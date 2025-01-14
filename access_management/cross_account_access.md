# 跨账号控制台访问

当其他UCloud账号向您申请通过控制台页面访问资源权限时，您可以通过【授信管理】功能为其分配权限，而无需共享访问秘钥。其访问范围 (如：项目范围、产品范围等) 由您赋予的策略所决定。

## 适用场景
1. 您需要同时管理多个云服务账号 
2. 您需要开放个别资源的访问权限给合作方(例: 其他部门/公司/组织）

## 功能特性
1. 您可以申请多个云服务账号，将生产环境与开发环境隔离，但只需管理一个账号的登录信息
2. 您可以掌握资源的所有权，仅开放一部分权限给合作方，并可以随时收回权限
3. 您可以根据业务变化，随时变更权限范围
4. 您无需分享账号的访问秘钥给合作方
5. 您无需关心合作方的人员变动


## 操作步骤 — 授信
  ![](/images/workflow.png)
 
1. 创建授信
  - 使用主账号登录控制台，进入 "访问控制 >> 授信管理"，创建授信
  - 填写需要授信的主账号
  - 指定可访问的项目
    - 若想为主账号授权“项目级策略”，可指定具体的授权项目。(项目级生效范围为指定项目。可在“策略管理”中查看策略级别)
![image](https://github.com/user-attachments/assets/21a45dad-0624-4040-bd30-47b996311938)
注意：【应用项目】将优先显示已授信“项目级策略”的项目。
![image](https://github.com/user-attachments/assets/467cb892-e18e-4344-a457-683ca3d233c1)

    - 若想为主账号授权“全局级策略”，请选择“不分项目”。(全局级范围为账号全局服务，不区分项目。可在“策略管理“中查看策略级别)
![image](https://github.com/user-attachments/assets/b0f23771-f73f-40d5-a0d2-e6c30ad8a7c1)

  - 选择授信策略
 <img width="1166" alt="image" src="https://github.com/user-attachments/assets/3e15933c-4ac1-424f-a29a-38fbf6e2f909">

2. 访问方指定角色扮演者 
  - 被授信的主账号登录控制台，进入 "访问控制 >> 授信管理"
  - 添加子账号至您需要访问的对象关系中
<img width="1039" alt="image" src="https://github.com/user-attachments/assets/80ce6e09-3465-44af-80c6-e6dbf0f727d5">


3. 访问授信方的资源
  - 使用子账号登录控制台
  - 切换身份至您要访问的对象
 ![image](https://github.com/user-attachments/assets/0ace119d-7553-4d80-bb35-3c0181ac7fcc)

  
 ## 操作步骤 — 更换访问人
  - 被授信的主账号登录控制台，进入 "访问控制 >> 授信管理 >>  我被授信"
  - 添加子账号操作更换访问人
![image](https://github.com/user-attachments/assets/b3f53635-3df7-46ee-8028-6e1996786c85)

 
 ## 操作步骤 — 解除授信
  - 使用主账号登录控制台，进入 "访问控制 >> 授信管理"，查看“我已授信”
  - 点击 “解除授信"
<img width="1171" alt="image" src="https://github.com/user-attachments/assets/88962171-a18d-4de3-8eda-00d3570f9428">


## 操作步骤 — 查看被授信信息
  - 使用主账号登录控制台，进入 "访问控制 >> 授信管理"，查看“我被授信”
  ![](/images/management1.png)
