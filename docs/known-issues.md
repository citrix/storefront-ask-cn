# SDK 中的已知问题

本主题列出了针对此 SDK 的所有已知问题，请在开始使用 SDK 之前仔细阅读。

  1. 当用户使用用户主体名称 (UPN) 格式的凭据登录 StoreFront 时，CustomizationContextData 中存储的 UserIdentity 对象的 AuthenticationType 字段将设置为空字符串，Name 字段将设置为登录时使用的 UPN 名称。

  2. 启用了扩展验证时，如果 StoreFront 遇到使用率过高的情况，尽管某些请求属于有效请求，也可能无法通过验证过程。 此问题间歇性出现，并且可能会显示以下错误消息：

    Citrix.DeliveryServices.ResourcesCommon.Customization.ValidationException: XML validation against schema failed". Ensure extended validation is set to off in the production environment.