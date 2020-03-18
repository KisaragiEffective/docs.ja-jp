---
title: 型文字
ms.date: 01/31/2018
helpviewer_keywords:
- '&H prefix for hexadecimal values'
- hexadecimal literals [Visual Basic]
- F literal type character [Visual Basic]
- '& identifier type character'
- type characters [Visual Basic]
- octal literals [Visual Basic]
- literals [Visual Basic], hexadecimal
- '&O prefix for octal values'
- literals [Visual Basic], default types
- defaults, literal types
- C literal type character [Visual Basic]
- type characters [Visual Basic], literal
- $ identifier type character
- L literal type character [Visual Basic]
- UI literal type characters [Visual Basic]
- default literal types [Visual Basic]
- D literal type character [Visual Basic]
- literals [Visual Basic], octal
- S literal type character [Visual Basic]
- '! identifier type character'
- US literal type characters [Visual Basic]
- '% identifier type character'
- data types [Visual Basic], type characters
- characters [Visual Basic], identifier type
- type characters [Visual Basic], identifier
- '# identifier type character'
- identifier type characters [Visual Basic]
- literal type characters [Visual Basic]
- I literal type character [Visual Basic]
- R literal type character [Visual Basic]
- '@ identifier type character'
- UL literal type characters [Visual Basic]
- literal types [Visual Basic], default
ms.assetid: 6353cb9b-6ee4-4af6-a5a8-88ce39f90cc5
ms.openlocfilehash: 628461c8136946dd902c0a52048eee7c516c52cd
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352935"
---
# <a name="type-characters-visual-basic"></a>型文字 (Visual Basic)

宣言ステートメントでデータ型を指定するだけでなく、一部のプログラミング要素のデータ型を*型文字*に強制的に適用することもできます。 型文字は、要素の直後に指定する必要があります。その際、任意の文字を使用することはできません。

型文字は、要素の名前の一部ではありません。 型文字で定義された要素は、型文字を使用せずに参照できます。

## <a name="identifier-type-characters"></a>識別子の型文字

Visual Basic には、変数または定数のデータ型を指定するために宣言で使用できる*識別子の型文字*のセットが用意されています。 使用できる識別子の型文字と使用例を次の表に示します。
  
|識別子の型文字|データ型|例|  
|-------------------------------|---------------|-------------|  
|`%`|`Integer`|`Dim L%`|  
|`&`|`Long`|`Dim M&`|  
|`@`|`Decimal`|`Const W@ = 37.5`|  
|`!`|`Single`|`Dim Q!`|  
|`#`|`Double`|`Dim X#`|  
|`$`|`String`|`Dim V$ = "Secret"`|  
  
 `Boolean`、`Byte`、`Char`、`Date`、`Object`、`SByte`、`Short`、`UInteger`、`ULong`、または `UShort` のデータ型、または配列や構造体などの複合データ型には、識別子の型文字が存在しません。

場合によっては、`$` 文字を `Left`ではなく `Left$` などの Visual Basic 関数に追加して、型 `String`の戻り値を取得することもできます。

いずれの場合も、識別子の型文字は識別子名の直後に指定する必要があります。

## <a name="literal-type-characters"></a>リテラルの型文字

*リテラル*は、データ型の特定の値のテキスト表現です。  

### <a name="default-literal-types"></a>既定のリテラル型

コードに表示されるリテラルの形式は、通常、そのデータ型を決定します。 これらの既定の型を次の表に示します。  
  
|リテラルのテキスト形式|既定のデータ型|例|  
|-----------------------------|-----------------------|-------------|  
|数値、小数部なし|`Integer`|`2147483647`|  
|数値、小数部を含まない、`Integer` に対して大きすぎます|`Long`|`2147483648`|  
|数値、小数部|`Double`|`1.2`|  
|二重引用符で囲む。|`String`|`"A"`|  
|シャープ記号で囲まれた|`Date`|`#5/17/1993 9:32 AM#`|  

### <a name="forced-literal-types"></a>強制されるリテラル型

Visual Basic には、リテラルの*型文字*のセットが用意されています。これを使用すると、リテラルが、フォームが示す以外のデータ型を強制的に想定することができます。 これを行うには、リテラルの末尾に文字を追加します。 使用できるリテラル型文字と使用例を次の表に示します。
  
|リテラルの型文字|データ型|例|  
|----------------------------|---------------|-------------|  
|`S`|`Short`|`I = 347S`|
|`I`|`Integer`|`J = 347I`|
|`L`|`Long`|`K = 347L`|
|`D`|`Decimal`|`X = 347D`|
|`F`|`Single`|`Y = 347F`|
|`R`|`Double`|`Z = 347R`|
|`US`|`UShort`|`L = 347US`|
|`UI`|`UInteger`|`M = 347UI`|
|`UL`|`ULong`|`N = 347UL`|
|`C`|`Char`|`Q = "."C`|

`Boolean`、`Byte`、`Date`、`Object`、`SByte`、または `String` のデータ型、または配列や構造体などの複合データ型のリテラル型文字は存在しません。

リテラルでは、変数、定数、および式と同様に、識別子の型文字 (`%`、`&`、`@`、`!`、`#`、`$`) を使用することもできます。 ただし、リテラルの型文字 (`S`、`I`、`L`、`D`、`F`、`R`、`C`) は、リテラルでのみ使用できます。

いずれの場合も、リテラルの型文字はリテラル値の直後にある必要があります。

## <a name="hexadecimal-binary-and-octal-literals"></a>16進数、バイナリ、および8進数のリテラル

通常、コンパイラは、整数リテラルを10進 (基数 10) の数値システムに配置します。 また、整数リテラルを16進数 (base 16) の数値として定義することもできます。これには、`&H` プレフィックス、`&B` プレフィックスを持つバイナリ (基数 2) 番号、および `&O` プレフィックスを持つ8進数 (基本 8) 番号を使用します。 プレフィックスの後の数字は、数値システムに適している必要があります。 次の表にこれを示します。  
  
|数値ベース|［プレフィックス］|有効な桁の値|例|
|-----------------|------------|------------------------|-------------|
|16 進数 (基数 16)。|`&H`|0-9 と A-F|`&HFFFF`|
|Binary (基数 2)|`&B`|0-1|`&B01111100`|
|8 進数 (基数 8)。|`&O`|0-7|`&O77`|

Visual Basic 2017 以降では、アンダースコア文字 (`_`) をグループ区切り記号として使用して、整数リテラルの読みやすさを向上させることができます。 次の例では、`_` 文字を使用して、バイナリリテラルを8ビットグループにグループ化します。

```vb
Dim number As Integer = &B00100010_11000101_11001111_11001101
```

プレフィックス付きリテラルの後にリテラル文字を使用できます。 この例を次に示します。

```vb
Dim counter As Short = &H8000S
Dim flags As UShort = &H8000US
```

前の例では、`counter` の小数点以下の値は-32768 で、`flags` の10進値は + 32768 です。

Visual Basic 15.5 以降では、アンダースコア文字 (`_`) をプレフィックスと16進数、バイナリ、または8進数の間の先頭の区切り記号として使用することもできます。 例 :

```vb
Dim number As Integer = &H_C305_F860
```

[!INCLUDE [supporting-underscores](../../../../../includes/vb-separator-langversion.md)]

## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [基本データ型](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [Visual Basic における型変換](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [データの種類](../../../../visual-basic/language-reference/data-types/index.md)
