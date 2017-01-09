# 自定义接口

本主题详细介绍了 StoreFront 公开的接口并说明了如何使用这些接口来应用自定义。有四种主要的自定义类型：

  1. 资源自定义 — 使用 IEnumerationResultModifier 接口
  2. 会话自定义 — 使用 ISessionListModifier 接口
  3. ICA 文件自定义 — 使用 ILaunchResultModifier 接口
  4. 请求自定义 — 使用 IInputModifier 接口

这些接口捆绑在 `Citrix.DeliveryServices.ResourcesCommon.Customization.Contract` 程序集中。

实现程序集必须以 .NET Framework 4.5 为目标。除非另行指定，否则每个接口都公开以下成员（有关更多详细信息，请参阅相应的接口）：

- `Modify`：此虚拟方法是接口的调用点。 实现类必须覆盖此方法，才能进行自定义。 此方法的签名在下文相应的接口部分中详细介绍。
- `RunExtendedValidation`：此属性指示 StoreFront 是否验证返回的值以确保其具有正确的格式。 由于扩展验证会对性能产生影响，因此，请仅在开发和调试期间设置此属性，并且务必在生产环境中将扩展验证设置为“关”。
- `ReturnOriginalValueOnFailure`：此属性指示每当此方法返回的自定义值中存在错误时，StoreFront 是否返回计算出的原始值。 我们建议您将此属性设置为 true。

## 资源自定义

使用此类型的自定义可在发送到客户端之前修改 StoreFront 计算出的资源枚举。 StoreFront 通过计算出的资源枚举结果以及上下文参数调用 IEnumerationResultModifier 接口的 Modify 方法。 实现类必须覆盖此方法，修改枚举结果并将其返回到 StoreFront。 StoreFront 随后将修改后的结果发送到客户端以便进一步进行处理。

有关如何使用此接口的示例，请参阅随 SDK 提供的 Store Service Customization API 文件和示例解决方案。

!!! tip "注意" 资源自定义仅对使用本机协议的 Receiver 有效；自定义对使用旧版协议访问应用商店的客户端无效，例如 Program Neighbourhood Agent。

### 接口

自定义类必须实现 `IEnumerationResultModifier` 接口。

### 类名称

将自定义类命名为 `EnumerationResultModifier`。

### 程序集名称

将自定义类置于 `StoreCustomization_Enumeration` 程序集中。

### 方法签名

字符串 `Modify` (`CustomizationContextData` context, string `valueToModify`)

要修改整个资源枚举结果时请使用此方法。用于自定义资源枚举的所有代码都必须包含在此方法内。

### 方法签名

字符串 `ModifySingleApp` (`CustomizationContextData` context, string `valueToModify`)

要修改单个资源时请使用此方法。用于自定义资源枚举的所有代码都必须包含在此方法内。

#### 参数

- `context`：此参数的类型为 `CustomizationContextData`，包含一组上下文数据，例如设备信息、`HttpContext`、用户标识、访问条件、场详细信息以及所有用户访问首选项。 有关此类型的详细信息，请参阅 Store Service Customization API 文件；请将此类型用作自定义逻辑的基础参考。

- `valueToModify`：此参数的类型为字符串并且包含 StoreFront 计算出的资源枚举结果。 此字符串遵循资源枚举结果，如在 Store Service Customization API 文件中所指定。

#### 返回值

此方法返回一个包含资源枚举结果的字符串，如在 StoreFront Service API 中所指定。 实现类必须在从此方法返回之前针对资源枚举结果运行所有验证。

### 会话自定义

使用此接口可在发送到客户端之前修改 StoreFront 计算出的 ICA 会话枚举。 StoreFront 通过计算出的会话枚举结果和上下文参数调用 ISessionListModifier 接口的 Modify 方法。 实现类必须覆盖此方法，修改枚举结果并将其返回到 StoreFront。 StoreFront 随后将修改后的结果发送到客户端以便进一步进行处理。

有关如何使用此接口的示例，请参阅随 SDK 提供的 Store Service Customization API 文件和示例解决方案。

!!! tip "注意" 会话自定义仅对使用本机协议的 Receiver 有效；自定义对使用旧版协议访问应用商店的客户端无效，例如 Program Neighbourhood Agent。

- **接口**：自定义类必须实现 `ISessionListModifier` 接口。
- **类名称**：将自定义类命名为 `SessionListModifier`。
- **程序集名称**：将自定义类置于 `StoreCustomization_SessionEnumeration` 程序集中。
- **方法签名**：字符串 `Modify`(`CustomizationContextData` context, string `valueToModify`)

使用此方法可修改会话枚举结果。用于自定义会话枚举的所有代码都必须包含在此方法内。

#### 参数

- `context`：此参数的类型为 CustomizationContextData。 其中包含一组上下文数据，例如设备信息、HttpContext、用户标识、访问条件、场详细信息以及所有用户访问首选项。 有关此类型的详细信息，请参阅 Store Service Customization API 文件；请将此类型用作自定义逻辑的基础参考。

- `valueToModify`：此参数的类型为字符串并且包含 StoreFront 计算出的会话枚举结果；请参阅 Store Service Customization API 文件。

#### 返回值

此方法返回一个包含会话枚举结果的字符串，如在 StoreFront Service API 中所指定。 实现类必须在从此方法返回之前针对会话枚举运行所有验证。

## ICA 文件自定义

使用此接口可在发送到客户端之前修改 StoreFront 计算出的 ICA 文件的内容。 StoreFront 通过计算出的 ICA 文件和上下文参数调用 ILaunchResultModifier 接口的 Modify 方法。 实现类必须覆盖此方法，修改 ICA 文件并将其返回到 StoreFront。 StoreFront 随后将修改后的文件发送到客户端以便进一步进行处理。

有关如何使用此接口的示例，请参阅随 SDK 提供的 Store Service Customization API 文件和示例解决方案。

- **接口**：自定义类必须实现 `ILaunchResultModifier` 接口。
- **类名称**：将自定义类命名为 `LaunchResultModifier`。
- **程序集名称**：将自定义类置于 `StoreCustomization_Launch` 程序集中。
- **方法签名**：字符串 `Modify`(`CustomizationContextData` context, string `valueToModify`)

使用此方法可修改 ICA 文件的内容。用于自定义 ICA 文件的所有代码都必须包含在此方法内。

### 参数

-`context`：此参数的类型为 `CustomizationContextData`，包含一组上下文数据，例如设备信息、`HttpContext`、用户标识、访问条件、场详细信息以及所有用户访问首选项。 有关此类型的详细信息，请参阅 Store Service Customization API 文件；请将此类型用作自定义逻辑的基础参考。 -`valueToModify`：此参数的类型为字符串，包含 StoreFront 计算出的 ICA 文件的内容。

### 返回值

此方法返回一个字符串，该字符串组成了 ICA 文件的内容。实现类必须在从此方法返回之前针对修改后的文件内容运行所有验证。

### 请求自定义

使用此接口可在转发到后端 XML Service（例如 XenDesktop Delivery Controller）之前修改请求。

与在 StoreFront 计算结果之后调用自定义代码的其他自定义点不同，此自定义代码在将请求发送到后端 XML Service 之前、StoreFront 处理部分请求之后调用。 StoreFront 通过一组请求参数和一个上下文参数调用 IInputModifier 接口的 Modify 方法。 实现类必须覆盖此方法，并且可以改变请求参数的属性值。 StoreFront 在联系后端 XML Service 时使用修改后的请求参数。

!!! tip "注意" 如果为站点配置了多站点场（使用 `userFarmMappings` 配置元素或 `Set-  DSUserMappings` cmdlet 配置），则不能使用请求自定义接口。

有关如何使用此接口的示例，请参阅随 SDK 提供的 Store Service Customization API 文件和示例解决方案。

- **接口**：自定义类必须实现 `IInputModifier` 接口。
- **类名称**：将自定义类命名为 `IInputModifier`。
- **程序集名称**：将自定义类置于 `StoreCustomization_Input` 程序集中。
- **方法签名**：void `Modify` (`CustomizationContextData` context, out `FarmSetsContext farmSetsContext`, out `DeviceInfo deviceInfo`, out `AccessConditions accessConditions`)

使用此方法可修改请求。用于修改请求的所有代码都必须包含在此方法内。

有关如何使用此接口的示例，请参阅随 SDK 提供的 Store Service Customization API 文件和示例解决方案。

- **接口**：自定义类必须实现 IInputModifier 接口。
- **类名称**：将自定义类命名为 IInputModifier。
- **程序集名称**：将自定义类置于 StoreCustomization_Input 程序集中。
- **方法签名**：void Modify (CustomizationContextData context, out FarmSetsContext farmSetsContext, out DeviceInfo deviceInfo, out AccessConditions accessConditions)

使用此方法可修改请求。用于修改请求的所有代码都必须包含在此方法内。

### 参数

- `context`：此参数的类型为 `CustomizationContextData`。 其中包含一组上下文数据，例如设备信息、`HttpContext`、用户标识、访问条件、场详细信息以及所有用户访问首选项。
- `farmSetsContext`：此参数的类型为 `FarmSetsContext`，包含 StoreFront 要访问的场列表。 此自定义修改该列表。 StoreFront 访问修改后的列表中的每个场。
- `deviceInfo`：此参数的类型为 `DeviceInfo`，包含客户端所发送的客户端设备信息。 使用此自定义可在 StoreFront 发送到后端 XML Service 之前修改此信息。
- `accessConditions`：此参数的类型为 `AccessConditions`。 其中包含客户端所发送的 StoreFront 计算出的访问条件。 使用此自定义可在 StoreFront 将此信息发送到后端 XML Service 之前修改这些条件。

有关参数的详细信息，请参阅 Store Service Customization API 文件。

## 部署自定义

本主题说明了如何部署您的自定义。自定义模块必须包含在各自的程序集中。要部署自定义，请执行以下操作：

  1. 备份原始程序集
  2. 部署自定义

这些步骤将在下文更加详细地进行说明。

### 备份原始程序集

Citrix 建议您在将原始程序集替换为新的自定义程序集之前对其进行备份。 还原默认行为时需要使用原始程序集。 请务必备份以下程序集：

  1. StoreCustomization_Enumeration.dll
  2. StoreCustomization_Input.dll
  3. StoreCustomization_Launch.dll
  4. StoreCustomization_SessionEnumeration.dll

### 部署自定义

要部署自定义，请手动将新的自定义程序集复制到相应 StoreFront 服务器上的应用商店 bin 文件夹中。这将替换现有程序集。

如果 StoreFront 服务器属于群集部署的一部分，请执行传播操作以将新程序集推送到群集的其他成员。

!!! tip "注意" 请仅使用传播操作传播这些文件以及附带的 pdb。如果存在其他文件，则必须手动将这些文件复制到群集中的所有计算机。

### 示例

下例显示了如何查找 StoreFront 服务器上的应用商店 bin 文件夹。

在此示例中，Web 站点的主目录为 `C:\inetpub\wwwroot`。

在此 Web 站点上为 StoreFront 配置了一个名为“Store”的应用商店。将新的自定义程序集复制到的完整有效目录路径如下：

`C:\inetpub\wwwroot\Citrix\Store\bin`

## 重新启动服务

部署自定义后，请重新启动以下服务。重新启动将强制这些服务从以前的程序集实现中清除缓存的所有数据。

!!! tip "注意" 部署自定义时服务应自动重新启动，但 Citrix 建议您检查是否自动重新启动。

- Internet Information Service：这是在 StoreFront 服务器上运行的 IIS 服务。
- `CitrixSubscriptionsStore`：这是在 StoreFront 服务器上运行的 Windows 服务。

### 测试和调试

本主题说明了如何测试和调试您的自定义，还说明了如何启用跟踪以及附加调试程序以便您能够在 Visual Studio 中进行调试。

要对您的实现进行故障排除，请先在事件日志中进行查找。 也可以查阅提供的信息比事件日志更加详细的跟踪。 测试期间，请将跟踪级别设置为“详细”，在生产环境中，请将跟踪级别设置为“错误”或“警告”；有关如何启用跟踪的详细信息，请参阅下文。

有关在 Visual Studio 中调试您的实现的信息，请参阅下文。

还可以将 StoreFront 配置为在开发和测试期间针对返回的值运行扩展验证。 为此，请将 RunExtendedValidation 设置为 true；有关详细信息，请参阅[自定义接口](customization-interfaces.md)。 但是，由于运行扩展验证时会对性能产生影响，因此，请务必在生产环境中将其设置为“关”。

### 启用跟踪

跟踪功能将详细信息写入到跟踪中。跟踪转储的默认位置为 `C:\Program Files\Citrix\Receiver StoreFront\Admin\trace`。

要启用跟踪并设置跟踪级别，请使用随 SDK 提供的 PowerShell 脚本 `SetDSStoreCustomizationTraceLevel.ps1`。此脚本附带以下参数：

- `SiteId`：部署了应用商店的 IIS 站点 ID
- `VirtualPath`：应用商店的虚拟路径
- `TraceLevel`：用于设置以下跟踪级别： a. `错误` b. `信息` c. `关` d. `详细` e. `警告`

!!! tip "注意" Citrix 建议您在开发期间将跟踪级别设置为`详细`，在生产环境中将跟踪级别设置为`错误`或`警告`。

!!! tip "注意" 如果不知道应用商店的 SiteId 和 VirtualPath，请运行以下 PowerShell 命令：

    cd 'c:\program files\Citrix\Receiver Storefront\Scripts'. 
    .\ImportModules.ps1
    Get-DSStoreFeatureInstances
    

### 写入跟踪

Store Customization SDK 包括一个名为 `Tracer` 的跟踪类。 此类具有以下允许您从 Store Customization SDK 输出到跟踪的方法： - `TraceWarning`：在 `TraceEventType.Warning` 处写入警告跟踪消息 - `TraceInfo`：在 `TraceEventType.Information` 处写入信息跟踪消息 - `TraceError`：在 `TraceEventType.Error` 处写入错误跟踪消息 - `Trace Method`：在 `TraceEventType.Verbose` 处写入跟踪消息

### 调试

本部分说明了如何附加调试程序以便能够在 Visual Studio 中逐行调试您的实现以及如何指定暂停代码执行的断点。

  1. 在 Visual Studio 2012 中打开您的自定义解决方案。
  2. 附加到用户名为 `IIS APPPOOL\Citrix Delivery Services Resources` 的进程 w3wp.exe。

使用首选客户端的后续资源请求在 Visual Studio 中将命中一个断点。

要设置远程调试，请参阅“启用远程调试”。 如果运行 Visual Studio 的开发计算机与运行您的自定义的 StoreFront 部署计算机不同，则需要执行此操作。