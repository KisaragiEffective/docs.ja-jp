---
title: Throw ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Throw
helpviewer_keywords:
- structured exception handling, throw statements
- try-catch exception handling, throw statements
- throw statement [Visual Basic], about throw statements
- throwing exceptions, throw statement
- exception handling, throw statement
- On Error statement [Visual Basic], throwing exceptions
- throwing exceptions
- exception handling, unstructured
- throw statement [Visual Basic]
ms.assetid: a6e07406-5c8a-4498-87a2-8339f3651d62
ms.openlocfilehash: 147345990b625e034e651e69b322bc098d0bd8de
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352793"
---
# <a name="throw-statement-visual-basic"></a>Throw ステートメント (Visual Basic)

プロシージャ内で例外をスローします。

## <a name="syntax"></a>構文

```vb
Throw [ expression ]
```

## <a name="part"></a>要素

`expression`\
スローされる例外に関する情報を提供します。 `Catch` ステートメント内に存在する場合は省略可能です。それ以外の場合は必須です。

## <a name="remarks"></a>コメント

`Throw` ステートメントは、構造化例外処理コード (`Try`...`Catch`...`Finally`) または非構造化例外処理コード (`On Error GoTo`) で処理できる例外をスローします。 `Throw` ステートメントを使用してコード内のエラーをトラップすることができます。これは、Visual Basic によって、適切な例外処理コードが見つかるまで呼び出し履歴が上に移動されるためです。

式が指定されていない `Throw` ステートメントは、`Catch` ステートメントでのみ使用できます。この場合、ステートメントは、現在 `Catch` ステートメントによって処理されている例外を再スローします。

`Throw` ステートメントは、`expression` 例外の呼び出し履歴をリセットします。 `expression` が指定されていない場合、呼び出し履歴は変更されません。 <xref:System.Exception.StackTrace%2A> プロパティを使用して、例外の呼び出し履歴にアクセスできます。

## <a name="example"></a>例

次のコードでは、`Throw` ステートメントを使用して例外をスローします。

[!code-vb[VbVbalrStatements#84](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#84)]

## <a name="see-also"></a>参照

- [Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)
- [On Error ステートメント](../../../visual-basic/language-reference/statements/on-error-statement.md)
