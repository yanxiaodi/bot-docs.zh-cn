---
ms.openlocfilehash: f09d0a7b81e3cfa69fd42356faf27f79e3bc038c
ms.sourcegitcommit: 980612a922b8290b2faadaca193496c4117e415a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2019
ms.locfileid: "64563647"
---
收到“送货地址更新”或“送货选项更新”回调时，机器人将从 `Activity.Value` 属性中的客户端获得付款详细信息的当前状态。
作为商家，你应该将这些回调视为静态，根据给定的输入付款详细信息计算某些输出付款详细信息。如果客户端提供的输入状态因故无效，则操作会失败。 
如果机器人确定给定信息按原样有效，则只需发送 HTTP 状态代码 `200 OK` 以及未修改的付款详细信息。 或者，机器人可以发送 HTTP 状态代码 `200 OK` 以及在处理订单之前应该应用的更新付款详细信息。 在某些情况下，机器人可能会确定更新的信息无效，并且无法按原样处理订单。 例如，用户的送货地址可能会指定产品供应商不发货的国家/地区。 在这种情况下，机器人可能会发送 HTTP 状态代码 `200 OK` 以及一个填充了付款详细信息对象的错误属性的消息。 发送 `400` 或 `500` 范围内的任何 HTTP 状态代码会导致客户出现一般错误。
