

# 用户组

通过用户组将职责相似或权限相同的子用户归为一类，统一授权，从而更高效得管理IAM子用户。

- 当整个用户组的职能范围变化，只需修改组的授权；
- 当个别用户的权限范围发生变化，e.g. 转岗、新成员加入，只需将其移出/移入用户组；

![](/images/group/group_mainpage.png)

### 创建用户组

“用户组名称”代表该用户组在这个云服务账号下的唯一标识，不可重复。
![](/images/group/group_create.png)

### 为用户组授权

为用户组选择需要绑定的权限策略以及该策略的生效范围。如图，该用户组在默认项目下拥有超级管理员权限。
财务中心权限、访问控制权限为全局策略，不需要指定生效范围。
![](/images/group/group_attach_policy.png)
![](/images/group/group_pick_a_project_policy.png)
![](/images/group/group_pick_a_global_policy.png)

### 为用户组添加成员

选择子用户加入用户组，组内成员自动获得用户组所绑定的权限。
![](/images/group/group_add_user.png)
![](/images/group/group_pick_a_user.png)

### 删除用户组

删除用户组将自动移除组内所有成员。
![](/images/group/group_delete.png)

### 查看用户组详情
![](/images/group/group_more_info.png)
