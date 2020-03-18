---
title: Visual Basic の新機能
ms.date: 10/24/2018
f1_keywords:
- VB.StartPage.WhatsNew
helpviewer_keywords:
- new features, Visual Basic
- what's new [Visual Basic]
- Visual Basic, what's new
ms.assetid: d7e97396-7f42-4873-a81c-4ebcc4b6ca02
ms.openlocfilehash: 45763504c2d25596b0adfb4b8a0236b332d89e8c
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802053"
---
# <a name="whats-new-for-visual-basic"></a>Visual Basic の新機能

このトピックでは、Visual Basic の各バージョンの主要機能の名前と、言語の最新バージョンでの新機能および拡張機能の詳しい説明を一覧表示します。

## <a name="current-version"></a>現在のバージョン

Visual Basic 16.0 / Visual Studio 2019 バージョン 16.0\
新機能については、「[Visual Basic 16.0](#visual-basic-160)」を参照してください。

## <a name="previous-versions"></a>以前のバージョン

Visual Basic 15.8 / Visual Studio 2017 バージョン 15.8\
新機能については、「[Visual Basic 15.8](#visual-basic-158)」を参照してください。

Visual Basic 15.5 / Visual Studio 2017 バージョン 15.5\
新機能については、「[Visual Basic 15.5](#visual-basic-155)」を参照してください。

Visual Basic 15.3 / Visual Studio 2017 バージョン 15.3\
新機能については、「[Visual Basic 15.3](#visual-basic-153)」を参照してください。

Visual Basic 2017 / Visual Studio 2017\
新機能については、「[Visual Basic 2017](#visual-basic-2017)」を参照してください。

Visual Basic / Visual Studio 2015\
新機能については、「[Visual Basic 14](#visual-basic-14)」を参照してください。

Visual Basic / Visual Studio 2013\
.NET コンパイラ プラットフォーム ("Roslyn") のテクノロジのプレビュー

Visual Basic / Visual Studio 2012\
`Async` と `await` のキーワード、反復子、呼び出し元情報属性

Visual Basic、Visual Studio 2010\
自動実装プロパティ、コレクション初期化子、暗黙的な行の連結、動的、ジェネリック co/負の分散、グローバル名前空間のアクセス

Visual Basic / Visual Studio 2008\
統合言語クエリ (LINQ)、XML リテラル、ローカル型の推定、オブジェクト初期化子、匿名型、拡張メソッド、ローカル `var` 型推論、ラムダ式、 `if` 演算子、部分メソッド、null 許容値型

Visual Basic / Visual Studio 2005\
`My` 型とヘルパーの種類 (アプリ、コンピューター、ファイル システム、ネットワークへのアクセス)

Visual Basic / Visual Studio .NET 2003\
ビット シフト演算子、ループ変数宣言

Visual Basic / Visual Studio .NET 2002\
Visual Basic .NET の最初のリリース

## <a name="visual-basic-160"></a>Visual Basic 16.0

Visual Basic 16.0 は、Visual Basic ランタイム (microsoft.visualbasic.dll) より多くの機能を .NET Core に多く提供することに重点を置いており、.NET Core に重点を置いた Visual Basic の最初のバージョンです。 Visual Basic ランタイムの多くの部分は WinForms に依存しており、これらは Visual Basic の今後のバージョンで追加される予定です。

**ステートメント内のより多くの場所で許可されているコメント**

Visual Basic 15.8 以前のバージョンでは、空白行、ステートメントの最後、または暗黙的な行の継続が許可されるステートメント内の特定の場所でのみ、コメントが許可されます。 Visual Basic 16.0 以降では、コメントは、明示的な行継続の後と、スペースとその後にアンダースコアで始まる行のステートメント内でも使用できます。

```vb
Public Sub Main()
    cmd.CommandText = ' Comment is allowed here without _
        "SELECT * FROM Titles JOIN Publishers " _ ' This is a comment
        & "ON Publishers.PubId = Titles.PubID " _
 _ ' This is a comment on a line without code
        & "WHERE Publishers.State = 'CA'"
End Sub
```

## <a name="visual-basic-158"></a>Visual Basic 15.8

**浮動小数点から整数への変換の最適化**

以前のバージョンの Visual Basic では、[倍精度浮動小数点型](../language-reference/data-types/double-data-type.md)と[単精度浮動小数点型](../language-reference/data-types/single-data-type.md)の値の整数への変換で比較的低いパフォーマンスが提供されていました。 Visual Basic 15.8 では、次のいずれかのメソッドによって返された値を[固有の Visual Basic 整数変換の関数](../language-reference/functions/type-conversion-functions.md) (CByte、CShort、CInt、CLng、CSByte、CUShort、CUInt、CULng) の 1 つに渡すとき、または [Option Strict](../language-reference/statements/option-strict-statement.md) が `Off` に設定されているときに、値が整数型に暗黙的にキャストされる次のいずれかのメソッドによって返されるときに、浮動小数点の整数への変換のパフォーマンスが大幅に向上します。

- <xref:Microsoft.VisualBasic.Conversion.Fix(System.Double)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Fix(System.Object)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Fix(System.Single)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Int(System.Double)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Int(System.Object)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Int(System.Single)?displayProperty=nameWithType>
- <xref:System.Math.Ceiling(System.Double)?displayProperty=nameWithType>
- <xref:System.Math.Floor(System.Double)?displayProperty=nameWithType>
- <xref:System.Math.Round(System.Double)?displayProperty=nameWithType>
- <xref:System.Math.Truncate(System.Double)?displayProperty=nameWithType>

この最適化によって、コードをより速く実行できます。大きい数値の整数型への変換を行うコードでは、最大で 2 倍速くなります。 次の例では、この最適化によって影響を受けるシンプルなメソッドの呼び出しを示しています。

```vb
Dim s As Single = 173.7619
Dim d As Double = s

Dim i1 As Integer = CInt(Fix(s))               ' Result: 173
Dim b1 As Byte = CByte(Int(d))                 ' Result: 173
Dim s1 AS Short = CShort(Math.Truncate(s))     ' Result: 173
Dim i2 As Integer = CInt(Math.Ceiling(d))      ' Result: 174
Dim i3 As Integer = CInt(Math.Round(s))        ' Result: 174
```

これは、浮動小数点の値を四捨五入するのではなく、切り詰められることに注意してください。

## <a name="visual-basic-155"></a>Visual Basic 15.5

[末尾以外の名前付き引数](../programming-guide/language-features/procedures/passing-arguments-by-position-and-by-name.md#mixing-arguments-by-position-and-by-name)

Visual Basic 15.3 以前のバージョンでは、位置と名前の両方による引数がメソッド呼び出しに含まれている場合、位置引数を名前付き引数より前に配置する必要がありました。 Visual Basic 15.5 以降では、最後の位置引数までのすべての引数が正しい位置にある限り、位置引数と名前付き引数を任意の順序で配置できます。 これは、コードを読みやすくするために名前付き引数を使用する場合に特に便利です。

たとえば、次のメソッド呼び出しには、名前付き引数の間に 2 つの位置引数があります。 名前付き引数では、値 19 が年齢を表していることが明確になります。

```vb
StudentInfo.Display("Mary", age:=19, #9/21/1998#)
```

[`Private Protected` メンバー アクセス修飾子](../language-reference/modifiers/private-protected.md)

この新しいキーワードの組み合わせでは、その親クラスのすべてのメンバーと、親クラスから派生した型でアクセスできるメンバーが定義されます。ただし、ここで言うアクセスする側が親アセンブリにも存在する場合に限られます。 構造体は継承できないため、`Private Protected` はクラスのメンバーにのみ適用できます。

**先頭の 16 進数/2 進数/8 進数の区切り記号**

Visual Basic 2017 では、桁区切り記号としてアンダースコア文字 (`_`) のサポートが追加されました。 Visual Basic 15.5 以降では、プレフィックスと 16 進数、2 進数、または 8 進数の間に先頭の区切り記号としてアンダースコア文字を使用することができます。 次の例では、3,271,948,384 を 16 進数として定義するために先頭の桁区切り記号を使用します。

```vb
Dim number As Integer = &H_C305_F860
```

アンダースコア文字を先頭の区切り記号として使用するには、以下の要素を Visual Basic プロジェクト (\*.vbproj) ファイルに追加する必要があります。

```xml
<PropertyGroup>
  <LangVersion>15.5</LangVersion>
</PropertyGroup>
```

## <a name="visual-basic-153"></a>Visual Basic 15.3

[**名前付きタプルの推論**](../programming-guide/language-features/data-types/tuples.md#inferred-tuple-element-names)

変数からタプル要素の値を割り当てる場合、Visual Basic は対応する変数名からタプル要素の名前を推論します。ユーザーがタプル要素に明示的に名前を付ける必要はありません。 以下の例では、推論を使用して、`state`、`stateName`、`capital` という 3 つの名前付き要素のタプルを作成します。

[!code-vb[Inferred tuple names](../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/named-tuples/program.vb#2)]

**追加のコンパイラ スイッチ**

Visual Basic コマンド ライン コンパイラで、参照アセンブリの出力を制御する [ **-refout**](../reference/command-line-compiler/refout-compiler-option.md) と [ **-refonly**](../reference/command-line-compiler/refonly-compiler-option.md) のコンパイラ オプションがサポートされるようになりました。 **-refout** は、参照アセンブリの出力ディレクトリを定義し、 **-refonly** はコンパイルで参照アセンブリだけが出力されるように指定します。

## <a name="visual-basic-2017"></a>Visual Basic 2017

[**タプル**](../programming-guide/language-features/data-types/tuples.md)

タプルは軽量のデータ構造であり、最も一般的には、1 回のメソッド呼び出しで複数の値を返すために利用されます。 通常、1 つのメソッドから複数の値を返すには、次のいずれかを行う必要があります。

- カスタムの型を定義します (`Class` または `Structure`)。 これは重量級のソリューションです。

- メソッドから 1 つの値を返すことに加え、1 つまたは複数の `ByRef` パラメーターを定義します。

Visual Basic はタプルに対応しているため、簡単にタプルを定義し、任意でその値にセマンティック名を割り当て、簡単にその値を取得できます。 次の例では、<xref:System.Int32.TryParse%2A> メソッドの呼び出しをラップして、タプルを返します。

[!code-vb[Tuple](../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple-returns.vb#2)]

メソッドを呼び出し、返されたタプルを処理できます。次のようなコードを利用します。

[!code-vb[ReturnTuple](../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple-returns.vb#3)]

**バイナリ リテラルと桁区切り文字**

プレフィックスとして `&B` または `&b` を使用し、バイナリ リテラルを定義できます。 さらに、下線文字 `_` を桁区切り文字として利用し、読みやすくすることができます。 次の例では両方の機能を利用し、`Byte` 値を割り当て、10 進数、16 進数、2 進数として表示しています。

[!code-vb[Binary](../../../samples/snippets/visualbasic/getting-started/bin-example.vb#1)]

詳細については、[Byte](../language-reference/data-types/byte-data-type.md#literal-assignments)、[Integer](../language-reference/data-types/integer-data-type.md#literal-assignments)、[Long](../language-reference/data-types/long-data-type.md#literal-assignments)、[Short](../language-reference/data-types/short-data-type.md#literal-assignments)、[SByte](../language-reference/data-types/sbyte-data-type.md#literal-assignments)、[UInteger](../language-reference/data-types/uinteger-data-type.md#literal-assignments)、[ULong](../language-reference/data-types/ulong-data-type.md#literal-assignments)、[UShort](../language-reference/data-types/ushort-data-type.md#literal-assignments) データ型の "Literal assignments" (リテラル割り当て) セクションをご覧ください。

[**C# 参照戻り値のサポート**](../programming-guide/language-features/procedures/ref-return-values.md)

C# 7.0 以降の C# は参照戻り値に対応しています。 つまり、メソッドを呼び出して、参照により返された値を受け取るとき、参照の値が変わることがあります。 Visual Basic の場合、参照戻り値でメソッドを作成することはできませんが、参照戻り値を利用したり、変更したりすることはできます。

たとえば、C# で記述された次の `Sentence` クラスには、指定された部分文字列で始まる文の次の単語を探す `FindNext` メソッドが含まれています。 文字列は参照戻り値として返され、参照によりメソッドに渡される `Boolean` 変数は検索が成功したかどうかを示します。 つまり、呼び出すことで戻り値を読めるだけでなく、変更することもできます。その変更は `Sentence` クラスで反映されます。

[!code-csharp[Ref-Return](../../../samples/snippets/visualbasic/getting-started/ref-returns.cs)]

最も単純な形式では、次のようなコードを利用し、文中で見つかった単語を変更できます。 メソッドに値を割り当てるのではなく、メソッドが返す式、つまり、参照戻り値に値を割り当てる点に注意してください。

[!code-vb[Ref-Return](../../../samples/snippets/visualbasic/getting-started/ref-return.vb#1)]

しかしながらこのコードには問題があり、一致が見つからない場合、メソッドは最初の単語を返します。 例では、`Boolean` 引数の値を調べて一致が見つかるかどうかを判断することがないため、一致がなければ最初の単語が変更されます。 次の例では、この問題が修正されています。一致がない場合、最初の単語をそれ自体で置換します。

[!code-vb[Ref-Return](../../../samples/snippets/visualbasic/getting-started/ref-return.vb#2)]

より良い解決策は、ヘルパー メソッドを利用し、参照によりヘルパー メソッドに参照戻り値を渡すことです。 ヘルパー メソッドは参照により渡された引数を変更できます。 次の例でこれを確認できます。

[!code-vb[Ref-Return](../../../samples/snippets/visualbasic/getting-started/ref-return-helper.vb#1)]

詳細については、[参照戻り値](../programming-guide/language-features/procedures/ref-return-values.md)に関するページを参照してください。

## <a name="visual-basic-14"></a>Visual Basic 14

[NameOf](../language-reference/operators/nameof.md)

文字列をハードコーディングせずにエラー メッセージで使用するための型またはメンバーの非修飾文字列名を取得できます。  これにより、リファクタリングするときにコードは正しい状態を保てます。  この機能は、またモデル-ビュー-コントローラーの MVC のリンクをフックし、プロパティ変更イベントを発生させるためにも役立ちます。

[文字列補間](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md)

文字列補間式を使用して、文字列を構築することができます。  補間文字列式は、式が含まれているテンプレート文字列のように見えます。  引数に関しては、補間文字列は[複合書式指定](../../standard/base-types/composite-formatting.md)より理解しやすくなっています。

[Null 条件メンバー アクセスとインデックス作成](../language-reference/operators/null-conditional-operators.md)

メンバー アクセス (`?.`) またはインデックス (`?[]`) 操作を実行する前に、構文的に非常に簡単な方法で null をテストできます。  これらの演算子を使用すると、null チェックの処理のために記述するコードを少なくすることができます (特に、データ構造を下っていく場合)。  左のオペランドまたはオブジェクト参照が null の場合、操作は null を返します。

[複数行の文字列リテラル](../../visual-basic/programming-guide/language-features/strings/string-basics.md)

文字列リテラルには、改行文字のシーケンスを含めることができます。  `<xml><![CDATA[...text with newlines...]]></xml>.Value` の使用に関する以前の次善策は不要になりました

**コメント**

暗黙的な行の連結の後の初期化子式内部、および LINQ 式項の間にコメントを入力できます。

**スマートな完全修飾名前解決**

`Threading.Thread.Sleep(1000)` などの特定のコードでは、以前 Visual Basic は名前空間 "Threading" を検索し、それが System.Threading と System.Windows.Threading の間であいまいであると判断し、エラーを報告していました。  これらはどちらも Visual Basic で可能な名前空間であると見なすようになりました。  コンプリート リストを表示する場合、Visual Studio エディターは、コンプリート リストの両方の種類のメンバーを一覧表示します。

**年が最初にくる日付リテラル**

日付リテラルの形式として yyyy-mm-dd `#2015-03-17 16:10 PM#` を使用できます。

**ReadOnly インターフェイスのプロパティ**

readwrite プロパティを使用して readonly インターフェイスのプロパティを実装できます。 このインターフェイスでは、最小限の機能が保証されています。これによって、実装するクラスでプロパティーが設定できなくなるということはありません。

[TypeOf \<expr> IsNot \<type>](../../visual-basic/language-reference/operators/typeof-operator.md)

コードを見やすくするために、`TypeOf` を `IsNot` とともに使用できるようになりました。

[#Disable Warning \<ID> と #Enable Warning \<ID>](../../visual-basic/language-reference/directives/index.md)

ソース ファイル内の領域の特定の警告を無効化および有効化することができます。

**XML ドキュメント コメントの機能強化**

ドキュメント コメントを記述する際、スマート エディターを取得し、パラメーター名の検証、`crefs` の適切な処理 (ジェネリック、演算子など)、色分け、リファクタリングのためのサポートを構築します。

[部分モジュールとインターフェイスの定義](../../visual-basic/language-reference/modifiers/partial.md)

クラスと構造体に加えて、部分モジュールとインターフェイスを宣言できます。

[メソッド本体内の #Region ディレクティブ](../../visual-basic/language-reference/directives/region-directive.md)

#Region…#End Region 区切り記号をファイルの任意の場所に挿入できます。関数内に装入することも、複数の関数本体に渡って挿入することもできます。

[オーバーライドの定義は暗黙的オーバーロードです](../../visual-basic/language-reference/modifiers/overrides.md)

`Overrides` 修飾子を定義に追加する場合には、コンパイラが `Overloads`  を暗黙的に追加し、共通のケースで入力するコードを少なくできるようにします。

**属性の引数で使用できる CObj**

以前、コンパイラーは、CObj(…) を属性の構築で使用するときにそれが定数でないというエラーを報告していました。

**異なるインターフェイスからのあいまいなメソッドの宣言と使用**

以前、以下のコードはエラーになり、`IMock` を宣言したり `GetDetails` を呼び出すことができなくなっていました (これらが C# で宣言されていた場合)。

```vb
Interface ICustomer
  Sub GetDetails(x As Integer)
End Interface

Interface ITime
  Sub GetDetails(x As String)
End Interface

Interface IMock : Inherits ICustomer, ITime
  Overloads Sub GetDetails(x As Char)
End Interface

Interface IMock2 : Inherits ICustomer, ITime
End Interface
```

コンパイラーは通常のオーバーロード解決規則を使用して、呼び出しに最も適切な `GetDetails` を選択するようになりました。サンプルで示されているようなインターフェイスのリレーションシップを Visual Basic で宣言できるようになりました。

## <a name="see-also"></a>関連項目

- [Visual Studio 2017 の新機能](/visualstudio/ide/whats-new-visual-studio-2017)
- [Visual Studio 2019 の新機能](/visualstudio/ide/whats-new-visual-studio-2019)
