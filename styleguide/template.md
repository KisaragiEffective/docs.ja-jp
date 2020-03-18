---
title:
- 記事のタイトル
description: ''
author:
- GITHUB USERNAME
ms.author:
- MICROSOFT ALIAS OF INTERNAL OWNER
ms.date:
- CREATION/UPDATE DATE - MM/dd/yyyy
ms.topic:
- TOPIC TYPE
ms.prod:
- PRODUCT VALUE
helpviewer_keywords:
- OFFLINE BOOK INDEX ENTRIES
ms.openlocfilehash: ed9fd55fd84606d2083e0576581391331769a1e6
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74089277"
---
# <a name="metadata-and-markdown-template"></a>メタデータとマークダウン テンプレート

この dotnet/ドキュメント テンプレートには、メタデータの設定に関するガイドラインだけでなく、マークダウン構文の例が含まれています。 これを最大限に活用するには、[生のマークダウン](https://raw.githubusercontent.com/dotnet/docs/master/styleguide/template.md)と[表示ビュー](https://github.com/dotnet/docs/blob/master/styleguide/template.md)の両方を表示する必要があります。

マークダウン ファイルを作成する場合、このテンプレートを新しいファイルにコピーして、下記のようにメタデータを記入し、上の H1 見出しを記事のタイトルに設定して、コンテンツを削除します。

## <a name="metadata"></a>メタデータ

完全なメタデータ ブロックは上部 ([生のマークダウン](https://raw.githubusercontent.com/dotnet/docs/master/styleguide/template.md)の) にあり、必須フィールドとオプション フィールドに分割されています。 次に注意事項をいくつか示します。

- コロン (:) とメタデータ要素の値の間にはスペースが**必要です**。
- 省略可能なメタデータ要素に値がない場合は、# を使ってその要素をコメント アウトするか、削除します (空白のままにしたり、"na" を使用したりしないでください)。 コメント アウトされた要素に値を追加する場合は、# を必ず削除してください。
- 値 (タイトルなど) にコロンが含まれていると、メタデータ パーサーが中断します。 この場合は、タイトルを二重引用符で囲みます (例: `title: "Writing .NET Core console apps: An advanced step-by-step guide"`)。
- **title**:検索エンジンの結果に表示されます。 タイトルは H1 見出しのタイトルと同一でなくてもかまいません。60 文字以下にする必要があります。
- **説明**:記事の内容を要約します。 これは通常、検索結果ページに表示されますが、検索ランキングには使用されません。 長さは、スペースを含め 115 から 145 文字にする必要があります。
- **author** および **ms.author**:"author" フィールドには、作成者のエイリアスではなく、**GitHub ユーザー名**を含める必要があります。  一方で、**ms.author** フィールドには、Microsoft エイリアスが記載され、記事の管理者が示されている必要があります。
- **ms.topic**:トピックの種類です。 最も一般的な値は `conceptual` であり、グローバル レベルに設定されています。 使用されるその他の一般的な値は、`tutorial`、`overview`、`reference` です。
- **dev_langs** では、トピックに表示される言語フィルターが定義されます。 「[サポートされる言語](#supported-languages)」セクションに、サポートされる値のリストが表示されます。 トピックの中に複数のプログラミング言語がある場合にのみ設定が必要です。 通常、コンテンツ内にあるこの値については、`csharp`、`vb`、`fsharp`、`cpp` のみを使用します。
- **ms.prod**:BI の目的に使用される製品 ID です。 これらは通常グローバル レベルに設定されているため、各記事のメタデータ ブロックには、通常は表示されません。
- **ms.technology**:追加の BI 分類です。 サポートされる値の一部として、C# トピックの場合は `devlang-csharp`、F# トピックの場合は `devlang-fsharp`、VB トピックの場合は `devlang-visual-basic` があります。 その他のガイドについては、値が異なるため、チームのメンバーに助言を求めてください。
- **ms.date**:MM/DD/YYYY 形式の日付です。 公開済みページに表示される日付です。前回、記事が大幅に編集されたか、"最新" であることが保証された (すなわち、記事がレビュー済みで、最新であるとみなされた) 日付を示します。
- **helpviewer_keywords**:エントリが、オフライン ブック インデックスに使用されます (Visual Studio の機能)。
- **f1_keywords**:記事を F1 キーに接続します (Visual Studio の機能)。

## <a name="basic-markdown-gfm-and-special-characters"></a>基本マークダウン、GFM、特殊文字

すべての基本マークダウンと GitHub Flavored Markdown (GFM) がサポートされています。 それらの詳細については、次を参照してください。

- [Baseline Markdown syntax (ベースライン マークダウン構文)](https://daringfireball.net/projects/markdown/syntax)
- [GFM documentation (GFM ドキュメント)](https://guides.github.com/features/mastering-markdown)

マークダウンでは書式設定に \*、\`、\# などの特殊文字を使用します。 これらの文字のいずれかをコンテンツに含める場合は、次の 2 つのどちらかを行う必要があります。

- 特殊文字の前にバックスラッシュを入力して、"エスケープ" する (たとえば、\* の場合は `\*`)
- 文字の [HTML エンティティ コード](https://www.ascii.cl/htmlcodes.htm)を使用する (たとえば、&#42 の場合は `&#42;`)。

## <a name="file-name"></a>ファイル名

ファイル名には次の規則を使用します。

- 小文字、数字、ハイフンのみを使用する。
- スペースや句読点を使用しない。 ハイフンを使用して、ファイル名の単語と数字を区切る。
- develop (開発)、buy (購入)、build (ビルド)、troubleshoot (トラブルシューティング) など、具体的な動作動詞を使用する。 -ing 形の語は使用しません。
- 小さな単語を使用しない。a、and、the、in、or などは含めません。
- マークダウンで記述し、.md ファイル拡張子を使用する必要がある。
- ファイル名を極力短くする。 記事の URL の一部となるためです。

## <a name="headings"></a>見出し

文スタイルで大文字化します。 見出しの最初の単語の最初の文字、固有名詞、コロンの後の最初の文字は大文字にします (たとえば、"Tutorial: Predict prices using regression with ML.NET")。

"How to" の後にはコロンを追加しないでください (たとえば、"How to sort an array" とし、"How to: Sort an array" のようにします)。

見出しは atx スタイルを使用して作成する必要があります。つまり、見出しを示すために行の先頭に 1 から 6 文字のハッシュ文字 (#) を使用します。これは、HTML 見出しレベルの H1 〜 H6 に対応します。 レベル 1 とレベル 2 のヘッダーの例が上で使用されています。

トピックのレベル 1 の見出し (H1) は 1 つだけにする**必要があります**。これがページ上のタイトルとして表示されます。

見出しが `#` 文字で終わっている場合は、タイトルが正しく表示されるようにエスケープする必要があります。 たとえば、`# Async programming in F\#` のようにします。

レベル 2 の見出しは、ページ上タイトルの下にある "この記事内" セクションに表示されるページ上の目次を生成します。

### <a name="third-level-heading"></a>レベル 3 の見出し
#### <a name="fourth-level-heading"></a>レベル 4 の見出し
##### <a name="fifth-level-heading"></a>レベル 5 の見出し
###### <a name="sixth-level-heading"></a>レベル 6 の見出し

## <a name="text-styling"></a>テキストのスタイル指定

"*斜体*" ファイル、フォルダー、パス (項目が長い場合は、独立した行に分割)、新しい用語に使用します。

**太字** UI 要素に使用します。

`Code` クリックできないようにするインライン コード、言語キーワード、NuGet パッケージ名、コマンドライン コマンド、データベース テーブルと列の名前、URL に使用します。

## <a name="links"></a>リンク

### <a name="internal-links"></a>内部リンク

同じマークダウン ファイル内のヘッダーにリンクする (アンカー リンクとも呼ばれます) には、リンク先のヘッダーの ID を確認する必要があります。 ID を確認するには、表示された記事のソースを表示して、ヘッダーの ID (例: `id="blockquote"`) を検索し、# に ID をつなげたものを使用してリンクします (例: `#blockquote`)。
ID はヘッダー テキストに基づいて自動生成されます。 そのため、たとえば、`## Step 2` という名前の一意のセクションの場合、ID は `id="step-2"` のようになります。

- 例: `[Declare inline blocks with a language identifier](#inline-code-blocks-with-language-identifier)` では、[言語識別子を使用して宣言インライン ブロック](#inline-code-blocks-with-language-identifier)が生成されます。

同じリポジトリ内のマークダウン ファイルにリンクするには、ファイル名の末尾に ".md" を含めた[相対リンク](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2)を使用します。

- 例: `[Readme file](../README.md)` では、[Readme ファイル](../README.md)が生成されます (リンクは、大文字と小文字が区別されます)。
- 例: `[Welcome to .NET](../docs/welcome.md)` では、「[.NET にようこそ](../docs/welcome.md)」が生成されます。

同じリポジトリ内のマークダウン ファイルのヘッダーにリンクするには、相対リンクとハッシュタグ リンクを使用します。

- 例: `[.NET Community](../docs/welcome.md#open-source)` では、[.NET コミュニティ](../docs/welcome.md#open-source)が生成されます。

ほとんどの場合は、相対リンクを使用し、リンクに `~/` を使用しないようにします。これは、相対リンクが GitHub のソース内で解決されるためです。 ただし、依存リポジトリ内のファイルにリンクする場合は、`~/` 文字を使用してパスを指定します。 依存リポジトリ内のファイルは GitHub の別の場所にあるため、リンクがどのように記述されているかを問わず、相対リンクは正しく解決されません。

C# 言語仕様と Visual Basic 言語仕様は、言語リポジトリのソースを含めると、.NET ドキュメントに含まれます。 マークダウン ソースは、[csharplang](https://github.com/dotnet/csharplang) リポジトリと [visual basic](https://github.com/dotnet/vblang) リポジトリ内で管理されます。

仕様へのリンクは、これらの仕様が含まれるソース ディレクトリを指す必要があります。 C# の場合は **~/_csharplang/spec**、VB の場合は **~/_vblang/spec** です。

- 例: `[C# Query Expressions](~/_csharplang/spec/expressions.md#query-expressions)` では、[C# クエリ式が生成されます](~/_csharplang/spec/expressions.md#query-expressions)。

### <a name="external-links"></a>外部リンク

外部ファイルにリンクするには、完全な URL をリンクとして使用します。

- 例: `[GitHub](https://www.github.com)` では、[GitHub](https://www.github.com) が生成されます。

URL がマークダウン ファイルに表示されると、クリック可能なリンクに変換されます。

- 例: `<https://www.github.com>` では、<https://www.github.com> が生成されます。

外部リンクの場合は、`https` プロトコルを優先します。 `https` をサポートしていないサイトの場合にのみ `http` リンクを使用します。

### <a name="links-to-apis"></a>API へのリンク

ビルド システムには、外部リンクを使用せずに .NET API にリンクできるようにするいくつかの拡張機能が備えられています。
API にリンクする場合は、ソース コードから自動生成される一意識別子 (UID) を使用できます。

UID は、完全修飾型とメンバー名に相当します。

UID の後に \* (または %2A) を追加した場合、リンクは特定の API ではなく、オーバーロード ページを表します。 たとえば、[List\<T>.BinarySearch(T, IComparer\<T>)](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1.binarysearch#System_Collections_Generic_List_1_BinarySearch__0_) などの特定のオーバーロードではなく、一般的な方法で [List\<T>.BinarySearch Method](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1.binarysearch) ページにリンクする場合に使用します。 また、メンバーがオーバーロードされていない場合は、\* を使ってメンバー ページにリンクすることもできます。これにより、UID にパラメーター リストを含める必要がなくなります。

特定のメソッドのオーバーロードにリンクするには、メソッドの各パラメーターの完全修飾型名を含める必要があります。 たとえば、\<xref:System.DateTime.ToString> はパラメーターなしの [DateTime.ToString](https://docs.microsoft.com/dotnet/api/system.datetime.tostring#System_DateTime_ToString) メソッドにリンクされますが、\<xref:System.DateTime.ToString(System.String,System.IFormatProvider)> は [DateTime.ToString(String,IFormatProvider)](https://docs.microsoft.com/dotnet/api/system.datetime.tostring#System_DateTime_ToString_System_String_System_IFormatProvider_) メソッドにリンクされます。 オーバーロードされた特定のメンバーの UID を <https://xref.docs.microsoft.com/autocomplete> から見つけることができます。 クエリ文字列 "?text= *\<type-member-name>* " では、UID を確認したい型またはメンバーを特定します。 たとえば、<https://xref.docs.microsoft.com/autocomplete?text=string.format> では、[String.Format](https://docs.microsoft.com/dotnet/api/system.string.format) オーバーロードが取得されます。

[System.Collections.Generic.List\<T>](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1) などのジェネリック型にリンクするには、\` (%60) 文字に続けてジェネリック型パラメーターの数字を使用します。 たとえば、\<xref:System.Nullable%601> は [System.Nullable\<T>](https://docs.microsoft.com/dotnet/api/system.nullable-1) 型にリンクされますが、\<xref:System.Func%602> は [System.Func\<T,TResult>](https://docs.microsoft.com/dotnet/api/system.func-2) デリゲートにリンクされます。

次のいずれかの構文を使用できます。

1. 自動リンク: `<xref:UID>` または `<xref:UID?displayProperty=nameWithType>`

   `displayProperty` クエリ パラメーターでは、完全修飾リンク テキストが生成されます。 既定では、リンクテキストには、メンバーまたは型の名前のみが表示されます。

2. マークダウン リンク: `[link text](xref:UID)`

   表示されるリンク テキストをカスタマイズする場合に使用します。

次に例を示します。

- `<xref:System.String>` は [String](https://docs.microsoft.com/dotnet/api/system.string) としてレンダリングされます
- `<xref:System.String?displayProperty=nameWithType>` は [System.String](https://docs.microsoft.com/dotnet/api/system.string) としてレンダリングされます
- `[String class](xref:System.String)` は [String class](https://docs.microsoft.com/dotnet/api/system.string) としてレンダリングされます

この表記の使用に関する詳細は、「[Using cross reference (相互参照の使用)](https://dotnet.github.io/docfx/tutorial/links_and_cross_references.html#using-cross-reference)」を参照してください。

UID を検索するには、2 つの方法があります。

- リンク先の API ページのソースを表示して、ms.assetid 値を検索します。 個別のオーバーロード値は、ソースには示されていないため注意してください。
- UID の検索には、次のツールを使用します: <https://xref.docs.microsoft.com/autocomplete?text=tostring> (tostring は、検索しようとしている API 名の一部に置き換えます)。 ツールにより、UID の任意の部分で、指定した `text` クエリ パラメーターが検索されます。 たとえば、メンバー名 (ToString)、メンバー名 (ToStri) の一部、型とメンバー名 (Double.ToString) などを検索できます。

UID に特殊文字 \`、\# または \* が含まれる場合、UID 値はそれぞれ `%60`、`%23`、`%2A` として HTML をエンコードする必要があります。 かっこがエンコードされていることもありますが、これは必須ではありません。

次に例を示します。

- System.Threading.Tasks.Task\`1 は `System.Threading.Tasks.Task%601` になります
- System.Exception.\#ctor は `System.Exception.%23ctor` になります
- System.Lazy\`1.\#ctor(System.Threading.LazyThreadSafetyMode) は `System.Lazy%601.%23ctor%28System.Threading.LazyThreadSafetyMode%29` になります

## <a name="lists"></a>表示内容

### <a name="ordered-lists"></a>番号付きリスト

1. This
1. Is
1. An
1. 順序あり
1. リスト

#### <a name="ordered-list-with-an-embedded-list"></a>埋め込みリストを含む番号付きリスト

1. Here
1. comes
1. 1 つ
1. embedded
    1. Miss Scarlett
    1. Professor Plum
1. ordered
1. リスト

### <a name="unordered-lists"></a>記号付きリスト

- This
- is
- a
- bulleted
- リスト

#### <a name="unordered-list-with-an-embedded-list"></a>埋め込みリストを含む記号付きリスト

- This
- bulleted
- リスト
  - Mrs. Peacock
  - Mr. Green
- 次の値を含む
- その他
  1. Colonel Mustard
  1. Mrs. White
- 一覧

## <a name="horizontal-rule"></a>水平線

---

## <a name="tables"></a>[テーブル]

| [テーブル]        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| 列 3 は      | 右揃え | $1600 |
| 列 2 は      | 中央揃え      |   $12 |
| 列 1 は既定の | 左揃え     |    $1 |

[Markdown Table Generator ツール](https://www.tablesgenerator.com/markdown_tables)を使用して、より簡単にテーブルを作成できます。

## <a name="code"></a>コード

コードを含める最善の方法は、実際に動作するサンプルからのスニペットを含める方法です。 [貢献に関するガイド](../CONTRIBUTING.md#contributing-to-samples)の指示に従って、サンプルを作成します。

次の構文を使用して、コードを含めることができます。

```markdown
[!code-<language>[<name>](<pathToFile><queryoption><queryoptionvalue>)]
```

- `-<language>` ("*省略可能*" ですが、使用が "*推奨*" されます)
  - 参照されるコード スニペットの言語。 サポートされる値のリストについては、「[サポートされる言語](#supported-languages)」をご覧ください。

- `<name>` ("*省略可能*")
  - コード スニペットの名前。 出力 HTML には影響を与えませんが、使用すると、マークダウン ソースの読みやすさを向上できます。

- `<pathToFile>` ("*必須*")
  - 参照するコード スニペット ファイルを示す、ファイルシステム内の相対パス。

- `<queryoption>` および `<queryoptionvalue>` ("*省略可能*")
  - ファイルからコードを取得する方法を指定するために一緒に使用します。
    - `#`: `#L{startlinenumber}-L{endlinenumber}` (行の範囲) "*または*" `#{tagname}` (タグ名)。
    行番号は変動するため、使用はお勧めしません。 コード スニペットの参照方法としては、タグ名が推奨されます。
    - `range`:`?range=1,3-5` 行の範囲。 この例では、行 1、3、4、5 が含まれます。
    - `dedent`:`?dedent=8` スペースの数だけ、行のインデントを解除します。この例では 8 です。 これは、ファイルの行のサブセットを選択する、`range` やその他のクエリ オプションと組み合わせることができます。
    - `outdent`:`?outdent=8` スペースの数だけ、行のインデントを戻します。この例では 8 です。 これは、ファイルの行のサブセットを選択する、`range` やその他のクエリ オプションと組み合わせることができます。

可能な限り、タグ名オプションを使用することをお勧めします。 タグ名は、領域の名前、またはソース コードに存在する `Snippettagname` 形式のコード コメントの名前です。 次の例は、タグ名 `1` を参照する方法を示します。

```markdown
[!code-csharp[csrefKeyword#1](../../../../samples/snippets/csharp/language-reference/keywords/throw/throw-1.cs#1)]
```

また、スニペット タグが[ここのソース ファイル](https://github.com/dotnet/samples/blob/master/snippets/csharp/language-reference/keywords/throw/throw-1.cs)でどのように構成されているかを確認できます。 コード スニペット ソース ファイルでの言語別のタグ名の表記の詳細については、[DocFX ガイドライン](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#tag-name-representation-in-code-snippet-source-file)に関するページを参照してください。

完全なプログラムからのスニペットを含めることで、すべてのコードが確実に継続的インテグレーション (CI) システムで処理されたものになります。 ただし、コンパイル時のエラーやランタイム エラーの原因を表示する必要がある場合は、インライン コード ブロックを使用できます。

### <a name="inline-code-blocks-with-language-identifier"></a>言語識別子を持つインライン コード ブロック

3 つのバッククォート (\`\`\`) と言語 ID を使用して、言語固有のカラー コーディングをコード ブロックに適用します。 次のサポートされる言語のリストは、各言語 ID に対するマークダウン ラベルを示しています。

#### <a name="supported-languages"></a>サポートされる言語

|name|マークダウン ラベル|
|-----|-------|
|.NET コンソール|dotnetcli|
|ASP.NET (C#)|aspx-csharp|
|ASP.NET (VB)|aspx-vb|
|Azure CLI|azurecli|
|AzCopy|azcopy|
|Azure PowerShell|azurepowershell|
|Bash|Bash|
|C++|cpp|
|C++/CX|cppcx|
|C++/WinRT|cppwinrt|
|C#|csharp|
|C# (ブラウザー内)|csharp-interactive|
|コンソール|コンソール|
|CSHTML|cshtml|
|DAX|dax|
|Dockerfile|dockerfile|
|F#|fsharp|
|移動|go|
|HTML|html|
|HTTP|http|
|Java|java|
|JavaScript|javascript|
|JSON|json|
|Kusto クエリ言語|kusto|
|Markdown|md|
|NodeJS|nodejs|
|Objective-C|objc|
|OData|odata|
|PHP|php|
|protobuf|protobuf|
|PowerApps (ドット小数点区切り記号)|powerapps-dot|
|PowerApps (コンマ小数点区切り記号)|powerapps-comma|
|PowerShell|powershell|
|Python|Python|
|Q#|qsharp|
|R|r|
|Ruby|ruby|
|SQL|sql|
|Swift|swift|
|TypeScript|typescript|
|Visual Basic|VB|
|VBScript|vbscript|
|XAML|xaml|
|XML|xml|
|yml|yml|

`csharp-interactive` 名で、C# 言語と、ブラウザーからサンプルを実行する機能を指定します。 このようなスニペットは Docker コンテナーでコンパイル、実行され、そのプログラム実行の結果がユーザーのブラウザー ウィンドウに表示されます。

C# (\`\`\`csharp)、Python (\`\`\`python)、PowerShell (\`\`\`powershell) の言語 ID を使用したコード ブロックの例を次に示します。

##### <a name="c"></a>C\#

```csharp
using System;
namespace HelloWorld
{
    class Hello
    {
        static void Main()
        {
            Console.WriteLine("Hello World!");

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```

#### <a name="python"></a>Python

```python
friends = ['john', 'pat', 'gary', 'michael']
for i, name in enumerate(friends):
    print "iteration {iteration} is {name}".format(iteration=i, name=name)
```

#### <a name="powershell"></a>PowerShell

```powershell
Clear-Host
$Directory = "C:\Windows\"
$Files = Get-Childitem $Directory -recurse -Include *.log `
-ErrorAction SilentlyContinue
```

### <a name="generic-code-block"></a>汎用のコード ブロック

汎用のコード ブロック コーディングには、3 つのバッククォート (&#96;&#96;&#96;) を使用します。

> お勧めする方法は、前のセクションで説明したようにコード ブロックを言語識別子と併用して、ドキュメント サイトで構文が正しく強調表示されるようにすることです。 汎用のコード ブロックは、必要な場合にのみ使用します。

```
function fancyAlert(arg) {
    if(arg) {
        $.docs({div:'#foo'})
    }
}
```

## <a name="blockquotes"></a>ブロック引用

> 1000 万年続いた干ばつにより、とうに恐竜の天下は終わっていた。 いつかアフリカと呼ばれるであろう大陸の、ここ赤道上では、生存を賭けた戦いは激しさを増した新たなクライマックスを迎え、勝利者が誰になるのかまだ分からなかった。 不毛で乾燥したこの地では、小型か敏捷かどう猛でなければ、栄えることはおろか、生き残る希望さえ持てなかった。

## <a name="images"></a>イメージ

### <a name="static-image-or-animated-gif"></a>静的イメージまたはアニメーション GIF

```markdown
![this is the alt text](../images/Logo_DotNet.png)
```

![これは代替テキストです](../images/Logo_DotNet.png)

### <a name="linked-image"></a>リンク画像

```markdown
[![alt text for linked image](../images/Logo_DotNet.png)](https://dot.net)
```

[![リンク画像の代替テキスト](../images/Logo_DotNet.png)](https://dot.net)

## <a name="videos"></a>ビデオ

現在、次の構文を使用して、Channel 9 と YouTube の両方のビデオを埋め込むことができます。

### <a name="channel-9"></a>Channel 9

```markdown
> [!VIDEO <channel9_video_link>]
```

ビデオの正しい URL を取得するには、ビデオ フレームの下にある **[埋め込み]** タブを選択して、`<iframe>` 要素から URL をコピーします。 次に例を示します。

```markdown
> [!VIDEO https://channel9.msdn.com/Blogs/dotnet/NET-Core-20-Released/player]
```

### <a name="youtube"></a>YouTube

ビデオの正しい URL を取得するには、ビデオ右クリックし、 **[埋め込みコードのコピー]** を選択して、`<iframe>` 要素から URL をコピーします。

```markdown
> [!VIDEO <youtube_video_link>]
```

次に例を示します。

```markdown
> [!VIDEO https://www.youtube.com/embed/Q2mMbjw6cLA]
```

## <a name="docsmicrosoft-extensions"></a>docs.microsoft 拡張機能

docs.microsoft により、GitHub Flavored Markdown にいくつかの拡張機能が追加されます。

### <a name="alerts"></a>アラート

ドキュメント サイトに適切なスタイルで表示できるようにするため、次のアラート スタイルを使用することが重要です。 ただし、GitHub のレンダリング エンジンではこれらは区別されません。

```markdown
> [!NOTE]
> Information the user should notice even if skimming.

> [!TIP]
> Optional information to help a user be more successful.

> [!IMPORTANT]
> Essential information required for user success.

> [!CAUTION]
> Negative potential consequences of an action.

> [!WARNING]
> Dangerous certain consequences of an action.
```

また、これらは次のように表示されます:![アラート スタイル](../images/alerts.png)

### <a name="includes"></a>含まれているバージョン

Include を使用して、あるファイルのマークダウンを別のファイルに埋め込むことができます。

[!INCLUDE[sample include file](../includes/sampleinclude.md)]

### <a name="checked-lists"></a>チェックされたファイル

リストには、カスタム スタイルを使用できます。 緑色のチェック マークが付けられたリストを表示できます。

> [!div class="checklist"]
>
> - .NET Core アプリの作成方法
> - Microsoft.XmlSerializer.Generator パッケージへの参照を追加する方法
> - MyApp.csproj を編集して依存関係を追加する方法
> - クラスと XmlSerializer を追加する方法
> - アプリケーションをビルドして実行する方法

使用中のチェックされたリストの例は、[.NET Core ドキュメント](https://docs.microsoft.com/dotnet/core/additional-tools/xml-serializer-generator)で確認できます。

### <a name="buttons"></a>ボタン

> [!div class="button"]
> [ボタン リンク](../docs/core/index.md)

使用中のボタンの例は、[Visual Studio ドキュメント](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-2---download-visual-studio)で確認できます。

### <a name="selectors"></a>セレクター

> [!div class="op_single_selector"]
>
> - [macOS](../docs/core/tutorials/using-on-macos.md)
> - [Windows](../docs/core/tutorials/with-visual-studio.md)

使用中のセレクターの例は、[Azure ドキュメント](https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-classic)で確認できます。

### <a name="step-by-steps"></a>操作手順

>[!div class="step-by-step"]
>[前へ](../docs/csharp/expression-trees-interpreting.md)
>[次へ](../docs/csharp/expression-trees-translating.md)

使用中の操作手順の例は、[C# ガイド](https://docs.microsoft.com/dotnet/csharp/tour-of-csharp/program-structure)で確認できます。
