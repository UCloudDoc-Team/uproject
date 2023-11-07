# 角色管理

角色与用户一样，都是 IAM 身份类型的一种。IAM 角色是一种虚拟用户，没有确定的身份认证密钥，需要受信的实体用户扮演这个角色才能正常使用。

### 创建角色

![image](images/role1.png)

### 为角色授权

![image](images/role2.png)

### 使用角色

通过调用 API 扮演 IAM 角色
有权限的 IAM 用户可以使用其访问密钥调用 `AssumeRole` API，以获取某个 IAM 角色的安全令牌（STS Token），从而使用安全令牌访问
UCloud。具体详情参考 [sts](/uproject/sts)。



