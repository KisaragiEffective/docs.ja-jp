---
title: ホスティング用の Direct3D9 コンテンツの作成
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- WPF [WPF], creating Direct3D9 content
- Direct3D9 [WPF interoperability], creating Direct3D9 content
ms.assetid: 286e98bc-1eaa-4b5e-923d-3490a9cca5fc
ms.openlocfilehash: 847ee74da5b295c2c9d3824b3df74f94bc98a4db
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727916"
---
# <a name="walkthrough-creating-direct3d9-content-for-hosting-in-wpf"></a>チュートリアル : WPF でホストするための Direct3D9 コンテンツの作成
このチュートリアルでは、Windows Presentation Foundation (WPF) アプリケーションでのホストに適した Direct3D9 コンテンツを作成する方法について説明します。 WPF アプリケーションで Direct3D9 コンテンツをホストする方法の詳細については、「 [wpf と Direct3D9 の相互運用](wpf-and-direct3d9-interoperation.md)」を参照してください。

 このチュートリアルでは、次のタスクを実行します。

- Direct3D9 プロジェクトを作成します。

- WPF アプリケーションでホストするための Direct3D9 プロジェクトを構成します。

 完了すると、WPF アプリケーションで使用する Direct3D9 コンテンツを含む DLL が作成されます。

## <a name="prerequisites"></a>前提条件
 このチュートリアルを実行するには、次のコンポーネントが必要です。

- Visual Studio 2010。

- DirectX SDK 9 以降。

## <a name="creating-the-direct3d9-project"></a>Direct3D9 プロジェクトの作成
 最初の手順では、Direct3D9 プロジェクトを作成して構成します。

#### <a name="to-create-the-direct3d9-project"></a>Direct3D9 プロジェクトを作成するには

1. `D3DContent`という名前のC++新しい Win32 プロジェクトを作成します。

     Win32 アプリケーションウィザードが開き、[ようこそ] 画面が表示されます。

2. **[次へ]** をクリックします。

     [アプリケーションの設定] 画面が表示されます。

3. **[アプリケーションの種類:]** セクションで、 **[DLL]** オプションを選択します。

4. **[完了]** をクリックします。

     D3DContent プロジェクトが生成されます。

5. ソリューションエクスプローラーで、D3DContent プロジェクトを右クリックし、 **[プロパティ]** を選択します。

     **[D3DContent プロパティページ]** ダイアログボックスが表示されます。

6. [ **C/C++**  node] を選択します。

7. **[追加のインクルードディレクトリ]** フィールドで、DirectX インクルードフォルダーの場所を指定します。 このフォルダーの既定の場所は、%ProgramFiles%\Microsoft DirectX SDK (*バージョン*) \ includeです。

8. **[リンカー]** ノードをダブルクリックして展開します。

9. **[追加のライブラリディレクトリ]** フィールドで、DirectX ライブラリフォルダーの場所を指定します。 このフォルダーの既定の場所は、%ProgramFiles%\Microsoft DirectX SDK (*バージョン*) \Lib\x86. です。

10. **入力**ノードを選択します。

11. **[追加の依存関係]** フィールドに、`d3d9.lib` ファイルと `d3dx9.lib` ファイルを追加します。

12. ソリューションエクスプローラーで、`D3DContent.def` という名前の新しいモジュール定義ファイル (.def) をプロジェクトに追加します。

## <a name="creating-the-direct3d9-content"></a>Direct3D9 コンテンツの作成
 最適なパフォーマンスを得るには、Direct3D9 コンテンツで特定の設定を使用する必要があります。 次のコードは、パフォーマンス特性が最高の Direct3D9 サーフェスを作成する方法を示しています。 詳細については、「 [Direct3D9 と WPF の相互運用性のパフォーマンスに関する考慮事項](performance-considerations-for-direct3d9-and-wpf-interoperability.md)」を参照してください。

#### <a name="to-create-the-direct3d9-content"></a>Direct3D9 コンテンツを作成するには

1. ソリューションエクスプローラーを使用してC++ 、次のように3つのクラスをプロジェクトに追加します。

     `CRenderer` (仮想デストラクターを使用)

     `CRendererManager`

     `CTriangleRenderer`

2. コードエディターでレンダラーを開き、自動的に生成されたコードを次のコードに置き換えます。

     [!code-cpp[System.Windows.Interop.D3DImage#RendererH](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderer.h#rendererh)]

3. コードエディターでレンダラーを開き、自動的に生成されたコードを次のコードに置き換えます。

     [!code-cpp[System.Windows.Interop.D3DImage#RendererCPP](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderer.cpp#renderercpp)]

4. コードエディターで RendererManager .h を開き、自動的に生成されたコードを次のコードに置き換えます。

     [!code-cpp[System.Windows.Interop.D3DImage#RendererManagerH](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderermanager.h#renderermanagerh)]

5. コードエディターで RendererManager .cpp を開き、自動的に生成されたコードを次のコードに置き換えます。

     [!code-cpp[System.Windows.Interop.D3DImage#RendererManagerCPP](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderermanager.cpp#renderermanagercpp)]

6. コードエディターで TriangleRenderer を開き、自動的に生成されたコードを次のコードに置き換えます。

     [!code-cpp[System.Windows.Interop.D3DImage#TriangleRendererH](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/trianglerenderer.h#trianglerendererh)]

7. コードエディターで TriangleRenderer を開き、自動的に生成されたコードを次のコードに置き換えます。

     [!code-cpp[System.Windows.Interop.D3DImage#TriangleRendererCPP](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/trianglerenderer.cpp#trianglerenderercpp)]

8. コードエディターで stdafx.h を開き、自動的に生成されたコードを次のコードに置き換えます。

     [!code-cpp[System.Windows.Interop.D3DImage#StdafxH](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/stdafx.h#stdafxh)]

9. コードエディターで dllmain を開き、自動的に生成されたコードを次のコードに置き換えます。

     [!code-cpp[System.Windows.Interop.D3DImage#DllMain](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/dllmain.cpp#dllmain)]

10. コードエディターで D3DContent を開きます。

11. 自動的に生成されたコードを次のコードに置き換えます。

    ```cpp
    LIBRARY "D3DContent"

    EXPORTS

    SetSize
    SetAlpha
    SetNumDesiredSamples
    SetAdapter

    GetBackBufferNoRef
    Render
    Destroy
    ```

12. プロジェクトをビルドします。

## <a name="next-steps"></a>次の手順

- WPF アプリケーションで Direct3D9 コンテンツをホストします。 詳細については、「[チュートリアル: WPF での Direct3D9 コンテンツのホスト](walkthrough-hosting-direct3d9-content-in-wpf.md)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Interop.D3DImage>
- [Direct3D9 および WPF の相互運用性のパフォーマンスに関する考慮事項](performance-considerations-for-direct3d9-and-wpf-interoperability.md)
- [チュートリアル: WPF での Direct3D9 コンテンツのホスト](walkthrough-hosting-direct3d9-content-in-wpf.md)
