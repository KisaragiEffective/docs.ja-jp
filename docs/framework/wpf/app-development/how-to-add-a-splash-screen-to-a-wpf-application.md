---
title: スプラッシュスクリーンを追加する方法
ms.date: 08/18/2018
helpviewer_keywords:
- WPF [WPF], splash screen
- startup window [WPF]
- SplashScreen class [WPF]
- splash screen [WPF]
ms.assetid: d70a25c4-5fb9-4c27-b01d-b1b8ef39b3fd
ms.openlocfilehash: 39f53e21c40f036c65894b4f275cd5fb414999be
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740450"
---
# <a name="how-to-add-a-splash-screen-to-a-wpf-application"></a>方法 : スプラッシュ スクリーンを WPF アプリケーションに追加する

このトピックでは、Windows Presentation Foundation (WPF) アプリケーションにスタートアップウィンドウまたは*スプラッシュスクリーン*を追加する方法について説明します。

## <a name="to-add-an-existing-image-as-a-splash-screen"></a>既存のイメージをスプラッシュスクリーンとして追加するには

1. スプラッシュスクリーンに使用するイメージを作成または検索します。 Windows Imaging Component (WIC) でサポートされている任意のイメージ形式を使用できます。 たとえば、BMP、GIF、JPEG、PNG、または TIFF 形式を使用できます。

2. WPF アプリケーションプロジェクトにイメージファイルを追加します。

3. **ソリューションエクスプローラー**で、画像を選択します。

4. プロパティウィンドウで、 **[ビルドアクション]** プロパティのドロップダウン矢印をクリックします。

5. ドロップダウンリストから **[SplashScreen]** を選択します。

6. **F5** キーを押してアプリケーションをビルドし、実行します。

     スプラッシュスクリーンのイメージが画面の中央に表示され、メインアプリケーションウィンドウが表示されるとフェードします。

## <a name="to-exclude-the-splash-screen-from-build"></a>ビルドからスプラッシュスクリーンを除外するには

1. **ソリューションエクスプローラー**で、スプラッシュスクリーンのイメージを選択します。

2. **[プロパティ]** ウィンドウで、[**ビルド] アクション**を **[なし**] に設定します。

## <a name="to-remove-the-splash-screen-from-an-application"></a>アプリケーションからスプラッシュスクリーンを削除するには

**ソリューションエクスプローラー**で、スプラッシュスクリーンのイメージを削除します。

## <a name="see-also"></a>参照

- <xref:System.Windows.SplashScreen>
- [方法: プロジェクトに既存の項目を追加する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/9f4t9t92(v=vs.100))
