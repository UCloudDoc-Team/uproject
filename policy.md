# 策略管理

权限 描述用户与资源之间的关系，决定了用户对资源的访问和控制能力。

策略 是一组通过语法构建的权限合集。他由五个部分组成：V (version) E (effect) A (action) R (resource) C (condition)。精确地描述了用户在什么条件下对什么资源 允许/拒绝 实现什么操作。

- V (version) 语法版本，由UCloud维护。系统策略当前版本V1，不可修改；自定义策略在编辑保存后会自动生成新版本。

- E (effect) 执行效力，Allow or Deny。

- A (action) API名称，支持一个或多个。

- R (resource) 资源，用urn表示。

- C (condition) 定义该条策略的生效前提。e.g. 时间、IP。

UCloud提供两种模式的权限策略：

- 系统策略 —— 由UCloud配置和管理
- 自定义策略 —— 由用户配置和管理

## 策略级别
策略按照生效范围可分为两种级别，全局级与项目级。
- 全局级生效范围为账号全局服务，不区分项目，全局级策略不支持分项目授权；
- 项目级生效范围为指定项目，项目级策略不支持分项目授权；
- 注：部分系统策略属于全局级/项目级，自定义策略无法创建这种类型。


### 查看系统策略
系统策略是一组按操作类型打包的权限。点击“详情”可以查看策略包含的API以及引用情况。
![image](https://user-images.githubusercontent.com/107971405/192424976-053496c1-5990-45ed-a52a-12b9a45066b1.png)
![image](https://user-images.githubusercontent.com/107971405/192425557-6024d261-21f3-4efc-9973-ad553c25f082.png)


### 创建自定义策略
自定义策略可以指定 允许/拒绝 操作具体的API。例如，不允许子用户新建云主机。
![image](https://user-images.githubusercontent.com/107971405/192424035-b988518c-b9db-4cf3-8199-f17c5919fb2f.png)
![image](https://user-images.githubusercontent.com/107971405/192424165-e41d1b3e-a583-48a8-a44f-45397b99c5f4.png)
![image](https://user-images.githubusercontent.com/107971405/192424241-259de94e-8767-4be7-8bd3-6ad45cb3ffaf.png)
![image](https://user-images.githubusercontent.com/107971405/192424495-51f4673c-8989-4e79-aa3b-7c146378734a.png)

- 自定义策略支持编辑、删除。
- 权限策略内容修改完成后，系统会自动生成一个新的权限策略版本，该新版本将变为当前版本。
- 支持设置历史某一版本设为当前版本。
![image](https://user-images.githubusercontent.com/107971405/192424539-9a861b86-8a40-4d4f-abef-8e631b7546d3.png)


### 为子用户授权
授权时需要区分全局级/项目级授权，选择项目级，需要对应选择该账号下的部分项目。
![image](https://user-images.githubusercontent.com/107971405/192424764-1826d2b6-2d05-4d05-84dc-c185a394a6ca.png)
![image](https://user-images.githubusercontent.com/107971405/192424841-f3f3664e-8b48-4148-95ca-1d512d4b1070.png)
