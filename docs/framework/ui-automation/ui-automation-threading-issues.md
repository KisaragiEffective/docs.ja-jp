---
title: UI オートメーション スレッド処理の問題点
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, threading issues
- threading issues with UI Automation
ms.assetid: 0ab8d42c-5b8b-481b-b788-2caecc2f0191
ms.openlocfilehash: 8dc21a680a19933e9db8d52a0e6b7e6ffdd333f8
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74800830"
---
# <a name="ui-automation-threading-issues"></a>UI オートメーション スレッド処理の問題点
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI Automation (Windows のオートメーション API: UI オートメーション)](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] の Windows メッセージを使用する方法が原因で、クライアント アプリケーションが [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] スレッド上のそれ自体の [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] と対話しようとするときに、競合が発生する可能性があります。 これらの競合が発生すると、パフォーマンスが著しく低下する恐れがあります。また、アプリケーションが応答を停止する可能性もあります。  
  
 クライアント アプリケーションがデスクトップ上のすべての要素 (それ自体の [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]を含む) と対話する場合、すべての [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] の呼び出しを個別のスレッドで実行する必要があります。 これには、要素の検索 (たとえば、 <xref:System.Windows.Automation.TreeWalker> または <xref:System.Windows.Automation.AutomationElement.FindAll%2A> メソッドを使用) とコントロール パターンの使用が含まれます。  
  
 イベント ハンドラーは常に [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 以外のスレッドで呼び出されるため、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] の呼び出しを[!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] イベント ハンドラー内で実行しても問題ありません。 ただし、クライアント アプリケーションの [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]から発生するイベントをサブスクライブする場合、 <xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A>以外のスレッド上の[!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] (または関連するメソッド) への呼び出しを実行する必要があります。 同じスレッドにあるイベント ハンドラーを削除します。
