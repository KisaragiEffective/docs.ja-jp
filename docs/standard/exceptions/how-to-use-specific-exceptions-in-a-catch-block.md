---
title: '方法 : catch ブロックで特定の例外を使用する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- exceptions, try/catch blocks
- try/catch blocks
- catch blocks
ms.assetid: 12af9ff3-8587-4f31-90cf-6c2244e0fdae
ms.openlocfilehash: 48b450e579263876725f96e0adfc4c16aac1d869
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160158"
---
# <a name="how-to-use-specific-exceptions-in-a-catch-block"></a>catch ブロックで特定の例外を使用する方法

一般的には、基本的な `catch` ステートメントを使用するよりも、特定の種類の例外をキャッチするプログラミング手法を勧めします。

例外が発生すると、スタックに渡され、各 catch ブロックが処理する機会を与えられます。 catch ステートメントの順序が重要です。 一般的な例外 catch ブロックまたはコンパイラがエラーを発行する前に、特定の例外を対象とした catch ブロックを配置します。 適切な catch ブロックは、例外の種類を catch ブロックで指定された例外の名前に一致させることで決まります。 特定の catch ブロックがない場合は、汎用 catch ブロック (ある場合) によってキャッチされます。

次のコード例では、`try`/`catch` ブロックを使用して <xref:System.InvalidCastException> をキャッチします。 サンプルでは、1 つのプロパティ、従業員レベル (`Employee`) を使用して、`Emlevel` と呼ばれるクラスを作成します。 メソッド `PromoteEmployee` は、オブジェクトを受け取って、従業員レベルをインクリメントします。 <xref:System.InvalidCastException> インスタンスが <xref:System.DateTime> メソッドに渡されたとき、`PromoteEmployee` が発生します。

[!code-cpp[CatchException#2](../../../samples/snippets/cpp/VS_Snippets_CLR/CatchException/CPP/catchexception1.cpp#2)]
[!code-csharp[CatchException#2](../../../samples/snippets/csharp/VS_Snippets_CLR/CatchException/CS/catchexception1.cs#2)]
[!code-vb[CatchException#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CatchException/VB/catchexception1.vb#2)]

## <a name="see-also"></a>参照

- [例外](index.md)
