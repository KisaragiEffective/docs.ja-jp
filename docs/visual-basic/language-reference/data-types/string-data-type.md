---
title: String データ型 (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.String
helpviewer_keywords:
- strings [Visual Basic], character
- strings [Visual Basic], fixed-length
- string keyword [Visual Basic]
- fixed-length strings [Visual Basic], string data type
- literals [Visual Basic], string
- text [Visual Basic], String data type
- $ identifier type character
- String data type
- fixed-length strings
- string literals [Visual Basic]
- data types [Visual Basic], assigning
- String literals [Visual Basic]
- identifier type characters [Visual Basic], $
ms.assetid: 15ac03f5-cabd-42cc-a754-1df3893c25d9
ms.openlocfilehash: c2c6f9632646c432abb7b6da8887253e526cc994
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343904"
---
# <a name="string-data-type-visual-basic"></a>String データ型 (Visual Basic)

0 ~ 65535 の範囲の値を範囲とする符号なし16ビット (2 バイト) コードポイントのシーケンスを保持します。 各*コードポイント*(文字コード) は、1つの Unicode 文字を表します。 文字列には、0 ~ 約 20億 (2 ^ 31) の Unicode 文字を含めることができます。  
  
## <a name="remarks"></a>コメント  

 `String` データ型を使用して、`Char` 要素の配列である `Char()`の配列管理オーバーヘッドを発生させることなく、複数の文字を保持します。  
  
 `String` の既定値は `Nothing` (null 参照) です。 これは空の文字列 (値 `""`) と同じではないことに注意してください。  
  
## <a name="unicode-characters"></a>Unicode 文字  

 Unicode の最初の128コードポイント (0 ~ 127) は、標準の U.S. キーボードの文字と記号に対応しています。 これらの最初の128コードポイントは、ASCII 文字セットで定義されているものと同じです。 2番目の128コードポイント (128 ~ 255) は、ラテン語に基づくアルファベット文字、アクセント、通貨記号、分数などの特殊文字を表します。 Unicode では、さまざまなシンボルについて、残りのコードポイント (256-65535) を使用します。 これには、世界中のテキスト文字、分音記号、数学、およびテクニカルシンボルが含まれます。  
  
 `String` 変数内の個々の文字に <xref:System.Char.IsDigit%2A> や <xref:System.Char.IsPunctuation%2A> などのメソッドを使用して、Unicode 分類を決定することができます。  
  
## <a name="format-requirements"></a>書式の要件  

 `String` リテラルは引用符 (`" "`) で囲む必要があります。 文字列内のいずれかの文字として引用符を含める必要がある場合は、2つの連続する引用符 (`""`) を使用します。 次に例を示します。  
  
```vb  
Dim j As String = "Joe said ""Hello"" to me."  
Dim h As String = "Hello"  
' The following messages all display the same thing:  
' "Joe said "Hello" to me."  
MsgBox(j)  
MsgBox("Joe said " & """" & h & """" & " to me.")  
MsgBox("Joe said """ & h & """ to me.")  
```  
  
 文字列内の引用符を表す連続する引用符は、`String` リテラルを開始および終了する引用符とは関係がないことに注意してください。  
  
## <a name="string-manipulations"></a>文字列操作  

 文字列を `String` 変数に代入すると、その文字列は*不変*になります。つまり、長さや内容を変更することはできません。 文字列を任意の方法で変更すると、Visual Basic によって新しい文字列が作成され、前の文字列が破棄されます。 `String` 変数は、新しい文字列を指します。  
  
 `String` 変数の内容を操作するには、さまざまな文字列関数を使用します。 <xref:Microsoft.VisualBasic.Strings.Left%2A> 関数の例を次に示します。  
  
```vb  
Dim S As String = "Database"  
' The following statement sets S to a new string containing "Data".  
S = Microsoft.VisualBasic.Left(S, 4)  
```  
  
 別のコンポーネントによって作成された文字列は、先頭または末尾にスペースが埋め込まれている可能性があります。 このような文字列を受け取った場合は、<xref:Microsoft.VisualBasic.Strings.Trim%2A>、<xref:Microsoft.VisualBasic.Strings.LTrim%2A>、および <xref:Microsoft.VisualBasic.Strings.RTrim%2A> の各関数を使用して、これらのスペースを削除できます。  
  
 文字列操作の詳細については、「[Visual Basic における文字列](../../../visual-basic/programming-guide/language-features/strings/index.md)」を参照してください。  
  
## <a name="programming-tips"></a>プログラミングのヒント  
  
- **負の数。** `String` によって保持されている文字は符号なしであり、負の値を表すことはできないことに注意してください。 どのような場合でも、`String` を使用して数値を保持しないでください。  
  
- **相互運用のための注意事項。** .NET Framework 用に作成されていないコンポーネント (たとえば、オートメーションや COM オブジェクト) とやり取りする場合は、他の環境で文字列文字のデータ幅が異なる (8 ビット) ことに注意してください。 8 ビット文字の文字列引数をこのようなコンポーネントに渡す場合は、新しい Visual Basic コードで `String` するのではなく、`Byte` 要素の配列 `Byte()`として宣言します。  
  
- **型文字。** 識別子の型に `$` 識別子を追加すると、その識別子が `String` データ型に強制されます。 `String` にリテラルの型文字がありません。 ただし、コンパイラは、引用符 (`" "`) で囲まれたリテラルを `String` として扱います。  
  
- **Framework の型。** .NET Framework 内の対応する型は、<xref:System.String?displayProperty=nameWithType> クラスです。  
  
## <a name="see-also"></a>関連項目

- <xref:System.String?displayProperty=nameWithType>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [Char データ型](../../../visual-basic/language-reference/data-types/char-data-type.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [方法 : 符号なしの型を使用する Windows の機能を呼び出す](../../../visual-basic/programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
