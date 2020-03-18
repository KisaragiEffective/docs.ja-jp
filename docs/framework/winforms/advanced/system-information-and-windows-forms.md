---
title: システム情報
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- domain names [Windows Forms], retrieving
- SystemInformation class [Windows Forms]
- user names [Windows Forms], retrieving
- system information [Windows Forms]
ms.assetid: 30cf43a3-8cb2-4ff3-862b-6c34576616a8
ms.openlocfilehash: a91e2fd8db0ef338ce30f89f11869f1b6698af3b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76732580"
---
# <a name="system-information-and-windows-forms"></a>システム情報と Windows フォーム
場合によっては、コードを決定するために、アプリケーションが実行されているコンピューターに関する情報を収集する必要があります。 たとえば、特定のネットワークドメインに接続されている場合にのみ適用される関数があるとします。この場合、ドメインを特定し、ドメインが存在しない場合は関数を無効にする方法が必要になります。  
  
 Windows フォームアプリケーションは、<xref:System.Windows.Forms.SystemInformation> クラスを使用して、実行時にコンピューターに関するさまざまなことを判断できます。 次の例は、<xref:System.Windows.Forms.SystemInformation> クラスを使用して <xref:System.Windows.Forms.SystemInformation.UserName%2A> と <xref:System.Windows.Forms.SystemInformation.UserDomainName%2A>を取得する方法を示しています。  
  
```vb  
Dim User As String = Windows.Forms.SystemInformation.UserName  
Dim Domain As String = Windows.Forms.SystemInformation.UserDomainName  
  
MessageBox.Show("Good morning " & User & ". You are connected to " _  
& Domain)  
```  
  
```csharp  
string User = SystemInformation.UserName;  
string Domain = SystemInformation.UserDomainName;  
  
MessageBox.Show("Good morning " + User + ". You are connected to "
+ Domain);
```  
  
 <xref:System.Windows.Forms.SystemInformation> クラスのすべてのメンバーは読み取り専用です。ユーザーの設定を変更することはできません。 クラスには100を超えるメンバーがあり、コンピューターに接続されているモニターの数 (<xref:System.Windows.Forms.SystemInformation.MonitorCount%2A>) から Windows エクスプローラーのアイコンの間隔 (<xref:System.Windows.Forms.SystemInformation.IconHorizontalSpacing%2A> と <xref:System.Windows.Forms.SystemInformation.IconVerticalSpacing%2A>) までのすべての情報が返されます。  
  
 <xref:System.Windows.Forms.SystemInformation> クラスの便利なメンバーの中には、<xref:System.Windows.Forms.SystemInformation.ComputerName%2A>、<xref:System.Windows.Forms.SystemInformation.DbcsEnabled%2A>、<xref:System.Windows.Forms.SystemInformation.PowerStatus%2A>、<xref:System.Windows.Forms.SystemInformation.TerminalServerSession%2A>などがあります。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.SystemInformation>
- [Windows フォームでの電源管理](power-management-in-windows-forms.md)
