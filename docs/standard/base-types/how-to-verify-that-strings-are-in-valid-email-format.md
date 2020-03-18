---
title: 文字列が有効な電子メール形式であるかどうかを検証する方法
ms.date: 12/10/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, examples
- user input, examples
- Regex.IsMatch method
- regular expressions [.NET Framework], examples
- examples [Visual Basic], strings
- IsValidEmail
- validation, email strings
- input, checking
- strings [.NET Framework], examples [Visual Basic]
- email [.NET Framework], validating
- IsMatch method
ms.assetid: 7536af08-4e86-4953-98a1-a8298623df92
ms.openlocfilehash: c02fc215fa66951ae3333175191ab96a226a2afe
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73197584"
---
# <a name="how-to-verify-that-strings-are-in-valid-email-format"></a>文字列が有効な電子メール形式であるかどうかを検証する方法

正規表現を使用して文字列の形式が有効な電子メール形式であるかどうかを検証する例を次に示します。

## <a name="example"></a>例

次の例では、 `IsValidEmail` メソッドを定義します。このメソッドは、文字列に有効な電子メール アドレスが含まれている場合に `true` を返し、含まれていない場合に `false` を返します。それ以外の動作は行いません。

電子メール アドレスが有効であることを確認するため、 `IsValidEmail` メソッドは <xref:System.Text.RegularExpressions.Regex.Replace%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.MatchEvaluator%29?displayProperty=nameWithType> メソッドを呼び出し、 `(@)(.+)$` の正規表現パターンを指定して、電子メール アドレスからドメイン名を切り離します。 3 番目のパラメーターは、一致したテキストを操作また置換するメソッドを表す <xref:System.Text.RegularExpressions.MatchEvaluator> デリゲートです。 正規表現パターンは次のように解釈されます。

|パターン|説明|
|-------------|-----------------|
|`(@)`|@ 文字と一致します。 これが最初のキャプチャ グループです。|
|`(.+)`|任意の文字の 1 回以上の出現に一致します。 これが 2 番目のキャプチャ グループです。|
|`$`|入力文字列の末尾で照合を終了します。|

ドメイン名は @ 文字と合わせて `DomainMapper` メソッドに渡されます。このメソッドは <xref:System.Globalization.IdnMapping> クラスを使用して、US-ASCII 文字の範囲に含まれない Unicode 文字を Punycode に変換します。 また、このメソッドは、 `invalid` メソッドがドメイン名に無効な文字を検出すると、 `True` フラグを <xref:System.Globalization.IdnMapping.GetAscii%2A?displayProperty=nameWithType> に設定します。 このメソッドは、Punycode ドメイン名の前に @ 記号を付けて、これを `IsValidEmail` メソッドに返します。

次に、 `IsValidEmail` メソッドは <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%29?displayProperty=nameWithType> メソッドを呼び出して、電子メール アドレスが正規表現パターンに準拠するかどうかを確認します。

`IsValidEmail` メソッドは、認証によって電子メール アドレスを検証するわけではありません。 電子メール アドレスの形式として有効かどうかを判断しているだけです。 さらに、 `IsValidEmail` メソッドは、最上位ドメイン名が [IANA ルート ゾーン データベース](https://www.iana.org/domains/root/db)に掲載されている有効なドメイン名であることを確認しません (これには検索操作が必要です)。 代わりに、正規表現によって単に最上位ドメイン名が 2 ～ 24 文字の ASCII 英数字で構成されており、最初の文字と末尾の文字は英数字、その他の文字は英数字またはハイフン (-) であることを確認します。

[!code-csharp[RegularExpressions.Examples.Email#7](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.Email/cs/example4.cs#7)]
[!code-vb[RegularExpressions.Examples.Email#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.Email/vb/example4.vb#7)]

この例の正規表現パターン ``^(?(")(".+?(?<!\\)"@)|(([0-9a-z]((\.(?!\.))|[-!#\$%&'\*\+/=\?\^`\{\}\|~\w])*)(?<=[0-9a-z])@))(?(\[)(\[(\d{1,3}\.){3}\d{1,3}\])|(([0-9a-z][-0-9a-z]*[0-9a-z]*\.)+[a-z0-9][\-a-z0-9]{0,22}[a-z0-9]))$`` の意味を次の凡例に示します。 正規表現のコンパイルには、<xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> フラグが使用されます。

パターン `^`:文字列の先頭から照合を開始します。

パターン `(?(")`:最初の文字が引用符であるかどうかを確認します。 `(?(")` は代替構成体の開始位置です。

パターン `(?(")(".+?(?<!\\)"@)`:最初の文字が引用符である場合、始まりの引用符に続く 1 つ以上の任意の文字と終わりの引用符に一致します。 終わりの引用符の前には円記号 (\\) を使用できません。 `(?<!` はゼロ幅の否定先読みアサーションの開始です。 文字列はアット マーク (@) で終わる必要があります。

パターン `|(([0-9a-z]`:最初の文字が引用符でない場合は、a ～ z または A ～ Z の任意の英字または 0 ～ 9 の任意の数字と一致します (比較では大文字小文字は区別されません)。

パターン `(\.(?!\.))`:次の文字がピリオドの場合は、その文字と一致します。 ピリオドでない場合は、次の文字を先読みして照合を継続します。 `(?!\.)` は、2 つの連続するピリオドが電子メール アドレスのローカル部分に出現することを防ぐゼロ幅の負の先読みアサーションです。

パターン ``|[-!#\$%&'\*\+/=\?\^`\{\}\|~\w]``:次の文字がピリオドではない場合、任意の単語文字または -!#$%&'\*+/=?^\`{}|~ のいずれかの文字と一致します。

パターン ``((\.(?!\.))|[-!#\$%&'\*\+/=\?\^`\{\}\|~\w])*``:0 回以上の代替パターン (ピリオドとそれに続くピリオド以外の文字、または複数の文字のうちのいずれかの 1 文字) と一致します。

パターン `@`:@ 文字と一致します。

パターン `(?<=[0-9a-z])`:@ 文字の前にくる文字が A ～ Z、a ～ z、または 0 ～ 9 である場合に照合を継続します。 このパターンは、ゼロ幅の正の後読みアサーションを定義します。

パターン `(?(\[)`:@ に続く文字が左角かっこかどうかを確認します。

パターン `(\[(\d{1,3}\.){3}\d{1,3}\])`:左角かっこの場合は、左角かっこ、それに続く IP アドレス (各セットがピリオドで区切られた、4 セットの 1 ～ 3 桁の数字)、および右角かっこと一致します。

パターン `|(([0-9a-z][-0-9a-z]*[0-9a-z]*\.)+`:@ に続く文字が左角かっこでない場合は、値が A - Z、a - z、または 0 - 9 の 1 つの英数字の後に、ハイフンの 0 回以上の出現、A - Z、a - z、または 0 - 9 の値の 0 個または 1 つの英数字、さらにピリオドが続くパターンと一致します。 このパターンは 1 回以上繰り返すことができ、後ろに最上位ドメイン名が続く必要があります。

パターン `[a-z0-9][\-a-z0-9]{0,22}[a-z0-9]))`:最上位ドメイン名では、最初の文字と最後の文字が英数字文字 (a ～ z、A ～ Z、0 ～ 9) である必要があります。 また、0 ～ 22 文字の ASCII 文字 (英数字またはハイフン) を含めることができます。

パターン `$`:入力文字列の末尾で照合を終了します。

## <a name="compile-the-code"></a>コードのコンパイル

`IsValidEmail` メソッドと `DomainMapper` メソッドは、正規表現ユーティリティ メソッドのライブラリに含めることができるほか、アプリケーション クラス内のプライベートな静的メソッドやインスタンス メソッドとして含めることもできます。

また、 <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType> メソッドを使用して、この正規表現を正規表現ライブラリに追加できます。

正規表現ライブラリ内で使用した場合は、次のようなコードを使用して呼び出すことができます。

[!code-csharp[RegularExpressions.Examples.Email#8](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.Email/cs/example4.cs#8)]
[!code-vb[RegularExpressions.Examples.Email#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.Email/vb/example4.vb#8)]

## <a name="see-also"></a>関連項目

- [.NET Framework 正規表現](../../../docs/standard/base-types/regular-expressions.md)
