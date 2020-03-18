---
title: '方法 : 例外を明示的にスローする'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- exceptions, try/catch blocks
- FileNotFoundException class
- try/catch blocks
- exceptions, throwing
- implicitly throwing exceptions
ms.assetid: 72bdd157-caa9-4478-9ee3-cb4500b84528
ms.openlocfilehash: 750da20b8c1c40901cc363ac0eff8af888821ce9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708863"
---
# <a name="how-to-explicitly-throw-exceptions"></a>例外を明示的にスローする方法

明示的に例外をスローするには、C# の [`throw`](../../csharp/language-reference/keywords/throw.md) または Visual Basic の [`Throw`](../../visual-basic/language-reference/statements/throw-statement.md) ステートメントを使用します。 `throw` ステートメントを使って、キャッチした例外をもう一度スローすることもできます。 再スローされる例外に情報を追加して、デバッグ時により多くの情報を提供するコーディング手法をお勧めします。

次のコード例では、`try`/`catch` ブロックを使用して可能性のある <xref:System.IO.FileNotFoundException> をキャッチします。 次の `try` ブロックは、`catch` をキャッチし、データ ファイルが見つからない場合に、メッセージをコンソールに出力する <xref:System.IO.FileNotFoundException> ブロックです。 次のステートメントは、新しい `throw` をスローして、テキスト情報を例外に追加する <xref:System.IO.FileNotFoundException> ステートメントです。

[!code-csharp[Exception.Throwing#1](~/samples/snippets/csharp/VS_Snippets_CLR/Exception.Throwing/CS/throw.cs#1)]
[!code-vb[Exception.Throwing#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/Exception.Throwing/VB/throw.vb#1)]  

## <a name="see-also"></a>参照

- [例外](index.md)
