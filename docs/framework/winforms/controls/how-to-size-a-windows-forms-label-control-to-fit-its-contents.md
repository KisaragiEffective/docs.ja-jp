---
title: コンテンツに合わせてラベルコントロールのサイズを変更する
ms.date: 03/30/2017
helpviewer_keywords:
- captions [Windows Forms], sizing
- sizing controls
- size [Windows Forms], controls
- labels [Windows Forms], sizing to fit contents
- Label control [Windows Forms], sizing to fit contents
ms.assetid: 99648964-63b2-438c-980e-d24103ad602b
ms.openlocfilehash: 6a563693feaa5074f5d13f0b82cc4d0305a79c23
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743772"
---
# <a name="how-to-size-a-windows-forms-label-control-to-fit-its-contents"></a>方法 : Windows フォーム Label コントロールのサイズを内容に合わせて変更する
Windows フォーム <xref:System.Windows.Forms.Label> コントロールは、単一行または複数行にすることができ、サイズを固定することも、キャプションに合わせて自動的にサイズを変更することもできます。 <xref:System.Windows.Forms.Label.AutoSize%2A> プロパティを使用すると、コントロールのサイズを大きくしたり小さくしたりすることができます。これは、実行時にキャプションが変化する場合に特に便利です。  
  
### <a name="to-make-a-label-control-resize-dynamically-to-fit-its-contents"></a>コンテンツに合わせてラベルコントロールのサイズを動的に変更するには  
  
1. <xref:System.Windows.Forms.Label.AutoSize%2A> プロパティを `true`に設定します。  
  
 <xref:System.Windows.Forms.Label.AutoSize%2A> が `false`に設定されている場合、可能であれば、<xref:System.Windows.Forms.Label.Text%2A> プロパティで指定された単語は次の行に折り返されますが、コントロールは拡張されません。  
  
## <a name="see-also"></a>参照

- [方法: Windows フォームの Label コントロールでアクセス キーを作成する](how-to-create-access-keys-with-windows-forms-label-controls.md)
- [Label コントロールの概要](label-control-overview-windows-forms.md)
- [Label コントロール](label-control-windows-forms.md)
