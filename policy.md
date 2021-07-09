# 策略管理

权限 描述用户与资源之间的关系，决定了用户对资源的访问和控制能力。

策略 是一组通过语法构建的权限合集。他由五个部分组成：V (version) E (effect) A (action) R (resource) C (condition)。精确得描述了用户在什么条件下对什么资源 允许/拒绝 实现什么操作。

- V (version) 语法版本，由UCloud维护。当前版本V1，不可修改。

- E (effect) 执行效力，Allow or Deny。

- A (action) API名称，支持一个或多个。

- R (resource) 资源，用urn表示。

- C (condition) 定义该条策略的生效前提。e.g. 时间、IP。

UCloud提供两种模式的权限策略：

- 系统策略 —— 由UCloud配置和管理
- 自定义策略 —— 由用户配置和管理

### 查看系统策略
系统策略是一组按操作类型打包的权限。点击“详情”可以查看策略包含的API以及引用情况。
![](/images/policy/policy_mainpage.png)
![](/images/policy/policy_view_more.png)

### 创建自定义策略
自定义策略可以指定 允许/拒绝 操作具体的API。例如，不允许子用户新建云主机。
![](/images/policy/policy_create_new.png)
![](/images/policy/policy_create_new_API.png)
![](/images/policy/policy_create_done.png)
![](/images/policy/policy_view_customized_policy.png)

### 为子用户授权
![](/images/policy/policy_attach_customized_policy.png)
