---
title: WPF コントロールの使用
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms Designer [Windows Forms], interoperability with WPF
- interoperability [WPF]
ms.assetid: 03c85dce-26ad-44cd-bc1d-8e0cb56de096
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: e9883e9d31fc9e3e2ec3ca06b4fc830bcdc338ae
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460697"
---
# <a name="use-wpf-controls-in-windows-forms-apps"></a>Windows フォームアプリで WPF コントロールを使用する

Windows フォームベースのアプリケーションでは、Windows Presentation Foundation (WPF) コントロールを使用できます。 これらは2つの異なるビューテクノロジですが、円滑に相互運用されます。

Visual Studio の Windows フォームデザイナーには、Windows Presentation Foundation コントロールをホストするためのビジュアルデザイン環境が用意されています。 WPF コントロールは、<xref:System.Windows.Forms.Integration.ElementHost>という名前の特殊な Windows フォームコントロールによってホストされます。 このコントロールにより、WPF コントロールはフォームのレイアウトに参加し、キーボードおよびマウスメッセージを受け取ることができます。 デザイン時には、Windows フォームコントロールの場合と同じように <xref:System.Windows.Forms.Integration.ElementHost> コントロールを配置できます。

WPF ベースのアプリケーションでは、Windows フォームコントロールを使用することもできます。 詳細については、「 [Visual Studio での XAML のデザイン](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)」を参照してください。

## <a name="see-also"></a>関連項目

- [WPF と Windows フォームの相互運用](../../wpf/advanced/wpf-and-windows-forms-interoperation.md)
