---
title: System.ServiceModel.Channels.MsmqPoisonMessageRejected
ms.date: 03/30/2017
ms.assetid: 0e64b9bd-1f12-43df-a189-d7be3c2bace1
ms.openlocfilehash: 27402017e5e79194578719fd0c921dfc1e047b80
ms.sourcegitcommit: 2eceb05f1a5bb261291a1f6a91c5153727ac1c19
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2018
ms.locfileid: "43512955"
---
# <a name="systemservicemodelchannelsmsmqpoisonmessagerejected"></a>System.ServiceModel.Channels.MsmqPoisonMessageRejected
拒绝的病毒消息。  
  
## <a name="description"></a>描述  
 该跟踪指示遇到病毒消息并随后将其拒绝。 当 NetMsmqBinding 或 MsmqIntegrationBinding 上的 `ReceiveErrorHandling` 属性设置为 `Reject` 时，将会出现此情况。 将被拒绝的消息传递回发件人[死信队列](https://go.microsoft.com/fwlink/?LinkId=99544)。  
  
 请参阅[病毒消息处理](https://go.microsoft.com/fwlink/?LinkId=99546)有关消息何时成为病毒和如何将服务配置为适当地处理更多详细信息。 请参阅[MQMarkMessageRejected](https://go.microsoft.com/fwlink/?LinkId=99548)有关被拒绝的消息在 MSMQ 中的含义的详细信息。  
  
## <a name="see-also"></a>请参阅  
 [跟踪](../../../../../docs/framework/wcf/diagnostics/tracing/index.md)  
 [使用跟踪来排除应用程序故障](../../../../../docs/framework/wcf/diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md)  
 [管理和诊断](../../../../../docs/framework/wcf/diagnostics/index.md)  
 [病毒消息处理](https://go.microsoft.com/fwlink/?LinkId=99546)  
 [MQMarkMessageRejected](https://go.microsoft.com/fwlink/?LinkId=99548)
