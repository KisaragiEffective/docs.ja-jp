---
title: 文字列 - C# プログラミング ガイド
ms.date: 06/27/2019
helpviewer_keywords:
- C# language, strings
- strings [C#]
ms.assetid: 21580405-cb25-4541-89d5-037846a38b07
ms.openlocfilehash: dd76450c2a6a1726d630285f652d252c5f66183f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711910"
---
# <a name="strings-c-programming-guide"></a>文字列 (C# プログラミング ガイド)
文字列は、値がテキストの <xref:System.String> 型のオブジェクトです。 内部では、テキストは <xref:System.Char> オブジェクトの順次読み取り専用コレクションとして格納されます。 C# の文字列の末尾には null 終端文字はありません。したがって、C# の文字列には任意の数の null 文字 ('\0') を埋め込むことができます。 文字列の <xref:System.String.Length%2A> プロパティは、Unicode 文字の数ではなく、文字列に含まれている `Char` オブジェクトの数を表します。 文字列内の個別の Unicode コード ポイントにアクセスするには、<xref:System.Globalization.StringInfo> オブジェクトを使用します。  
  
## <a name="string-vs-systemstring"></a>文字列と System.String  
 C# では、`string` キーワードは <xref:System.String> のエイリアスです。 したがって、`String` と `string` は等価であり、どちらの名前付け規則を使用してもかまいません。 `String` クラスは、文字列を安全に作成、操作、比較するためのさまざまなメソッドを提供します。 また、C# 言語は、一般的な文字列操作を簡略化するためにいくつかの演算子をオーバーロードします。 キーワードの詳細については、「[string](../../language-reference/builtin-types/reference-types.md)」を参照してください。 型およびメソッドの詳細については、「<xref:System.String>」を参照してください。  
  
## <a name="declaring-and-initializing-strings"></a>文字列の宣言と初期化  
 次の例に示すように、文字列はさまざまな方法で宣言および初期化できます。  
  
 [!code-csharp[csProgGuideStrings#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#1)]  
  
 文字列を文字の配列で初期化する場合を除き、文字列オブジェクトの作成に [new](../../language-reference/operators/new-operator.md) 演算子を使用しないでください。  
  
 文字列の長さが 0 の新しい <xref:System.String> オブジェクトを作成するには、<xref:System.String.Empty> 定数値で文字列を初期化します。 長さ 0 の文字列のリテラル文字列表現は "" です。 [null](../../language-reference/keywords/null.md) の代わりに <xref:System.String.Empty> 値を使用して文字列を初期化すると、<xref:System.NullReferenceException> が発生する可能性を減らすことができます。 静的な <xref:System.String.IsNullOrEmpty%28System.String%29> メソッドを使用すると、アクセスを試行する前に文字列の値を検証できます。  
  
## <a name="immutability-of-string-objects"></a>文字列オブジェクトの不変性  
 文字列オブジェクトは*変更不可*です。つまり、作成した文字列オブジェクトは変更できません。 文字列を変更するように見える <xref:System.String> メソッドと C# 演算子はすべて、実際には新しい文字列オブジェクトで結果を返します。 次の例では、`s1` と `s2` の内容を連結して 1 つの文字列を形成するときに、2 つの元の文字列は変更されません。 `+=` 演算子で、連結した内容を含む新しい文字列が作成されます。 新しいオブジェクトは変数 `s1` に代入され、`s1` に代入された元のオブジェクトはガベージ コレクションに対して解放されます。これは、他の変数がこのオブジェクトへの参照を保持していないためです。  
  
 [!code-csharp[csProgGuideStrings#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#2)]  
  
 文字列の "変更" では、実際には文字列が新しく作成されるため、文字列への参照を作成するときには注意が必要です。 文字列の参照を作成し、元の文字列を "変更" する場合、参照は文字列が変更されたときに作成された新しいオブジェクトではなく、元のオブジェクトを指したままになります。 この動作を表すコードの例を次に示します。  
  
 [!code-csharp[csProgGuideStrings#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#25)]  
  
 元の文字列での検索操作や置換操作などの変更に基づく新しい文字列を作成する方法の詳細については、[文字列の内容を変更する方法](../../how-to/modify-string-contents.md)に関する記事をご覧ください。  
  
## <a name="regular-and-verbatim-string-literals"></a>標準リテラル文字列と逐語的リテラル文字列  
 次の例に示すように、C# で提供されるエスケープ文字を埋め込む必要がある場合は、標準リテラル文字列を使用します。  
  
 [!code-csharp[csProgGuideStrings#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#3)]  
  
 文字列テキストに円記号が含まれる場合 (ファイル パスなど) は、使いやすさと読みやすさを考慮して、逐語的文字列を使用します。 逐語的文字列は、文字列テキストの一部として改行文字を保持するため、複数行文字列の初期化に使用できます。 引用符を逐語的文字列に埋め込むには、二重引用符を使用します。 逐語的文字列の一般的な使用方法の例を次に示します。  
  
 [!code-csharp[csProgGuideStrings#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#4)]  
  
## <a name="string-escape-sequences"></a>文字列のエスケープ シーケンス  
  
|エスケープ シーケンス|文字名|Unicode エンコーディング|  
|---------------------|--------------------|----------------------|  
|\\'|単一引用符|0x0027|  
|\\"|二重引用符|0x0022|  
|\\\\ |円記号|0x005C|  
|\0|Null|0x0000|  
|\a|警告|0x0007|  
|\b|バックスペース|0x0008|  
|\f|フォーム フィード|0x000C|  
|\n|改行|0x000A|  
|\r|キャリッジ リターン|0x000D|  
|\t|水平タブ|0x0009|  
|\v|垂直タブ|0x000B|  
|\u|Unicode エスケープ シーケンス (UTF-16)|`\uHHHH` (範囲:0000 - FFFF; 例: `\u00E7` = "ç")|  
|\U|Unicode エスケープ シーケンス (UTF-32)|`\U00HHHHHH` (範囲:000000 - 10FFFF; 例: `\U0001F47D` = "&#x1F47D;")|  
|\x|可変長である点を除き "\u" に類似した Unicode エスケープ シーケンス|`\xH[H][H][H]` (範囲:0 - FFFF; 例: `\x00E7`、`\x0E7`、または `\xE7` = "ç")|  
  
> [!WARNING]
> `\x` のエスケープ シーケンスを使用していて、指定している 16 進数が 4 桁未満である場合に、エスケープ シーケンスの直後の文字が有効な 16 進数 (0-9、A-F、a-f) であると、それらはエスケープ シーケンスの一部として解釈されます。 たとえば、`\xA1` はコード ポイント U+00A1 の "&#161;" を生成します。 ただし、次の文字が "A" または "a" である場合、エスケープ シーケンスは代わりに `\xA1A` であると解釈され、コード ポイント U+0A1A の "&#x0A1A;" を生成します。 そのような場合、4 桁の 16 進数すべてを指定する (例: `\x00A1`) と、誤って解釈される可能性がすべて排除されます。  
  
> [!NOTE]
> コンパイル時に、逐語的文字列はエスケープ シーケンスと同様に通常の文字列に変換されます。 したがって、逐語的文字列をデバッガーのウォッチ ウィンドウで表示すると、ソース コードの逐語的バージョンではなく、コンパイラが追加したエスケープ文字が表示されます。 たとえば、逐語的文字列 `@"C:\files.txt"` は、ウォッチ ウィンドウでは "C:\\\files.txt" と表示されます。  
  
## <a name="format-strings"></a>書式指定文字列  
 書式指定文字列は、その内容が実行時に動的に決定される文字列です。 書式指定文字列を作成するには、文字列内の中かっこの内側に "*挿入式*" かプレースホルダーを埋め込みます。 中かっこ (`{...}`) 内にあるものはすべて値に解決され、実行時に書式設定された文字列として出力されます。 書式指定文字列を作成するには、文字列補間と複合書式設定の 2 つの方法があります。

### <a name="string-interpolation"></a>文字列補間
C# 6.0 以降で使用できる ["*補間文字列*"](../../language-reference/tokens/interpolated.md) は、特殊文字 `$` によって識別され、中かっこ内に挿入式を含みます。 文字列補間を初めて使用する場合は、簡単な概要として「[文字列補間 - C# の対話形式チュートリアル](../../tutorials/exploration/interpolated-strings.yml)」をご覧ください。

文字列補間を使ってコードの読みやすさと保守性を向上させます。 文字列補間がもたらす結果は `String.Format` メソッドと同じですが、使いやすさとインラインのわかりやすさが向上します。

[!code-csharp[csProgGuideFormatStrings](~/samples/snippets/csharp/programming-guide/strings/Strings_1.cs#StringInterpolation)]

### <a name="composite-formatting"></a>複合書式指定
<xref:System.String.Format%2A?displayProperty=nameWithType> では、中かっこ内のプレースホルダーを使って書式指定文字列を作成します。 この例では、上で使用した文字列補間の方法と同様の出力が生じます。
  
[!code-csharp[csProgGuideFormatStrings](~/samples/snippets/csharp/programming-guide/strings/Strings_1.cs#StringFormat)]

.NET 型の書式設定について詳しくは、「[.NET での型の書式設定](../../../standard/base-types/formatting-types.md)」をご覧ください。
  
## <a name="substrings"></a>部分文字列  
 部分文字列は、1 つの文字列に含まれる一連の文字です。 元の文字列の一部から新しい文字列を作成するには、<xref:System.String.Substring%2A> メソッドを使用します。 <xref:System.String.IndexOf%2A> メソッドを使用して、1 つまたは複数の部分文字列を検索できます。 指定されたすべての部分文字列を新しい文字列に置換するには、<xref:System.String.Replace%2A> メソッドを使用します。 <xref:System.String.Substring%2A> メソッドと同様に、<xref:System.String.Replace%2A> は実際に新しい文字列を返し、元の文字列は変更しません。 詳細については、「[文字列を検索する方法](../../how-to/search-strings.md)」と[文字列の内容を変更する方法](../../how-to/modify-string-contents.md)に関する記事をご覧ください。
  
 [!code-csharp[csProgGuideStrings#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#9)]  
  
## <a name="accessing-individual-characters"></a>各文字へのアクセス  
 次の例に示すように、配列表記とインデックス値を使用すると、それぞれの文字への読み取り専用アクセスが可能になります。  
  
 [!code-csharp[csProgGuideStrings#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#8)]  
  
 <xref:System.String> メソッドが、文字列内の個別の文字を変更する必要がある機能を提供していない場合は、<xref:System.Text.StringBuilder> オブジェクトを使用して個別の文字の "埋め込み先" を変更し、<xref:System.Text.StringBuilder> メソッドを使用することで、結果を格納する新しい文字列を作成できます。 次の例では、特定の方法で元の文字列を変更し、将来使用するためにその結果を保存する必要があるとします。  
  
 [!code-csharp[csProgGuideStrings#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#27)]  
  
## <a name="null-strings-and-empty-strings"></a>null 文字列と空の文字列  
 空の文字列はゼロ文字を含む <xref:System.String?displayProperty=nameWithType> オブジェクトのインスタンスです。 空の文字列は、空のテキスト フィールドを表すため、さまざまなプログラミング シナリオでよく使用されます。 有効な <xref:System.String?displayProperty=nameWithType> オブジェクトであるため、空の文字列でメソッドを呼び出すことができます。 空の文字列は、次のように初期化されます。  
  
```csharp  
string s = String.Empty;  
```  
  
 これに対し、null 文字列は <xref:System.String?displayProperty=nameWithType> オブジェクトのインスタンスを参照しないので、null 文字列でメソッドを呼び出そうとすると <xref:System.NullReferenceException> が発生します。 しかし、null 文字列を他の文字列に連結したり、他の文字列と比較することは可能です。 次に、null 文字列の参照によって例外がスローされる場合とされない場合の例を示します。  
  
 [!code-csharp[csProgGuideStrings#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#20)]  
  
## <a name="using-stringbuilder-for-fast-string-creation"></a>文字列を迅速に作成するための StringBuilder の使用  
 .NET での文字列操作は高度に最適化されており、ほとんどの場合パフォーマンスに大きく影響することはありません。 ただし、短いループが数百回または数千回実行されている場合など、シナリオによっては文字列操作がパフォーマンスに影響する可能性があります。 <xref:System.Text.StringBuilder> クラスが作成する文字列バッファーにより、プログラムで大量の文字列操作を実行する場合のパフォーマンスが向上します。 <xref:System.Text.StringBuilder> 文字列を使用すると、組み込み文字列データ型ではサポートされていない個別の文字を再割り当てできます。 たとえば、このコードでは、新しい文字列を作成せずに、文字列の内容を変更します。  
  
 [!code-csharp[csProgGuideStrings#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#15)]  
  
 この例では、<xref:System.Text.StringBuilder> オブジェクトを使用して、複数の数値型から 1 つの文字列を作成します。  
  
 [!code-csharp[TestStringBuilder#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/TestStringBuilder.cs)]
  
## <a name="strings-extension-methods-and-linq"></a>文字列、拡張メソッド、LINQ  
 <xref:System.String> 型は、<xref:System.Collections.Generic.IEnumerable%601> を実装するので、文字列には <xref:System.Linq.Enumerable> クラスで定義した拡張メソッドを使用できます。 見やすさを考慮して、これらのメソッドは <xref:System.String> 型の IntelliSense からは除外されていますが、使用できます。 文字列で LINQ クエリ式を使用することもできます。 詳細については、「[LINQ と文字列](../concepts/linq/linq-and-strings.md)」を参照してください。  
  
## <a name="related-topics"></a>関連トピック  
  
|トピック|説明|  
|-----------|-----------------|  
|[文字列の内容を変更する方法](../../how-to/modify-string-contents.md)|文字列の変換および文字列の内容を変更する手法を示します。|  
|[文字列を比較する方法](../../how-to/compare-strings.md)|文字列の序数とカルチャ固有の比較を実行する方法を示します。|  
|[複数の文字列を連結する方法](../../how-to/concatenate-multiple-strings.md)|複数の文字列を 1 つに結合するさまざまな方法を示します。|
|[String.Split を使用して文字列を解析する方法](../../how-to/parse-strings-using-split.md)|`String.Split` メソッドを使用して文字列を解析するコード例を紹介します。|  
|[文字列を検索する方法](../../how-to/search-strings.md)|特定のテキストまたは文字列のパターンの検索を使用する方法について説明します。|  
|[文字列が数値を表しているかどうかを確認する方法](./how-to-determine-whether-a-string-represents-a-numeric-value.md)|文字列を安全に解析して、有効な数値があるかどうかを確認する方法を示します。|  
|[文字列補間](../../language-reference/tokens/interpolated.md)|書式指定文字列に便利な構文を提供する文字列補間機能について説明します。|
|[基本的な文字列操作](../../../standard/base-types/basic-string-operations.md)|<xref:System.String?displayProperty=nameWithType> メソッドおよび <xref:System.Text.StringBuilder?displayProperty=nameWithType> メソッドを使用し文字列の基本操作を実行する、トピックへのリンクがあります。|  
|[文字列の解析](../../../standard/base-types/parsing-strings.md)|.NET の基本データ型の文字列形式を対応する型のインスタンスに変換する方法について説明します。|  
|[.NET での日付と時刻文字列の解析](../../../standard/base-types/parsing-datetime.md)|"01/24/2008" などの文字列を、<xref:System.DateTime?displayProperty=nameWithType> オブジェクトに変換する方法を示します。|  
|[文字列の比較](../../../standard/base-types/comparing.md)|文字列を比較する方法について説明し、C# および Visual Basic での例を示します。|  
|[StringBuilder クラスの使用](../../../standard/base-types/stringbuilder.md)|<xref:System.Text.StringBuilder> クラスの動的な文字列オブジェクトを作成および変更する方法について説明します。|  
|[LINQ と文字列](../concepts/linq/linq-and-strings.md)|LINQ クエリを使用してさまざまな文字列操作を実行する方法について説明します。|  
|[C# プログラミング ガイド](../index.md)|C# のプログラミング要素について説明するトピックへのリンクを示します。|  
