---
description: '詳細情報: 最後の要素以外のすべてのフィールド幅は 0 より大きくなければなりません'
title: 最後の要素以外のすべてのフィールド幅は 0 より大きくなければなりません
ms.date: 07/20/2015
f1_keywords:
- vbrTextFieldParser_FieldWidthsMustPositive
ms.assetid: 41d8c661-a749-4c89-be56-905c6e7c3c9d
ms.openlocfilehash: cf2edb21f017031c7dd333893d554353eceebe73
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99787384"
---
# <a name="all-field-widths-except-the-last-element-must-be-greater-than-zero"></a>最後の要素以外のすべてのフィールド幅は 0 より大きくなければなりません

最後の要素以外のすべてのフィールド幅は 0 より大きくなければなりません。 最後の要素内の 0 以下のフィールド幅は、最後のフィールドが可変長であることを示しています。  
  
 最後の要素以外のフィールド幅が 0 以下に設定されているため、処理に失敗しました。 0 以下のフィールド幅を最後の要素として使用し、最後のフィールドが可変長であることを示すことができますが、他の要素として使用することはできません。  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- フィールド幅を正しい長さに設定します。  
  
## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetFieldWidths%2A?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.FieldWidths>
- [方法: 固定幅のテキスト ファイルを読み取る](../developing-apps/programming/drives-directories-files/how-to-read-from-fixed-width-text-files.md)
- [TextFieldParser Object](../language-reference/objects/textfieldparser-object.md)
