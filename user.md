

# 子用户

子用户是IAM用户身份的一种，他拥有以下特征：

- 归属于云服务账号 (即主账号) ，由主账号创建和管理，一般对应企业员工或应用程序；
- 默认没有任何权限，主账号授权后才能登陆/访问资源；
- 每个子用户独立拥有长期有效的API秘钥；
- 不支持独立计费，子用户产生的消费都通过主账号结算；
- 子用户删除，不影响其创建的资源；

![](/images/user/user_mainpage.png)


### 创建子用户

子用户通过邮箱登录，用户名是子用户在该云服务账号下的唯一标识。
![](/images/user/user_create.png)

### 为子用户添加权限

子用户需要获取资源权限后才能开展一系列管理工作。IAM允许子用户直接绑定资源权限，也可以通过加入用户组行使组的资源权限。
![](/images/user/user_attach_policy.png)
![](/images/user/user_pick_a_project_policy.png)
![](/images/user/user_pick_a_global_policy.png)


### 子用户加入用户组

子用户加入用户组后，自动获取组的资源权限。子用户可以加入多个用户组。
![](/images/user/user_join_group.png)
![](/images/user/user_pick_a_group.png)


### 冻结/解冻/注销子用户

子用户被冻结后将无法登录，解冻后恢复。子用户注销不可逆。子用户的任何变化都不影响云服务账号下的资源使用。
![](/images/user/user_freezing_delete.png)

### 查看子用户详情
通过详情页管理子用户的具体权限和组信息。注意子用户不可以管理其所在组的资源权限。
![](/images/user/user_more_info.png)
