# 策略管理

权限 描述用户与资源之间的关系，决定了用户对资源的访问和控制能力。

策略 是一组通过语法构建的权限合集。他由五个部分组成：V (version) E (effect) A (action) R (resource)。精确地描述了用户在什么条件下对什么资源
允许 / 拒绝 实现什么操作。

- V (version) 语法版本，由 UCloud 维护。系统策略当前版本 V1，不可修改；自定义策略在编辑保存后会自动生成新版本。
- E (effect) 执行效力，Allow or Deny。
- A (action) API 名称，支持一个或多个。
- R (resource) 资源，用 urn 表示。

UCloud 提供两种模式的权限策略：

- 系统策略 —— 由 UCloud 配置和管理
- 自定义策略 —— 由用户配置和管理

## 策略级别

策略按照生效范围可分为两种级别，全局级与项目级。

- 全局级生效范围为账号全局服务，不区分项目，全局级策略不支持分项目授权；
- 项目级生效范围为指定项目，项目级策略支持分项目授权；
- 注：部分系统策略属于全局级 / 项目级，自定义策略无法创建这种类型。

### 查看系统策略

系统策略是一组按操作类型打包的权限。点击“详情”可以查看策略包含的 API 以及引用情况。
![策略列表](/images/policy/policy_list.png)
![系统策略详情](/images/policy/policy_detail.png)

### 创建自定义策略

自定义策略可以指定 允许 / 拒绝 操作具体的 API。例如，不允许子用户新建云主机。
![创建自定义策略](/images/policy/create_policy_deny_create_uhostinstance.png)
![创建自定义策略](/images/policy/create_policy_deny_create_uhostinstance_1.png)
![创建自定义策略](/images/policy/create_policy_deny_create_uhostinstance_2.png)
![创建自定义策略](/images/policy/create_policy_deny_create_uhostinstance_3.png)

- 自定义策略支持编辑、删除。
- 权限策略内容修改完成后，系统会自动生成一个新的权限策略版本，该新版本将变为当前版本。
- 支持设置历史版本设为当前版本。
  ![自定义策略](/images/policy/create_policy_deny_create_uhostinstance_4.png)

### 为子用户授权

授权时需要区分全局级 / 项目级授权  
全局级策略：如果您选择的策略均为全局级别，那么这些策略将自动应用到全局范围，无需您额外指定生效范围。
![企业微信截图_b58693c9-9cbd-4d47-b3b0-2551ef4d646a](https://github.com/user-attachments/assets/28ea49b7-fecc-4730-91cc-fffaa3502cb7)
项目级策略：如果您选择的策略中包含项目级别的策略，您将有机会选择具体的生效范围。您可以选择将策略应用到特定的项目，或者选择“全部项目”，这意味着策略将覆盖您账号下的所有现有项目以及未来新增的项目。请注意，“全部项目”选项将影响到您账号下的所有项目，因此在选择时请务必谨慎。


![image](https://github.com/user-attachments/assets/d862202d-d195-4ba8-bd5d-0e67b0d77a09)

