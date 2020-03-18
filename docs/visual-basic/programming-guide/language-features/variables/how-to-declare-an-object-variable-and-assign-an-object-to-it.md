---
title: '方法: オブジェクト変数を宣言し、それにオブジェクトを割り当てる'
ms.date: 07/20/2015
helpviewer_keywords:
- object variables [Visual Basic], declaring
- declaring object variables [Visual Basic]
ms.assetid: 2fa77dde-1fb2-439a-80d4-3e9787649fad
ms.openlocfilehash: eaaeda2a986584e6e1a2e0d2cda3890fb6187598
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75344232"
---
# <a name="how-to-declare-an-object-variable-and-assign-an-object-to-it-in-visual-basic"></a>方法 : Visual Basic でオブジェクト変数を宣言し、オブジェクト変数にオブジェクトを代入する

[オブジェクトデータ型](../../../../visual-basic/language-reference/data-types/object-data-type.md)の変数を宣言するには、 [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)で `As Object` を指定します。 このような変数にオブジェクトを代入するには、代入ステートメントまたは初期化句で等号 (`=`) の後にオブジェクトを配置します。

## <a name="example"></a>使用例

次の例では、`Object` 変数を宣言し、現在のインスタンスをその変数に代入します。

```vb
Dim thisObject As Object
thisObject = "This is an Object"
```

宣言の一部として変数を初期化することで、宣言と代入を組み合わせることができます。 次の例は、前の例と同じです。

```vb
Dim thisObject As Object= "This is an Object"
```

## <a name="compile-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- <xref:System> 名前空間への参照

- `Dim` ステートメントを格納するクラス、構造体、またはモジュール。

- 代入ステートメントを配置するプロシージャ。

## <a name="see-also"></a>関連項目

- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [オブジェクト変数の宣言](../../../../visual-basic/programming-guide/language-features/variables/object-variable-declaration.md)
- [Object Data Type](../../../../visual-basic/language-reference/data-types/object-data-type.md)
- [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)
