---
title: WebBrowser セキュリティ
ms.date: 03/30/2017
helpviewer_keywords:
- WebBrowser control [Windows Forms], security
- security [Windows Forms], WebBrowser control
ms.assetid: 0968846e-48ee-485a-9797-65b5b9a622f8
ms.openlocfilehash: b25cabca050d06dbfe97c563eb56622d1f21be54
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095256"
---
# <a name="webbrowser-security"></a>WebBrowser セキュリティ
<xref:System.Windows.Forms.WebBrowser> コントロールは、完全信頼のみで動作するように設計されています。 コントロールに表示される HTML コンテンツは、外部 Web サーバーから取得でき、スクリプトまたは Web コントロールの形式でアンマネージコードを含むことができます。 このような場合に <xref:System.Windows.Forms.WebBrowser> コントロールを使用すると、Internet Explorer の場合よりもコントロールの安全性は低くなりますが、マネージ <xref:System.Windows.Forms.WebBrowser> コントロールでは、このようなアンマネージコードの実行が妨げられることはありません。  
  
 基になる ActiveX `WebBrowser` コントロールに関連するセキュリティの問題の詳細については、「 [WebBrowser コントロール](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/aa752040(v=vs.85))」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.WebBrowser>
- [WebBrowser コントロールの概要](webbrowser-control-overview.md)
- [WebBrowser コントロール](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/aa752040(v=vs.85))
