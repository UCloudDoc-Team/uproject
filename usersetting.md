# 用户设置

## 生效范围
密码规则、全局安全、多因素认证对所有子用户生效；  
网络访问限制对所有子用户及角色生效，实现统一安全管控。

![image](https://github.com/user-attachments/assets/a89a86ad-5469-4f68-94bc-525a67657b8d)


## 密码策略
### 密码长度  
支持设置 6 - 32 个字符，支持自定义最小字符数。

### 字符类型要求  
可设置密码必须包含的字符类型，可选择大写字母、小写字母、数字、符号。勾选的字符类型在子账号设置密码时必须出现，若全不选则表示不做此规则限制。

### 定期修改密码策略  
开启后，每隔一定天数系统会强制要求子账号更改一次密码。

![image](https://github.com/user-attachments/assets/6c148727-e990-4e25-aeea-47b674366999)

## 全局安全
### 登录保持时间  
系统默认时间为 12 小时，登录保持时间可设置范围为 15 分钟至 24 小时。设置登录保持的时间长度后，若超过该时间系统会自动退出登录。该功能仅对 Web 端登录有效。

### 安全锁  
开启后，在进行删除资源等高危操作时，需要通过验证方式再次确认身份。

![image](https://github.com/user-attachments/assets/cacad329-3590-4699-b674-87b93c2ec49e)


## 多因素认证
### 虚拟 MFA  
开启后，子用户需要绑定MFA，并在登录时使用 MFA 进行二次认证。

![image](https://github.com/user-attachments/assets/c97065b1-d354-48e2-81a8-c320be3b0371)


## 网络访问限制
### 访问限制  
配置后，账号只能从指定IP登录或发起API访问。
![image](https://github.com/user-attachments/assets/eca12b91-e95f-4d90-ba4d-d2896176aa37)


