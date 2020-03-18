---
title: ワークフロー デザイン操作のカスタマイズ
ms.date: 03/30/2017
helpviewer_keywords:
- extending [WF], Workflow Designer
ms.assetid: 98135077-0f5d-4d16-9337-01094e843537
ms.openlocfilehash: 27be398d874747b65ae051224070d3f40f1fbbb0
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715136"
---
# <a name="customizing-the-workflow-design-experience"></a>ワークフロー デザイン操作のカスタマイズ

カスタムアクティビティを設計し、Windows ワークフローデザイナーを再ホストするシナリオは、.NET Framework 4 で大幅に簡素化されています。 開発も配置も簡単になり、柔軟性も向上しました。 主要なインフラストラクチャの変更は、新しいアクティビティデザイナーのプログラミングモデルが Windows Presentation Foundation (WPF) に基づいて構築されることです。 これにより、アクティビティデザイナーを宣言によって定義し、他のアプリケーションのワークフローデザイナーを比較しやすくすることができます。 再ホストするときに、カスタム式エディターを開発して、IntelliSense や簡略化された式ドメインをサポートできます。 ワークフローサービスを使用すると、Windows Communication Foundation (WCF) との統合がシームレスになりました。 カスタム アクティビティ デザイナーおよびモデル アイテム ツリーを使用して、再ホストされたワークフロー デザイナーのデザイン時の操作を拡張できます。

## <a name="in-this-section"></a>このセクションの内容

 [カスタム アクティビティ デザイナーおよびテンプレートの使用](using-custom-activity-designers-and-templates.md)

 新しいカスタム アクティビティ デザイナーおよびテンプレートを作成する方法について説明します。

 [ワークフロー デザイナーのホスト変更](rehosting-the-workflow-designer.md)

 Visual Studio の外部で Windows ワークフローデザイナーを再ホストする方法と、検証エラーを表示する方法について説明します。

 [カスタム式エディターの使用](using-a-custom-expression-editor.md)

 Visual Studio 2010 の外部で再ホストされたワークフローデザイナーで使用するカスタム式エディターを実装する方法について説明します。

## <a name="reference"></a>辞書／辞典／その他

<xref:System.Activities.Presentation.ActivityDesigner>

## <a name="see-also"></a>参照

- [Windows Workflow Foundation の拡張](extend.md)
- [デザイナー](./samples/designer.md)
- [カスタム アクティビティ デザイナー](./samples/custom-activity-designers.md)
- [デザイナーのホスト変更](./samples/designer-rehosting.md)
