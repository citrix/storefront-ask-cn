# 使用配置元素

本主题介绍了成功向 /Home/Configuration 发送请求后返回的 XML 文档的元素和属性。 这些设置指定客户端的行为并提供后续操作的 Web 代理 URL。

!!! tip "注意" 默认值以粗体显示。

## 会话

XPath：/clientSettings/session

| 属性            | 值                 | 说明                              |
| ------------- | ----------------- | ------------------------------- |
| timeout       | 时间，单位为分钟 (**20**) | Web 会话超时并且用户必须重新进行身份验证之前的空闲分钟数。 |
| userLanguages | **浏览器发送的接受语言头的值** | 客户端（通常为浏览器）支持的语言列表。             |

## 身份验证管理器

XPath：/clientSettings/authManager

| 属性                   | 值                                          | 说明                                    |
| -------------------- | ------------------------------------------ | ------------------------------------- |
| getUsernameURL       | **Authentication/ GetUserName**            | 用于获取完整用户名的 URL。                       |
| changeCredentialsURL | **ExplicitAuth/ GetChangeCredentialFor m** | 用于启动更改密码操作的 URL。                      |
| logoffURL            | **Authentication/Logoff**                  | 用于注销 Citrix Receiver for Web 会话的 URL。 |
| loginFormTimeout     | 时间，单位为分钟 (**5**)                           | Citrix Receiver for Web 显示登录表单的时间长度。  |

## 应用商店代理

XPath：/clientSettings/storeProxy

| 属性           | 值（默认值以粗体显示）        | 说明                 |
| ------------ | ------------------ | ------------------ |
| keepAliveURL | **Home/KeepAlive** | 用于保持会话处于活动状态的 URL。 |

## 资源代理

XPath：/clientSettings/storeProxy/resourcesProxy

| 属性              | 值（默认值以粗体显示）        | 说明                         |
| --------------- | ------------------ | -------------------------- |
| listURL         | **Resources/List** | 用于枚举用户的可用资源的 URL。          |
| resourceDetails | **default**        | full | 返回默认值还是返回扩展的资源枚举数据。 |

## 会话代理

XPath：/clientSettings/storeProxy/sessionsProxy

| 属性               | 值（默认值以粗体显示）                | 说明                |
| ---------------- | -------------------------- | ----------------- |
| listAvailableURL | **Sessions/ListAvailable** | 用于枚举用户的可用会话的 URL。 |
| disconnectURL    | **Sessions/Disconnect**    | 用于断开用户的会话连接的 URL。 |
| logoffURL        | **Sessions/Logoff**        | 用于注销用户的会话的 URL。   |

## 插件助手

XPath：/clientSettings/pluginAssistant

| 属性             | 值        | 说明                                             |
| -------------- | -------- | ---------------------------------------------- |
| enabled        | **true** | false | 未检测到 HDX 引擎时以及升级可用时是否显示 Receiver 下载信息。 |
| upgradeAtLogin | true     | **false** | 是否检测 Receiver 升级是否可用。              |

XPath：/configuration/citrix.deliveryservices/webReceiver/clientSettings/ pluginAssistant/win32

| 属性   | 值         | 说明                                  |
| ---- | --------- | ----------------------------------- |
| path | 相对或绝对 URL | 下载 Citrix Receiver for Windows 的位置。 |

XPath：/configuration/citrix.deliveryservices/webReceiver/clientSettings/ pluginAssistant/macOS

| 属性                         | 值              | 说明                                        |
| -------------------------- | -------------- | ----------------------------------------- |
| path                       | 相对或绝对 URL      | 下载 Citrix Receiver for MacOS 的位置。         |
| minimumSupportedOSVersi on | 版本号 (**10.6**) | Citrix Receiver for MacOS 所需的最低 MacOS 版本。 |

XPath：/configuration/citrix.deliveryservices/webReceiver/clientSettings/ pluginAssistant/html5

| 属性               | 值                                                                                                               | 说明                                                                                   |
| ---------------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| enabled          | Always/Fallback/Never                                                                                           | 是否启用 Receiver for HTML5。                                                             |
| platforms        | 以分号分隔的正则表达式列表。**Firefox;Chrome;Version/ ([6-9]\d\d).*Safari;MSIE \d\d;Trident; Android;iPad;iPhone;iPod** ; | 用于搜索 UserAgent 字符串以确定受支持的浏览器的正则表达式。                                                  |
| launchURL        | 相对 URL                                                                                                          | Receiver for HTML5 启动页面的位置。                                                          |
| singleTabLaunch  | true / **false**                                                                                                | 是否在与 Citrix Receiver for Web 主页相同的浏览器选项卡上启动应用程序。                                     |
| chromeAppOrigins | **chrome-extension:// bgiigkppjadidglloadcadeih lnbpagp**                                                       | 用于标识 Google Chrome Web 应用商店中 Citrix Chrome 应用程序的官方发行版的源 URL 列表。每个 URL 使用管道符号 (|) 分隔。 |

## 用户界面

XPath：/configuration/citrix.deliveryservices/webReceiver/clientSettings/userInterface

| 属性                | 值                     | 说明                                           |
| ----------------- | --------------------- | -------------------------------------------- |
| frameOptions      | allow/deny/sameorigin | 是否允许 Citrix Receiver for Web 在框架/iframe 中显示。 |
| autoLaunchDesktop | true/false            | 仅有一个桌面对用户可用时，是否允许在登录时自动启动桌面。                 |

XPath：/configuration/citrix.deliveryservices/webReceiver/clientSettings/ userInterface/workspaceControl

| 属性                   | 值                         | 说明                                              |
| -------------------- | ------------------------- | ----------------------------------------------- |
| enabled              | true/false                | 是否启用工作区控制。                                      |
| autoReconnectAtLogon | true/false                | 是否在登录时执行自动重新连接。                                 |
| logoffAction         | disconnect/terminate/none | 主动注销 Citrix Receiver for Web 时断开还是终止 HDX 会话的连接。 |
| showReconnectButton  | true/false                | 是否显示重新连接按钮/链接。                                  |
| showDisconnectButton | true/false                | 是否显示断开连接按钮/链接。                                  |

XPath：/configuration/citrix.deliveryservices/webReceiver/clientSettings/ userInterface/receiverConfiguration

| 属性      | 值          | 说明                    |
| ------- | ---------- | --------------------- |
| enabled | true/false | 是否启用 Receiver 置备文件下载。 |

XPath：/configuration/citrix.deliveryservices/webReceiver/clientSettings/ userInterface/appShortcuts

| 属性                    | 值          | 说明                                                |
| --------------------- | ---------- | ------------------------------------------------- |
| enabled               | true/false | 是否启用应用程序快捷方式。                                     |
| allowSessionReconnect | true/false | 通过应用程序快捷方式启用 Citrix Receiver for Web 时是否启用会话重新连接。 |