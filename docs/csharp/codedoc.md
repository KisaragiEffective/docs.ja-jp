---
title: XML コメントによるコードの文書化
description: XML ドキュメント コメントを含むコードを文書化し、コンパイル時に XML ドキュメント ファイルを生成する方法を説明します。
ms.date: 01/21/2020
ms.technology: csharp-fundamentals
ms.assetid: 8e75e317-4a55-45f2-a866-e76124171838
ms.openlocfilehash: 1ed39c4733c36b3932fcb85bf50d4f4c0e53aa6f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146318"
---
# <a name="document-your-code-with-xml-comments"></a>XML コメントを含むコードの文書化

XML 文書化コメントは、ユーザー定義型またはユーザー定義メンバーの定義の上に追加する特殊なコメントです。
このコメントが特殊な理由は、コンパイル時にコンパイラで処理して、XML 文書化ファイルを生成できることです。
コンパイラによって生成された XML ファイルは、.NET アセンブリと共に配布できます。これにより、Visual Studio や他の IDE で、IntelliSense を使用して型やメンバーに関する概要情報を表示できます。 さらに、[DocFX](https://dotnet.github.io/docfx/) や [Sandcastle](https://github.com/EWSoftware/SHFB) のようなツールを使用して XML ファイルを実行し、API リファレンスの Web サイトを生成することができます。

XML 文書化コメントは、その他すべてのコメントと同じように、コンパイラによって無視されます。

コンパイル時に XML ファイルを生成するには、次のいずれかを実行します。

- .NET Core を使用してコマンド ラインからアプリケーションを開発している場合は、.csproj プロジェクト ファイルの `<PropertyGroup>` セクションに `GenerateDocumentationFile` 要素を追加できます。 また、[`DocumentationFile` 要素](/visualstudio/msbuild/common-msbuild-project-properties)を使用して、ドキュメント ファイルへのパスを直接指定することもできます。 次の例では、プロジェクト ディレクトリの中に、アセンブリと同じルート ファイル名で XML ファイルが生成されます。

   ```xml
   <GenerateDocumentationFile>true</GenerateDocumentationFile>
   ```

   これは、次の指定と同じです。

   ```xml
   <DocumentationFile>bin\$(Configuration)\$(TargetFramework)\$(AssemblyName).xml</DocumentationFile>
   ```

- Visual Studio を使用してアプリケーションを開発する場合は、プロジェクトを右クリックして、 **[プロパティ]** を選択します。 プロパティ ダイアログ ボックスで、 **[ビルド]** タブをクリックし、 **[XML ドキュメント ファイル]** をオンにします。 コンパイラがファイルを書き込む場所を変更することもできます。

- コマンド ラインから .NET Framework アプリケーションをコンパイルする場合は、コンパイル時に [-doc コンパイラ オプション](language-reference/compiler-options/doc-compiler-option.md)を追加してください。  

XML 文書化コメントには、3 つのスラッシュ (`///`) と、XML 形式のコメント本文を使用します。 例:

[!code-csharp[XML Documentation Comment](../../samples/snippets/csharp/concepts/codedoc/xml-comment.cs)]

## <a name="walkthrough"></a>チュートリアル

基本的な数式ライブラリを文書化して、新しい開発者が理解/参加しやすく、サードパーティの開発者が使用しやすいものにする方法を確認しましょう。

シンプルな数式ライブラリのコードを次に示します。

[!code-csharp[Sample Library](../../samples/snippets/csharp/concepts/codedoc/sample-library.cs)]

サンプル ライブラリでは、4 つの主要な算術演算 (`add`、`subtract`、`multiply`、`divide`) が、`int` と `double` のデータ型でサポートされています。

このライブラリを使用するがそのソース コードにはアクセスできないサード パーティの開発者向けに、コードから API リファレンス ドキュメントを作成できるようにする必要があります。
前述のように、このために XML ドキュメント タグを使用できます。 そこで、C# コンパイラがサポートする標準の XML タグを紹介します。

## <a name="summary"></a>\<summary>

`<summary>` タグは、型またはメンバーに関する簡単な情報を追加します。
この例では、タグの使い方を示すために、このタグを `Math` クラス定義と最初の `Add` メソッドに追加しています。 コードのそれ以外の部分にも適用してかまいません。

[!code-csharp[Summary Tag](~/samples/snippets/csharp/concepts/codedoc/summary-tag.cs)]

`<summary>` は重要なタグです。IntelliSense や API のリファレンス ドキュメントでは、このタグの内容が型やメンバーに関する主要な情報源であるため、このタグを含めることをお勧めします。

## <a name="remarks"></a>\<remarks>

`<remarks>` タグは、`<summary>` タグで提供される型やメンバーに関する情報を補足します。 この例では、このタグをクラスに追加します。

[!code-csharp[Remarks Tag](~/samples/snippets/csharp/concepts/codedoc/remarks-tag.cs)]

## <a name="returns"></a>\<returns>

`<returns>` タグは、メソッド宣言の戻り値を記述します。
前と同様に、次の例は最初の `Add` メソッドに追加した `<returns>` タグを示しています。 その他のメソッドにも同様に追加できます。

[!code-csharp[Returns Tag](~/samples/snippets/csharp/concepts/codedoc/returns-tag.cs)]

## <a name="value"></a>\<value>

`<value>` タグは、プロパティに対して使用する点を除いて、`<returns>` タグと同じです。
`Math` ライブラリに `PI` という静的プロパティがある場合、このタグを次のように使用します。

[!code-csharp[Value Tag](~/samples/snippets/csharp/concepts/codedoc/value-tag.cs)]

## <a name="example"></a>\<example>

`<example>` タグを使用して XML 文書化情報に例を挿入します。
そのために、子 `<code>` タグを使用します。

[!code-csharp[Example Tag](~/samples/snippets/csharp/concepts/codedoc/example-tag.cs)]

`code` タグは、長い例で改行とインデント設定を維持します。

## <a name="para"></a>\<para>

`<para>` タグは、親タグ内の内容を書式設定するために使用します。 通常、`<para>` は `<remarks>` や `<returns>` などのタグの内側で使用して、テキストを段落に分割します。
クラス定義で `<remarks>` タグの内容を書式設定できます。

[!code-csharp[Para Tag](~/samples/snippets/csharp/concepts/codedoc/para-tag.cs)]

## <a name="c"></a>\<c>

これも書式設定のタグです。`<c>` タグは、テキストの一部をコードとしてマークするために使用します。
`<code>` タグに似ていますが、インラインで記述する点が異なります。 タグの内容の一部として簡単なコード例を示すときに便利です。
`Math` クラスのドキュメントを更新してみましょう。

[!code-csharp[C Tag](~/samples/snippets/csharp/concepts/codedoc/c-tag.cs)]

## <a name="exception"></a>\<exception>

`<exception>` タグを使用すると、メソッドで特定の例外がスローされる可能性を開発者に知らせることができます。
`Math` ライブラリを見てみると、特定の条件が満たされた場合、両方の `Add` メソッドで例外がスローされることがわかります。 一方、少しわかりにくいですが、`b`パラメーターが 0 の場合は整数の `Divide` メソッドでも例外がスローされます。 ここで、このメソッドに例外のドキュメントを追加します。

[!code-csharp[Exception Tag](~/samples/snippets/csharp/concepts/codedoc/exception-tag.cs)]

`cref` 属性は、現在のコンパイル環境から使用できる例外の参照を表します。
プロジェクトまたは参照されたアセンブリに定義されている任意の型を指定できます。 コンパイラはその値を解決できない場合、警告を出します。

## <a name="see"></a>\<see>

`<see>` タグでは、別のコード要素のドキュメント ページへのクリック可能なリンクを作成できます。 次の例では、2 つの `Add` メソッドの間にクリック可能なリンクを作成します。

[!code-csharp[See Tag](~/samples/snippets/csharp/concepts/codedoc/see-tag.cs)]

`cref` は**必須**属性です。現在のコンパイル環境から使用できる型またはその型のメンバーへの参照を表します。
プロジェクトまたは参照されたアセンブリに定義されている任意の型を指定できます。

## <a name="seealso"></a>\<seealso>

`<seealso>` タグは、`<see>` タグと同じように使用します。 唯一の違いは、このタグの内容が一般的に「関連項目」セクションに配置されることです。 ここで、`seealso` タグを整数の `Add` メソッドに追加して、クラス内の、整数パラメーターを受け取る他のメソッドを参照します。

[!code-csharp[Seealso Tag](~/samples/snippets/csharp/concepts/codedoc/seealso-tag.cs)]

`cref` 属性は、現在のコンパイル環境から使用できる型またはその型のメンバーへの参照を表します。
プロジェクトまたは参照されたアセンブリに定義されている任意の型を指定できます。

## <a name="param"></a>\<param>

`<param>` タグは、メソッドのパラメーターを記述するために使用します。 double 型の `Add` メソッドでの例を示します。タグで記述するパラメーターは**必須** `name` 属性です。

[!code-csharp[Param Tag](~/samples/snippets/csharp/concepts/codedoc/param-tag.cs)]

## <a name="typeparam"></a>\<typeparam>

`<typeparam>` タグは、`<param>` タグと同じように使用しますが、ジェネリック型またはメソッド宣言で、ジェネリック パラメーターを記述するために使用する点が異なります。
簡単なジェネリック メソッドを `Math` クラスに追加して、ある数量が別の数量より大きいかどうかを確認します。

[!code-csharp[Typeparam Tag](~/samples/snippets/csharp/concepts/codedoc/typeparam-tag.cs)]

## <a name="paramref"></a>\<paramref>

場合によって、`<summary>` タグでメソッドの動作を記述している最中に、パラメーターを参照することが必要になることがあります。 そのような場合は、`<paramref>` タグがまさに適しています。 double 型に基づく `Add` メソッドの概要を更新しましょう。 `<param>` タグと同様に、パラメーター名は**必須** `name` 属性で指定されます。

[!code-csharp[Paramref Tag](~/samples/snippets/csharp/concepts/codedoc/paramref-tag.cs)]

## <a name="typeparamref"></a>\<typeparamref>

`<typeparamref>` タグは、`<paramref>` タグと同じように使用しますが、ジェネリック型またはメソッド宣言で、ジェネリック パラメーターを記述するために使用する点が異なります。
以前に作成した同じジェネリック メソッドを使用することができます。

[!code-csharp[Typeparamref Tag](~/samples/snippets/csharp/concepts/codedoc/typeparamref-tag.cs)]

## <a name="list"></a>\<list>

`<list>` タグを使用して、ドキュメント情報を、順序指定済みリスト、順序指定されていないリスト、または表として書式設定します。 `Math` ライブラリがサポートするそれぞれの算術演算の順不同のリストを作成します。

[!code-csharp[List Tag](~/samples/snippets/csharp/concepts/codedoc/list-tag.cs)]

`type` 属性を `number` または `table` に変更することで、順序付きリストまたは表をそれぞれ作成できます。

## <a name="inheritdoc"></a>\<inheritdoc>

`<inheritdoc>` タグを使用して、基底クラス、インターフェイス、および同様のメソッドから XML コメントを継承できます。 これにより、重複する XML コメントの不要なコピーと貼り付けを行う必要がなくなり、XML コメントが自動的に同期されたままになります。

[!code-csharp-interactive[InheritDoc Tag](~/samples/snippets/csharp/concepts/codedoc/inheritdoc-tag.cs)]

### <a name="put-it-all-together"></a>まとめ

ここまで、チュートリアルに沿ってコードに必要なタグを適用し、コードは次のようになっています。

[!code-csharp[Tagged Library](~/samples/snippets/csharp/concepts/codedoc/tagged-library.cs)]

コードから、クリック可能な相互参照を含む、詳細なドキュメント Web サイトを生成できます。 ただし、別の問題に直面します。コードが読みにくくなります。
大量の情報を処理する必要があり、このコードを利用する開発者にとって非常に厄介です。
さいわい、これに対処するのに役立つ XML タグがあります。

## <a name="include"></a>\<include>

`<include>` タグでは、文書化コメントをソース コード ファイルに直接配置するのではなく、ソース コードの型とメンバーを記述した別個の XML ファイル内のコメントを参照できます。

ここで、すべての XML タグを、`docs.xml` という別の XML ファイルに移動します。 ファイルの名前は何でもかまいません。

[!code-xml[Sample XML](~/samples/snippets/csharp/concepts/codedoc/include.xml)]

上に示した XML では、各メンバーの文書化コメントが、タグの働きを表す名前の付いたタグの内側に直接記述されています。 自分の方法を選択できます。
XML コメントを別のファイルに移動したので、`<include>` タグを使用して、コードがどのように読みやすくなるか見てみましょう。

[!code-csharp[Include Tag](~/samples/snippets/csharp/concepts/codedoc/include-tag.cs)]

このようになります。コードは読みやすい状態に戻り、しかも文書化の情報は失われていません。

`file` は、文書化の情報を含む XML ファイルの名前を表す属性です。

`path` は、指定した `file` に含まれる `tag name` への [XPath](../standard/data/xml/xpath-queries-and-namespaces.md) クエリを表す属性です。

`name` は、コメントの前に配置するタグの名前指定子を表す属性です。

`id` 属性は、`name` の代わりに使用でき、コメントの前にあるタグの ID を表します。

### <a name="user-defined-tags"></a>ユーザー定義タグ

上に示したすべてのタグは、C# コンパイラで認識されるタグを表します。 ただし、ユーザー独自のタグも自由に定義できます。
Sandcastle などのツールを使用すると、[\<event>](https://ewsoftware.github.io/XMLCommentsGuide/html/81bf7ad3-45dc-452f-90d5-87ce2494a182.htm)、[\<note>](https://ewsoftware.github.io/XMLCommentsGuide/html/4302a60f-e4f4-4b8d-a451-5f453c4ebd46.htm) などの追加のタグや、[名前空間の文書化](https://ewsoftware.github.io/XMLCommentsGuide/html/BD91FAD4-188D-4697-A654-7C07FD47EF31.htm)もサポートされます。
カスタムまたは社内ドキュメント生成ツールを標準タグと共に使用して、HTML から PDF への複数の出力形式をサポートできます。

## <a name="recommendations"></a>推奨事項

コードを文書化することをお勧めするのには、さまざまな理由があります。 いくつかのベスト プラクティス、一般的なユース ケースのシナリオ、XML 文書化タグを C# コードで使用するときに知っておく必要があることを以下に示します。

- 整合性を保つため、一般に公開されているすべての型とそのメンバーを文書化する必要があります。 必要な文書化はすべて実行してください。
- プライベート メンバーも XML コメントを使用して文書化できます。 ただし、これによりライブラリの内部 (機密の可能性がある) の動作が公開されます。
- 型とそのメンバーには、少なくとも `<summary>` タグが必要です。そのタグの内容が IntelliSense で必要なためです。
- 文書化のテキストは、句点で終わる完全な文を使用して作成する必要があります。
- 部分クラスは完全にサポートされ、文書化の情報は、その型の 1 つのエントリに連結されます。
- コンパイラは、`<exception>`、`<include>`、`<param>`、`<see>`、`<seealso>`、`<typeparam>` の各タグの構文を確認します。
- コンパイラは、ファイルのパスおよびコードの他の部分への参照を含むパラメーターを検証します。

## <a name="see-also"></a>関連項目

- [XML ドキュメント コメント (C# プログラミング ガイド)](programming-guide/xmldoc/index.md)
- [ドキュメント コメント用の推奨タグ (C# プログラミング ガイド)](programming-guide/xmldoc/recommended-tags-for-documentation-comments.md)
