# 关于 StoreFront Store Customization SDK

这些主题介绍了 Citrix StoreFront Store Customization SDK（简称为“StoreCustomization SDK”），说明了可用接口及其使用方法。 同时还详细介绍了可用的其他资源，例如脚本和示例。 这些主题面向经验丰富的管理员以及具有编程技能并且熟悉如何使用 SDK 的软件开发人员。

借助 Store Customization SDK，您可以执行与按 Citrix StoreFront 中提供的资源类型或关键字进行的简单过滤相比更加高级的过滤和自定义。 如果您仅希望对应用商店配置简单过滤（例如，阻止某些用户看到特定应用程序），则不需要使用 SDK；有关使用 PowerShell cmdlet 配置过滤的详细信息，请参阅 StoreFront 文档。

Store Customization SDK 允许您对向用户显示资源的过程应用自定义逻辑，并且还允许您调整启动参数。 例如，可以使用 SDK 控制向用户显示的应用程序和桌面、更改 ICA 虚拟通道参数或者通过 XenApp 和 XenDesktop 策略选择修改访问条件。

下图显示了能够使用 SDK 应用自定义的主要自定义点。

这些自定义点如下：

- 枚举后 — 用于修改资源枚举的结果。 此自定义点允许您改变资源的属性，提供了与使用 PowerShell cmdlet 按可用资源类型/关键字进行的简单过滤相比更加高级的过滤。

![流程](./flow.png)

- 启动后 ICA 文件 — 用于修改生成的 ICA 文件。例如，使用此自定义点可以更改 ICA 虚拟通道参数以及阻止用户访问其剪贴板。

- 会话后枚举 — 用于修改会话枚举的结果，例如，筛选出不需要的会话。

- 访问条件（启动前和枚举前）— 用于修改影响资源可见性以及启动资源能力的访问条件。

- 提供程序列表 — 用于在联系 XML Service 之前修改 Delivery Controller 的服务器列表。

- 设备信息 — 用于修改客户提供的设备信息，包括 ClientName 和 DeviceId。

### 其他资源

以下资源同时随此 SDK 提供：

- Store Service Customization API 文件 — 这是自定义协定中使用的接口和类的 MSDN 样式的库。

- Visual Studio 示例解决方案 — 包含典型自定义示例的示例解决方案。

- 跟踪脚本 — PowerShell 脚本随 SDK 提供，可用于启用跟踪。此脚本在对自定义进行测试和故障排除时非常有用。