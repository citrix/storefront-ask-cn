# Citrix Receiver for Web 概述

Citrix Receiver for Web 是 Citrix StoreFront 的一个组件。 Citrix Receiver for Web 提供对 Windows 应用程序、桌面、文档、Web 应用程序和 SaaS 应用程序等 IT 资源的访问权限，全部使用 Web 进行访问。 Citrix Receiver for Web 由一个用户界面层（称为“UI 层”）和一个 Citrix StoreFront 服务 Web 代理层（称为“Web 代理”）组成。 Citrix Receiver for Web 体系结构如下图所示。

![体系结构](./architecture.png)

## UI 层

UI 层负责显示 Citrix Receiver for Web 的用户界面以及响应用户交互。 UI 层是使用 JavaScript/Ajax 技术实现的，加载到 Web 浏览器并在 Web 浏览器中执行。 UI 层分为三个区域：

* 插件助手 — 检测是否已安装本机 HDX 引擎并提供用于下载 Citrix Receiver 的 UI。
* 身份验证 — 处理身份验证事务，例如，登录、更改密码和注销。
* 应用商店 — 负责主要的 Citrix Receiver for Web 功能，例如应用商店浏览、搜索、订阅、启动、工作区控制等。

## Web 代理层

Web 代理是 UI 层与 Citrix StoreFront 服务之间的桥梁。 Web 代理向 Citrix StoreFront 服务提供一个适用于在 Web 浏览器中运行的 JavaScript/Ajax 客户端的简化版 API。 Web 代理由两部分组成，即一个 Web 身份验证管理器和一个应用商店代理，这两部分托管在一个 Web 应用程序中。

* **Web 身份验证管理器**提供对 UI 层的支持以便对用户进行身份验证。 此外，此管理器还在与 Citrix StoreFront 应用商店服务进行通信时管理应用商店代理使用的身份验证令牌。 身份验证服务发布的身份验证令牌保留在 Web 代理上，不传递到 UI 层。 Web 身份验证管理器反而会将浏览器 cookie（例如，HTTP 会话）映射到身份验证令牌，防止客户端免受 Citrix StoreFront 身份验证过程的一些复杂性的影响。
* **应用商店代理**提供适用于 UI 层的 API 以访问 Citrix StoreFront 应用商店服务提供的资源。 此外，此代理还提供对 Web 访问的优化，例如，永久性图标缓存、合并部分应用商店服务操作以及将资源信息从 XML 转换为 JSON。

通过应用商店服务 API 提供的许多工具也可以通过 Web 代理提供，后者通常代理发送至应用商店服务的请求（提供必需的身份验证令牌）并根据需要将响应转化为 JavaScript 友好的 JSON 表示。 可以通过 Web 代理执行的主要操作包括：

* 通过多种机制对用户进行身份验证：显式表单、域直通、智能卡、网关单点登录以及 POST 凭据。
* 从单一 Citrix StoreFront 应用商店服务枚举资源。 此操作可能会自身聚合来自不同服务器的资源，同时支持对包含多个 Controller 的 XenApp 和 XenDesktop 站点进行负载平衡和故障转移。
* 使用 ICA 协议远程启动应用程序或桌面。
* 使用 RDP 协议远程启动桌面。
* 使用单点登录启动在 App Controller 上发布的 SaaS 和 Web 应用程序。
* 关闭特定的 XenDesktop 桌面。
* 检索资源的图像和图标。
* 订阅资源，允许不同的客户端存储与订阅的资源关联的数据。
* 枚举可用的用户会话以及与会话有关的操作：重新连接、断开连接和注销。