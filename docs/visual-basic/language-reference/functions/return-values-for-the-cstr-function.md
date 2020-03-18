---
title: CStr 関数の戻り値
ms.date: 07/20/2015
helpviewer_keywords:
- times [Visual Basic], CStr Function return values
- type conversion [Visual Basic]
- dates [Visual Basic], CStr Function return values
- CStr function
- strings [Visual Basic], return value
- Date data type [Visual Basic], converting
- dates [Visual Basic]
- String data type [Visual Basic], converting
ms.assetid: 3aa744e7-1419-45d5-85e3-e5abc2953673
ms.openlocfilehash: 4a40777c7290ec6d8c0d032f2edca5d889e20f04
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349995"
---
# <a name="return-values-for-the-cstr-function-visual-basic"></a>CStr 関数の戻り値 (Visual Basic)
次の表では、`expression`のさまざまなデータ型に対する `CStr` の戻り値について説明します。  
  
|`expression` 型がの場合|`CStr` 戻り値|  
|-----------------------------|--------------------|  
|[Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md)|"True" または "False" を含む文字列。|  
|[Date データ型](../../../visual-basic/language-reference/data-types/date-data-type.md)|システムの短い日付形式の `Date` 値 (日付と時刻) を含む文字列。|  
|[数値のデータ型](../../../visual-basic/programming-guide/language-features/data-types/numeric-data-types.md)|数値を表す文字列。|  
  
## <a name="cstr-and-date"></a>CStr と日付  
 `Date` 型には、常に日付と時刻の両方の情報が含まれます。 型変換の場合、Visual Basic は 1/1/0001 (1 年1月1日) を日付の*ニュートラル値*と見なし、00:00:00 (午前0時) はその時刻のニュートラル値と見なされます。 `CStr` には、結果の文字列にニュートラル値は含まれません。 たとえば、`#January 1, 0001 9:30:00#` を文字列に変換した場合、結果は "9:30:00 AM" になります。日付情報は表示されません。 ただし、日付情報は依然として元の `Date` 値に存在し、<xref:Microsoft.VisualBasic.DateAndTime.DatePart%2A>などの関数を使用して回復できます。  
  
> [!NOTE]
> `CStr` 関数は、アプリケーションの現在のカルチャ設定に基づいて変換を実行します。 特定のカルチャの数値の文字列形式を取得するには、数値の `ToString(IFormatProvider)` メソッドを使用します。 たとえば、型 `Double` の値を `String`に変換する場合は、<xref:System.Double.ToString%2A?displayProperty=nameWithType> を使用します。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.DateAndTime.DatePart%2A>
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md)
- [Date データ型](../../../visual-basic/language-reference/data-types/date-data-type.md)
