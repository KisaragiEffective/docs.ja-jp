---
description: '詳細情報: 方法: アプリケーションを起動してキーストロークを送る ‐ Visual Basic'
title: '方法: アプリケーションを起動してキーストロークを送る ‐ Visual Basic'
ms.date: 10/23/2019
helpviewer_keywords:
- keystrokes, sending
- Shell command example [Visual Basic]
- processes, starting and sending keystrokes
- SendKeys.SendWait examples
ms.assetid: f1303184-fce4-44fb-88b4-aac5f42d5d77
ms.openlocfilehash: ea54b940b528e0833d9c7a6cbef67f65a4cb4f6b
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99666460"
---
# <a name="how-to-start-an-application-and-send-it-keystrokes-visual-basic"></a>方法: アプリケーションを起動してキーストロークを送る ‐ Visual Basic

この例では、<xref:Microsoft.VisualBasic.Interaction.Shell%2A> メソッドを使用してメモ帳アプリケーションを起動した後、[My.Computer.Keyboard.SendKeys](xref:Microsoft.VisualBasic.Devices.Keyboard.SendKeys%2A) メソッドを使用してキーストロークを送信することで、文を印刷します。

## <a name="example"></a>例

[!code-vb[VbVbalrMyComputer#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#25)]

## <a name="robust-programming"></a>信頼性の高いプログラミング

要求されたプロセス ID のアプリケーションが見つからない場合には、<xref:System.ArgumentException> 例外が発生します。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ

`Shell` 関数の呼び出しには完全な信頼が必要です (<xref:System.Security.SecurityException> クラス)。

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.Devices.Keyboard.SendKeys%2A>
- <xref:Microsoft.VisualBasic.Interaction.Shell%2A>
- <xref:Microsoft.VisualBasic.Interaction.AppActivate%2A>
