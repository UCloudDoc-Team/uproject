# SSO
支持基于SAML 2.0和OIDC的SSO（Single Sign On，单点登录），也称为身份联合登录。本文为您介绍企业如何使用自有的身份系统实现与UCloud 的SSO。

### 基本概念
* 身份提供商（IdP）：一个包含有关外部身份提供商元数据的IAM实体，身份提供商可以提供身份管理服务。
* 企业本地IdP：Microsoft Active Directory Federation Service （AD FS）、Shibboleth等。
* Cloud IdP：UCloud 应用身份服务、Azure AD、Google Workspace、Okta、OneLogin等。
* 服务提供商（SP）	利用IdP的身份管理功能，为用户提供具体服务的应用，SP会使用IdP提供的用户信息。一些非SAML协议的身份系统（例如：OpenID Connect），也把服务提供商称作IdP的信赖方。
* 安全断言标记语言（SAML 2.0）	实现企业级用户身份认证的标准协议，它是SP和IdP之间实现沟通的技术实现方式之一。SAML 2.0已经是目前实现企业级SSO的一种事实标准。
* SAML断言（SAML assertion）	SAML协议中用来描述认证请求和认证响应的核心元素。例如：用户的具体属性就包含在认证响应的断言里。
* 信赖（Trust）	建立在SP和IdP之间的互信机制，通常由公钥和私钥来实现。SP通过可信的方式获取IdP的SAML元数据，元数据中包含IdP签发SAML断言的签名验证公钥，SP则使用公钥来验证断言的完整性。
* 验证指纹	为了防止颁发者URL被恶意劫持或篡改，您需要配置外部IdP的HTTPS CA证书生成的验证指纹。UCloud 会辅助您自动计算该验证指纹，但是建议您在本地自己计算一次（例如：使用OpenSSL计算指纹），与UCloud 计算的指纹进行对比。如果对比发现不同，则说明该颁发者URL可能已经受到攻击，请您务必再次确认，并填写正确的指纹。
* 颁发者URL	颁发者URL由外部IdP提供，对应OIDC Token中的iss字段值。颁发者URL必须以https开头，符合标准URL格式，但不允许带有query参数（以?标识）、fragment片段（以#标识）和登录信息（以@标识）。
* 临时身份凭证	STS（Security Token Service）是UCloud 提供的一种临时访问权限管理服务，通过STS获取可以自定义时效和访问权限的临时身份凭证（STS Token）。

### UCloud SP的SAML配置
####  操作步骤
打开访问控制——SSO管理，此功能默认为关闭，此时UCloud用户可以使用密码登录，所有SSO设置不生效。
![image](https://github.com/UCloudDoc-Team/uproject/assets/107971405/287d59f8-1635-4808-a127-67c106d2b95f)
如果选择开启此功能，此时子用户密码登录方式将会被关闭，统一跳转到企业IdP登录服务进行身份认证。如果再次关闭，用户密码登录方式自动恢复。
元数据文档：单击上传文件，上传企业IdP提供的元数据文档。
说明 元数据文档由企业IdP提供，一般为XML格式，包含IdP的登录服务地址以及X.509公钥证书（用于验证IdP所颁发的SAML断言的有效性）。单击开启，上传元数据文件。
![image](https://github.com/UCloudDoc-Team/uproject/assets/107971405/c12752c7-7a78-43bf-9de7-91a768271af6)
说明：该功能只对UCloud账号下的所有IAM子用户生效，不会影响UCloud主账号的登录。

####  后续步骤
完成SAML配置后，创建与企业IdP相匹配的IAM用户，用户管理-邀请子用户
<img width="882" alt="image" src="https://github.com/UCloudDoc-Team/uproject/assets/107971405/73191284-5c07-473c-8d8c-004eab740d19">
填写用户名，无需邮箱，系统会为该子用户生成虚拟邮箱，邀请成功后即激活状态。注：关闭SSO功能后，需要将开启SSO功能期间创建的子用户开启控制台访问能力。便于关闭SSO功能后子账号可以使用账号邮箱登录等功能。
<img width="1152" alt="image" src="https://github.com/UCloudDoc-Team/uproject/assets/107971405/2f68a555-2816-42a6-81a2-f2b2f7d5649c">

##### 企业IdP的SAML配置
###### 从UCloud获取SAML服务提供商元数据URL。
<img width="590" alt="image" src="https://github.com/UCloudDoc-Team/uproject/assets/107971405/cb318d78-5279-4b09-a599-5166219751e6">

###### 在企业IdP中创建一个SAML SP，并根据实际情况选择下面任意一种方式配置UCloud 为信赖方。
直接使用步骤1所述的UCloud元数据URL进行配置。
如果您的IdP不支持URL配置，您可以通过步骤1所述URL下载元数据文件并上传至您的IdP。
如果您的IdP不支持元数据文件上传，则需要手动配置以下参数：
* Entity ID：下载的元数据XML中，md:EntityDescriptor元素的entityID属性值。
* ACS URL：下载的元数据XML中，md:AssertionConsumerService元素的Location属性值。
* RelayState（可选）：如果您的IdP支持设置RelayState参数，您可以将其配置成SSO登录成功后希望跳转到的页面URL。如果不进行配置，SSO登录成功后，将会跳转到UCloud 控制台首页。


