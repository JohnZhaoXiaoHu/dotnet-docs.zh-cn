---
title: 如何：远程调查打印机的状态
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- surveying printer status remotely [WPF]
- printer status [WPF], surveying remotely
- remotely surveying printer status [WPF]
- status [WPF], printers [WPF], surveying remotely
ms.assetid: d6324759-8292-4c23-9584-9c708887dc94
ms.openlocfilehash: eca1720aea1620683ebc9ed08b47a0f5625da9d9
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/04/2018
ms.locfileid: "33545966"
---
# <a name="how-to-remotely-survey-the-status-of-printers"></a>如何：远程调查打印机的状态
在大中型公司，在任何给定时间里，都可能发生由于卡纸、纸张用完或某些其他有问题而导致多台打印机无法工作的情况。 打印机属性中公开的丰富[!INCLUDE[TLA#tla_api#plural](../../../../includes/tlasharptla-apisharpplural-md.md)]Microsoft.NET framework 提供一种执行快速调查的打印机的状态。  
  
## <a name="example"></a>示例  
 以下是创建此类实用程序的主要步骤。  
  
1.  获取所有打印服务器的列表。  
  
2.  循环访问服务器以查询其打印队列。  
  
3.  在每一轮服务器循环访问过程中，循环访问所有服务器的队列并读取每个属性，这些属性可能指示队列当前不在工作。  
  
 以下代码是一系列代码段。 为简单起见，本示例假定存在通过 CRLF 分隔的打印服务器列表。 变量`fileOfPrintServers`是<xref:System.IO.StreamReader>此文件的对象。 由于每个服务器名称是在其对应行中，任何调用的<xref:System.IO.StreamReader.ReadLine%2A>获取下一个服务器的名称，并将移动<xref:System.IO.StreamReader>的光标所在位置到下一行的开头。  
  
 在外部循环中，该代码创建<xref:System.Printing.PrintServer>最新的打印服务器对象，并指定应用程序是具有管理权限的服务器。  
  
> [!NOTE]
>  如果有大量的服务器，您可以通过改进性能<xref:System.Printing.PrintServer.%23ctor%28System.String%2CSystem.String%5B%5D%2CSystem.Printing.PrintSystemDesiredAccess%29>只能初始化你将需要的属性的构造函数。  
  
 然后该示例使用<xref:System.Printing.PrintServer.GetPrintQueues%2A>来创建所有服务器的集合的排队，并开始循环访问它们。 此内部循环包含一个分支结构，该结构对应于检查打印机状态的两种方法：  
  
-   你可以阅读的标志<xref:System.Printing.PrintQueue.QueueStatus%2A>属性的类型<xref:System.Printing.PrintQueueStatus>。  
  
-   你可以读取每个相关的属性，如<xref:System.Printing.PrintQueue.IsOutOfPaper%2A>，和<xref:System.Printing.PrintQueue.IsPaperJammed%2A>。  
  
 此示例演示这两种方法，以便用户以前已提示要使用的方法，并响应并且具有"y"如果他或她想要使用的标志<xref:System.Printing.PrintQueue.QueueStatus%2A>属性。 请参阅以下有关这两种方法的详细信息。  
  
 最后，会向用户显示结果。  
  
 [!code-cpp[PrinterStatusSurvey#SurveyQueues](../../../../samples/snippets/cpp/VS_Snippets_Wpf/PrinterStatusSurvey/CPP/Program.cpp#surveyqueues)]
 [!code-csharp[PrinterStatusSurvey#SurveyQueues](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PrinterStatusSurvey/CSharp/Program.cs#surveyqueues)]
 [!code-vb[PrinterStatusSurvey#SurveyQueues](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PrinterStatusSurvey/visualbasic/program.vb#surveyqueues)]  
  
 若要检查使用的标志的打印机状态<xref:System.Printing.PrintQueue.QueueStatus%2A>属性，你将检查每个相关的标志，以查看它是否设置。 检查是否在一组位标志中设置了一个位的标准方法是执行一个逻辑 AND 运算，其中将该组标志作为一个操作数，将标志本身作为另一操作数。 由于该标志本身仅设置一个位，因此逻辑 AND 的结果至多为设置了该相同位。 若要查明事实是否如此，只需将逻辑 AND 的结果与标志本身进行比较。 有关详细信息，请参阅<xref:System.Printing.PrintQueueStatus>、 [& 运算符 （C# 参考）](~/docs/csharp/language-reference/operators/and-operator.md)，和<xref:System.FlagsAttribute>。  
  
 对于已设置了其位的各个特性，代码会将一条通知添加到将向用户显示的最终报告中。 （下面会讨论代码结束时调用的 **ReportAvailabilityAtThisTime** 方法。）  
  
 [!code-cpp[PrinterStatusSurvey#SpotTroubleUsingQueueAttributes](../../../../samples/snippets/cpp/VS_Snippets_Wpf/PrinterStatusSurvey/CPP/Program.cpp#spottroubleusingqueueattributes)]
 [!code-csharp[PrinterStatusSurvey#SpotTroubleUsingQueueAttributes](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PrinterStatusSurvey/CSharp/Program.cs#spottroubleusingqueueattributes)]
 [!code-vb[PrinterStatusSurvey#SpotTroubleUsingQueueAttributes](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PrinterStatusSurvey/visualbasic/program.vb#spottroubleusingqueueattributes)]  
  
 若要使用各个属性检查打印机状态，只需读取各个属性，并将注释添加到最终报告，如果属性为 `true`，将向用户显示最终报告。 （下面会讨论代码结束时调用的 **ReportAvailabilityAtThisTime** 方法。）  
  
 [!code-cpp[PrinterStatusSurvey#SpotTroubleUsingQueueProperties](../../../../samples/snippets/cpp/VS_Snippets_Wpf/PrinterStatusSurvey/CPP/Program.cpp#spottroubleusingqueueproperties)]
 [!code-csharp[PrinterStatusSurvey#SpotTroubleUsingQueueProperties](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PrinterStatusSurvey/CSharp/Program.cs#spottroubleusingqueueproperties)]
 [!code-vb[PrinterStatusSurvey#SpotTroubleUsingQueueProperties](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PrinterStatusSurvey/visualbasic/program.vb#spottroubleusingqueueproperties)]  
  
 创建了 **ReportAvailabilityAtThisTime** 方法，以应对需要确定队列在一天的当前时间是否可用的情况。  
  
 该方法将不执行任何操作如果<xref:System.Printing.PrintQueue.StartTimeOfDay%2A>和<xref:System.Printing.PrintQueue.UntilTimeOfDay%2A>属性相等，则因为在这种情况下打印机不可用在所有时间。 如果它们不同，该方法将获取当前时间，这样就会具有要转换到午夜的总分钟数，因为<xref:System.Printing.PrintQueue.StartTimeOfDay%2A>和<xref:System.Printing.PrintQueue.UntilTimeOfDay%2A>属性<xref:System.Int32>s 不表示分钟后午夜<xref:System.DateTime>对象。 最后，该方法会检查当前时间是否介于开始时间和“截至”时间之间。  
  
 [!code-cpp[PrinterStatusSurvey#UsingStartAndUntilTimes](../../../../samples/snippets/cpp/VS_Snippets_Wpf/PrinterStatusSurvey/CPP/Program.cpp#usingstartanduntiltimes)]
 [!code-csharp[PrinterStatusSurvey#UsingStartAndUntilTimes](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PrinterStatusSurvey/CSharp/Program.cs#usingstartanduntiltimes)]
 [!code-vb[PrinterStatusSurvey#UsingStartAndUntilTimes](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PrinterStatusSurvey/visualbasic/program.vb#usingstartanduntiltimes)]  
  
## <a name="see-also"></a>请参阅  
 <xref:System.Printing.PrintQueue.StartTimeOfDay%2A>  
 <xref:System.Printing.PrintQueue.UntilTimeOfDay%2A>  
 <xref:System.DateTime>  
 <xref:System.Printing.PrintQueueStatus>  
 <xref:System.FlagsAttribute>  
 <xref:System.Printing.PrintServer.GetPrintQueues%2A>  
 <xref:System.Printing.PrintServer>  
 <xref:System.Printing.LocalPrintServer>  
 <xref:System.Printing.EnumeratedPrintQueueTypes>  
 <xref:System.Printing.PrintQueue>  
 [& 运算符 （C# 参考）](~/docs/csharp/language-reference/operators/and-operator.md)  
 [WPF 中的文档](../../../../docs/framework/wpf/advanced/documents-in-wpf.md)  
 [打印概述](../../../../docs/framework/wpf/advanced/printing-overview.md)
