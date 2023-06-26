# SSO
支持基于SAML 2.0和OIDC的SSO（Single Sign On，单点登录），也称为身份联合登录。本文为您介绍企业如何使用自有的身份系统实现与UCloud 的SSO。

### 基本概念
身份提供商（IdP）：一个包含有关外部身份提供商元数据的IAM实体，身份提供商可以提供身份管理服务。
企业本地IdP：Microsoft Active Directory Federation Service （AD FS）、Shibboleth等。
Cloud IdP：UCloud 应用身份服务、Azure AD、Google Workspace、Okta、OneLogin等。
服务提供商（SP）	利用IdP的身份管理功能，为用户提供具体服务的应用，SP会使用IdP提供的用户信息。一些非SAML协议的身份系统（例如：OpenID Connect），也把服务提供商称作IdP的信赖方。
安全断言标记语言（SAML 2.0）	实现企业级用户身份认证的标准协议，它是SP和IdP之间实现沟通的技术实现方式之一。SAML 2.0已经是目前实现企业级SSO的一种事实标准。
SAML断言（SAML assertion）	SAML协议中用来描述认证请求和认证响应的核心元素。例如：用户的具体属性就包含在认证响应的断言里。
信赖（Trust）	建立在SP和IdP之间的互信机制，通常由公钥和私钥来实现。SP通过可信的方式获取IdP的SAML元数据，元数据中包含IdP签发SAML断言的签名验证公钥，SP则使用公钥来验证断言的完整性。
验证指纹	为了防止颁发者URL被恶意劫持或篡改，您需要配置外部IdP的HTTPS CA证书生成的验证指纹。UCloud 会辅助您自动计算该验证指纹，但是建议您在本地自己计算一次（例如：使用OpenSSL计算指纹），与UCloud 计算的指纹进行对比。如果对比发现不同，则说明该颁发者URL可能已经受到攻击，请您务必再次确认，并填写正确的指纹。
颁发者URL	颁发者URL由外部IdP提供，对应OIDC Token中的iss字段值。颁发者URL必须以https开头，符合标准URL格式，但不允许带有query参数（以?标识）、fragment片段（以#标识）和登录信息（以@标识）。
临时身份凭证	STS（Security Token Service）是UCloud 提供的一种临时访问权限管理服务，通过STS获取可以自定义时效和访问权限的临时身份凭证（STS Token）。

### UCloud SP的SAML配置
操作步骤
UCloud 账号登录IAM控制台。
在左侧导航栏，选择集成管理 > SSO管理。
单击用户SSO页签，查看当前SSO登录设置相关信息。
单击编辑，配置SSO登录设置相关信息。
SSO功能状态：选择开启或关闭。
说明 该功能只对UCloud 账号下的所有IAM用户生效，不会影响UCloud 账号的登录。
此功能默认为关闭，此时IAM用户可以使用密码登录，所有SSO设置不生效。
如果选择开启此功能，此时IAM用户密码登录方式将会被关闭，统一跳转到企业IdP登录服务进行身份认证。如果再次关闭，用户密码登录方式自动恢复。
元数据文档：单击上传文件，上传企业IdP提供的元数据文档。
说明 元数据文档由企业IdP提供，一般为XML格式，包含IdP的登录服务地址以及X.509公钥证书（用于验证IdP所颁发的SAML断言的有效性）。
辅助域名（可选）：开启辅助域名开关，可以设置一个辅助域名。
如果设置了辅助域名，SAML断言中的NameID元素将可以使用此辅助域名作为后缀。
如果没有设置辅助域名，SAML断言中的NameID元素将只能使用当前账号的默认域名或域别名作为后缀。
关于NameID元素的取值，请参见用户SSO的SAML响应。

说明 如果您同时设置了域别名和辅助域名，辅助域名将不会生效。此时，NameID元素只能使用域别名或默认域名作为后缀。
单击确定。
后续步骤
完成SAML配置后，选择以下一种方法创建与企业IdP相匹配的IAM用户：
### 企业IdP的SAML配置
从UCloud 获取SAML服务提供商元数据URL。
使用UCloud 账号登录IAM控制台。
在左侧导航栏，选择集成管理 > SSO管理。
在SSO管理页面，单击用户SSO页签。
在SSO登录设置区域，查看当前UCloud 账号的SAML服务提供商元数据URL。
在企业IdP中创建一个SAML SP，并根据实际情况选择下面任意一种方式配置UCloud 为信赖方。
直接使用步骤1所述的UCloud 元数据URL进行配置。
如果您的IdP不支持URL配置，您可以通过步骤1所述URL下载元数据文件并上传至您的IdP。
如果您的IdP不支持元数据文件上传，则需要手动配置以下参数：
Entity ID：下载的元数据XML中，md:EntityDescriptor元素的entityID属性值。
ACS URL：下载的元数据XML中，md:AssertionConsumerService元素的Location属性值。
RelayState（可选）：如果您的IdP支持设置RelayState参数，您可以将其配置成SSO登录成功后希望跳转到的页面URL。如果不进行配置，SSO登录成功后，将会跳转到UCloud 控制台首页。
### 用户SSO的SAML响应
在基于SAML 2.0的SSO流程中，当企业用户在IdP登录后，IdP将根据SAML 2.0 HTTP-POST绑定的要求生成包含SAML断言的认证响应，并由浏览器（或程序）自动转发给UCloud 。这个SAML断言会被用来确认用户登录状态并从中解析出登录的主体。因此，断言中必须包含UCloud要求的元素，否则登录用户的身份将无法被确认，导致SSO失败。
SAML响应
请确保您的IdP向UCloud 发出符合如下要求的SAML响应，每一个元素都必须要有，否则SSO将会失败。
SAML断言中的元素说明
SAML 2.0协议的通用元素
元素	说明
Issuer	Issuer的值必须与您在UCloud 用户SSO设置中上传的元数据文件中的EntityID匹配。
Signature	UCloud 要求SAML断言必须被签名以确保没有篡改，Signature及其包含的元素必须包含签名值、签名算法等信息。
Subject	
Subject必须包含以下元素：

有且仅有一个NameID元素，是UCloud 账号下的某个IAM用户的身份标识。详情请参见本文下面所述的NameID元素和NameID示例。
有且仅有一个SubjectConfirmation元素，其中包含一个SubjectConfirmationData元素。SubjectConfirmationData必须有以下两个属性：
NotOnOrAfter：规定SAML断言的有效期。
Recipient：UCloud 通过检查该元素的值来确保UCloud 是该断言的目标接收方，其取值必须为https://signin.aliyun.com/saml/SSO。
以下是一个Subject元素的示例：

<Subject>
  <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">Alice@example.onaliyun.com</NameID>        
  <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">   
    <SubjectConfirmationData NotOnOrAfter="2019-01-01T00:01:00.000Z" Recipient="https://signin.aliyun.com/saml/SSO"/>    
  </SubjectConfirmation>
</Subject>
Conditions	
在Conditions元素中，必须包含一个AudienceRestriction元素，其中可包含一至多个Audience元素，但必须有一个Audience元素的取值为 https://signin.aliyun.com/${accountId}/saml/SSO，${accountId}为UCloud 账号ID。

以下是一个Conditions元素的示例：

<Conditions>
  <AudienceRestriction>
    <Audience>https://signin.aliyun.com/${accountId}/saml/SSO</Audience>
  </AudienceRestriction>
</Conditions>          
NameID元素
UCloud 需要通过UPN（User Principal Name）来定位一个IAM用户，所以要求企业IdP生成的SAML断言包含用户的UPN。UCloud 通过解析SAML断言中的NameID元素，来匹配IAM用户的UPN从而实现用户SSO。

因此，在配置IdP颁发的SAML断言时，需要将对应于IAM用户UPN的字段映射为SAML断言中的NameID元素。

NameID元素必须是以下几种：

使用域别名作为NameID元素的后缀，即<username>@<domain_alias>。其中<username>为IAM用户的用户名，<domain_alias>为域别名。关于如何设置域别名，请参见创建并验证域别名。
使用辅助域名作为NameID元素的后缀，即<username>@<auxiliary_domain>。其中<username>为IAM用户的用户名，<auxiliary_domain> 为辅助域名。关于如何设置辅助域名，请参见进行用户SSO时UCloud SP的SAML配置。
说明 如果您同时设置了域别名和辅助域名，辅助域名将不会生效。此时，NameID元素只能使用域别名作为后缀。
使用默认域名作为NameID元素的后缀，即<username>@<default_domain>。其中<username>为IAM用户的用户名，<default_domain>为默认域名。关于如何设置默认域名，请参见查看和修改默认域名。
说明 即使设置了域别名或辅助域名，仍可以使用默认域名作为NameID的后缀。
NameID示例
IAM用户名为Alice，默认域名为example.onaliyun.com。

如果设置了域别名为example.com，SAML断言中的NameID取值为Alice@example.onaliyun.com或Alice@example.com。
如果没有设置域别名，设置了辅助域名为example.net，SAML断言中的NameID取值为Alice@example.onaliyun.com或Alice@example.net。
如果设置了域别名为example.com后，又设置了辅助域名为example.net，SAML断言中的NameID取值为Alice@example.onaliyun.com或Alice@example.com。注意此时辅助域名不生效。
