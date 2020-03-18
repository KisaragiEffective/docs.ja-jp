---
title: MaskedTextBox コントロールによる正規表現を使用する
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], regular expressions
- strings [Visual Basic], masked edit
ms.assetid: 2a048fb0-7053-487d-b2c5-ffa5e22ed6f9
ms.openlocfilehash: 12d500fa0ff4945dcf2d5009bdba6d337834707e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346263"
---
# <a name="using-regular-expressions-with-the-maskedtextbox-control-in-visual-basic"></a>Visual Basic の MaskedTextBox コントロールによる正規表現を使用する
この例では、単純な正規表現を変換して、<xref:System.Windows.Forms.MaskedTextBox> コントロールを操作する方法を示します。  
  
## <a name="description-of-the-masking-language"></a>マスク言語の説明  
 標準 <xref:System.Windows.Forms.MaskedTextBox> マスク言語は Visual Basic 6.0 の `Masked Edit` コントロールによって使用される言語に基づいており、そのプラットフォームから移行するユーザーには慣れている必要があります。  
  
 <xref:System.Windows.Forms.MaskedTextBox> コントロールの <xref:System.Windows.Forms.MaskedTextBox.Mask%2A> プロパティでは、使用する入力マスクを指定します。 マスクは、次の表の1つ以上のマスク要素で構成される文字列である必要があります。  
  
|マスク要素|説明|正規表現の要素|  
|---------------------|-----------------|--------------------------------|  
|0|0から9までの任意の1桁。 エントリが必要です。|\d|  
|9|数字またはスペース。 エントリは省略可能です。|[\d]?|  
|#|数字またはスペース。 エントリは省略可能です。 マスクでこの位置を空白のままにすると、スペースとして表示されます。 正符号 (+) とマイナス記号 (-) は使用できます。|[\d +-]?|  
|L|ASCII 文字。 エントリが必要です。|[zA-Z]|  
|?|ASCII 文字。 エントリは省略可能です。|[zA-Z]?|  
|&|文字です。 エントリが必要です。|[\p{Ll}\p{Lu}\p{Lt}\p{Lm}\p{Lo}]|  
|C|文字です。 エントリは省略可能です。|[\p{Ll}\p{Lu}\p{Lt}\p{Lm}\p{Lo}]?|  
|A|英数字. エントリは省略可能です。|\W|  
|。|カルチャに応じた小数点のプレースホルダー。|使用できません。|  
|,|カルチャに応じた桁プレースホルダー。|使用できません。|  
|:|カルチャに適した時間区切り。|使用できません。|  
|/|カルチャに適した日付の区切り記号。|使用できません。|  
|$|カルチャに適した通貨記号。|使用できません。|  
|\<|後続のすべての文字を小文字に変換します。|使用できません。|  
|>|に続くすべての文字を大文字に変換します。|使用できません。|  
|&#124;|前のシフトを元に戻すか、シフトします。|使用できません。|  
|&#92;|マスク文字をエスケープし、その文字をリテラルに変換します。 "\\\\" は、円記号のエスケープシーケンスです。|&#92;|  
|その他のすべての文字。|リテラル. マスクされていないすべての要素は、<xref:System.Windows.Forms.MaskedTextBox>内に自身として表示されます。|その他のすべての文字。|  
  
 10進数 (.)、桁区切り記号 (,)、時刻 (:)、日付 (/)、および通貨 ($) のシンボルは、既定では、アプリケーションのカルチャによって定義されている記号を表示します。 <xref:System.Windows.Forms.MaskedTextBox.FormatProvider%2A> プロパティを使用して、別のカルチャのシンボルを表示するように強制できます。  
  
## <a name="regular-expressions-and-masks"></a>正規表現とマスク  
 正規表現とマスクを使用してユーザー入力を検証することもできますが、完全に同等であるとは限りません。 正規表現はマスクよりも複雑なパターンを表現できますが、マスクは同じ情報をより簡潔で、カルチャに関連する形式で表現することができます。  
  
 次の表では、4つの正規表現とそれぞれの対応するマスクを比較しています。  
  
|正規表現|マスク|説明|  
|------------------------|----------|-----------|  
|`\d{2}/\d{2}/\d{4}`|`00/00/0000`|マスク内の `/` 文字は論理日付の区切り記号であり、アプリケーションの現在のカルチャに適した日付の区切り記号としてユーザーに表示されます。|  
|`\d{2}-[A-Z][a-z]{2}-\d{4}`|`00->L<LL-0000`|米国形式の日付 (日、月の省略形、および年)。3文字の月の省略形は、最初の大文字と2つの小文字で表示されます。|  
|`(\(\d{3}\)-)?\d{3}-d{4}`|`(999)-000-0000`|米国電話番号、市外局番は省略可能です。 ユーザーがオプションの文字を入力したくない場合は、スペースを入力するか、最初の0によって表されるマスク内の位置にマウスポインターを直接配置できます。|  
|`$\d{6}.00`|`$999,999.00`|0 ~ 999999 の範囲の通貨値。 通貨、分、および10進文字は、実行時にカルチャ固有の同等のものに置き換えられます。|  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.MaskedTextBox.Mask%2A>
- <xref:System.Windows.Forms.MaskedTextBox>
- [Visual Basic における文字列の検証](../../../../visual-basic/programming-guide/language-features/strings/validating-strings.md)
- [MaskedTextBox コントロール](../../../../framework/winforms/controls/maskedtextbox-control-windows-forms.md)
