---
title: 省略可能なパラメーター
ms.date: 07/20/2015
helpviewer_keywords:
- parameters [Visual Basic], optional
- Visual Basic code, procedures
- procedures [Visual Basic], optional arguments
- optional arguments
- named arguments [Visual Basic], and optional arguments
- optional parameters
- Optional keyword [Visual Basic], optional arguments
- arguments [Visual Basic], optional
- optional arguments [Visual Basic], and named arguments
ms.assetid: 398d2845-1069-4e94-b934-a73b545c8b87
ms.openlocfilehash: d859f7eaaefa051cfdf703d8589bc8c679a3ee85
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345964"
---
# <a name="optional-parameters-visual-basic"></a>省略可能なパラメーター (Visual Basic)
プロシージャのパラメーターを省略可能にすると、呼び出し時に引数を指定する必要がなくなります。 *省略可能なパラメーター*は、プロシージャの定義で `Optional` キーワードによって示されます。 次の規則が適用されます。  
  
- プロシージャ定義のすべての省略可能なパラメーターについて、既定値を指定する必要があります。  
  
- 省略可能なパラメーターの既定値には、定数式を指定する必要があります。  
  
- プロシージャ定義で省略可能なパラメーターの後に続くパラメーターは、すべて省略可能であることが必要です。  
  
 次の構文は、省略可能なパラメーターを含むプロシージャ宣言を示しています。  
  
```vb  
Sub name(ByVal parameter1 As datatype1, Optional ByVal parameter2 As datatype2 = defaultvalue)  
```  
  
## <a name="calling-procedures-with-optional-parameters"></a>省略可能なパラメーターを使ったプロシージャ呼び出し  
 省略可能なパラメーターを使ってプロシージャを呼び出すときには、引数を指定するかどうかを選択できます。 引数を指定しない場合は、そのパラメーターに対して宣言されている既定値が使用されます。  
  
 引数リストで省略可能な引数を省略する場合は、コンマを続けて、省略する引数の位置を表します。 次の例では、1 番目と 4 番目の引数は指定されていますが、2 番目と 3 番目の引数は省略されています。  
  
```vb  
Sub name(argument 1, , , argument 4)  
```  
  
 次の例では、`MsgBox` 関数を数回呼び出します。 `MsgBox` には、必須パラメーターと2つの省略可能なパラメーターがあります。  
  
 `MsgBox` の最初の呼び出しでは、`MsgBox` で定義された順番で 3 つの引数を指定します。 2 番目の呼び出しでは、必須の引数だけを指定します。 3 番目と 4 番目の呼び出しでは、1 つ目と 3 つ目の引数を指定します。 3 番目の呼び出しでは引数を位置で指定し、4 番目の呼び出しでは引数を名前で指定します。  
  
 [!code-vb[VbVbcnProcedures#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#47)]  
  
## <a name="determining-whether-an-optional-argument-is-present"></a>省略可能な引数があるかどうかの確認  
 引数が省略されているのか、呼び出し元のコードで既定値が明示的に指定されているのかについて、プロシージャで実行時に検出することはできません。 この区別が必要な場合は、ありそうにない値を既定値に設定します。 次の手順では、省略可能なパラメーター `office`を定義し、その既定値 `QJZ`をテストして、呼び出しで省略されているかどうかを確認します。  
  
 [!code-vb[VbVbcnProcedures#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#46)]  
  
 省略可能なパラメーターが `String` などの参照型の場合は、`Nothing` を既定値として使用できます。ただし、{3}{4} が引数の値として使用されることが予想される場合を除きます。  
  
## <a name="optional-parameters-and-overloading"></a>省略可能なパラメーターとオーバーロード  
 省略可能なパラメーターを持つプロシージャを定義するには、オーバーロードを使用する方法もあります。 省略可能なパラメーターが 1 つあるとすると、パラメーターを受け取る場合と受け取らない場合の、2 つのオーバーロードされたバージョンのプロシージャを定義できます。 この方法は、省略可能なパラメーターの数が増えるにつれて複雑になります。 しかし、それぞれの省略可能な引数が呼び出しプログラムによって指定されているかどうかを確実に把握できるという利点があります。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [位置と名前による引数渡し](./passing-arguments-by-position-and-by-name.md)
- [パラメーター配列](./parameter-arrays.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [Optional](../../../../visual-basic/language-reference/modifiers/optional.md)
- [ParamArray](../../../../visual-basic/language-reference/modifiers/paramarray.md)
