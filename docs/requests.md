# 请求

下面的主题描述了可以使用 API 获取的请求。这些主题介绍了每个请求以及可能的响应代码和示例。

!!! tip "注意" 在某些示例中，添加了换行符和空格以便于阅读。

## 客户端配置

使用此请求可获取站点的客户端配置设置。 这些设置定义客户端的行为并提供其他服务（例如身份验证和资源枚举）的 Web 代理 URL。 可以在进行身份验证之前以及建立 Web 会话之前执行此请求。

### 请求

| URL                 | 方法  | 说明                      |
| ------------------- | --- | ----------------------- |
| /Home/Configuration | GET | 请求客户端配置，该配置以 XML 文档格式返回 |

| 响应代码 | 说明                          |
| ---- | --------------------------- |
| 200  | 成功                          |
| 403  | 由于以下原因之一被禁止：                |
|      | - CSRF 令牌无效                 |
|      | - X-Citrix-IsUsingHTTPS 头无效 |

### 成功响应内容

如果返回成功响应（HTTP 状态代码 200 OK），响应正文中将包含一个用于描述客户端的 Citrix Receiver for Web 配置设置的 XML 文档，其根节点为 `clientSettings`。 有关各种配置元素和属性的详细信息，请参阅第 81 页上的“使用配置元素”。

### 示例：客户端配置

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Home/Configuration HTTP/
1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: 4F4F307CAEAB89EEA86EF511788B1BB1
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.0
Set-Cookie: CsrfToken=597E2BAC2A18AF3816B229D5022B6E2A; path=/
Citrix/StoreWeb/
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 20 Sep 2013 15:03:40 GMT
Content-Length: 1592
<clientSettings>
  <session timeout="20" userLanguages="en-US,en;q=0.5" />
  <authManager getUsernameURL="Authentication/GetUserName"
               changeCredentialsURL="Authentication/
GetChangeCredentialForm"
               logoffURL="Authentication/Logoff"
loginFormTimeout="5" />
  <storeProxy keepAliveURL="Home/KeepAlive">
    <resourcesProxy listURL="Resources/List"
resourceDetails="default" />
    <sessionsProxy listAvailableURL="Sessions/ListAvailable"
                   disconnectURL="Sessions/Disconnect"
                   logoffURL="Sessions/Logoff" />
  </storeProxy>
  <pluginAssistant enabled="true" upgradeAtLogin="true">
    <win32 path="http://
downloadplugins.citrix.com.edgesuite.net/Windows/
CitrixReceiverWeb.exe" />
    <macOS path=http://
downloadplugins.citrix.com.edgesuite.net/Mac/
CitrixReceiverWeb.dmg
           minimumSupportedOSVersion="10.6" />
    <html5 enabled="Off"
           platforms="Firefox;Chrome;Version/([6-9]|\d
\d).*Safari;MSIE \d\d;Trident;"
           launchURL="clients/HTML5Client/src/
SessionWindow.html"
           preferences="" singleTabLaunch="false"
chromeAppOrigins="chrome-extension://
haiffjcadagjlijoggckpgfnoeiflnem|chrome-extension://
bgiigkppjadidglloadcadeihlnbpagp" />
  </pluginAssistant>
  <userInterface frameOptions="deny" autoLaunchDesktop="true">
    <workspaceControl enabled="true"
autoReconnectAtLogon="true" logoffAction="disconnect"
                      showReconnectButton="true"
showDisconnectButton="true" />
    <receiverConfiguration enabled="true"
                           downloadURL="ServiceRecord/
GetDocument/receiverconfig.cr" />
    <uiViews showDesktopsView="true" showAppsView="true"
defaultView="desktops" />
    <appShortcuts enabled="false"
allowSessionReconnect="false" />
  </userInterface>
</clientSettings>
```

## 会话保持活动状态

使用此请求可延长会话的持续时间。 可以在进行身份验证之前以及建立 Web 会话之前执行此请求。 只联系服务器即可保持服务器端会话处于活动状态以及重置 ASP.NET 会话空闲计时器。 例如，如果会话超时为 20 分钟，并且在 15 分钟空闲时间后执行此调用，则会另外向用户提供 20 分钟时间再使会话超时。

### 请求

| URL             | 方法   | 说明                                                                           |
| --------------- | ---- | ---------------------------------------------------------------------------- |
| /Home/KeepAlive | HEAD | 返回一条状态代码为 HTTP 200 的空响应。 此请求由客户端用来延长 HTTP 会话的生存时间，如果在 20 分钟时间内未收到任何请求，则默认超时。 |

### 响应

| 响应代码 | 说明                          |
| ---- | --------------------------- |
| 200  | 成功                          |
| 403  | 由于以下原因之一被禁止：                |
|      | * CSRF 令牌无效                 |
|      | * X-Citrix-IsUsingHTTPS 头无效 |

### 成功响应内容

如果返回成功响应（HTTP 状态代码 200 OK），则响应正文为空。

### 示例：会话保持活动状态

#### 请求

```curl
HEAD http://webserver/Citrix/StoreWeb/Home/KeepAlive HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: text/plain, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: 4DE109CB32EBE310E1902D05D8DD5BDF
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
CsrfToken=4DE109CB32EBE310E1902D05D8DD5BDF;
CtxsAuthId=B3C9A3C0D34C04EE7CC7C0C6B94A509D;
ASP.NET_SessionId=jti1oxzixf5pvwisz5zw05wg; AGSSOHasOccurred=;
PasswordChangeAllowed=
Connection: keep-alive
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Length: 0
Content-Type: text/html
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Mon, 23 Sep 2013 09:16:35 GMT
```

## 身份验证方法

使用以下 URL 可在启动身份验证之前确定可用的身份验证方法。

### 请求

| URL（仅供参考）                       | 方法   | 说明                    |
| ------------------------------- | ---- | --------------------- |
| /Authentication/ GetAuthMethods | POST | 返回 XML 格式的可用身份验证方法列表。 |

!!! tip "注意" * 客户端必须先向 /Resources/List 发出一个 POST 请求。 由于用户尚未通过身份验证，因此，此请求将在位置字段中返回一个带 GetAuthMethods URL 的 CitrixWebReceiver- Authenticate 头格式的质询。 例如：

```curl
CitrixWebReceiver-Authenticate:
reason="TokenRequired",location="Authentication/
GetAuthMethods"`
```

!!! tip "注意" * 客户端不得通过对任何身份验证 URL 进行硬编码来跳过此请求，因为这些 URL 可能会因版本而异。 * 可用身份验证方法在 Citrix Receiver for Web 配置文件中指定。 返回的方法列表是为 Citrix Receiver for Web 站点配置的方法与为 StoreFront 身份验证服务配置的方法的交集。 * 网关身份验证可用时，请优先使用此身份验证方法，并排除所有其他身份验证方法。 这是因为仅当用户通过 NetScaler Gateway 在外部连接到 Citrix Receiver for Web 时网关身份验证才可用，而其他身份验证方法仅供 Intranet 使用。

### 响应

| 响应代码 | 说明                          |
| ---- | --------------------------- |
| 200  | 成功                          |
| 403  | 由于以下原因之一被禁止：                |
|      | * CSRF 令牌无效                 |
|      | * X-Citrix-IsUsingHTTPS 头无效 |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| XML  | application/xml        |

### 成功响应内容

返回成功响应（HTTP 状态代码 200）时，响应正文中将包含一个根节点为 authMethods 并且包含零个或多个方法子元素的 XML 文档。 每个方法子元素都包含一个用于标识身份验证方法的名称属性以及一个客户端用于通过给定方法启动身份验证的 URL 属性。 可能的身份验证方法名称如下：

| 方法名称              | 典型 URL（仅供参考）                 | 说明                                |
| ----------------- | ---------------------------- | --------------------------------- |
| ExplicitForms     | ExplicitAuth/Login           | 使用用户提供的凭据（例如用户名、域和密码）的显式身份验证。     |
| IntegratedWindows | DomainPassthroughAuth/ Login | 域直通身份验证                           |
| Certificate       | SmartcardAuth/Login          | 智能卡（客户端证书）身份验证                    |
| CitrixAGBasic     | GatewayAuth/Login            | 对 NetScaler Gateway 的自动身份验证（单点登录） |
| PostCredentials   | PostCredentialsAuth/Login    | 通过直接将用户凭据发布到 Web 代理实现的身份验证。       |

### 示例：获取身份验证方法

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Authentication/
GetAuthMethods HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Csrf-Token: EE87320F00C8694137FBB9319DE3EE80
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=EE87320F00C8694137FBB9319DE3EE80;
ASP.NET_SessionId=vo0ii2tbv2oaqcrko5zjjjlx
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Wed, 15 Jan 2014 13:17:19 GMT
Content-Length: 282
<?xml version="1.0" encoding="UTF-8"?>
<authMethods>
    <method name="IntegratedWindows"
url="DomainPassthroughAuth/Login"/>
    <method name="Certificate" url="SmartcardAuth/Login"/>
    <method name="ExplicitForms" url="ExplicitAuth/Login"/>
</authMethods>
```

## 域直通和智能卡身份验证

使用这些请求可使用直通身份验证或智能卡设备对用户进行身份验证。

### 请求

| URL（仅供参考）                     | 方法   | 说明                           |
| ----------------------------- | ---- | ---------------------------- |
| /DomainPassthroughAuth/ Login | POST | 使用用户登录计算机时使用的凭据对用户进行身份验证。    |
| /SmartcardAuth/Login          | POST | 使用连接到用户的客户端计算机的智能卡对用户进行身份验证。 |

!!! tip "注意" * 域直通身份验证要求客户端浏览器在 Microsoft Windows 上运行。 调用方必须在启动直通身份验证之前检查客户端平台操作系统。 * 直通 URL 在 IIS 中配置为受集成 Windows 身份验证 (IWA) 保护。 实际身份验证发生在浏览器与 IIS 之间，以透明方式对 Citrix Receiver for Web 执行。 * 智能卡 URL 在 IIS 中配置为仅可通过 HTTPS 进行访问并且要求浏览器提供客户端证书。 * 直通身份验证和智能卡身份验证都要求在后端 XenDesktop/XenApp 服务器上启用选项“Trust requests to the XML Service”（信任发送至 XML Service 的请求）。 * 使用直通身份验证对 Citrix Receiver for Web 进行身份验证时，用户的密码对 StoreFront 不可用。 要启动以开始工作但不提示用户提供凭据，请按 <http://support.citrix.com/article/CTX133855> 中所述配置 Citrix Receiver。 * 智能卡身份验证需要两个 PIN 提示，一个用于对 Web 站点进行身份验证，另一个用于对远程会话进行身份验证。 * 如果身份验证失败，请检查 StoreFront 事件日志，该日志中可能会显示更多信息以帮助诊断故障。

### 响应

| 响应代码 | 说明                                                     |
| ---- | ------------------------------------------------------ |
| 200  | 成功或失败。                                                 |
| 401  | 未获授权。在以下情况下返回：                                         |
|      | * 用户尝试从未加入 StoreFront 域的计算机进行直通身份验证，并取消提示输入域凭据的浏览器对话框。 |
|      | * 用户尝试进行智能卡身份验证并取消其中一个插入智能卡、选择证书或输入 PIN 码的浏览器提示。       |
| 403  | 由于以下原因之一被禁止：                                           |
|      | * CSRF 令牌无效                                            |
|      | * X-Citrix-IsUsingHTTPS 头无效                            |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| XML  | application/xml        |

### 成功响应内容

返回成功响应（HTTP 状态代码 200）时，响应正文中将包含一个根节点为 `AuthenticationStatus` 并且包含以下子元素的 XML 文档：

| 元素名称       | 必需 | 说明                                                                                                                 |
| ---------- | -- | ------------------------------------------------------------------------------------------------------------------ |
| Result     | 是  | 值为“success”或“failure”。                                                                                             |
| AuthType   | 否  | 身份验证类型，身份验证方法字符串“ExplicitForms”、“IntegratedWindows”、“Certificate”、“CitrixAGBasic”、“PostCredentials”之一。仅在身份验证成功时发送。 |
| LogMessage | 否  | 指示身份验证失败原因的错误 ID。 通常返回泛型值“fatalerror”，但是，如果 Citrix Receiver for Web 会话已超时，则会返回“sessiontimeout”。 仅在身份验证失败时发送。       |

### 示例：成功的智能卡身份验证

#### 请求

```curl
POST https://webserver.domain.com/Citrix/StoreWeb/
SmartcardAuth/Login HTTP/1.1
Csrf-Token: C34A98583E032C5D26D4332A54738447
Accept: application/xml, text/xml, */*; q=0.01
X-Citrix-IsUsingHTTPS: Yes
X-Requested-With: XMLHttpRequest
Referer: https://webserver.domain.com/Citrix/StoreWeb/
Accept-Language: en-GB
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.3;
WOW64; Trident/7.0; .NET4.0E; .NET4.0C; .NET CLR
3.5.30729; .NET CLR 2.0.50727; .NET CLR 3.0.30729; InfoPath.3)
Host: webserver.domain.com
Content-Length: 0
DNT: 1
Connection: Keep-Alive
Cache-Control: no-cache
Cookie: CsrfToken=C34A98583E032C5D26D4332A54738447;
ASP.NET_SessionId=aisncy3nsdcz2snnpc045y1h
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/7.5
Set-Cookie: CtxsAuthId=5989E6314A2ADEB10D4F4A71093CAF6D; path=/
Citrix/StoreWeb/; secure; HttpOnly
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 17 Jan 2014 17:39:44 GMT
Content-Length: 255
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticationStatus">
  <Result>success</Result>
  <AuthType>Certificate</AuthType>
</AuthenticationStatus>
```

### 示例：失败的直通身份验证

在此示例中，计算机不在域中，并且用户取消了输入域凭据的浏览器提示。

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/DomainPassthroughAuth/
Login HTTP/1.1
Host: webserver
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Csrf-Token: 74F22328CCE9A0C62097A5A77FC7D58B
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=74F22328CCE9A0C62097A5A77FC7D58B;
ASP.NET_SessionId=flz4othwj5g5sc0fw3ywtzos
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 401 Unauthorized
Content-Type: text/html; charset=utf-8
Server: Microsoft-IIS/7.5
WWW-Authenticate: Negotiate
WWW-Authenticate: NTLM
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 17 Jan 2014 17:44:14 GMT
Content-Length: 1293
Proxy-Support: Session-Based-Authentication
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html;
charset=iso-8859-1"/>
<title>401 - Unauthorized: Access is denied due to invalid
credentials.</title>
<style type="text/css">
<!--
body{margin:0;font-size:.7em;font-family:Verdana, Arial,
Helvetica, sans-serif;background:#EEEEEE;}
fieldset{padding:0 15px 10px 15px;}
h1{font-size:2.4em;margin:0;color:#FFF;}
h2{font-size:1.7em;margin:0;color:#CC0000;}
h3{font-size:1.2em;margin:10px 0 0 0;color:#000000;}
#header{width:96%;margin:0 0 0 0;padding:6px 2% 6px 2%;font-
family:"trebuchet MS", Verdana, sans-serif;color:#FFF;
background-color:#555555;}
#content{margin:0 0 0 2%;position:relative;}
.content-container{background:#FFF;width:96%;margin-top:
8px;padding:10px;position:relative;}
-->
</style>
</head>
<body>
<div id="header"><h1>Server Error</h1></div>
<div id="content">
 <div class="content-container"><fieldset>
 <h2>401 - Unauthorized: Access is denied due to invalid
credentials.</h2>
  <h3>You do not have permission to view this directory or
page using the credentials that you supplied.</h3>
 </fieldset></div>
</div>
</body>
</html>
```

## 显式表单身份验证

使用此请求可使用显式表单身份验证对用户进行身份验证。 显式身份验证（用户手动输入凭据）是使用 Citrix 通用表单协议执行的。 有关通用表单身份验证的详细信息，请参阅 Citrix StoreFront API 文档。

客户端通过向（/Authentication/GetAuthMethods 返回的）Web 代理显式表单 URL 发出请求以获取可能的表单系列中的第一个表单来启动表单对话。 Web 代理将表单请求转发至 StoreFront 身份验证服务并将答复传递回客户端。 Web 代理不会修改所有表单，但唯一的例外是所有回发 URL 都会被修改为指向 Web 代理 URL，以使客户端从不直接联系身份验证服务。

表单总是以 XML 格式返回，该格式必须由客户端解析，转换为相应的 HTML 并向最终用户显示。 用户完成并提交表单后，客户端会将表单数据发布回在原始表单 XML 中提供的回发 URL。 这可能会导致向客户端发送更多表单以请求用户提供其他信息。 客户端将继续执行呈现和提交表单的过程，直至收到身份验证成功或失败响应。

客户端不得假定返回的初始表单代表登录表单。 通常是这种情况，但在某些情况下可能会显示另一个表单；例如，如果配置了密码过期策略并且用户的密码已过期，则会返回更改密码表单。 第三方也可能会使用 StoreFront 身份验证 SDK 在身份验证过程中注入自定义表单。

### 请求

| URL（仅供参考）                              | 方法   | 接受 POST 数据 | 说明                                                              |
| -------------------------------------- | ---- | ---------- | --------------------------------------------------------------- |
| /ExplicitAuth/Login                    | POST | 否          | 请求显式表单对话中的第一个表单。URL 由 / Authentication/ GetAuthMethods 返回。      |
| /ExplicitAuth/ LoginAttempt            | POST | 是          | 尝试显式登录。URL 以登录表单中的 PostBack 元素形式返回。                             |
| /ExplicitAuth/ SendForm                | POST | 是          | 将表单代理回 StoreFront 身份验证服务。URL 以表单中的 PostBack 元素形式返回。             |
| /ExplicitAuth/ GetChangeCredentialForm | POST | 否          | 请求更改凭据表单。在 StoreFront 身份验证服务中启用了选择性更改密码支持时使用。URL 在 RfWeb 配置中提供。 |
| /ExplicitAuth/ CancelForm              | POST | 否          | 取消当前表单。URL 以表单中的 CancelPostBack 元素形式返回。                         |

用户提交表单（例如，通过激活登录按钮）时，客户端必须按 Citrix StoreFront API 文档中的“Citrix Receiver Common Authentication Forms Language v2.0”（Citrix Receiver 通用身份验证表单语言 v2.0）文档的“Processing Form Data”（处理表单数据）部分所述处理用户提供的数据。

!!! tip "注意" * 如果客户端省略了发布所需的任何数据，身份验证服务将拒绝表单，导致登录失败。 * 用户提交致使身份验证成功的表单后，StoreFront 身份验证服务将为用户返回一个主身份验证令牌。 所有令牌，包括主身份验证令牌以及其他 StoreFront 服务（例如，资源服务）后续所需的任何辅助令牌，都将代表用户保存在 Web 代理上，以使客户端不需要处理身份验证系统的复杂性。 * 令牌保存在 Web 代理中的 ASP.NET 会话对象中，通过 ASP.NET_SessionId cookie 绑定到用户的浏览器会话。

### 响应

| 响应代码 | 说明                          |
| ---- | --------------------------- |
| 200  | 成功、失败或 XML 表单               |
| 403  | 由于以下原因之一被禁止：                |
|      | * CSRF 令牌无效                 |
|      | * X-Citrix-IsUsingHTTPS 头无效 |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| XML  | application/xml        |

### 成功响应内容

返回成功响应（HTTP 状态代码 200）时，响应正文中将包含一个根节点为 AuthenticationStatus 或 AuthenticateResponse 的 XML 文档。

AuthenticationStatus 响应指示成功或失败，并且包含以下子元素：

| 元素名称                        | 必需 | 说明                                                                                                                       |
| --------------------------- | -- | ------------------------------------------------------------------------------------------------------------------------ |
| Result                      | 是  | 值为“success”或“failure”。                                                                                                   |
| AuthType                    | 否  | 身份验证类型，通过身份验证方法字符串“ExplicitForms”、“IntegratedWindows”、“Certificate”、“CitrixAGBasic”、“PostCredentials” 之一指示。 仅在身份验证成功时设置。 |
| LogMessage                  | 否  | 指示身份验证失败原因的错误 ID。 通常返回泛型值“fatalerror”，但是，如果 Citrix Receiver for Web 会话已超时，则会返回“sessiontimeout”。 仅在身份验证失败时发送。             |
| IsChangePasswordEnabled     | 否  | 值为“true”。仅在身份验证成功并且启用了选择性更改密码时设置。                                                                                        |
| IsExpiryNotificationEnabled | 否  | 值为“true”。仅在身份验证成功、启用了密码过期通知并且用户的密码在所配置的时间内即将过期时设置。                                                                       |
| TimeRemaining               | 否  | 用户的密码过期之前的剩余时间（单位为分钟）。 仅在身份验证成功、启用了密码过期通知并且用户的密码在所配置的时间内即将过期时设置。                                                         |

`AuthenticateResponse` 是向用户显示的表单的 XML 描述，用于收集身份验证操作（通常为登录以更改密码）所需的其他信息。 有关各种表单元素的信息，请参阅“通用表单身份验证”文档。

### 示例：显式身份验证 — 返回登录表单

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/ExplicitAuth/Login HTTP/
1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Csrf-Token: 23E18D9002817048C931EA636E0D5C81
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CtxsAuthMethod=ExplicitForms;
CsrfToken=23E18D9002817048C931EA636E0D5C81;
ASP.NET_SessionId=hdqnxu21dz3p2nb2zcfo2sfs
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/
vnd.citrix.authenticateresponse-1+xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
X-Citrix-ExplicitAuthProtocol: ExplicitForms
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 17 Jan 2014 16:25:09 GMT
Content-Length: 1555
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticateResponse xmlns="http://citrix.com/authentication/
response/1">
  <Status>success</Status>
  <Result>more-info</Result>
  <StateContext />
  <AuthenticationRequirements>
    <PostBack>ExplicitAuth/LoginAttempt</PostBack>
    <CancelPostBack>ExplicitAuth/CancelForm</CancelPostBack>
    <CancelButtonText>Cancel</CancelButtonText>
    <Requirements>
      <Requirement>
    <Credential>
      <ID>username</ID>
      <SaveID>ExplicitForms-Username</SaveID>
      <Type>username</Type>
    </Credential>
    <Label>
      <Text>User name:</Text>
      <Type>plain</Type>
    </Label>
    <Input>
      <AssistiveText>domain\user or user@domain.com</
AssistiveText>
      <Text>
        <Secret>false</Secret>
        <ReadOnly>false</ReadOnly>
        <InitialValue>
        </InitialValue>
        <Constraint>.+</Constraint>
      </Text>
    </Input>
      </Requirement>
      <Requirement><Credential>
      <ID>password</ID>
      <SaveID>ExplicitForms-Password</SaveID>
      <Type>password</Type>
    </Credential>
    <Label>
      <Text>Password:</Text>
      <Type>plain</Type>
    </Label>
    <Input>
      <Text>
        <Secret>true</Secret>
        <ReadOnly>false</ReadOnly>
        <InitialValue>
        </InitialValue>
        <Constraint>.+</Constraint>
      </Text>
    </Input>
      </Requirement>
      <Requirement>
    <Credential>
      <ID>saveCredentials</ID>
      <Type>savecredentials</Type>
    </Credential>
    <Label>
      <Text>Remember my password</Text>
      <Type>plain</Type>
    </Label>
    <Input>
      <CheckBox>
        <InitialValue>false</InitialValue>
      </CheckBox>
    </Input>
      </Requirement>
      <Requirement>
    <Credential>
      <ID>loginBtn</ID>
      <Type>none</Type>
    </Credential>
    <Label>
      <Type>none</Type>
    </Label>
    <Input>
      <Button>Log On</Button>
    </Input>
      </Requirement>
    </Requirements>
  </AuthenticationRequirements>
</AuthenticateResponse>
```

客户端会将 XML 表单描述转换为 HTML 表示，然后动态添加到 Web 页面 DOM。 上述 XML 会致使产生一个由一个**用户名**标签和文本输入字段、一个**密码**标签和密码输入字段以及一个**登录**按钮组成的 HTML 表单。

用户在输入字段中键入其凭据，并通过单击**登录**按钮提交表单。

客户端拦截表单提交并按照通用表单身份验证规则构建要发送回 Web 代理的表单数据（以转发到身份验证服务）。 表单将提交到 PostBack URL；在这种情况下为 Authentication/LoginAttempt。 但是，用户输错了密码，导致身份验证服务返回另一个登录表单 XML 文档，此时将突出显示该错误以及一个额外的 Requirements 节点。

### 示例：显式身份验证 — 提交了不正确的凭据

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/ExplicitAuth/
LoginAttempt HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: E2FF5D342BA9193DF707062EA7A31C54
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 72
Cookie: CtxsAuthMethod=ExplicitForms;
CsrfToken=E2FF5D342BA9193DF707062EA7A31C54;
ASP.NET_SessionId=1zfyotwrcjr3uzry3jbqcpvg
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
username=acmecorp%5Cuser1&password=rubbish&loginBtn=Log
+On&StateContext=
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
Set-Cookie: CtxsAuthId=22BE25569A283D63B1C5668580119320; path=/
Citrix/StoreWeb/; HttpOnly
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 17 Jan 2014 16:42:49 GMT
Content-Length: 157
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticationStatus>
  <Result>success</Result>
  <AuthType>ExplicitForms</AuthType>
</AuthenticationStatus>
```

### 示例：显式身份验证 — 密码已过期

如果 StoreFront 配置为允许用户更改过期的密码，并且用户在其密码已过期时尝试登录，则当用户提交其凭据时将返回一个更改密码 XML 表单。 此表单包括一个用于通知用户必须更改其密码的标签和几个文本输入字段（以及相应的标签），以供用户输入旧密码以及输入并确认新密码。 请注意，回发 URL 也会更新为指回 Web 代理 URL Authentication/ChangePassword。

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/ExplicitAuth/
LoginAttempt HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: A75AC234CA8C565EEEBE96FF81E495BA
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 70
Cookie: CtxsAuthMethod=ExplicitForms;
CsrfToken=A75AC234CA8C565EEEBE96FF81E495BA;
ASP.NET_SessionId=ulemrewlolkvhallkgcbwm3l
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
username=acmecorp%5Cuser1&password=mypassword&loginBtn=Log
+On&StateContext=
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/
vnd.citrix.authenticateresponse-1+xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
X-Citrix-ExplicitAuthProtocol: ExplicitForms
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Mon, 20 Jan 2014 10:53:59 GMT
Content-Length: 2111
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticateResponse xmlns="http://citrix.com/authentication/
response/1">
  <Status>success</Status>
  <Result>update-credentials</Result>
  <StateContext />
  <AuthenticationRequirements>
    <PostBack>ExplicitAuth/SendForm</PostBack>
    <CancelPostBack>ExplicitAuth/CancelForm</CancelPostBack>
    <CancelButtonText>Cancel</CancelButtonText>
    <Requirements>
      <Requirement>
    <Credential>
      <Type>none</Type>
    </Credential>
    <Label>
      <Text>Change Password</Text>
      <Type>heading</Type>
    </Label>
    <Input />
      </Requirement>
      <Requirement>
    <Credential>
      <Type>none</Type>
    </Credential>
    <Label>
      <Text>Your password has expired and must be changed.</
Text>
      <Type>information</Type>
    </Label>
    <Input />
      </Requirement>
      <Requirement>
    <Credential>
  <Type>username</Type>
</Credential>
<Label>
  <Text>User name:</Text>
  <Type>plain</Type>
</Label>
<Input>
  <Text>
    <Secret>false</Secret>
    <ReadOnly>true</ReadOnly>
    <InitialValue>acmecorp\user1</InitialValue>
    <Constraint>.+</Constraint>
  </Text>
</Input>
  </Requirement>
  <Requirement>
<Credential>
  <ID>oldPassword</ID>
  <Type>password</Type>
</Credential>
<Label>
  <Text>Old password:</Text>
  <Type>plain</Type>
</Label>
<Input>
  <Text>
    <Secret>true</Secret>
    <ReadOnly>false</ReadOnly>
    <InitialValue>
    </InitialValue>
    <Constraint>.+</Constraint>
  </Text>
</Input>
  </Requirement>
  <Requirement>
<Credential>
  <ID>newPassword</ID>
  <SaveID>ExplicitForms-Password</SaveID>
  <Type>newpassword</Type>
</Credential>
<Label>
  <Text>New password:</Text>
  <Type>plain</Type>
</Label>
<Input>
  <Text>
    <Secret>true</Secret>
    <ReadOnly>false</ReadOnly>
    <InitialValue>
    </InitialValue>
    <Constraint>.+</Constraint>
  </Text>
</Input>
  </Requirement>
  <Requirement>
<Credential>
  <ID>confirmPassword</ID>
<Type>newpassword</Type>
    </Credential>
    <Label>
      <Text>Confirm password:</Text>
      <Type>plain</Type>
    </Label>
    <Input>
      <Text>
        <Secret>true</Secret>
        <ReadOnly>false</ReadOnly>
        <InitialValue>
        </InitialValue>
        <Constraint>.+</Constraint>
      </Text>
    </Input>
      </Requirement>
      <Requirement>
    <Credential>
      <ID>changePasswordBtn</ID>
      <Type>none</Type>
    </Credential>
    <Label>
      <Type>none</Type>
    </Label>
    <Input>
      <Button>OK</Button>
    </Input>
      </Requirement>
    </Requirements>
  </AuthenticationRequirements>
</AuthenticateResponse>
```

用户输入请求的信息并提交表单时，身份验证服务将返回一个深层表单以供用户确认其密码是否已成功更改。 请注意，此请求指定在上述表单中指定的按钮 ID changePasswordBtn；无法指定正确的按钮 ID 会导致表单被拒绝。 确认表单仅由一个文本标签和“确定”按钮组成。

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/ExplicitAuth/SendForm
HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: 94FEF17799905C51ADD4599AC0056209
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 95
Cookie: CtxsAuthMethod=ExplicitForms;
CsrfToken=94FEF17799905C51ADD4599AC0056209;
ASP.NET_SessionId=01omrcuoenllin1eugyr142j
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
oldPassword=mypassword&newPassword=newpassword&confirmPassword=
newpassword&changePasswordBtn=OK&StateContext=
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/
vnd.citrix.authenticateresponse-1+xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
X-Citrix-ExplicitAuthProtocol: ExplicitForms
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Mon, 20 Jan 2014 11:13:18 GMT
Content-Length: 720
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticateResponse xmlns="http://citrix.com/authentication/
response/1">
  <Status>success</Status>
  <Result>more-info</Result>
  <StateContext />
  <AuthenticationRequirements>
    <PostBack>ExplicitAuth/SendForm</PostBack>
    <CancelPostBack>
    </CancelPostBack>
    <Requirements>
      <Requirement>
    <Credential>
      <Type>none</Type>
    </Credential>
    <Label>
      <Text>Your password has been changed successfully.</Text>
      <Type>confirmation</Type>
    </Label>
    <Input />
      </Requirement>
      <Requirement>
    <Credential>
      <ID>changePasswordConfirmBtn</ID>
      <Type>none</Type>
    </Credential>
    <Label>
      <Type>none</Type>
    </Label>
    <Input>
      <Button>OK</Button>
    </Input>
      </Requirement>
    nticationRequirements>
</AuthenticateResponse>
```

用户单击“确定”按钮后，将提交确认表单并获取成功身份验证响应。

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/ExplicitAuth/SendForm
HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: 94FEF17799905C51ADD4599AC0056209
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 41
Cookie: CtxsAuthMethod=ExplicitForms;
CsrfToken=94FEF17799905C51ADD4599AC0056209;
ASP.NET_SessionId=01omrcuoenllin1eugyr142j
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
changePasswordConfirmBtn=OK&StateContext=
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
Set-Cookie: CtxsAuthId=7BFB98069F1D33A0FB4268F702B0C983; path=/
Citrix/StoreWeb/; HttpOnly
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Mon, 20 Jan 2014 11:13:24 GMT
Content-Length: 157
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticationStatus>
  <Result>success</Result>
  <AuthType>ExplicitForms</AuthType>
</AuthenticationStatus>
```

### 示例：显式身份验证 — 选择性更改密码

如果 StoreFront 配置为允许用户随时更改其密码，并且用户在其密码在所配置的过期期限内即将过期时尝试登录，则将返回成功身份验证响应。 此响应包括用于指示过期通知已启用以及密码过期之前的剩余天数的附加元素。

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/ExplicitAuth/
LoginAttempt HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: 94FD74E2F4DF05D6B0642D25021F632A
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 70
Cookie: CtxsAuthMethod=ExplicitForms;
CsrfToken=94FD74E2F4DF05D6B0642D25021F632A;
ASP.NET_SessionId=mgucndg5j24cgvymxgrjblr4
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
username=acmecorp%5Cuser1&password=mypassword&loginBtn=Log
+On&StateContext=

####Response
```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
Set-Cookie: CtxsAuthId=F431D32D995E3CF6A59F5728A0F54085; path=/
Citrix/StoreWeb/; HttpOnly
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Mon, 20 Jan 2014 11:25:46 GMT
Content-Length: 320
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticationStatus>
  <Result>success</Result>
  <AuthType>ExplicitForms</AuthType>
  <IsChangePasswordEnabled>true</IsChangePasswordEnabled>
  <IsExpiryNotificationEnabled>true</
IsExpiryNotificationEnabled>
  TimeRemaining>89</TimeRemaining>
</AuthenticationStatus>
```

客户端随后可能会提供一个供用户随时使用从 XPath /clientSettings/authManager 位置处的 Receiver for Web 配置中获取的 changeCredentials URL 启动选择性更改密码请求的界面。 请求此 URL 会启动一个通用表单协议对话，方式与 /ExplicitAuth/ Login 类似。 如果用户在输入无效数据（或省略所需数据）后提交表单，则可能会生成深层表单。 客户端必须准备好连续接受多个表单并处理这些表单，直至收到成功或失败响应。 如果密码更改成功，则将返回一个密码确认表单；提交确认表单时，将返回一个成功 AuthenticationStatus 响应。 换言之，更改密码对话完全按照用户的密码在登录时已过期情况下的示例中所述继续进行。

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Authentication/
GetChangeCredentialForm HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Csrf-Token: A4BF7CB3334846575421F41AD44EBA67
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CtxsAuthMethod=ExplicitForms;
CsrfToken=A4BF7CB3334846575421F41AD44EBA67;
CtxsAuthId=910C6BFC2C98184CA57F77556041E0D3;
ASP.NET_SessionId=cxd25yc3xwq10qevwpxywyxu
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/
vnd.citrix.authenticateresponse-1+xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
X-Citrix-ExplicitAuthProtocol: ExplicitForms
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Mon, 20 Jan 2014 11:48:00 GMT
Content-Length: 2097
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticateResponse xmlns="http://citrix.com/authentication/
response/1">
  <Status>success</Status>
  <Result>update-credentials</Result>
  <StateContext />
  <AuthenticationRequirements>
    <PostBack>ExplicitAuth/SendForm</PostBack>
    <CancelPostBack>ExplicitAuth/CancelForm</CancelPostBack>
    <CancelButtonText>Cancel</CancelButtonText>
    <Requirements>
      <Requirement>
    <Credential>
      <Type>none</Type>
    </Credential>
    <Label>
      <Text>Change Password</Text>
      <Type>heading</Type>
    </Label>
    <Input />
      </Requirement>
      <Requirement>
    <Credential>
      <Type>none</Type>
    </Credential>
    <Label>
      <Text>Enter your old and new passwords</Text>
      <Type>information</Type>
    </Label>
    <Input />
      </Requirement>
      <Requirement>
    <Credential>
      <Type>username</Type>
    </Credential>
    <Label>
      <Text>User name:</Text>
      <Type>plain</Type>
    </Label>
    <Input>
      <Text>
        <Secret>false</Secret>
        <ReadOnly>true</ReadOnly>
        <InitialValue>acmecorp\user1</InitialValue>
        <Constraint>.+</Constraint>
      </Text>
    </Input>
      </Requirement>
      <Requirement>
    <Credential>
      <ID>oldPassword</ID>
      <Type>password</Type>
    </Credential>
    <Label>
      <Text>Old password:</Text>
      <Type>plain</Type>
    </Label>
<Input>
<Text>
    <Secret>true</Secret>
    <ReadOnly>false</ReadOnly>
    <InitialValue>
    </InitialValue>
    <Constraint>.+</Constraint>
  </Text>
</Input>
  </Requirement>
  <Requirement>
<Credential>
  <ID>newPassword</ID>
  <SaveID>ExplicitForms-Password</SaveID>
  <Type>newpassword</Type>
</Credential>
<Label>
  <Text>New password:</Text>
  <Type>plain</Type>
</Label>
<Input>
  <Text>
    <Secret>true</Secret>
    <ReadOnly>false</ReadOnly>
    <InitialValue>
    </InitialValue>
    <Constraint>.+</Constraint>
  </Text>
</Input>
  </Requirement>
  <Requirement>
<Credential>
  <ID>confirmPassword</ID>
  <Type>newpassword</Type>
</Credential>
<Label>
  <Text>Confirm password:</Text>
  <Type>plain</Type>
</Label>
<Input>
  <Text>
    <Secret>true</Secret>
    <ReadOnly>false</ReadOnly>
    <InitialValue>
    </InitialValue>
    <Constraint>.+</Constraint>
  </Text>
</Input>
  </Requirement>
  <Requirement>
<Credential>
  <ID>changePasswordBtn</ID>
  <Type>none</Type>
</Credential>
<Label>
  <Type>none</Type>
</Label>
<Input>
 <Button>OK</Button>
    </Input>
      </Requirement>
    </Requirements>
  </AuthenticationRequirements>
</AuthenticateResponse>
```

通过选择性更改密码，用户已通过对 Citrix Receiver for Web 的身份验证。 由于更改密码请求不是访问站点的必需请求，因此，客户端 UI 应在表单中提供一个“取消”按钮。 如果用户选择取消，则必须使用上述表单中的“Web 代理取消回发 URL”来答复身份验证服务，告知其请求已被取消。

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/ExplicitAuth/CancelForm
HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: 3E5784A870D03FF16BF7A37415F9E0AB
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 73
Cookie: CtxsAuthMethod=ExplicitForms;
CsrfToken=3E5784A870D03FF16BF7A37415F9E0AB;
CtxsAuthId=C389FA6A3BD8E1E3811C184C01918411;
ASP.NET_SessionId=chgeeknlk5aiewkyyqfwp43p
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
oldPassword=&newPassword=&confirmPassword=&cancelBtn=Cancel&Sta
teContext=
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/
vnd.citrix.authenticateresponse-1+xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Mon, 20 Jan 2014 12:02:33 GMT
Content-Length: 201
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticateResponse xmlns="http://citrix.com/authentication/
response/1">
  <Status>success</Status>
  <Result>cancelled</Result>
  <StateContext />
</AuthenticateResponse>
```

## POST 凭据身份验证

使用 POST 凭据身份验证时，客户端直接使用 HTTP POST 提供用户的凭据。 这将提供比显式表单更加简单的身份验证机制，因为客户端不需要解析 XML 表单以及处理表单对话。 但是，此方法不如显式表单身份验证灵活，并且不支持更改密码机制（使用通用表单协议）。

### 请求

| URL（仅供参考）                   | 方法   | 说明                                                                    |
| --------------------------- | ---- | --------------------------------------------------------------------- |
| /PostCredentialsAuth/ Login | POST | 使用在 POST 主体中提供的凭据对用户进行身份验证。URL 由 / Authentication/ GetAuthMethods 返回。 |

| POST 参数  | 说明                                                              |
| -------- | --------------------------------------------------------------- |
| username | 用户名，包括域规范（如有需要），SAM 或 UPN 格式。例如，domain\fred 或 fred@some.domain |
| password | 密码。                                                             |

!!! tip "注意" * 必须为身份验证服务启用 Http Basic 身份验证方法，因为 Web 代理在内部使用 HttpBasic 对 StoreFront 验证用户的身份。 * 必须提供非空用户名字符串值，但是必须为密码提供空值。

| 响应代码 | 说明                          |
| ---- | --------------------------- |
| 200  | 成功或失败                       |
| 400  | 未提供用户名或密码。                  |
| 403  | 由于以下原因之一被禁止：                |
|      | * CSRF 令牌无效                 |
|      | * X-Citrix-IsUsingHTTPS 头无效 |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| XML  | application/xml        |

### 成功响应内容

返回成功响应（HTTP 状态代码 200）时，响应正文中将包含一个根节点为 AuthenticationStatus 并且包含以下子元素的 XML 文档：

| 元素名称       | 必需 | 说明                                                                                                                                          |
| ---------- | -- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Result     | 是  | 值为“success”或“failure”。                                                                                                                      |
| AuthType   | 否  | 身份验证类型，通过身份验证方法字符串“ExplicitForms”、“IntegratedWindows”、“Certificate”、“CitrixAGBasic”、“PostCredentials” 之一指示。 仅在身份验证成功时设置。                    |
| LogMessage | 否  | 指示身份验证失败原因的错误 ID。 通常返回泛型值“fatalerror”，但是，如果 Citrix Receiver for Web 会话已超时，则会返回“sessiontimeout”。 凭据被身份验证服务拒绝时，将返回“loginfailed”。 仅在身份验证失败时发送。 |

### 示例：成功的 POST 凭据身份验证

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/PostCredentialsAuth/
Login HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: 04416E5028580B5783E1A051732214B4
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/ApiExample.html
Content-Length: 42
Cookie: CsrfToken=04416E5028580B5783E1A051732214B4;
ASP.NET_SessionId=m5utlgwykupmhhbq11c2gt0s
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
username=acmecorp%5Cuser1&password=mypassword
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
Set-Cookie: CtxsAuthId=488731E2C57EF91E84729A3ECB3E5CAF; path=/
Citrix/StoreWeb/; HttpOnly
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Tue, 28 Jan 2014 13:44:18 GMT
Content-Length: 159
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticationStatus>
  <Result>success</Result>
  <AuthType>PostCredentials</AuthType>
</AuthenticationStatus> 
```

## NetScaler Gateway 单点登录

使用此请求可自动对已通过 NetScaler Gateway 的身份验证的用户进行身份验证。

### 请求

| URL                | 方法   | 说明                                  |
| ------------------ | ---- | ----------------------------------- |
| /GatewayAuth/Login | POST | 使用 NetScaler Gateway 单点登录对用户进行身份验证。 |

!!! tip "注意" * 通过 NetScaler Gateway 访问 Citrix Receiver for Web 时，登录期间更改过期的密码由网关进行处理。 * 由于显式表单身份验证中不涉及 Citrix Receiver for Web，因此，在用户的密码即将过期时不提供密码过期通知。 * 启用了选择性更改密码并且用户成功更改了其密码时，客户端必须注销用户，因为没有与 NetScaler Gateway 进行同步的方法。 用户之后必须再次登录网关。 * 身份验证失败时，StoreFront 事件日志可能会显示更多信息以帮助诊断故障。

### 响应

| 响应代码 | 说明                          |
| ---- | --------------------------- |
| 200  | 成功或失败                       |
| 403  | 由于以下原因之一被禁止：                |
|      | * CSRF 令牌无效                 |
|      | * X-Citrix-IsUsingHTTPS 头无效 |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| XML  | application/xml        |

### 成功响应内容

返回成功的响应（HTTP 状态代码 200）时，响应正文中将包含一个根节点为 AuthenticationStatus 并且包含与为显式身份验证所描述的子元素相同的子元素的 XML 文档。

### 示例：成功的 NetScaler Gateway 身份验证

#### 请求

```curl
POST https://gateway.acme.com/Citrix/StoreWeb/GatewayAuth/
Login HTTP/1.1
Host: gateway.acme.com
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:26.0) Gecko/
20100101 Firefox/26.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-us,en;q=0.7,fr;q=0.3
Accept-Encoding: gzip, deflate
Csrf-Token: AD11BBE759D84186CE8753ABA4246FDD
X-Citrix-IsUsingHTTPS: Yes
X-Requested-With: XMLHttpRequest
Referer: https://gateway.acme.com/Citrix/StoreWeb
Cookie: CsrfToken=AD11BBE759D84186CE8753ABA4246FDD;
NSC_AAAC=84dc5a2985fe2cdddcb18697f37d49ec0094e9e2145525d5f4f584
55e445a4a42; ASP.NET_SessionId=rozslfqggjk32jbjggkwptpj
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
Response
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/xml; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
Set-Cookie: CtxsAuthId=9ADB4CFDA7CE38BEA0C64A64C5914145; path=/
Citrix/StoreWeb/; secure; HttpOnly
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Mon, 20 Jan 2014 15:27:41 GMT
Content-Length: 216
<?xml version="1.0" encoding="UTF-8"?>
<AuthenticationStatus>
  <Result>success</Result>
  <AuthType>CitrixAGBasic</AuthType>
  <IsChangePasswordEnabled>true</IsChangePasswordEnabled>
</AuthenticationStatus>
```

## 获取用户名

使用此请求可获取在 Active Directory 中所配置的完整用户名。如果完整用户名不可用，则改为返回用户的登录名。

### 请求

| URL                          | 方法   | 说明                                                                                                    |
| ---------------------------- | ---- | ----------------------------------------------------------------------------------------------------- |
| /Authentication/ GetUserName | POST | 返回用户名。使用 XPath /clientSettings/ authManager 位置处的 getUsernameURL 属性从 Citrix Receiver for Web 配置中获取 URL |

!!! tip "注意" * 此请求需要已通过身份验证的会话，由 cookie ASP.NET_SessionId 和 CtxsAuthId 指示。 使用未经身份验证的应用商店时，所有用户实际都未登录，并返回 HTTP 403 响应。 * Web 代理使用 StoreFront 令牌验证服务从身份验证令牌中获取用户名。

### 响应

| 响应代码 | 说明                                                 |
| ---- | -------------------------------------------------- |
| 200  | 成功或身份验证质询                                          |
| 403  | 由于以下原因之一被禁止：                                       |
|      | * CSRF 令牌无效                                        |
|      | * X-Citrix-IsUsingHTTPS 头无效并且 CtxsAuthId cookie 无效 |
|      | * 用户尚未登录时，服务器会话中缺少身份验证令牌                           |

### 成功响应内容

如果返回成功响应（HTTP 状态代码 200 OK），响应正文中将包含纯文本字符串格式的用户名，编码为 UTF-8。

### 示例：获取用户名

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Authentication/
GetUserName HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:24.0) Gecko/
20100101 Firefox/24.0
Accept: text/plain, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: 128B2AA41FE56F42CA4550FEE88198CB
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=128B2AA41FE56F42CA4550FEE88198CB;
CtxsAuthId=82221BEE46C4C14CAFD2405F9A3C600F;
ASP.NET_SessionId=5cfuoz0dc0p3y0o2nidhglkh
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: text; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 18 Oct 2013 13:43:17 GMT
Content-Length: 9
Joe Blogs
```

## 注销

使用此请求可注销用户的 Web 会话。

### 请求

| URL                    | 方法   | 说明                                                                                                       |
| ---------------------- | ---- | -------------------------------------------------------------------------------------------------------- |
| /Authentication/Logoff | POST | 终止用户的 Web 会话。使用 XPath /clientSettings/ authManager 位置处的 logoffURL 属性从 Citrix Receiver for Web 配置中获取 URL。 |

!!! tip "注意" * 此请求需要已通过身份验证的会话，由 cookie ASP.NET_SessionId 和 CtxsAuthId 指示。 * 调用 StoreFront 身份验证服务的目的是销毁用户的主令牌。 * Web 会话已终止并且清除了以下 cookie：ASP.NET_SessionId、CtxsAuthId, CsrfToken。

### 响应

| 响应代码 | 说明                                                 |
| ---- | -------------------------------------------------- |
| 200  | 成功                                                 |
| 403  | 由于以下原因之一被禁止：                                       |
|      | * CSRF 令牌无效                                        |
|      | * X-Citrix-IsUsingHTTPS 头无效并且 CtxsAuthId cookie 无效 |

### 成功响应内容

如果返回成功响应（HTTP 状态代码 200 OK），则响应正文为空。

### 示例：注销

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Authentication/Logoff
HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:24.0) Gecko/
20100101 Firefox/24.0
Accept: application/xml, text/xml, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: 128B2AA41FE56F42CA4550FEE88198CB
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=128B2AA41FE56F42CA4550FEE88198CB;
CtxsAuthId=82221BEE46C4C14CAFD2405F9A3C600F;
ASP.NET_SessionId=5cfuoz0dc0p3y0o2nidhglkh; AGSSOHasOccurred=;
PasswordChangeAllowed=
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/
vnd.citrix.webreceiver.authenticationlogoffresponse+xml
Expires: -1
Server: Microsoft-IIS/8.0
Set-Cookie: CsrfToken=128B2AA41FE56F42CA4550FEE88198CB;
expires=Tue, 18-Oct-1983 13:43:23 GMT; path=/Citrix/StoreWeb/
Set-Cookie: CtxsAuthId=82221BEE46C4C14CAFD2405F9A3C600F;
expires=Tue, 18-Oct-1983 13:43:23 GMT; path=/Citrix/StoreWeb/
Set-Cookie: ASP.NET_SessionId=; path=/
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 18 Oct 2013 13:43:22 GMT
Content-Length: 0
```

## 资源枚举

使用此请求可获取对用户可用的所有资源的列表。

### 请求

| URL（仅供参考）       | 方法   | 说明                        |
| --------------- | ---- | ------------------------- |
| /Resources/List | POST | 返回 JSON 格式的对用户可用的所有资源的列表。 |

| POST 参数          | 说明                            |
| ---------------- | ----------------------------- |
| resourcesDetails | 要在响应中包含的资源详细信息。使用受支持的以下值之一：   |
|                  | * Default — 一组默认的资源详细信息（见下文）。 |
|                  | * Full — 一组扩展的资源详细信息（见下文）。    |
|                  | 如果未指定参数，则假定使用“Default”。       |

!!! tip "注意" * 此请求通常需要已通过身份验证的会话，由 cookie ASP.NET_SessionId 和 CtxsAuthId 指示。 但是，Web 代理配置为使用未经身份验证的应用商店时，不需要已通过身份验证的会话。 * Web 代理始终通过与 StoreFront 应用商店服务进行通信以获取可能已发生的任何更改，为用户执行全新枚举。

### 响应

| 响应代码 | 说明                                                 |
| ---- | -------------------------------------------------- |
| 200  | 成功或身份验证质询                                          |
| 403  | 由于以下原因之一被禁止：CSRF 令牌无效                              |
|      | * X-Citrix-IsUsingHTTPS 头无效并且 CtxsAuthId cookie 无效 |
|      | * CtxsAuthId cookie 无效                             |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| JSON | application/json       |

### 成功响应内容

返回成功响应（HTTP 状态代码 200）时，响应正文中将包含一个用于描述对已通过身份验证的用户可用的资源的 JSON 对象。 此对象包含以下字段：

| 字段                      | JavaScript 类型 | 说明                                                   |
| ----------------------- | ------------- | ---------------------------------------------------- |
| resources               | 对象数组          | 对用户可用的每个已发布资源的详细信息。数组中的每个对象都与一个资源相对应，并且包含一组字段，如下文所述。 |
| isSubscriptionEna bled  | 布尔值           | 是否为应用商店启用订阅操作。                                       |
| isUnauthenticated Store | 布尔值           | 应用商店是否仅可在未进行第一次身份验证时访问。                              |

上表中的 resources 字段是资源对象的数组。每个资源对象始终包含下面一组字段：

| 字段                     | JavaScript 类型 | 说明                                                                                                                                                                                      |
| ---------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| clienttypes            | 字符串数组         | 用于标识支持启动的客户端的字符串值的数组。 已知值包括 **ica30**、**rade**、**rdp** (XenApp/XenDesktop)、**application/ios**、**application/android** (AppController)、**g2m**、**g2t** 等。 （使用通用应用程序的 Citrix Online 集成）。 |
| iconurl                | 字符串           | 用于请求资源的图标图像数据的 URL。                                                                                                                                                                     |
| id                     | 字符串           | 唯一资源标识符，由 Web 代理创建。                                                                                                                                                                     |
| launchstatusurl        | 字符串           | 用于确定资源是否已准备好启动的 URL（请参阅“ICA 启动”）。                                                                                                                                                       |
| launchurl              | 字符串           | 用于发出启动请求的 URL（请参阅“ICA 启动”）。                                                                                                                                                             |
| name                   | 字符串           | 资源的用户友好名称。                                                                                                                                                                              |
| path                   | 字符串           | 在资源的 XenApp/XenDesktop 站点（场）中定义的路径。                                                                                                                                                     |
| shortcutvalidation url | 字符串           | 应用程序快捷方式验证 URL，以确认是否能够使用快捷方式启动应用程序。                                                                                                                                                     |
| subscriptionstatus     | 字符串           | 订阅状态（请参阅“资源订阅”）。                                                                                                                                                                        |

此外，每个资源对象都可能包含以下可选字段：

| 字段                    | JavaScript 类型 | 说明                                                                          |
| --------------------- | ------------- | --------------------------------------------------------------------------- |
| assigndesktopurl      | 字符串           | 调用此 URL 以将桌面分配给用户。仅对 XenDesktop 首次使用时分配的桌面提供。                               |
| content               | 布尔值           | 如果资源表示文档而非已发布的应用程序/桌面，则与值 *true* 一起提供，否则将被省略。                               |
| description           | 字符串           | 资源的较长文本描述。                                                                  |
| desktopassignmenttype | 字符串           | 桌面分配类型：值 *shared*、*assign-on-first-use* 或 *assigned* 之一。仅对 XenDesktop 资源提供。 |
| desktophostname       | 字符串           | 桌面主机名（如果已知）。仅对 XenDesktop 已分配的桌面提供。                                         |
| disabled              | 布尔值           | 如果未启用资源，则与值 *true* 一起提供，否则将被省略。                                             |
| isdesktop             | 布尔值           | 如果资源为桌面，则与值 *true* 一起提供，否则将被省略。                                             |
| keywords              | 字符串数组         | 用于标识与资源关联的关键字的字符串值的数组。                                                      |
| mandatory             | 布尔值           | 如果资源通过后端资源提供程序标记为 mandatory，则与值 true 一起提供，否则将被省略。                           |
| position              | 字符串           | 表示 Citrix Receiver for Web UI 中的已订阅资源的相对位置的字符串。仅对已定义其位置的资源生成。               |
| poweroffurl           | 字符串           | 用于发出计算机关闭请求的 URL。仅当能够关闭资源时提供，如 StoreFront 资源服务所报告。                          |
| prelaunchurl          | 字符串           | 访问 AppController 预启动服务以获取单点登录的更新后的内容 URL 时要调用的 URL。                         |
| recommended           | 布尔值           | 如果资源通过后端资源提供程序标记为 **featured** 或 **recommended**，则与值 true 一起提供，否则将被省略。      |
| requiresvpn           | 布尔值           | 如果资源标记（使用资源服务 VPN 关键字）为仅当使用 VPN 时才可远程启动，则与值 true 一起提供，否则将被省略。               |
| requiresworkflow      | 布尔值           | 如果为资源启用了工作流，则与值 true 一起提供（通常由于存在 **WFS** 关键字）。                              |
| subscriptionurl       | 字符串           | 用于发出订阅请求的 URL（请参阅“资源订阅”）。                                                   |

`resourceDetails` 请求参数指定应执行“Full”枚举时，可能会返回以下附加的可选字段：

| 字段                     | JavaScript 类型 | 说明                                                                                                                     |
| ---------------------- | ------------- | ---------------------------------------------------------------------------------------------------------------------- |
| images                 | 对象数组          | 用于标识 StoreFront 服务服务器保留的资源图标的图像大小和颜色深度的对象的数组（可能用于确定在被客户端请求时哪些图像大小结果可能比较好）。 数组中的每个对象都包含一对名为 **size** 和 **depth** 的整数字段。 |
| playsfiletypes         | 对象数组          | 用于描述资源 [仅限应用程序] 支持的文件扩展名的对象的数组。数组中的每个对象可能都包含以下字段：                                                                      |
|                        |               | *name*（字符串）：文件类型关联名称                                                                                                   |
|                        |               | *description*（字符串）：文件类型关联描述                                                                                            |
|                        |               | *isdefault*（布尔值）：是否属于默认文件类型关联                                                                                          |
|                        |               | *fileextensions*（字符串数组）：此资源可以打开的文件扩展名的数组                                                                               |
|                        |               | *mimetypes*（字符串数组）：资源支持的 MIME 类型的数组                                                                                    |
|                        |               | *parameters*（字符串数组）：用于启动资源的参数的数组                                                                                       |
| properties             | 对象数组          | 资源属性 — 与资源关联的名称值对的数组，最终由资源提供程序提供。 数组中的每个对象都包含一个用于描述属性 *name* 的字符串字段名称以及一个用于描述属性值列表的字符串数组字段 *values*。                   |
| publishername          | 字符串           | 发布者的名称（XenApp 的场名称）。                                                                                                   |
| shortcuturl            | 字符串           | 应用程序快捷方式 URL，在将链接签入到门户页面中的资源时使用。                                                                                       |
| subscriptionproperties | 对象数组          | 订阅属性 — 与资源订阅关联并且存储在订阅数据库中的名称值对的数组。 数组中的每个对象都包含一个属性 *name* 的字符串字段名称以及一个属性值列表的可选字符串数组字段 *values*。 有关详细信息，请参阅“资源订阅”。      |
| type                   | 字符串           | 资源类型，通常为以下值之一：                                                                                                         |
|                        |               | * Citrix.MPS.Application                                                                                               |
|                        |               | * Citrix.MPS.Desktop                                                                                                   |
|                        |               | * Citrix.MPS.Document                                                                                                  |

!!! tip "注意" * iconurl 值按照在 Web.config 站点配置文件（serverSettings 中的 resourcesService 部分）中所配置的方式对特定图标大小进行编码。 StoreFront 应用商店服务返回最符合所请求的大小的图标。 * iconurl 表示执行枚举时的图像数据。 如果资源的图标随后发生变化，URL 将不指向更新后的图像。 但是，尽管可能会在 XenApp/XenDesktop 服务器上更新图标后返回 404，但 URL 仍保持不变，因为它始终指向相同的图标。 * subscriptionUrl 用于通过 POST 参数选择的操作来订阅以及取消订阅资源。

### 示例：请求所有资源

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Resources/List HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: EC8A9490C8D32616B21C3EED398FE9EE
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 32
Cookie: CsrfToken=EC8A9490C8D32616B21C3EED398FE9EE;
CtxsAuthId=BB8B836131AA35C54DF588E631838946;
ASP.NET_SessionId=jel5evcolmhl4bcm1i31exqo; AGSSOHasOccurred=;
PasswordChangeAllowed=
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
format=json&resourceDetails=Default
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Tue, 24 Sep 2013 11:03:37 GMT
Content-Length: xxxxx
{"resources":[
  {"clienttypes":["ica30","rdp"],
   "description":"",
   "iconurl":"Resources\/Icon\/
NUM2RDNDRDU1QzdBMUE0RjU0NjA4NjRBNTkwMzY0NzQyMEM2OTQ3QQ--\/48",
   "id":"Q29udHJvbGxlci4hJCVeJl8rLQ--",
   "launchurl":"Resources\/LaunchIca\/
Q29udHJvbGxlci4hJCVeJl8rLQ--",
   "name":"Microsoft Word",
   "path":"\\Office Apps\\",
   "shortcutvalidationurl":"Resources\/
ValidateAppShortcutLaunch\/Q29udHJvbGxlci4hJCVeJl8rLQ--",
   "subscriptionstatus":null,
   "subscriptionurl":"Resources\/Subscription\/
Q29udHJvbGxlci4hJCVeJl8rLQ--"
  },
  {"clienttypes":["ica30","rdp"],
   "description":"",
   "disabled":true,
   "iconurl":"Resources\/Icon\/
NUM2RDNDRDU1QzdBMUE0RjU0NjA4NjRBNTkwMzY0NzQyMEM2OTQ3QQ--\/48",
   "id":"Q29udHJvbGxlci4hJF4kJV4-",
   "launchurl":"Resources\/LaunchIca\/
Q29udHJvbGxlci4hJF4kJV4-",
   "name":"!$^$%^",
   "path":"\\Yabba\\",
   "shortcutvalidationurl":"Resources\/
ValidateAppShortcutLaunch\/Q29udHJvbGxlci4hJF4kJV4-",
   "subscriptionstatus":null,
   "subscriptionurl":"Resources\/Subscription\/
Q29udHJvbGxlci4hJF4kJV4-"
   },
... ],
 "subscriptionsEnabled":true,
}
```

### 示例：请求所有资源（完整枚举）

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Resources/List HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:24.0) Gecko/
20100101 Firefox/24.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: 1062D9DA90BA01931DB472C09D61A6EB
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 56
Cookie: CsrfToken=1062D9DA90BA01931DB472C09D61A6EB;
CtxsAuthId=FEB9E833C66E9D7B4124E04AEC12C3B5;
ASP.NET_SessionId=2zsqy14dgxdctivbs351fmvo
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
format=json&resourceDetails=Default&resourceDetails=Full
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Tue, 24 Sep 2013 11:03:37 GMT
Content-Length: xxxxx
{"categories":[],
 "resources":[
  {
    "type": "Citrix.MPS.Application",
    "subscriptionurl": "Resources\/Subscription\/
X83heiDGddg1ldasf87asdfg--",
    "shortcutvalidationurl": "Resources\/
ValidateAppShortcutLaunch\/X83heiDGddg1ldasf87asdfg--",
    "shortcuturl": "http:\/\/kontiki\/Citrix\/StoreWeb\/#\/
launch\/Notepad",
    "publishername": "foobar-ic6659",
    "path": "\\",
    "name": "Notepad",
    "launchurl": "Resources\/LaunchIca\/
X83heiDGddg1ldasf87asdfg--",
    "launchstatusurl": "Resources\/GetLaunchStatus\/
X83heiDGddg1ldasf87asdfg--",
    "images": [
{
"size": 48,
"depth": 4 },
{
"size": 32,
"depth": 4 },
{
"size": 48,
"depth": 8 },
{
"size": 32,
"depth": 8 },
       {
         "size": 256,
         "depth": 32
       },
{
"size": 48,
         "depth": 32
       },
{
"size": 32,
         "depth": 32
       },
     ],
    "id": "X83heiDGddg1ldasf87asdfg--",
    "iconurl": "Resources\/Icon\/
NUM2RDNDRDU1QzdBMUE0RjU0NjA4NjRBNTkwMzY0NzQyMEM2OTQ3QQ--?
size=48",
    "description": "Take note!",
    "clienttypes": [
"ica30",
"rdp" ]
},
... ],
 "subscriptionsEnabled":true,
}
```

## ICA 启动

使用此请求可检查资源是否已准备好启动，或者获取用于启动资源的 ICA 文件。

### 请求

| URL（仅供参考）                        | 方法   | 说明                  |
| -------------------------------- | ---- | ------------------- |
| /Resources/ GetLaunchStatus/{id} | POST | 请求指定资源是否已准备好启动。     |
| /Resources/LaunchIca/{id}        | GET  | 请求用于启动指定资源的 ICA 文件。 |

| 参数                      | 说明                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| {id}                    | 特定资源的唯一标识符。注意，此参数是 URL 的组件。                                                                                               |
| displayNameDesktopTitle | 桌面显示名称。 此名称插入到生成的 ICA 文件中作为“标题”设置的值。 此名称随后将在 Desktop Viewer 的标题栏中显示。 注意：这是 GetLaunchStatus 的 POST 参数和 LaunchIca 的查询字符串参数。 |

!!! tip "注意" * 这些请求通常需要已通过身份验证的会话，由 cookie ASP.NET_SessionId 和 CtxsAuthId 指示。 但是，Web 代理配置为使用未经身份验证的应用商店时，不需要已通过身份验证的会话。 * 上文指定的 URL 仅用于说明；客户端使用的实际 URL 必须从之前向 /Resources/List 发出的枚举请求的结果中获取。 * LaunchIca 属于 GET 而非 POST，以便其能够在隐藏的 iframe 中由浏览器加载（使用 iframe 的 src="url" 属性）以通过文件类型关联启动。 * GetLaunchStatus 向应用商店服务发出启动请求以确定指定资源是否已准备好启动。 如果此请求返回一个 ICA 文件，该 ICA 文件将缓存达 90 秒。 如果在此时间内对 LaunchIca 执行后续调用，则将返回缓存的 ICA 文件，但不向应用商店服务发出额外的启动请求。 * 对于 LaunchIca，客户端必须附加一个附带随机值的查询字符串键，以阻止浏览器在对相同的资源发出多个启动请求时返回缓存的响应。 请参见下文示例中的 launchId 查询字符串参数。 * 调用 GetResourceStatus 会触发启动桌面 VM。

### 响应

| 响应代码 | 说明                                        |
| ---- | ----------------------------------------- |
| 200  | 成功/需要重试/失败，或身份验证质询。                       |
| 403  | 由于以下原因之一被禁止：                              |
|      | * CSRF 令牌无效                               |
|      | * X-Citrix-IsUsingHTTPS 头无效（只有 POST 请求需要） |
|      | * CtxsAuthId cookie 无效                    |
| 404  | {id} 标识的资源对 Web 代理而言属于未知资源。               |

| 响应格式   | 请求接受/响应 Content-Type 头             |
| ------ | ---------------------------------- |
| JSON   | application/json 或 text/plain（见下文） |
| ICA 文件 | application/octet-stream           |

### 成功响应内容

返回 GetLaunchStatus 的成功响应（HTTP 状态代码 200）时，响应正文中将包含一个用于描述所提供的资源是否已准备好启动的 JSON 字符串以及以下字段：

| 属性             | JavaScript 类型 | 必需 | 说明                                                       |
| -------------- | ------------- | -- | -------------------------------------------------------- |
| status         | 字符串           | 是  | 以下字符串之一：                                                 |
|                |               |    | * success：资源已准备好启动。                                      |
|                |               |    | * retry：资源当前未准备好启动（例如，首先需要打开才能与其建立连接的桌面 VM），但可以以后重新尝试启动。 |
|                |               |    | * failure：尝试检索启动信息失败。                                    |
| pollTimeout    | 数值            | 否  |                                                          |
| errorId        | 字符串           | 否  |                                                          |
| suggestRestart | 布尔值           | 否  |                                                          |

返回 LaunchIca 的成功响应时，响应正文可以是 JSON 字符串（适用于 GetLaunchStatus，如上文所述），或者如果资源已准备好启动，则可以是 ICA 文件。 客户端通常会将 LaunchIca URL 加载到 iframe 中以获取 ICA 文件并通过文件类型关联触发启动。 如果 LaunchIca 返回 JSON 响应而非 ICA 文件，内容类型将设置为“text/plain”，因为 Internet Explorer 不会在 iframe 文档中呈现“application/json”响应，而是提示用户打开或保存该文档。 如果客户端使用 jQuery，则可以通过调用 $.parseJSON(responseData) 将 JSON 文本响应反序列化为 JavaScript 对象。

### 示例：成功的 ICA 启动

#### 请求

```curl
GET http://webserver/Citrix/StoreWeb/Resources/LaunchIca/
Q29udHJvbGxlci5EZXNrdG9w.ica?
CsrfToken=85E9295DFD9CF333F31F0A53B769D915&launchId=13800307750
58&displayNameDesktopTitle=Desktop HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: text/html,application/xhtml+xml,application/
xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=85E9295DFD9CF333F31F0A53B769D915;
CtxsAuthId=5A0B7A991C8136CE17A0D43113C00740;
ASP.NET_SessionId=jel5evcolmhl4bcm1i31exqo
Connection: keep-alive
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: private, max-age=0, s-maxage=0
Content-Type: application/octet-stream; charset=utf-8
Expires: Tue, 24 Sep 2013 13:52:55 GMT
Last-Modified: Tue, 24 Sep 2013 13:52:55 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Tue, 24 Sep 2013 13:52:55 GMT
Content-Length: 1263
[Encoding]
InputEncoding=UTF8
[WFClient]
ProxyFavorIEConnectionSetting=Yes
ProxyTimeout=30000
ProxyType=Auto
ProxyUseFQDN=Off
RemoveICAFile=yes
TransportReconnectEnabled=On
<... etc. >
```

### 示例：失败的 ICA 启动

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Resources/
GetLaunchStatus/Q29udHJvbGxlci5Db3B5IG9mIFdvcmRQYWQz HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: 2730933D4A9C7EFCDA52BD89EC345DD5
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=2730933D4A9C7EFCDA52BD89EC345DD5;
CtxsAuthId=669C20DF67B0ABC5C0E1F35451EF02A5;
ASP.NET_SessionId=jel5evcolmhl4bcm1i31exqo; AGSSOHasOccurred=;
PasswordChangeAllowed=
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Tue, 24 Sep 2013 15:49:50 GMT
Content-Length: 77
{"status":"failure"}
```

### 示例：重试 ICA 启动

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Resources/
GetLaunchStatus/Q29udHJvbGxlci5GaXNoY2FrZSE- HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:24.0) Gecko/
20100101 Firefox/24.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: 0035B0DB846D2399576612132C95AF33
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=0035B0DB846D2399576612132C95AF33;
CtxsAuthId=421E99814A2B99CFA57BB146EE036710;
ASP.NET_SessionId=vmia5swazanokecxwjefwvub
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
displayNameDesktopTitle=Desktop+Two!
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Tue, 15 Oct 2013 16:22:51 GMT
Content-Length: 34
{"status":"retry","pollTimeout":5}
```

## 图标

使用此请求可获取与一个或多个资源有关的图标图像数据。 此请求使用“指纹”的概念以允许与等效图像共享资源的图像数据。 “指纹”设计为对内容相同的图像相同。

### 请求

| URL（仅供参考）                                 | 方法  | 说明                                  |
| ----------------------------------------- | --- | ----------------------------------- |
| /Resources/Icon/ {thumbprint}?size={size} | GET | 返回指定的图标图像。有关格式的详细信息以及响应的详细信息，请参见下文。 |

| 参数           | 说明                                                                                                                                       |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| {thumbprint} | 图标图像的指纹。此值包括在从之前的枚举请求返回的 iconurl 中。                                                                                                      |
| {size}       | 要返回的图标图像的像素大小。 仅支持值 16、24、32、48、64、96、128、256 和 512。 此值包括在从之前的枚举请求返回的 iconurl 中，具体取决于在 Citrix Receiver for Web 站点配置文件中所配置的图像大小（默认值为 48）。 |

!!! tip "注意" * 此请求需要已通过身份验证的会话，由 cookie ASP.NET_SessionId 和 CtxsAuthId 指示。 * CSRF 令牌非必需，并且不属于 X-Citrix-IsUsingHTTPS 头，从而允许图标 URL 在被指定为 HTML img 元素的 src 时由浏览器直接提取 (HTTP GET)。 * 上述 URL 仅用于说明；客户端使用的实际 URL 必须从之前的枚举请求的结果中获取，这向每个资源提供一个独立的图标 URL。 * 指纹表示枚举时资源的图像数据，因此，如果图像数据发生变化，在后续枚举中可能会有所差别。 * 始终返回 PNG 格式的响应图像，其中 Content-Type 设置为 image/png。 * Web 代理使用 StoreFront 资源 API 获取图标图像。 除非在 Citrix Receiver for Web 配置文件中禁用了永久缓存，否则图像数据默认缓存到磁盘中作为性能优化。 * 缓存头在响应中设置，以指示图标图像可能会被客户端缓存一年。

### 响应

| 响应代码      | 说明                                                               |
| --------- | ---------------------------------------------------------------- |
| 200（正常）   | 成功                                                               |
| 304（未修改）  | 请求为“条件 GET”，由 If-Modified-Since 头的存在指示。如果已在后端更改图像，则仅在下一次枚举时才会发现。 |
| 400（错误请求） | 指定的大小与受支持的其中一个图标大小不匹配。                                           |
| 403（被禁止）  | 由于 `CtxsAuthId` cookie 无效被禁止。                                    |
| 404（未找到）  | 在以下情况下返回：                                                        |
|           | * 未知/缺少指纹                                                        |
|           | * 请求的大小与为 Citrix Receiver for Web 站点配置的大小不匹配                     |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| PNG  | image/png              |

### 示例：成功请求 48x48 图像

#### 请求

```curl
GET http://webserver/Citrix/StoreWeb/Resources/Icon/
RjczNDEyMzc3ODlENzE5REZDRUQ4MDczQ0QxMDBGRjI4MDY5MDg2RQ--/48
HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: image/png,image/*;q=0.8,*/*;q=0.5
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=A902FEE631743C79C5B125FE4BC4DB56;
CtxsAuthId=5E4A8BF161FBBBC90F64241E40A7D849;
ASP.NET_SessionId=tzqrwypgtof5fnna1fh1jmme
Connection: keep-alive
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: private, max-age=31536000, s-maxage=0
Content-Type: image/png
Expires: Thu, 25 Sep 2014 12:28:46 GMT
Last-Modified: Wed, 25 Sep 2013 12:28:46 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Wed, 25 Sep 2013 12:28:46 GMT
Content-Length: 3184
... 48 x 48 png image data ...
```

## 计算机关闭

使用此请求可触发指定资源的关闭操作。 只有某些资源支持此操作，并且通常仅适用于有限的一组用户。 如果关闭操作在 XenDesktop 服务器上定义的时间限制内未发生，则可能会返回一条错误响应，即使关闭成功也是如此。

### 请求

| URL（仅供参考）                | 方法   | 说明        |
| ------------------------ | ---- | --------- |
| /Resources/PowerOff/{id} | POST | 尝试关闭指定资源。 |

| 参数   | 说明          |
| ---- | ----------- |
| {id} | 特定资源的唯一标识符。 |

!!! tip "注意" * 此请求需要已通过身份验证的会话，由 cookie ASP.NET_SessionId 和 CtxsAuthId 指示。 * 上面的 URL 仅用于说明；请求中使用的实际 URL 必须从之前向 /Resources/List 发出的枚举请求的结果中获取。

### 响应

| 响应代码 | 说明                                        |
| ---- | ----------------------------------------- |
| 200  | 发送到计算机的导致关闭操作成功/失败的 PowerOff 请求，或者身份验证质询。 |
| 403  | 由于以下原因之一被禁止：                              |
|      | * CSRF 令牌无效                               |
|      | * X-Citrix-IsUsingHTTPS 头无效               |
|      | * CtxsAuthId cookie 无效                    |
| 404  | 找不到通过 {id} 标识的资源，或者关闭操作不存在或者未对当前用户的该资源启用。 |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| JSON | application/json       |

响应返回一个包含以下字段的简单 JSON 对象：

| 字段      | JavaScript 类型 | 说明                                        |
| ------- | ------------- | ----------------------------------------- |
| status  | 字符串           | 如果报告关闭操作已成功完成，则返回“success”，否则返回“failure”。 |
| errorId | 字符串           | 提供关闭失败原因的字符串（见下文）。                        |

如果关闭操作失败，错误原因将通过以下错误 ID 字符串之一指示：

| 错误 ID                 | 说明                                                        |
| --------------------- | --------------------------------------------------------- |
| in-maintenance-mode   | 由于计算机处于维护状态而无法执行请求的操作。                                    |
| no-machine            | 找不到已分配给指定计算机组中的用户的计算机。                                    |
| not-supported         | 请求的操作在请求的计算机组类型中不受支持。                                     |
| operation-in-progress | 服务器正忙于在指定计算机上执行控制操作，当前无法处理其他请求。                           |
| timed-out             | 服务器在等待请求完成时超时。注意：关闭操作可能已成功完成，但不在 XenDesktop 服务器上定义的超时期限内。 |
| not-trusted           | 客户端不受信任，无法执行请求的操作。这并不指示所提供的凭据是否有效。                        |
| unspecified           | 其他值未涵盖的错误条件。                                              |

### 示例：成功关闭

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Resources/PowerOff/
Q29udHJvbGxlci5NaWtlQiBEZXNrdG9wIEdyb3VwICRTMS0x HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:24.0) Gecko/
20100101 Firefox/24.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: 5798C908E45771AD767E66F1D176E2D1
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=5798C908E45771AD767E66F1D176E2D1;
CtxsAuthId=01FBA1051F0D45129D308C6E2F2C303D;
ASP.NET_SessionId=j3xcfc2fdvmjuynowgqlpad4
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Thu, 17 Oct 2013 09:12:53 GMT
Content-Length: 43
{"status":"success"}
```

## 资源订阅

使用这些请求可执行资源订阅和取消订阅以及更新已订阅的资源的相对位置（如在 UI 中所示）。 已订阅的资源在 Receiver for Web 的“主页”屏幕上显示，在用户登录时才可见。 这些代表供快速访问的“收藏项”列表。 Receiver for Web 需要先订阅应用程序资源才能进行启动，尽管协议中不存在此类限制也是如此。 桌面资源被视为特殊情况，始终通过 Receiver for Web 显示（在单独的 UI 选项卡上），与订阅状态无关。

### 请求

| URL（供参考）                      | 方法   | 说明                                                                            |
| ----------------------------- | ---- | ----------------------------------------------------------------------------- |
| /Resources/Subscription/ {id} | POST | 操作取决于在 POST 参数中指定的操作属性值：                                                      |
|                               |      | * **subscribe** — 尝试订阅资源。这可能会创建一个新订阅（如果尚不存在订阅），并且默认情况下会将该订阅设置为 subscribed 状态。 |
|                               |      | * **unsubscribe** — 尝试取消订阅资源。这可能会删除订阅，并且默认情况下会将订阅设置为 notSubscribed 状态。        |
|                               |      | * **update** — 更新资源的订阅属性。                                                     |

| POST 参数    | 说明                                                                  |
| ---------- | ------------------------------------------------------------------- |
| Id         | 特定资源的唯一标识符。注意：此参数是 URL 的组件。                                         |
| operation  | 要执行的订阅操作：**subscribe**、**unsubscribe** 或 **update**。                |
| updateMode | 所提供的所有订阅属性都将与现有属性合并，还是所提供属性的批处理将完全替换任何现有属性；**merge** 或 **replace**。 |
| *订阅属性*     | 作为请求参数提供的其他键=值对用于表示订阅属性。                                            |

!!! tip "注意" * 此请求需要已通过身份验证的会话，由 cookie `ASP. NET_SessionId` 和 `CtxsAuthId` 指示。 * 上面的 URL 仅用于说明；请求中使用的实际 URL 必须从之前向 /Resources/List 发出的枚举请求的结果中获取。 * Web 代理配置为使用未经身份验证的应用商店时，订阅操作不可用，并且将从资源枚举响应中省略这些 URL。 还会对在 XenApp 或 XenDesktop 中配置为强制性资源的资源省略这些 URL，强制性资源始终向所有用户显示。 * 订阅信息保存在应用商店服务的订阅应用商店中，在枚举期间返回，从而允许订阅在 Citrix Receiver for Web 会话中保持不变并对其他 Receiver 可见。

| 响应代码（及友好名称） | 说明                                                       |
| ----------- | -------------------------------------------------------- |
| 200（正常）     | 已成功订阅或取消订阅的资源，或者身份验证质询。                                  |
| 400（错误请求）   | 为 operation 或 updateMode 提供的值不受支持。                       |
| 403（被禁止）    | 由于以下原因之一被禁止：                                             |
|             | * CSRF 令牌无效                                              |
|             | * X-Citrix-IsUsingHTTPS 头无效                              |
|             | * CtxsAuthId cookie 无效                                   |
| 404（未找到）    | 找不到通过 {id} 标识的资源。                                        |
| 503（服务不可用）  | 由于以下原因之一，订阅操作对指定的资源不可用：                                  |
|             | * 发出之前的枚举请求时未返回所请求的资源的订阅 URL，指示执行该枚举时 StoreFront 订阅服务不可用 |
|             | * 应用商店服务订阅数据库当前不可用                                       |

### 成功响应内容

返回成功响应（HTTP 状态代码 200）时，响应正文中将包含一个包含以下字段之一的 JSON 对象：

| 字段     | JavaScript 类型 | 说明                                                                                                                                                                                               |
| ------ | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| status | 字符串           | 资源的新订阅状态，通过以下值之一指示：subscribed、unsubscribed、pending、approved、denied、pendingunsubscribe、subscribedpendingunsubsc ribe、failure。 订阅与工作流无关时，将为成功的操作返回前两个值。 值“failure”指示订阅请求未成功代理到应用商店服务。 其他值与工作流场景有关。 |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| JSON | application/json       |

### 示例：成功的订阅

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Resources/Subscription/
Q29udHJvbGxlci5Xb3JkUGFk HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: 35D3DAC189174D65DC6D843C9FDAB46A
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 42
Cookie: CsrfToken=35D3DAC189174D65DC6D843C9FDAB46A;
CtxsAuthId=535CF2577B2FF0DF53F76D3C2D994B92;
ASP.NET_SessionId=0eu5dnpqapdgeu2j1n3sz4zh
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
operation=subscribe&position=B
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Wed, 25 Sep 2013 15:58:41 GMT
Content-Length: 23
{"status":"subscribed"}
```

### 示例：不成功的取消订阅

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Resources/Subscription/
Q29udHJvbGxlci5Xb3JkUGFk HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: 35D3DAC189174D65DC6D843C9FDAB46A
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 33
Cookie: CsrfToken=35D3DAC189174D65DC6D843C9FDAB46A;
CtxsAuthId=535CF2577B2FF0DF53F76D3C2D994B92;
ASP.NET_SessionId=0eu5dnpqapdgeu2j1n3sz4zh
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
operation=unsubscribe
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Wed, 25 Sep 2013 16:00:23 GMT
Content-Length: 20
{"status":"failure"}
```

### 示例：成功更新订阅属性

这是成功更新与订阅相关联的数据的示例。 从资源服务的角度来说，属性 ID 和值具有任意性。 请注意，bar 属性键重复使用以指定多值属性。

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Resources/Subscription/
Q29udHJvbGxlci5GaXNoY2FrZSE- HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:24.0) Gecko/
20100101 Firefox/24.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: 48C7A008D81F87116B4BDE418FB63B3C
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 24
Cookie: CsrfToken=48C7A008D81F87116B4BDE418FB63B3C;
CtxsAuthId=1AA0681CE60D7E625A504BA0B1E70824;
ASP.NET_SessionId=stoi5utb5zfam1xtfqkjklfd
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
operation=update&updateMode=replace&foo=fooValue&bar=barValue&b
ar=barValue2
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Tue, 15 Oct 2013 17:12:49 GMT
Content-Length: 23
{"status":"subscribed"}
```

## 会话枚举

使用此请求可枚举当前客户端未连接到的已通过身份验证的用户的 ICA 会话，与会话在其他客户端处于断开连接还是活动状态无关。 枚举后，可以使用会话启动 API 重新连接这些会话。

### 请求

| URL（供参考）                | 方法   | 说明                       |
| ----------------------- | ---- | ------------------------ |
| /Sessions/ListAvailable | POST | 以 JSON 格式返回用户的所有可用会话的列表。 |

| POST 参数       | 说明                                                                                  |
| ------------- | ----------------------------------------------------------------------------------- |
| resourceTypes | 要在响应中包括的资源类型，使用以下值之一：Citrix.MPS.Application - 请求应用程序会话；Citrix.MPS.Desktop - 请求桌面会话。 |

!!! tip "注意" * 此请求需要已通过身份验证的会话，由 cookie ASP.NET_SessionId 和 CtxsAuthId 指示。 Web 代理配置为使用未经身份验证的应用商店时，将返回空会话列表。 * 当前客户端启动的会话（通过 DeviceId 指示）不包括在返回的会话列表中。 * 从 StoreFront 应用商店服务枚举会话的下游请求始终添加该参数以包括活动会话。 * 在早期版本的 XenApp 和 XenDesktop 中使用时，此请求还会断开所有活动会话的连接。 此问题在 XenDesktop 7 中已修复。 * 预期的使用模式是为客户端枚举所有可用会话（可能针对某种特定类型的资源），然后连续启动每个资源。 这是 Citrix Receiver for Web“登录时自动重新连接”功能的实现方式。

### 响应

| 响应代码（及友好名称） | 说明                                                 |
| ----------- | -------------------------------------------------- |
| 200（正常）     | 可用会话的列表（可能为空），或者身份验证质询。                            |
| 403（被禁止）    | 由于以下原因之一被禁止：                                       |
|             | * CSRF 令牌无效                                        |
|             | * X-Citrix-IsUsingHTTPS 头无效并且 CtxsAuthId cookie 无效 |

### 成功响应内容

返回成功响应（HTTP 状态代码 200）时，响应正文中将包含一个对象的 JSON 数组，其中每个对象都包含以下字段：

| 字段              | JavaScript 类型 | 说明                                       |
| --------------- | ------------- | ---------------------------------------- |
| launchurl       | 字符串           | 用于发出启动请求的 URL（请参阅第 57 页上的“ICA 启动”）       |
| launchstatusurl | 字符串           | 用于确定资源是否已准备好启动的 URL（请参阅第 57 页上的“ICA 启动”） |
| initialApp      | 字符串           | 以前启动的导致会话最初被创建的名称。                       |
| publishername   | 字符串           | 发布者的名称（XA 的场名称）。                         |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| JSON | application/json       |

### 示例：会话枚举

在此示例中，所有会话都通过不符合服务器和客户端类型的条件进行请求。

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Sessions/ListAvailable
HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:24.0) Gecko/
20100101 Firefox/24.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Csrf-Token: BC8E733316E514C1C1E31541D9E44AF9
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Content-Length: 36
Cookie: CtxsDeviceId=WR_AjT2owY5yJ4ED0J-x;
CsrfToken=BC8E733316E514C1C1E31541D9E44AF9;
CtxsAuthId=35037C55868C4C1AEE3A24488062C3BD;
ASP.NET_SessionId=ltutfqnaollcehgvre25mwip
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
resourceTypes=Citrix.MPS.Application
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 18 Oct 2013 09:25:45 GMT
Content-Length: 191
[
 {
  "initialapp":"Notepad",
  "launchstatusurl":"Sessions\/GetLaunchStatus\/
Q29udHJvbGxlci4xODM5MC4y",
  "launchurl":"Sessions\/LaunchIca\/Q29udHJvbGxlci4xODM5MC4y",
  "publishername":"johnc-ic6659"
 }
]
```

## 会话启动

使用此请求可尝试重新连接到现有会话（在其他客户端上处于断开连接或活动状态），由会话 ID 指示。

### 请求

| URL（供参考）                        | 方法   | 说明                  |
| ------------------------------- | ---- | ------------------- |
| /Sessions/Launch/{id}           | GET  | 请求指定会话是否已准备好启动。     |
| /Sessions/ GetLaunchStatus/{id} | POST | 请求用于启动指定会话的 ICA 文件。 |

| URL 参数 | 说明          |
| ------ | ----------- |
| {id}   | 特定会话的唯一标识符。 |

!!! tip "注意" * 这些请求需要已通过身份验证的会话，由 cookie `ASP. NET_SessionId` 和 `CtxsAuthId` 指示。 * 上述 URL 仅用于说明；客户端使用的实际 URL 必须从之前向 /Sessions/ListAvailable 发出的会话枚举请求的结果中获取。 Web 代理配置为使用未经身份验证的应用商店时，会话枚举将返回一个空列表，因此，会话启动不可用。 * Launch 属于 GET 而非 POST，以便其能够在隐藏的 iframe 中由浏览器加载（使用 iframe 的 src="url" 属性）以通过文件类型关联启动。 * GetLaunchStatus 向应用商店服务发出启动请求以确定指定资源是否已准备好启动。 如果此请求返回一个 ICA 文件，该 ICA 文件将缓存达 90 秒。 如果在此时间内对 LaunchIca 执行后续调用，则将返回缓存的 ICA 文件，而不向应用商店服务发出额外的启动请求。 * 对于 LaunchIca，客户端必须附加一个附带随机值的查询字符串键，以阻止浏览器在对相同的资源发出多个启动请求时返回缓存的响应。 请参见下文示例中的 launchId 查询字符串参数。

### 响应

| 响应代码 | 说明                                                                             |
| ---- | ------------------------------------------------------------------------------ |
| 200  | 成功/需要重试/失败，或身份验证质询。                                                            |
| 403  | 由于以下原因之一被禁止：CSRF 令牌无效 b) `X-Citrix-IsUsingHTTPS` 头无效 c) `CtxsAuthId` cookie 无效 |
| 404  | 找不到通过 {id} 标识的资源。                                                              |

| 响应格式   | 请求接受/响应 Content-Type 头   |
| ------ | ------------------------ |
| JSON   | application/json         |
| ICA 文件 | application/octet-stream |

### 成功响应内容

返回 GetLaunchStatus 的成功响应（HTTP 状态代码 200）时，响应正文中将包含一个用于描述所提供的资源是否已准备好启动的 JSON 对象以及以下可能的字段：

| 属性             | 必需 | 说明                                                                                            |
| -------------- | -- | --------------------------------------------------------------------------------------------- |
| status         | 是  | 以下字符串之一：                                                                                      |
|                |    | * success — 资源已准备好启动                                                                          |
|                |    | * retry — 资源当前未准备好启动（例如，首先需要打开才能与其建立连接的桌面 VM），但可以以后重新尝试启动。                                    |
|                |    | * failure — 尝试检索启动信息失败                                                                        |
| pollTimeout    | 否  | 仅在状态为 **retry** 时提供，重试启动请求之前等待的秒数。                                                            |
| errorId        | 否  | 仅在状态为 **failure** 时提供，在 StoreFront 应用商店服务 API 规范的“启动”部分中所描述的错误代码。                             |
| suggestRestart | 否  | 仅在状态为 **failure** 时提供，指示是否可以通过重新启动桌面来解决包括可能会解决失败桌面启动的“解决方法提示”的应用商店服务的 **true** 或 **false** 值。 |

返回 LaunchIca 的成功响应时，响应正文可以是 JSON 字符串（适用于 `GetLaunchStatus`，如上文所述），或者如果资源已准备好启动，则可以是 ICA 文件。

### 示例：会话获取启动状态成功

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Sessions/GetLaunchStatus/
Q29udHJvbGxlci4xODM5MC4y HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:24.0) Gecko/
20100101 Firefox/24.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: BC8E733316E514C1C1E31541D9E44AF9
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=BC8E733316E514C1C1E31541D9E44AF9;
CtxsAuthId=35037C55868C4C1AEE3A24488062C3BD;
ASP.NET_SessionId=ltutfqnaollcehgvre25mwip
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 18 Oct 2013 09:25:45 GMT
Content-Length: 20
{"status":"success"}
```

### 示例：会话启动成功

#### 请求

```curl
GET http://webserver/Citrix/StoreWeb/Sessions/Launch/
Q29udHJvbGxlci4xODM5MC41.ica?
CsrfToken=0068C0184E13600DE24B6C1469952BEB&launchId=13801992004
94 HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: text/html,application/xhtml+xml,application/
xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=0068C0184E13600DE24B6C1469952BEB;
CtxsAuthId=6E4966414BE6CE4E46D791655730672B;
ASP.NET_SessionId=kxh5cmlkkjbm5njp22feodrt
Connection: keep-alive
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/x-ica; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Thu, 26 Sep 2013 12:40:00 GMT
Content-Length: 1279
[Encoding]
InputEncoding=UTF8
[WFClient]
ProxyFavorIEConnectionSetting=Yes
ProxyTimeout=30000
ProxyType=Auto
<etc...>
```

## 会话断开连接和注销

使用这些请求可断开或注销服务器上用户的当前已连接会话。

### 请求

| URL（供参考）             | 方法   | 说明       |
| -------------------- | ---- | -------- |
| /Sessions/Disconnect | POST | 断开用户的会话。 |
| /Sessions/Logoff     | POST | 注销用户的会话。 |

!!! tip "注意" * 这些请求需要已通过身份验证的会话，由 cookie `ASP.NET_  SessionId` 和 `CtxsAuthId` 指示。 * Web 代理配置为使用未经身份验证的应用商店时，不存在可以断开其会话连接的关联用户，并且会话断开连接不起作用。 * 这些请求仅针对从当前客户端启动的会话，通过 DeviceId cookie 指示。

### 响应

| URL（供参考）             | 方法   | 说明       |
| -------------------- | ---- | -------- |
| /Sessions/Disconnect | POST | 断开用户的会话。 |
| /Sessions/Logoff     | POST | 注销用户的会话。 |

| 响应代码 | 说明                          |
| ---- | --------------------------- |
| 200  | 成功/失败，或者身份验证质询。             |
| 403  | 由于以下原因之一被禁止：                |
|      | * CSRF 令牌无效                 |
|      | * X-Citrix-IsUsingHTTPS 头无效 |
|      | * CtxsAuthId cookie 无效      |

| 响应格式 | 请求接受/响应 Content-Type 头 |
| ---- | ---------------------- |
| JSON | application/json       |

### 成功响应内容

返回断开连接或注销的成功响应（HTTP 状态代码 200）时，响应正文中将包含一个单个 JavaScript 字符串字段“status”具有以下值之一的 JSON 对象：“success”、“failure”、“partial”，指示用户的会话是否已成功断开连接。 值“partial”表示已成功断开部分用户会话的连接，而其他会话未成功断开连接。

### 示例：会话断开连接

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Sessions/Disconnect HTTP/
1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:23.0) Gecko/
20100101 Firefox/23.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: D6E47EE6160CA2121C3797D0E850FFA0
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=D6E47EE6160CA2121C3797D0E850FFA0;
CtxsAuthId=BC5CC0F3EC36E6577B3810A12063280C;
ASP.NET_SessionId=jzvwqr2uh1ksws4c3bd2x4gp
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Thu, 26 Sep 2013 14:23:56 GMT
Content-Length: 20
{"status":"success"}
```

### 示例：会话注销

#### 请求

```curl
POST http://webserver/Citrix/StoreWeb/Sessions/Logoff HTTP/1.1
Host: kontiki
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:24.0) Gecko/
20100101 Firefox/24.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Csrf-Token: EA871815F7B33602B391EA5F85757FE8
X-Citrix-IsUsingHTTPS: No
X-Requested-With: XMLHttpRequest
Referer: http://webserver/Citrix/StoreWeb/
Cookie: CsrfToken=EA871815F7B33602B391EA5F85757FE8;
CtxsAuthId=C1796CE16F1841181ABFD7DC5DC3E7E2;
ASP.NET_SessionId=zegkvwsdy0hqglxkgtgdcxda
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

#### 响应

```curl
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json
Expires: -1
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET
X-Citrix-Application: Receiver for Web
Date: Fri, 18 Oct 2013 11:12:19 GMT
Content-Length: 20
{"status":"success"}
```