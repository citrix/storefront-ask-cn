# API 的工作原理

本主题介绍了 StoreFront Web API 的行为，包括身份验证行为、cookie、本地化细节、版本兼容性以及 URL 发现。

## 身份验证质询

某些 Web 代理 URL 受到保护，并且需要身份验证信息才能允许进行访问。 如果所需的身份验证信息不存在，Web 代理将返回一条空响应（HTTP 状态代码 200）以及以下 HTTP 头：

    CitrixWebReceiver-Authenticate: reason="TokenRequired",
    location="Authentication/GetAuthMethods"
    

质询中的位置值指定客户端请求的（通过 Ajax）用于确定可用身份验证方法的 URL。 质询可能包含一个附加属性 `mode=Redirect`，该属性将触发客户端重定向到第三方页面，而非执行身份验证步骤。

即使在用户通过身份验证后，也可能会返回质询。 例如，如果用户的 Web 代理会话因任何原因终止，`ASP.NET_SessionId cookie` 在客户端请求中不存在，或者 StoreFront Credential Wallet 服务已重新启动。

因此，客户端必须通过提示用户重新对站点进行身份验证来随时准备好响应此类身份验证质询。

## 客户端名称、客户端地址和设备 ID

Web 代理与某些 Store Service API 进行通信时，将提供客户端名称、客户端地址和设备 ID 的值。 需要这些值以使工作区控制功能正常运行。 Web 代理将这些参数的一致数据传递到附带这些参数的所有 Store Service 非常重要。

客户端名称指定客户端计算机的主机名，设备 ID 指定对客户端设备来说独一无二的值。 由于典型的基于浏览器的客户端对客户端操作系统具有有限访问权限，并且不能确定主机名，因此，Web 代理将自动生成一个随机字符串值以供客户端名称和设备 ID 使用。 此值在对资源枚举的成功响应（HTTP 状态 200）中以永久性 cookie 的形式（名为 CtxsDeviceId）返回到客户端。 此 cookie 在后续请求中由客户端返回，这是预期行为。

客户端地址标识客户端 IP 地址。 可能会在某些 XenApp/XenDesktop 系统中使用客户端地址，以确定是否应向某个客户端提供特定资源。 Web 代理自动确定来自 Microsoft ASP.NET API `HttpRequest.UserHostAddress` 的客户端地址，该地址返回的客户端 IP 地址与在 Web 服务器上显示的 IP 地址相同。 这可能不是实际的客户端地址；例如，如果通过 NetScaler Gateway 或其他代理服务器进行连接。

如果多个 Web 浏览器从相同的客户端计算机连接到 Citrix Receiver for Web 站点，则会向每个浏览器返回一个不同的（随机）CtxsDeviceId cookie 值以表示客户端名称和设备 ID。 这会导致 Store Service 将会话视为在单独的设备上运行。 这可能会导致执行会话操作时应用程序消失并重新显示；例如，查询并重新连接到会话。

## cookie

Web 代理按下表中所述设置若干 cookie。客户端必须在后续请求中返回 cookie，如 cookie 生存时间所示。

| cookie                | 生存时间   | 仅限 Http | 设置时             | 说明                                     |
| --------------------- | ------ | ------- | --------------- | -------------------------------------- |
| **ASP.NET_SessionId** | 会话     | 是       | 来自 Web 代理的第一个响应 | ASP.NET 会话 ID，表示客户端与 Web 代理之间的 Web 会话。 |
| **CtxsAuthId**        | 会话     | 是       | 指示成功身份验证的响应     | 保护免受会话固定攻击                             |
| **CsrfToken**         | 会话     | 否       | 来自 Web 代理的第一个响应 | 保护免受跨站点请求伪造攻击                          |
| **CtxsDeviceId**      | 持续，2 年 | 是       | 指示成功资源枚举的响应     | 指定用于客户端名称和设备 ID 的值（见上文）                |

## 指示安全 cookie 的 HTTPS 指示器

除非另行指定，否则 Web 代理 API 要求客户端通过提供名为 `X-Citrix- IsUsingHTTPS`、值为“Yes”或“No”的头来指示是否使用 HTTPS 连接。 值设置为“Yes”时，Web 代理将为其生成的所有 cookie 设置 Secure 属性，以保护其免受来自非 HTTPS 连接的查看。 这是为了满足以下 NetScaler Gateway 场景的需求：从浏览器到网关的连接是通过 HTTPS 建立的，但从网关到内部 Citrix Receiver for Web 站点（Web 代理在其中运行）的连接是通过 HTTP 建立的。

## 跨站点请求伪造令牌

除非另行指定，否则 Web 代理 API 要求客户端提供 CSRF 令牌，以防止跨站点请求伪造 (CSRF) 攻击。 这是 Web 代理为会话的持续时间生成的并使用会话 cookie 传达给客户端的一个随机字符串。 在大多数 API 调用中，客户端必须以 POST 请求的名为 Csrf-Token（请注意连字符）的头的值形式或者以 GET 请求的名为 CsrfToken 的查询字符串参数的值形式读取此 cookie 的值并将其发送回 Web 代理。

## 本地化

一般而言，Web 代理在 UI 中不返回要显示的面向用户的字符串。 但是，身份验证过程使用 Citrix 通用格式协议来获取登录格式的定义（从其他格式中），通常包括 UI 控件的字符串（例如，“用户名:”标签或“登录”按钮）。 在接受语言 HTTP 头中指定的值用于确定此类字符串的语言。 此值还会在 /Home/ Configuration 请求的响应中反射回客户端，以便客户端可能会执行任何其他本地化以匹配用于身份验证格式的语言。

## 超媒体控制和参数化 URL

StoreFront Web API 使用超媒体控制的概念。 使用超媒体控制时，指向资源和操作的链接将从服务器获取，即从客户端配置 (/Home/Configuration) 获取或直接以调用此 API 的结果形式获取。 这意味这客户端必须从不构建 URL。 在这些主题中将探讨各种参数化 URL，例如 /Resources/ Subscription/{id}。 注意这些参数仅属于指示性参数，这一点非常重要，因为客户端代码不得直接构建 URL。 相反，所有参数化 URL 都必须从服务本身获取。 例如，用于启动特定资源的 URL 必须从对 /Resources/List/ 的请求中返回的 JSON 数据中提取。 在某些情况下，客户向 GET HTTP 请求调用的服务提供其他参数。 这些参数通过向基础服务 URL 中添加查询字符串参数来提供给服务。 客户端不得对服务 URL 的格式做任何假设，格式可能因不同的版本而异。 特别是，客户端不得就基础服务 URL 中是否存在查询字符串参数做任何假设，在每种情况下，必须确定应在新参数后附加“?”还是“&”。

## 向后兼容性和向前兼容性

这是 Web 代理 API 的初步修订版。不对与任何早期版本或将来版本的 Citrix Receiver for Web 的向后兼容性或向前兼容性做任何保证。

## URL 发现

JavaScript 代码在浏览器中执行时，先向通过变量 CTXS.configUrl（默认值为 /Home/ Configuration，但可以被 JavaScript 自定义覆盖）定义的 Web 代理 URL 发出一个 Ajax 请求。 这将返回一个用于描述各种 Citrix Receiver for Web 配置设置的 XML 文档，其中包含客户端必须用于后续 API 调用的若干 URL 路径（相对于站点根目录）。 除这些“固定”URL 外，以后可能还会向客户端发送指定资源特有的其他 URL 路径。 例如，调用 Resource/List 返回用于描述用户可用的所有资源的 JSON 数据，包括诸如启动指定资源或订阅某个资源等资源特定操作的 URL 路径（根据资源）。 这些 URL 路径包含一个嵌入在其中的资源 ID。

## API 使用模式

客户端在高级别通过按以下顺序执行调用来访问 Web 代理 API：

  1. 提取站点配置，以确定后续操作的行为并发现 URL
  2. 执行身份验证，以允许访问资源
  3. 列出会话，以在登录时执行会话重新连接
  4. 列出资源，以提取启动和后续操作的其他（根据资源）URL
  5. 启动并订阅资源以响应 UI 中的用户操作

## HTTP POST 参数

在整个 API 中，POST 主体中的任何请求参数都必须遵循标准格式 URL 编码规则，其中 Content-Type 设置为：

    application/x-www-form-urlencoded