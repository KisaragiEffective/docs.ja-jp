---
title: '方法 : 複合コントロールを作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- UserControl class [Windows Forms], creating composite controls
- user controls [Windows Forms], creating
- user controls [Windows Forms], inheriting from
- composite controls [Windows Forms], creating
ms.assetid: 79c9cf05-5ab6-4a18-886d-88a64748b098
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 42ea424507b89576df8099fd4849dd2665135a55
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459437"
---
# <a name="how-to-author-composite-controls"></a>方法: 複合コントロールを作成する

複合コントロールはさまざまな方法で使用できます。 Windows デスクトップ アプリケーション プロジェクトの一部として複合コントロールを作成し、プロジェクト内のフォーム上でのみ使用することができます。 または、Windows コントロール ライブラリ プロジェクトで複合コントロールを作成し、プロジェクトをアセンブリにコンパイルして、他のプロジェクトで使用することもできます。 また、それらを継承し、ビジュアル継承を使用して、特殊な目的ですばやくカスタマイズすることもできます。

## <a name="to-author-a-composite-control"></a>複合コントロールを作成するには

1. Visual Studio で、新しい**Windows アプリケーション**プロジェクトを作成し、 **democontrolhost**という名前を指定します。

2. **[プロジェクト]** メニューの **[ユーザー コントロールの追加]** をクリックします。

3. **[新しい項目の追加]** ダイアログ ボックスで、クラス ファイル (.vb または .cs ファイル) に複合コントロールに付ける名前を指定します。

4. **[追加]** ボタンを選択して、複合コントロールのクラスファイルを作成します。

5. 複合コントロールのサーフェスに **[ツールボックス]** からコントロールを追加します。

6. 複合コントロールまたはその内在コントロールによって発生したイベントを処理するために、イベント プロシージャにコードを配置します。

7. 複合コントロールのデザイナーを閉じて、メッセージが表示されたらファイルを保存します。

8. **[ビルド]** メニューの **[ソリューションのビルド]** をクリックします。

     カスタム コントロールが **[ツールボックス]** に表示されるようにプロジェクトをビルドする必要があります。

9. **[ツールボックス]** の **[DemoControlHost]** タブを使用して、コントロールのインスタンスを `Form1` に追加します。

## <a name="to-author-a-control-class-library"></a>コントロール クラス ライブラリを作成するには

1. 新しい **Windows コントロール ライブラリ** プロジェクトを開きます。

     既定では、プロジェクトに複合コントロールが含まれます。

2. 上記の手順の説明に従ってコントロールとコードを追加します。

3. 継承クラスで変更しないコントロールを選択して、このコントロールの **Modifiers** プロパティを **Private** に設定します。

4. DLL をビルドします。

## <a name="to-inherit-from-a-composite-control-in-a-control-class-library"></a>コントロール クラス ライブラリの複合コントロールから継承するには

1. **[ファイル]** メニューで、 **[追加]** をポイントし、 **[新しいプロジェクト]** を選択して、新しい **Windows アプリケーション** プロジェクトをソリューションに追加します。

2. **ソリューション エクスプローラー**で、新しいプロジェクトの **[参照設定]** フォルダーを右クリックし、 **[参照の追加]** をクリックして **[参照の追加]** ダイアログ ボックスを開きます。

3. **[プロジェクト]** タブを選択し、コントロール ライブラリ プロジェクトをダブルクリックします。

4. **[ビルド]** メニューの **[ソリューションのビルド]** をクリックします。

5. **ソリューション エクスプローラー**で、コントロール ライブラリ プロジェクトを右クリックし、ショートカット メニューの **[新しい項目の追加]** をクリックします。

6. **[新しい項目の追加]** ダイアログ ボックスの **[継承されたユーザー コントロール]** テンプレートを選択します。

7. **[継承ピッカー]** ダイアログ ボックスで、継承元のコントロールをダブルクリックします。

     新しいコントロールがプロジェクトに追加されます。

8. 新しいコントロールのビジュアル デザイナーを開き、別の内在コントロールを追加します。

     DLL の複合コントロールから継承された内在コントロールを表示し、**Modifiers** プロパティが **Public** であるコントロールのプロパティを変更することができます。 **Modifiers** プロパティが **Private** であるコントロールのプロパティを変更することはできません。

## <a name="see-also"></a>関連項目

- [チュートリアル: 複合コントロールの作成](walkthrough-authoring-a-composite-control-with-visual-csharp.md)
- [チュートリアル: Windows フォームコントロールからの継承](walkthrough-inheriting-from-a-windows-forms-control-with-visual-csharp.md)
- [コントロールの種類に関するアドバイス](control-type-recommendations.md)
- [方法: Windows フォームのコントロールを作成する](how-to-author-controls-for-windows-forms.md)
- [さまざまなカスタム コントロール](varieties-of-custom-controls.md)
