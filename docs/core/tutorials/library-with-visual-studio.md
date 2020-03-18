---
title: Visual Studio で .NET Standard クラス ライブラリを構築する
description: Visual Studio を使用して C# または Visual Basic で記述された .NET Standard クラス ライブラリを構築する方法について説明します
ms.date: 12/09/2019
ms.custom: vs-dotnet
ms.openlocfilehash: 748a1499e0c3a4a41613a69b715dbcfbd585bfe3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75714017"
---
# <a name="build-a-net-standard-library-in-visual-studio"></a>Visual Studio で .NET Standard ライブラリを構築する

"*クラス ライブラリ*" は、アプリケーションから呼び出される型とメソッドを定義します。 .NET Standard 2.0 をターゲットとするクラス ライブラリでは、お使いのライブラリを、そのバージョンの .NET Standard をサポートする任意の .NET 実装によって呼び出すことができます。 クラス ライブラリを完了させたら、サードパーティ製のコンポーネントとして配布するかどうか、あるいは 1 つまたは複数のアプリケーションにバンドルされたコンポーネントとして含めるかどうかを決定できます。

> [!NOTE]
> .NET Standard のバージョンとそれらがサポートするプラットフォームの一覧は、「[.NET Standard](../../standard/net-standard.md)」を参照してください。

このトピックでは、1 つの文字列処理メソッドを含む簡単なユーティリティ ライブラリを作成します。 それを[拡張メソッド](../../csharp/programming-guide/classes-and-structs/extension-methods.md)として実装し、<xref:System.String> クラスのメンバーと同じように呼び出すことができるようにします。

## <a name="create-a-visual-studio-solution"></a>Visual Studio ソリューションを作成する

まず、クラス ライブラリ プロジェクトを配置する空のソリューションを作成します。 1 つの Visual Studio のソリューションは、1 つまたは複数のプロジェクトのコンテナーとして機能します。 このチュートリアル シリーズを続行する場合は、関連するプロジェクトを同じソリューションに追加します。

空のソリューションを作成するには:

1. Visual Studio を開きます。

2. スタート ウィンドウで、 **[新しいプロジェクトの作成]** を選択します。

3. **[新しいプロジェクトの作成]** ページで、検索ボックスに「**ソリューション**」と入力します。 **[空のソリューション]** テンプレートを選択して、 **[次へ]** を選択します。

   ![Visual Studio での空のソリューション テンプレート](media/library-with-visual-studio/blank-solution.png)

4. **[新しいプロジェクトの構成]** ページで、 **[プロジェクト名]** ボックスに「**ClassLibraryProjects**」と入力します。 次に、 **[作成]** を選択します。

> [!TIP]
> この手順をスキップして、次の手順でプロジェクトを作成するときに Visual Studio によってソリューションが自動的に作成されるようにすることもできます。 **[新しいプロジェクトの構成]** ページでソリューション オプションを探します。

## <a name="create-a-class-library-project"></a>クラス ライブラリ プロジェクトを作成する

<!-- markdownlint-disable MD025 -->

# <a name="c"></a>[C#](#tab/csharp)

1. "StringLibrary" という名前の新しい C# .NET Standard クラス ライブラリ プロジェクトをソリューションに追加します。

   1. **ソリューション エクスプローラー**で、ソリューションを右クリックし、 **[追加]**  >  **[新しいプロジェクト]** の順に選択します。

   1. **[新しいプロジェクトの追加]** ページで、検索ボックスに「**ライブラリ**」と入力します。 言語の一覧から **[C#]** を選択し、プラットフォームの一覧から **[すべてのプラットフォーム]** を選択します。 **[クラス ライブラリ (.NET Standard)]** テンプレートを選択し、 **[次へ]** を選択します。

   1. **[新しいプロジェクトの構成]** ページで、 **[プロジェクト名]** ボックスに「**StringLibrary**」と入力します。 次に、 **[作成]** を選択します。

1. ライブラリが正しいバージョンの .NET Standard をターゲットにしていることを確認します。 **ソリューション エクスプローラー** でライブラリ プロジェクトを右クリックし、 **[プロパティ]** を選択します。 **[ターゲット フレームワーク]** テキスト ボックスに、.NET Standard 2.0 がプロジェクトのターゲットになっていることが示されています。

   ![クラス ライブラリのプロジェクト プロパティ](./media/library-with-visual-studio/library-project-properties.png)

1. コード ウィンドウ内のコードを次のコードに置き換えて、ファイルを保存します。

   [!CODE-csharp[ClassLib#1](../../../samples/snippets/csharp/getting_started/with_visual_studio_2017/classlib.cs)]

   クラス ライブラリ `UtilityLibraries.StringLibrary` には、`StartsWithUpper` という名前のメソッドが含まれています。 このメソッドによって、現在の文字列のインスタンスが大文字で始まるかどうかを示す <xref:System.Boolean> 値が返されます。 Unicode 規格では、大文字と小文字が区別されます。 <xref:System.Char.IsUpper(System.Char)?displayProperty=nameWithType> メソッドは文字が大文字の場合に `true` を返します。

1. メニュー バーで **[ビルド]**  >  **[ソリューションのビルド]** の順に選択します。

# <a name="visual-basic"></a>[Visual Basic](#tab/vb)

1. "StringLibrary" という名前の新しい Visual Basic .NET Standard クラス ライブラリ プロジェクトをソリューションに追加します。

   1. **ソリューション エクスプローラー**で、ソリューションを右クリックし、 **[追加]**  >  **[新しいプロジェクト]** の順に選択します。

   1. **[新しいプロジェクトの追加]** ページで、検索ボックスに「**ライブラリ**」と入力します。 言語の一覧から **[Visual Basic]** を選択し、次に、プラットフォームの一覧から **[すべてのプラットフォーム]** を選択します。 **[クラス ライブラリ (.NET Standard)]** テンプレートを選択し、 **[次へ]** を選択します。

   1. **[新しいプロジェクトの構成]** ページで、 **[プロジェクト名]** ボックスに「**StringLibrary**」と入力します。 次に、 **[作成]** を選択します。

1. ライブラリが正しいバージョンの .NET Standard をターゲットにしていることを確認します。 **ソリューション エクスプローラー** でライブラリ プロジェクトを右クリックし、 **[プロパティ]** を選択します。 **[ターゲット フレームワーク]** テキスト ボックスに、.NET Standard 2.0 がプロジェクトのターゲットになっていることが示されています。

   ![クラス ライブラリのプロジェクト プロパティ](./media/library-with-visual-studio/vb/library-project-properties.png)

1. **[プロパティ]** ダイアログで、 **[ルート名前空間]** テキスト ボックス内のテキストをクリアします。 プロジェクトごとに、そのプロジェクト名に対応する名前空間が Visual Basic によって自動的に作成されます。 このチュートリアルでは、コード ファイルで [`namespace`](../../visual-basic/language-reference/statements/namespace-statement.md) キーワードを使用して、最上位の名前空間を定義します。

1. コード ウィンドウ内のコードを次のコードに置き換えて、ファイルを保存します。

   [!CODE-vb[ClassLib#1](../../../samples/snippets/core/tutorials/vb-library-with-visual-studio/stringlibrary.vb)]

   クラス ライブラリ `UtilityLibraries.StringLibrary` には、`StartsWithUpper` という名前のメソッドが含まれています。 このメソッドによって、現在の文字列のインスタンスが大文字で始まるかどうかを示す <xref:System.Boolean> 値が返されます。 Unicode 規格では、大文字と小文字が区別されます。 <xref:System.Char.IsUpper(System.Char)?displayProperty=nameWithType> メソッドは文字が大文字の場合に `true` を返します。

1. メニュー バーで **[ビルド]**  >  **[ソリューションのビルド]** の順に選択します。

---

   プロジェクトはエラーなしでコンパイルされます。

## <a name="next-steps"></a>次のステップ

ライブラリを正常に作成できました。 そのメソッドの呼び出しをまだ行っていないので、期待どおりに動作するかわかりません。 ライブラリの開発における次のステップでは、それをテストします。

> [!div class="nextstepaction"]
> [単体テスト プロジェクトを作成する](testing-library-with-visual-studio.md)
