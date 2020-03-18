---
title: Call ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Call
helpviewer_keywords:
- procedures [Visual Basic], Call statement
- Call statement [Visual Basic]
- procedures [Visual Basic], calling
ms.assetid: e5b31571-6867-406f-b8e7-a3f9aae4723a
ms.openlocfilehash: 7de194ea23827e08c49f4519c1000708a4bd91b4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350158"
---
# <a name="call-statement-visual-basic"></a>Call ステートメント (Visual Basic)

`Function`、`Sub`、またはダイナミックリンクライブラリ (DLL) プロシージャに制御を転送します。  
  
## <a name="syntax"></a>構文  
  
```vb  
[ Call ] procedureName [ (argumentList) ]  
```  
  
## <a name="parts"></a>指定項目  

|||
|---|---|
|`procedureName`|必須。 呼び出すプロシージャの名前。|
|`argumentList`|省略可。 プロシージャが呼び出されたときにプロシージャに渡される引数を表す変数または式のリスト。 複数の引数は、コンマで区切ります。 `argumentList`を含める場合は、かっこで囲む必要があります。|
|||
  
## <a name="remarks"></a>コメント

 プロシージャを呼び出すときに、`Call` キーワードを使用できます。 ほとんどのプロシージャ呼び出しでは、このキーワードを使用する必要はありません。

 通常、呼び出された式が識別子で始まらない場合は、`Call` キーワードを使用します。 他の用途には `Call` キーワードを使用しないことをお勧めします。

 プロシージャが値を返す場合、`Call` ステートメントによって値が破棄されます。

## <a name="example"></a>例

 次のコードは、プロシージャを呼び出すために `Call` キーワードが必要な2つの例を示しています。 どちらの例でも、呼び出された式の先頭が識別子ではありません。

 [!code-vb[VbVbalrStatements#97](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#97)]  
  
## <a name="see-also"></a>参照

- [Function ステートメント](function-statement.md)
- [Sub ステートメント](sub-statement.md)
- [Declare ステートメント](declare-statement.md)
- [ラムダ式](../../programming-guide/language-features/procedures/lambda-expressions.md)
