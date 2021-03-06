---
title: 在网上聊天中启用语音 | Microsoft Docs
description: 了解如何在网上聊天控件中为已连接到网上聊天通道的机器人启用语音。
keywords: 语音, 网上聊天, 语音, 麦克风, 音频
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.service: bot-service
ms.subservice: sdk
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: cdb512b0efe111870b34c440f978105786b6f36a
ms.sourcegitcommit: 8161753641368567f239e24a35ad61768acccd8e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2019
ms.locfileid: "54202573"
---
# <a name="enable-speech-in-web-chat"></a>在网上聊天中启用语音
可以在网上聊天控件中启用语音界面。 用户使用网上聊天控件中的麦克风来与语音界面交互。

![网上聊天语音示例](~/media/bot-service-channel-webchat/webchat-sample-speech.png)

如果用户通过键入而不是讲述做出响应，则网上聊天会关闭语音功能，而机器人只会提供文本响应，而不会讲出响应。 若要重新启用语音响应，用户下一次可以使用麦克风来响应机器人。 接受输入时，麦克风会灰显或者呈现填满状态。 如果灰显，用户可以通过单击来启用麦克风。

## <a name="prerequisites"></a>先决条件

  在运行本示例之前，需要获取想要通过网上聊天控件运行的机器人的 Direct Line 机密或令牌。 
  * 有关如何获取与机器人关联的 Direct Line 机密的信息，请参阅[将机器人连接到 Direct Line](bot-service-channel-connect-directline.md)。
  * 有关交换令牌机密的信息，请参阅[生成 Direct Line 令牌](rest-api/bot-framework-rest-direct-line-3-0-authentication.md)。

## <a name="customizing-web-chat-for-speech"></a>自定义网上聊天的语音
若要在网上聊天中启用语音功能，需要自定义调用网上聊天控件的 JavaScript 代码。 可以使用以下步骤，在本地尝试已启用语音的网上聊天。

1. 下载[示例 index.html](https://aka.ms/web-chat-speech-sample)。 <!-- this aka.ms link needs to be updated if the sample location changes -->
2. 根据要添加的语音支持类型编辑 `index.html` 中的代码。 [启用语音服务](#enable-speech-services)中介绍了语音实现的类型。 
3. 启动 Web 服务器。 可以在 Node.js 命令提示符下使用 `npm http-server` 执行此操作。

   * 若要全局安装 `http-server` 以便可以从命令行运行它，请运行以下命令：

     ```
     npm install http-server -g
     ```

   * 若要使用端口 8000 启动 Web 服务器，请从包含 `index.html` 的目录运行以下命令：

     ```
     http-server -p 8000
     ```
4. 在浏览器中打开 `http://localhost:8000/samples?parameters`。 例如，`http://localhost:8000/samples?s=YOURDIRECTLINESECRET` 使用 Direct Line 机密调用机器人。 下表描述了可在查询字符串中设置的参数：

   | 参数 | Description |
   |-----------|-------------|
   | s | Direct Line 机密。 有关如何获取 Direct Line 机密的信息，请参阅[将机器人连接到 Direct Line](bot-service-channel-connect-directline.md)。 |
   | t | Direct Line 令牌。 有关如何生成此令牌的信息，请参阅[生成 Direct Line 令牌](rest-api/bot-framework-rest-direct-line-3-0-authentication.md)。 |
   | 域 | 可选。 备用 Direct Line 终结点的 URL。  |
   | webSocket | 可选。 设置为“true”会使用 WebSocket 来接收消息。 默认为 `false`。 |
   | userid | 可选。 机器人用户的 ID。  |
   | username | 可选。 机器人用户的用户名。  |
   | botid | 可选。 机器人的 ID。 |
   | botname | 可选。 机器人的名称。 |


## <a name="enable-speech-services"></a>启用语音服务
使用自定义可以通过以下任何方式添加语音功能：

* **浏览器提供的语音** - 使用 Web 浏览器中内置的语音功能。 目前，只有 Chrome 浏览器中提供了此功能。
<!--* **Use Bing Speech service** - You can use the Bing Speech service to provide speech recognition and synthesis. This way of access speech functionality is supported by a variety of browsers. In this case, the processing is done on a server instead of on the browser.-->
* **创建自定义的语音服务** - 可以创建自己的自定义语音识别与语音合成组件。

### <a name="browser-provided-speech"></a>浏览器提供的语音

以下代码实例化浏览器随附的语音识别器和语音合成组件。 并非所有浏览器都支持这种语音添加方法。 

> [!NOTE] 
> Google Chrome 支持浏览器语音识别器。 但是，在以下情况下，Chrome 可能会阻止麦克风：
> * 如果包含网上聊天的页面的 URL 以 `http://` 而不是 `https://` 开头。
> * 如果 URL 是使用 `file://` 协议而不是 `http://localhost:8000` 的本地文件。

[!code-js[Specify speech options to use in-browser speech (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#BrowserSpeech)]

<!--### Bing Speech service

The following code instantiates speech recognizer and speech synthesis components that use the Bing Speech service. The recognition and generation of speech is performed on the server. This mechanism is supported in multiple browsers. 

> [!TIP]
> You can use speech recognition priming to improve your bot's speech recognition accuracy if you use the Bing Speech service. For more information, check out the [Speech Support in Bot Framework](https://blog.botframework.com/2017/06/26/Speech-To-Text) blog post.

[!code-js[Specify speech options to use the Bing Speech API (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#BingSpeech)]

#### Use the Bing Speech service with a token

You also have the option to enable Cognitive Services speech recognition using a token. The token is generated in a secure back end using your API key.

The following example code shows how the token fetch is done from a secure back end to avoid exposing the API key.

[!code-js[Fetch a token to use with the Bing Speech API (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#FetchToken)]
-->
### <a name="custom-speech-service"></a>自定义语音服务

还可以提供自己的自定义语音识别，用于实现 ISpeechRecognizer 或语音合成（实现 ISpeechSynthesis）。 

[!code-js[Fetch a token to use with a custom speech service (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#CustomSpeechService)]

### <a name="pass-the-speech-options-to-web-chat"></a>将语音选项传递给网上聊天

以下代码将语音选项传递给网上聊天控件：

[!code-js[Pass speech options to Web Chat (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#PassSpeechOptionsToWebChat)]

## <a name="next-steps"></a>后续步骤
了解如何启用与网上聊天的语音交互后，接下来请了解机器人如何构建语音消息和调整麦克风的状态：
* [将语音添加到消息 (C#)](dotnet/bot-builder-dotnet-text-to-speech.md)
* [将语音添加到消息 (Node.js)](nodejs/bot-builder-nodejs-text-to-speech.md)

## <a name="additional-resources"></a>其他资源

* 可在 GitHub 上为网上聊天控件[下载源代码](https://github.com/Microsoft/BotFramework-WebChat)。
* [必应语音 API 文档](https://docs.microsoft.com/azure/cognitive-services/speech/home)提供了有关必应语音 API 的详细信息。

