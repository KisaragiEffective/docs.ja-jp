---
title: コントロールに対するデザイナーアクションを使用した一般的なタスクの実行
ms.date: 02/13/2019
helpviewer_keywords:
- designer actions
ms.assetid: cac337e6-00f6-4584-80f4-75728f5ea113
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 342741b9ce032b1b8ec50a6c689d9109d12f5b3b
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77634895"
---
# <a name="walkthrough-perform-common-tasks-using-designer-actions"></a>チュートリアル: デザイナーアクションを使用した一般的なタスクの実行

Windows フォームアプリケーションのフォームとコントロールを構築する際には、繰り返し実行する多くのタスクがあります。 次の一覧は、一般的に実行されるタスクの一部を示しています。

- <xref:System.Windows.Forms.TabControl>のタブを追加または削除する。
- コントロールをその親にドッキングします。
- <xref:System.Windows.Forms.SplitContainer> コントロールの向きを変更する。

開発を高速化するために、多くのコントロールはデザイナーアクションを提供しています。これは、デザイン時に1つのジェスチャでこのような一般的なタスクを実行できるようにする、状況依存のメニューです。 これらのタスクは、*デザイナーアクション動詞*と呼ばれます。

デザイナーのアクションは、デザイナーの有効期間にわたってコントロールインスタンスにアタッチされたままになり、常に使用できるようになります。

## <a name="create-the-project"></a>プロジェクトを作成する

最初にプロジェクトを作成し、フォームを設定します。

1. Visual Studio で、 **DesignerActionsExample**という名前の Windows ベースのアプリケーションプロジェクトを作成します。

2. **Windows フォームデザイナー**でフォームを選択します。

## <a name="use-designer-actions"></a>デザイナーアクションを使用する

デザイナーアクションは、デザイン時に、それらを提供するコントロールに対して常に使用できます。

1. **[ツールボックス]** から <xref:System.Windows.Forms.TabControl> をフォームにドラッグします。 <xref:System.Windows.Forms.TabControl>の横に表示されるデザイナーアクショングリフ (![小さい黒い矢印](./media/designer-actions-glyph.gif)) に注意してください。

2. [デザイナーアクション] グリフをクリックします。 グリフの横に表示されるショートカットメニューで、 **[タブの追加]** 項目を選択します。 新しいタブページが <xref:System.Windows.Forms.TabControl>に追加されていることを確認します。

3. <xref:System.Windows.Forms.TableLayoutPanel> ツールボックス **から** コントロールをフォームにドラッグします。

4. [デザイナーアクション] グリフをクリックします。 グリフの横に表示されるショートカットメニューで、 **[列の追加]** 項目を選択します。 新しい列が <xref:System.Windows.Forms.TableLayoutPanel> コントロールに追加されていることを確認します。

5. <xref:System.Windows.Forms.SplitContainer> ツールボックス **から** コントロールをフォームにドラッグします。

6. [デザイナーアクション] グリフをクリックします。 グリフの横に表示されるショートカットメニューで、 **[横スプリッターの方向]** 項目を選択します。 <xref:System.Windows.Forms.SplitContainer> コントロールのスプリッターバーが水平方向に配置されていることを確認します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.TextBox>
- <xref:System.Windows.Forms.TabControl>
- <xref:System.Windows.Forms.SplitContainer>
- <xref:System.ComponentModel.Design.DesignerActionList>
