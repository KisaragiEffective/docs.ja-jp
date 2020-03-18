---
title: 文字列関数
ms.date: 07/20/2015
helpviewer_keywords:
- string functions
ms.assetid: f1bf9ac2-cbcf-4298-ae51-53182076bdc8
ms.openlocfilehash: 2608159e28ee63a0fdb10c82054fd65efe79ac62
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349979"
---
# <a name="string-functions-visual-basic"></a>文字列関数 (Visual Basic)

次の表は、文字列を検索して操作するために <xref:Microsoft.VisualBasic.Strings?displayProperty=nameWithType> クラスで提供 Visual Basic れる関数を示しています。 これらは Visual Basic 組み込み関数と見なすことができます。つまり、例に示すように、クラスの明示的なメンバーとして呼び出す必要はありません。 <xref:System.String?displayProperty=nameWithType> クラスでは、追加のメソッド、および場合によっては補完メソッドを使用できます。

|.NET Framework メソッド|説明|
|---------------------------|-----------------|
|<xref:Microsoft.VisualBasic.Strings.Asc%2A>, <xref:Microsoft.VisualBasic.Strings.AscW%2A>|文字に対応する文字コードを表す `Integer` 値を返します。|
|<xref:Microsoft.VisualBasic.Strings.Chr%2A>, <xref:Microsoft.VisualBasic.Strings.ChrW%2A>|指定された文字コードに対応する文字を返します。|
|<xref:Microsoft.VisualBasic.Strings.Filter%2A>|指定されたフィルター条件に基づいた文字列 (`String`) 配列のサブセットを含むゼロ ベースの配列を返します。|
|<xref:Microsoft.VisualBasic.Strings.Format%2A>|書式指定文字列 (`String`) 式に含まれる指示に従って書式設定された文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.FormatCurrency%2A>|システムの [コントロール パネル] で定義されている通貨記号を使って通貨形式の文字列に書式設定して返す文字列処理関数です。|
|<xref:Microsoft.VisualBasic.Strings.FormatDateTime%2A>|日時の値を表す文字列式を返します。|
|<xref:Microsoft.VisualBasic.Strings.FormatNumber%2A>|数値形式の文字列に書式設定して返す文字列処理関数です。|
|<xref:Microsoft.VisualBasic.Strings.FormatPercent%2A>|パーセント記号 (%) が付加されたパーセント形式 (100 で乗算した) の文字列に書式設定して返す文字列処理関数です。|
|<xref:Microsoft.VisualBasic.Strings.InStr%2A>|ある文字列の中から指定した文字列を検索し、最初に見つかった文字列の開始位置を示す整数型の値を返します。|
|<xref:Microsoft.VisualBasic.Strings.InStrRev%2A>|ある文字列の中から指定された文字列を最後の文字位置から検索を開始し、最初に見つかった文字位置 (先頭からその位置までの文字数) を返します。|
|<xref:Microsoft.VisualBasic.Strings.Join%2A>|配列に含まれる多数の部分文字列を結合して作成される文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.LCase%2A>|小文字に変換した文字列または文字を返します。|
|<xref:Microsoft.VisualBasic.Strings.Left%2A>|指定された文字数を含む文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.Len%2A>|文字列内の文字数を含む整数を返します。|
|<xref:Microsoft.VisualBasic.Strings.LSet%2A>|指定の文字列が含まれている文字列を左寄せで指定の長さに調整して返します。|
|<xref:Microsoft.VisualBasic.Strings.LTrim%2A>|指定された文字列から、先頭の空白を除いたコピーを格納する文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.Mid%2A>|文字列から指定された文字数分の文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.Replace%2A>|指定された文字列の一部を指定された回数分別の部分文字列で置換した文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.Right%2A>|文字列の右端から指定された文字数分の文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.RSet%2A>|文字列と長さが指定され、その長さに調整された文字列右揃えにして文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.RTrim%2A>|指定された文字列から、末尾の空白を除いたコピーを格納する文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.Space%2A>|指定された数のスペースから成る文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.Split%2A>|部分文字列ごとに区切られた文字列からゼロ ベースの 1 次元配列を作成し、返します。|
|<xref:Microsoft.VisualBasic.Strings.StrComp%2A>|文字列比較の結果により、-1、0、または 1 のいずれかを返します。|
|<xref:Microsoft.VisualBasic.Strings.StrConv%2A>|指定に従って変換された文字列型の値を返します。|
|<xref:Microsoft.VisualBasic.Strings.StrDup%2A>|指定された文字が指定された回数繰り返されている文字列型またはオブジェクト型の値を返します。|
|<xref:Microsoft.VisualBasic.Strings.StrReverse%2A>|指定された文字列の文字の並び順を逆にした文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.Trim%2A>|指定された文字列から、先頭または末尾の空白を除いたコピーを格納する文字列を返します。|
|<xref:Microsoft.VisualBasic.Strings.UCase%2A>|指定された文字列を大文字に変換して文字列型または char 型の値を返します。|

[Option Compare](../../../visual-basic/language-reference/statements/option-compare-statement.md)ステートメントを使用すると、システムのロケール (`Text`) または文字の内部バイナリ表現 (`Binary`) によって決定される、大文字と小文字を区別しないテキストの並べ替え順序を使用して文字列を比較するかどうかを設定できます。 既定のテキスト比較方法は `Binary` です。

## <a name="example-ucase"></a>例: UCase

`UCase` 関数を使って文字列を大文字に変換して返す例を次に示します。
[!code-vb[VbVbalrStrings#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#31)]

## <a name="example-ltrim"></a>例: LTrim

この例では、文字列変数から、`LTrim` 関数を使って先頭の空白を除去し、`RTrim` 関数を使って後続の空白を除去しています。 また、`Trim` 関数を使って両方のタイプの空白を除去しています。

[!code-vb[VbVbalrStrings#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#25)]

## <a name="example-mid"></a>例: Mid

`Mid` 関数を使って、文字列から指定された字数を返す例を次に示します。

[!code-vb[VbVbalrStrings#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#17)]

## <a name="example-len"></a>例: Len

`Len` 関数を使って文字列の文字数を返す例を次に示します。

[!code-vb[VbVbalrStrings#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#33)]

## <a name="example-instr"></a>例: InStr

`InStr` 関数を使って、ある文字列の中から指定された文字列を検索し、最初に見つかった文字位置を返す例を次に示します。

[!code-vb[VbVbalrStrings#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#8)]

## <a name="example-format"></a>例: 形式

`Format` の書式指定とユーザー定義の書式指定の両方を使って値の書式を指定する、`String` 関数のさまざまな使用例を次に示します。 日付の区切り記号 (`/`)、時刻の区切り記号 (`:`)、および午前/午後を示す文字 (`t` および `tt`) について、システムで実際に表示される書式は、コードが使用するロケール設定によって決まります。 時刻と日付を開発環境で表示する場合は、コード ロケールの短い時刻書式と短い日付書式が使用されます。

> [!NOTE]
> 24 時間制を使用するロケールでは、午前/午後を示す記号 (`t` および `tt`) では何も表示されません。

[!code-vb[VbVbalrStrings#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#27)]

## <a name="see-also"></a>参照

- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
- [Visual Basic ランタイム ライブラリのメンバー](../../../visual-basic/language-reference/runtime-library-members.md)
- [文字列操作の概要](../../../visual-basic/language-reference/keywords/string-manipulation-summary.md)
- [System.string クラスのメソッド](xref:System.String#methods)
