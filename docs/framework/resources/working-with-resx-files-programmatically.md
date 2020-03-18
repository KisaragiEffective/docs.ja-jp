---
title: プログラムによる .resx ファイルの使用
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- resource files, .resx files
- .resx files
ms.assetid: 168f941a-2b84-43f8-933f-cf4a8548d824
ms.openlocfilehash: 2bbca5712639e14370d090e95b78bb89eba134e4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73129906"
---
# <a name="working-with-resx-files-programmatically"></a>プログラムによる .resx ファイルの使用
XML リソース (.resx) ファイルは適切に定義された XML で構成する必要があり、特定のスキーマに従ったヘッダーの後に、名前と値のペアになったデータが続きます。そのため、手動で作成するとエラーが発生しやすくなります。 代わりに、.NET クラス ライブラリの型とメンバーを使用して、.resx ファイルをプログラムで作成できます。 また、.NET クラス ライブラリを使用して、.resx ファイルに格納されているリソースを取得することもできます。 このトピックでは、 <xref:System.Resources> 名前空間の型とメンバーを使って、.resx ファイルを操作する方法を説明します。

 なお、この記事では、リソースを含む XML (.resx) ファイルの操作について説明します。 アセンブリに埋め込まれたバイナリ リソース ファイルの操作について詳しくは、 <xref:System.Resources.ResourceManager> トピックをご覧ください。

> [!WARNING]
> プログラムでの操作以外にも、.resx ファイルを操作する方法はあります。 リソース ファイルを [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) プロジェクトに追加するときは、Visual Studio で .resx ファイルを作成して維持するためのインターフェイスが提供され、コンパイル時に .resx ファイルは .resources ファイルに自動的に変換されます。 .resx ファイルを直接操作するためにテキスト エディターを使うこともできます。 ただし、ファイルの破損を避けるため、ファイルに格納されているバイナリ情報を変更しないように注意してください。

## <a name="create-a-resx-file"></a>.resx ファイルを作成する

<xref:System.Resources.ResXResourceWriter?displayProperty=nameWithType> クラスを使い、次の手順に従って .resx ファイルをプログラムで作成できます。

1. <xref:System.Resources.ResXResourceWriter> メソッドを呼び出して、.resx ファイルの名前を指定することにより、 <xref:System.Resources.ResXResourceWriter.%23ctor%28System.String%29?displayProperty=nameWithType> オブジェクトをインスタンス化します。 ファイル名には、.resx 拡張子を含める必要があります。 <xref:System.Resources.ResXResourceWriter> ブロック内の `using` オブジェクトをインスタンス化する場合、手順 3 で <xref:System.Resources.ResXResourceWriter.Close%2A?displayProperty=nameWithType> メソッドを明示的に呼び出す必要はありません。

2. ファイルに追加するリソースごとに <xref:System.Resources.ResXResourceWriter.AddResource%2A?displayProperty=nameWithType> メソッドを呼び出します。 このメソッドのオーバーロードを使って、文字列、オブジェクト、バイナリ (バイト配列) のデータを追加します。 リソースがオブジェクトの場合は、シリアル化可能でなければなりません。

3. <xref:System.Resources.ResXResourceWriter.Close%2A?displayProperty=nameWithType> メソッドを呼び出して、リソース ファイルを生成し、すべてのリソースを解放します。 <xref:System.Resources.ResXResourceWriter> オブジェクトが `using` ブロック内で作成された場合、リソースは .resx ファイルに書き込まれ、 <xref:System.Resources.ResXResourceWriter> オブジェクトが使うリソースは `using` ブロックの最後に解放されます。

結果として得られる .resx ファイルには、適切なヘッダーと `data` メソッドによって追加された各リソースの <xref:System.Resources.ResXResourceWriter.AddResource%2A?displayProperty=nameWithType> タグが含まれます。

> [!WARNING]
> パスワード、セキュリティの配慮が必要な情報、プライベートなデータなどの格納には、リソース ファイルを使用しないでください。

次の例では、6 つの文字列、1 つのアイコン、2 つのアプリケーション定義オブジェクト (2 つの `Automobile` オブジェクト) を格納する CarResources.resx という名前の .resx ファイルを作成します。 例で定義されてインスタンス化された `Automobile` クラスは、<xref:System.SerializableAttribute> 属性でタグ付けされることに注意してください。

[!code-csharp[Conceptual.Resources.ResX#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resx/cs/create1.cs#1)]
[!code-vb[Conceptual.Resources.ResX#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resx/vb/create1.vb#1)]

> [!TIP]
> [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) を使用して .resx ファイルを作成することもできます。 コンパイル時に、Visual Studio は [リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) を使って、.resx ファイルをバイナリ リソース (.resources) ファイルに変換し、アプリケーション アセンブリかサテライト アセンブリのいずれかに埋め込みます。

.resx ファイルをランタイムの実行可能ファイルに埋め込むことや、サテライト アセンブリにコンパイルすることはできません。 [リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md)を使って、.resx ファイルをバイナリ リソース (.resources) ファイルに変換する必要があります。 結果として得られる .resources ファイルは、アプリケーション アセンブリやサテライト アセンブリに埋め込むことができます。 詳細については、「 [Creating Resource Files](creating-resource-files-for-desktop-apps.md)」を参照してください。

## <a name="enumerate-resources"></a>リソースを列挙する
 場合によっては、.resx ファイルから、特定のリソースではなく、すべてのリソースを取得したいことがあります。 これを行うには、.resx ファイル内のすべてのリソースの列挙子を提供する <xref:System.Resources.ResXResourceReader?displayProperty=nameWithType> クラスを使います。 <xref:System.Resources.ResXResourceReader?displayProperty=nameWithType> クラスは <xref:System.Collections.IDictionaryEnumerator>を実装します。これは、ループの反復処理ごとに特定のリソースを示す <xref:System.Collections.DictionaryEntry> を返します。 その <xref:System.Collections.DictionaryEntry.Key%2A?displayProperty=nameWithType> プロパティはリソースのキーを返し、その <xref:System.Collections.DictionaryEntry.Value%2A?displayProperty=nameWithType> プロパティはリソースの値を返します。

 次の例では、前の例で作成した CarResources.resx ファイルの <xref:System.Resources.ResXResourceReader> オブジェクトを作成して、リソース ファイルを反復処理します。 リソース ファイルに定義された 2 つの `Automobile` オブジェクトを <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> オブジェクトに追加し、6 個中 5 個の文字列を <xref:System.Collections.SortedList> オブジェクトに追加します。 <xref:System.Collections.SortedList> オブジェクト内の値は、コンソールに列見出しを表示するために使われるパラメーター配列に変換されます。 `Automobile` プロパティ値もコンソールに表示されます。

 [!code-csharp[Conceptual.Resources.ResX#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resx/cs/enumerate1.cs#2)]
 [!code-vb[Conceptual.Resources.ResX#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resx/vb/enumerate1.vb#2)]

## <a name="retrieve-a-specific-resource"></a>特定のリソースを取得する
 .resx ファイル内の項目を列挙することに加えて、 <xref:System.Resources.ResXResourceSet?displayProperty=nameWithType> クラスを使って特定のリソースを名前によって取得できます。 <xref:System.Resources.ResourceSet.GetString%28System.String%29?displayProperty=nameWithType> メソッドは、名前付きの文字列リソースの値を取得します。 <xref:System.Resources.ResourceSet.GetObject%28System.String%29?displayProperty=nameWithType> メソッドは、名前付きオブジェクトの値やバイナリ データを取得します。 メソッドはオブジェクトを返します。そのオブジェクトはその後適切な型のオブジェクトにキャスト (C#) するか、変換 (Visual Basic) する必要があります。

 次の例では、そのリソース名を使って、フォームのキャプション文字列とアイコンを取得します。 また、前の例で使ったアプリケーション定義の `Automobile` オブジェクトを取得して、<xref:System.Windows.Forms.DataGridView> コントロールに表示します。

 [!code-csharp[Conceptual.Resources.ResX#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resx/cs/retrieve1.cs#3)]
 [!code-vb[Conceptual.Resources.ResX#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resx/vb/retrieve1.vb#3)]

## <a name="convert-resx-files-to-binary-resources-files"></a>.resx ファイルをバイナリの .resources ファイルに変換する
 .resx ファイルを埋め込みバイナリ リソース (.resources) ファイルに変換することには、大きな利点があります。 .resx ファイルは、アプリケーション開発中の読み取りや保守を容易に行うことができますが、完成したアプリケーションに含まれることはほとんどありません。 .resx ファイルがアプリケーションと共に配布される場合、アプリケーションの実行可能ファイルとそれに付随するライブラリとは異なる独立したファイルとして存在します。 これに対し、.resources ファイルは、アプリケーションの実行可能ファイルやそれに付随するアセンブリに埋め込まれます。 また、ローカライズされたアプリケーションの場合、実行時に .resx ファイルに依存すると、開発者がリソース フォールバックを処理することになります。 これに対し、埋め込みの .resources ファイルを含むサテライト アセンブリのセットが作成された場合、共通言語ランタイムがリソース フォールバック プロセスを処理します。

 .resx ファイルを .resources ファイルに変換するには、 [リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md)を使います。リソース ファイル ジェネレーターの基本的な構文は次のとおりです。

 **Resgen.exe** *.resxFilename*

 結果は、.resx ファイルと同じルート ファイル名と .resources ファイル拡張子を持つバイナリ リソース ファイルになります。 その後、コンパイル時に、このファイルを実行可能ファイルやライブラリにコンパイルできます。 Visual Basic コンパイラを使っている場合、次の構文を使って、.resources ファイルをアプリケーションの実行可能ファイルに埋め込みます。

 **vbc** *filename* **.vb /resource:** *.resourcesFilename*

 C# を使っている場合、構文は次のとおりです。

 **csc** *filename* **.cs /resource:** *.resourcesFilename*

 [アセンブリ リンカー (AL.exe)](../tools/al-exe-assembly-linker.md)を使って、.resources ファイルをサテライト アセンブリに埋め込むこともできます。アセンブリ リンカーの基本的な構文は次のとおりです。

 **al** *resourcesFilename* **/out:** *assemblyFilename*

## <a name="see-also"></a>関連項目

- [リソース ファイルの作成](creating-resource-files-for-desktop-apps.md)
- [Resgen.exe (リソース ファイル ジェネレーター)](../tools/resgen-exe-resource-file-generator.md)
- [Al.exe (アセンブリ リンカー)](../tools/al-exe-assembly-linker.md)
