---
title: デザイン時にコントロールを開発する
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms controls [Windows Forms]
- Windows Forms controls, creating
- controls [Windows Forms]
- controls [Windows Forms], creating
- user controls [Windows Forms]
- custom controls [Windows Forms]
ms.assetid: e5a8e088-7ec8-4fd9-bcb3-9078fd134829
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 7626e1efbb30ef3bfe9b5b1278c0adb18dd5944b
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628710"
---
# <a name="develop-windows-forms-controls-at-design-time"></a>デザイン時に Windows フォームコントロールを開発する

コントロールの作成者は、.NET Framework のさまざまなコントロール作成テクノロジを利用できます。 既存のコントロールを組み合わせた複合コントロールしかデザインできないといった制約はなくなりました。 継承により、既存の複合コントロールや既存の Windows フォーム コントロールから、独自のコントロールを作成できます。 カスタム描画を実装する独自のコントロールを設計することもできます。 これらのオプションを使うと、非常に柔軟にビジュアル インターフェイスのデザインと機能を決めることができます。 これらの機能を利用するには、オブジェクト ベースのプログラミング概念について理解しておく必要があります。

> [!NOTE]
> 継承について十分に理解している必要はありませんが、[継承の基本 (Visual Basic)](~/docs/visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)を参照すると役立つ場合があります。

Web フォームで使うカスタム コントロールを作る場合は、「[カスタム ASP.NET サーバー コントロールの開発](https://docs.microsoft.com/previous-versions/aspnet/zt27tfhy(v=vs.100))」をご覧ください。

## <a name="in-this-section"></a>このセクションの内容

[チュートリアル: 複合コントロールの作成](walkthrough-authoring-a-composite-control-with-visual-csharp.md)\
C# で簡単な複合コントロールを作る方法を示します。

[チュートリアル: Windows フォームコントロールからの継承](walkthrough-inheriting-from-a-windows-forms-control-with-visual-csharp.md)\
C# で継承を使って簡単な Windows フォーム コントロールを作る方法を示します。

[チュートリアル: デザイナーアクションを使用した一般的なタスクの実行](perform-common-tasks-design-actions.md)\
Windows フォーム コントロールでスマート タグ機能を使う方法を示します。

[チュートリアル: DesignerSerializationVisibilityAttribute\ を使用した標準型のコレクションのシリアル](serializing-collections-designerserializationvisibilityattribute.md)化
<xref:System.ComponentModel.DesignerSerializationVisibilityAttribute.Content?displayProperty=nameWithType> 属性を使用してコレクションをシリアル化する方法について説明します。

[チュートリアル : カスタム Windows フォーム コントロールのデザイン時のデバッグ](walkthrough-debugging-custom-windows-forms-controls-at-design-time.md)\
Windows フォーム コントロールのデザイン時動作をデバッグする方法を示します。

[チュートリアル: Visual Studio のデザイン時機能を利用する Windows フォームコントロールの作成](creating-a-wf-control-design-time-features.md)\
デザイン環境に複合コントロールを緊密に統合する方法を示します。

[方法 : Windows フォームのコントロールを作成する](how-to-author-controls-for-windows-forms.md)\
Windows フォーム コントロールの実装に関する考慮事項の概要を説明します。

[方法 : 複合コントロールを作成する](how-to-author-composite-controls.md)\
複合コントロールから継承することでコントロールを作る方法を示します。

[方法 : UserControl クラスを継承する](how-to-inherit-from-the-usercontrol-class.md)\
複合コントロール作成手順の概要を説明します。

[方法 : 既存の Windows フォーム コントロールから継承する](how-to-inherit-from-existing-windows-forms-controls.md)\
<xref:System.Windows.Forms.Button> コントロールクラスから継承することによって拡張コントロールを作成する方法について説明します。

[方法 : コントロール クラスを継承する](how-to-inherit-from-the-control-class.md)\
拡張コントロールの作成の概要を説明します。

[方法: デザイン時にフォームの端に合わせてコントロールを配置](how-to-align-a-control-to-the-edges-of-forms-at-design-time.md)する\
<xref:System.Windows.Forms.Control.Dock%2A> プロパティを使用して、コントロールをフォームの端に揃えて配置する方法について説明します。

[方法: [ツールボックス アイテムの選択] ダイアログ ボックスにコントロールを表示する](how-to-display-a-control-in-the-choose-toolbox-items-dialog-box.md)\
**ツールボックスのカスタマイズ** ダイアログ ボックスに表示されるようにコントロールをインストールする手順を示します。

[方法: コントロールのツールボックスビットマップを指定](how-to-provide-a-toolbox-bitmap-for-a-control.md)する\
<xref:System.Drawing.ToolboxBitmapAttribute> を使用して、**ツールボックス**のカスタムコントロールの横にアイコンを表示する方法について説明します。

[方法 : UserControl の実行時の動作をテストする](how-to-test-the-run-time-behavior-of-a-usercontrol.md)\
**UserControl テスト コンテナー**を使って複合コントロールの動作をテストする方法を示します。

[Windows フォームデザイナー\ でのデザイン時エラー](design-time-errors-in-the-windows-forms-designer.md)
Windows フォーム デザイナーで読み込みに失敗したときに Microsoft Visual Studio に表示されるデザイン時エラー リストの意味と使用法について説明します。

[コントロールとコンポーネントの作成時のトラブルシューティング](troubleshooting-control-and-component-authoring.md)\
カスタム コンポーネントやコントロールを作るときに発生する可能性がある一般的な問題を診断して解決する方法を示します。

## <a name="reference"></a>参照

- <xref:System.Windows.Forms.Control?displayProperty=nameWithType>

- <xref:System.Windows.Forms.UserControl?displayProperty=nameWithType>

## <a name="related-sections"></a>関連項目

[.NET Framework を使用したカスタム Windows フォーム コントロールの開発](developing-custom-windows-forms-controls.md)\
.NET Framework で独自のカスタム コントロールを作る方法について説明します。

[Language Independence and Language-Independent Components](../../../standard/language-independence-and-language-independent-components.md)\
コンポーネントの作成と使用を簡略化するように設計されている共通言語ランタイムの概要について説明します。 この簡略化の重要な側面は、さまざまなプログラミング言語で記述されたコンポーネント間の相互運用性の拡張です。 共通言語仕様 (CLS) を使うと、複数のプログラミング言語で動作するツールやコンポーネントを作ることができます。

[チュートリアル : ツールボックスへのカスタム コンポーネントの自動設定](walkthrough-automatically-populating-the-toolbox-with-custom-components.md)\
コンポーネントやコントロールを**ツールボックスのカスタマイズ** ダイアログ ボックスに表示できるようにする方法を説明します。
