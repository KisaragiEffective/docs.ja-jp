---
title: try-catch を使用して例外を処理する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- exception handling [C#], try/catch blocks
- exceptions [C#], try/catch blocks
- try/catch blocks [C#]
ms.assetid: ca8e3773-980e-4767-8633-7408540e9818
ms.openlocfilehash: adfc53cbe4fd603ac3a6de6b9a0162320d5a2e19
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712287"
---
# <a name="how-to-handle-an-exception-using-trycatch-c-programming-guide"></a>try/catch を使用して例外を処理する方法 (C# プログラミング ガイド)
[try-catch](../../language-reference/keywords/try-catch.md) ブロックの目的は、作業コードによって生成された例外をキャッチし、処理することです。 例外によっては、`catch` ブロックで処理し、例外を再スローせずに問題を解決できるものもありますが、多くの場合、適切な例外がスローされるようにする必要があります。  
  
## <a name="example"></a>例  
 この例の <xref:System.IndexOutOfRangeException> は最も適切な例外というわけではありません。呼び出し元から <xref:System.ArgumentOutOfRangeException> 引数が渡されてエラーが発生しているため、より適切なメソッドは `index` になります。  
  
 [!code-csharp[csProgGuideExceptions#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#5)]  
  
## <a name="comments"></a>コメント  
 例外を発生させるコードは `try` ブロックに囲まれています。 `catch` が発生した場合にこれを処理するための `IndexOutOfRangeException` ステートメントが、すぐ後に追加されています。 `catch` ブロックは `IndexOutOfRangeException` を処理し、代わりにより適切な `ArgumentOutOfRangeException` 例外をスローします。 呼び出し元にできるだけ多くの情報を提供するため、元の例外を新しい例外の <xref:System.Exception.InnerException%2A> として指定することを検討してください。 <xref:System.Exception.InnerException%2A> プロパティは [readonly](../../language-reference/keywords/readonly.md) であるため、新しい例外のコンストラクターで割り当てる必要があります。  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [例外と例外処理](./index.md)
- [例外処理](./exception-handling.md)
