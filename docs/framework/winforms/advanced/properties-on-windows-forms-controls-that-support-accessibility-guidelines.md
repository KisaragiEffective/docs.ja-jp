---
title: コントロールのアクセシビリティプロパティ
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms, accessibility properties of controls
- accessibility [Windows Forms], Windows Forms control properties
ms.assetid: ad3567a6-313b-4708-9e15-f487a831f049
ms.openlocfilehash: 73d81f5b5f656ed5ef90bdd9b6fe1b3293123f97
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743421"
---
# <a name="properties-on-windows-forms-controls-that-support-accessibility-guidelines"></a>ユーザ補助ガイドラインをサポートする Windows フォーム コントロールのプロパティ
キーボードフォーカスの公開や画面要素の公開など、アクセシビリティに関する多くのガイドラインをサポートするために、Windows フォームの標準ツールボックスのコントロール。  
  
## <a name="planning-ahead-for-accessibility"></a>ユーザー補助のための事前計画  
 コントロールのプロパティは、次の表に示すように、他のアクセシビリティガイドラインをサポートするために使用できます。 また、メニューを使用して、プログラムの機能へのアクセスを提供する必要があります。  
  
|Control プロパティ|アクセシビリティに関する考慮事項|  
|----------------------|--------------------------------------|  
|AccessibleDescription|説明は、スクリーンリーダーなどのユーザー補助機能に報告されます。 ユーザー補助機能は専用のプログラムおよびデバイスで、障碍を持つユーザーがコンピューターをより効果的に使用するよう助けます。|  
|AccessibleName|ユーザー補助機能に報告される名前。|  
|AccessibleRole|ユーザーインターフェイスでの要素の使用方法について説明します。|  
|TabIndex|フォームを使用して、実用的なナビゲーションパスを作成します。 テキストボックスなどの固有のラベルを持たないコントロールは、関連付けられているラベルをタブオーダーの直前に配置することが重要です。|  
|Text|アクセスキーを作成するには、"&" 文字を使用します。 アクセスキーの使用は、ドキュメント化されたキーボードアクセスを機能に提供することの一部です。|  
|フォント サイズ|フォントサイズを調整できない場合は、10ポイント以上に設定する必要があります。 フォームのフォントサイズを設定すると、その後フォームに追加されたすべてのコントロールのサイズが同じになります。|  
|前景色|このプロパティが既定値に設定されている場合は、ユーザーの色の設定がフォームで使用されます。|  
|背景色|このプロパティが既定値に設定されている場合は、ユーザーの色の設定がフォームで使用されます。|  
|BackgroundImage|テキストを読みやすくするには、このプロパティを空白のままにします。|  
  
## <a name="see-also"></a>参照

- [チュートリアル: ユーザー補助対応の Windows ベースのアプリケーションの作成](walkthrough-creating-an-accessible-windows-based-application.md)
