---
title: データ型の有効な使用方法
ms.date: 07/20/2015
helpviewer_keywords:
- performance, data type efficiency
- data types [Visual Basic], weak typing
- AscW function [Visual Basic], preferred to Asc
- data types [Visual Basic], using efficiently
- optimization [Visual Basic], data types
- data types [Visual Basic], strong typing
- strong typing
- typing, strong
- data types [Visual Basic], optimizing
- ChrW function [Visual Basic], preferred to Chr
ms.assetid: 28f5e4ba-ec24-4f37-b90a-e8ee822f778a
ms.openlocfilehash: 621dec7537e9c993024e271b96ab8706baf89885
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350106"
---
# <a name="efficient-use-of-data-types-visual-basic"></a>データ型の有効な使用方法 (Visual Basic)
宣言されていない変数およびデータ型を使用せずに宣言された変数には、`Object` データ型が割り当てられます。 これにより、プログラムをすばやく簡単に記述できるようになりますが、実行速度が低下する可能性があります。

## <a name="strong-typing"></a>厳密な型指定
 すべての変数のデータ型を指定することを、*厳密な*型指定と呼びます。 厳密な型指定を使用すると、いくつかの利点があります。

- これにより、変数の IntelliSense サポートが有効になります。 これにより、コードで入力するときに、プロパティや他のメンバーを表示できます。

- コンパイラの型チェックを利用します。 これは、オーバーフローなどのエラーによって実行時に失敗する可能性があるステートメントをキャッチします。 また、サポートされていないオブジェクトのメソッドの呼び出しもキャッチします。

- これにより、コードの実行時間が短縮されます。

## <a name="most-efficient-data-types"></a>最も効率的なデータ型
 分数を含まない変数の場合、整数データ型は非整数型よりも効率的です。 Visual Basic では、`Integer` と `UInteger` が最も効率的な数値型です。

 小数値の場合、現在のプラットフォームのプロセッサは倍精度で浮動小数点演算を実行するため、`Double` は最も効率的なデータ型です。 ただし、`Double` の操作は、`Integer`などの整数型ほど高速ではありません。

## <a name="specifying-data-type"></a>データ型の指定
 [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)を使用して、特定の型の変数を宣言します。 次の例に示すように、 [Public](../../../../visual-basic/language-reference/modifiers/public.md)、 [Protected](../../../../visual-basic/language-reference/modifiers/protected.md)、 [Friend](../../../../visual-basic/language-reference/modifiers/friend.md)、または[Private](../../../../visual-basic/language-reference/modifiers/private.md)キーワードを使用して、アクセスレベルを同時に指定できます。

```vb
Private x As Double
Protected s As String
```

## <a name="character-conversion"></a>文字変換
 `AscW` 関数と `ChrW` 関数は Unicode で動作します。 これらの設定は、`Asc` と `Chr`に使用する必要があります。これは、Unicode との間で変換を行う必要があります。

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Strings.Asc%2A>
- <xref:Microsoft.VisualBasic.Strings.AscW%2A>
- <xref:Microsoft.VisualBasic.Strings.Chr%2A>
- <xref:Microsoft.VisualBasic.Strings.ChrW%2A>
- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [数値のデータ型](../../../../visual-basic/programming-guide/language-features/data-types/numeric-data-types.md)
- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [IntelliSense の使用](/visualstudio/ide/using-intellisense)
