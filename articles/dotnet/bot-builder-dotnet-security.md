---
title: 保护机器人 | Microsoft Docs
description: 了解如何使用 HTTPS 和 Bot Framework 身份验证来保护机器人。
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.service: bot-service
ms.subservice: sdk
ms.date: 12/13/17
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 455ee87f31e70470d8d118f010c77327347c032d
ms.sourcegitcommit: b15cf37afc4f57d13ca6636d4227433809562f8b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2019
ms.locfileid: "54224882"
---
# <a name="secure-your-bot"></a>保护机器人

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

机器人可以通过 Bot Framework Connector 服务连接到许多不同的通信通道（Skype、SMS、电子邮件和其他）。 本文介绍如何使用 HTTPS 和 Bot Framework 身份验证来保护机器人。

## <a name="use-https-and-bot-framework-authentication"></a>使用 HTTPS 和 Bot Framework 身份验证

若要确保机器人的终结点只能通过 Bot Framework [Connector](bot-builder-dotnet-concepts.md#connector) 访问，请将机器人的终结点配置为仅使用 HTTPS 并通过[注册](~/bot-service-quickstart-registration.md)机器人启用 Bot Framework 身份验证来获取其 appID 和密码。 

## <a name="configure-authentication-for-your-bot"></a>为机器人配置身份验证

在机器人的 web.config 文件中指定机器人的 appID 和密码。 

> [!NOTE]
> 若要查找机器人的“AppID”和“AppPassword”，请参阅 [MicrosoftAppID 和 MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword)。

```xml
<appSettings>
    <add key="MicrosoftAppId" value="_appIdValue_" />
    <add key="MicrosoftAppPassword" value="_passwordValue_" />
</appSettings>
```

然后，在使用 Bot Framework SDK for .NET 创建机器人时，使用 `[BotAuthentication]` 属性指定身份验证凭据。 

若要使用存储在 web.config 文件中的身份验证凭据，请指定不带参数的 `[BotAuthentication]`。

[!code-csharp[Use botAuthentication attribute](../includes/code/dotnet-security.cs#attribute1)]

若要使用其他值作为身份验证凭据，请指定 `[BotAuthentication]` 属性并传入这些值。

[!code-csharp[Use botAuthentication attribute with parameters](../includes/code/dotnet-security.cs#attribute2)]

## <a name="additional-resources"></a>其他资源

- [用于 .NET 的 Bot Framework SDK](bot-builder-dotnet-overview.md)
- [用于 .NET 的 Bot Builder SDK 中的重要概念](bot-builder-dotnet-concepts.md)
- [使用 Bot Framework 注册机器人](~/bot-service-quickstart-registration.md)
