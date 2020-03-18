---
title: 例外処理 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- exception handling [C#], about exception handling
- exceptions [C#], handling
ms.assetid: b4e4ecf2-b907-4e58-891f-2563762258e9
ms.openlocfilehash: ee1e5bd15183dad9ffe97824f9b194668e9d3b17
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75705302"
---
# <a name="exception-handling-c-programming-guide"></a>例外処理 (C# プログラミング ガイド)
[try](../../language-reference/keywords/try-catch.md) ブロックは、例外の影響を受ける可能性があるコードを区分化するために、 C# プログラマによって使用されます。 関連付けられた [catch](../../language-reference/keywords/try-catch.md) ブロックは、スローされた例外を処理するために使用されます。 [finally](../../language-reference/keywords/try-finally.md) ブロックには、`try` ブロックで例外がスローされたかどうかにかかわらず実行されるコードが記述されます (`try` ブロックに割り当てられたリソースの解放など)。 `try` ブロックには、1 つ以上の `catch` ブロック、`finally` ブロック、またはその両方が関連付けられる必要があります。  
  
 次のコードは、`try-catch` ステートメント、`try-finally` ステートメント、および `try-catch-finally` ステートメントの例です。  
  
 [!code-csharp[csProgGuideExceptions#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#6)]  
  
 [!code-csharp[csProgGuideExceptions#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#7)]  
  
 [!code-csharp[csProgGuideExceptions#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#8)]  
  
 `try` ブロックに `catch` または `finally` ブロックがない場合は、コンパイル エラーが発生します。  
  
## <a name="catch-blocks"></a>catch ブロック  
 `catch` ブロックでは、キャッチする例外の種類を指定できます。 この型指定は、*例外フィルター*と呼ばれます。 例外の種類は、<xref:System.Exception> から派生している必要があります。 一般に、<xref:System.Exception> ブロックでスローされる可能性があるすべての例外の処理方法を把握している場合や、`try` ブロックの末尾に [throw](../../language-reference/keywords/throw.md) ステートメントを記述している場合を除いては、例外フィルターとして `catch` を指定することは避けてください。  
  
 複数の `catch` ブロックに異なる例外フィルターが含まれている場合は、それらを連結することができます。 `catch` ブロックはコード内で上から下に評価されますが、スローされた各例外に対して実行される `catch` ブロックは 1 つだけです。 スローされた例外の厳密な型か、その基底クラスを指定する最初の `catch` ブロックが実行されます。 一致する例外フィルターを指定した `catch` ブロックがない場合は、フィルターのない `catch` ブロックが選択されます (それがステートメント内に存在する場合)。 最初に配置する `catch` ブロックでは、最も具体的な (つまり、最終派生の) 例外の種類を指定することが重要です。  
  
 次の条件に該当する場合は、例外をキャッチする必要があります。  
  
- 例外がスローされる理由を十分に理解していて、かつ特定の回復手段を実装できる (<xref:System.IO.FileNotFoundException> オブジェクトをキャッチした場合に、ユーザーに新しいファイル名を入力するよう求めるなど)。  
  
- より具体的な例外を新規に作成し、スローできる。  
  
     [!code-csharp[csProgGuideExceptions#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#9)]  
  
- 例外を追加処理に渡す前に、その例外を部分的に処理する必要がある。 次の例では、例外を再スローする前に、エラー ログにエントリを追加する目的で `catch` ブロックが使用されています。  
  
     [!code-csharp[csProgGuideExceptions#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#10)]  
  
## <a name="finally-blocks"></a>Finally ブロック  
 `finally` ブロックでは、`try` ブロックで実行されるアクションをクリーンアップすることができます。 `finally` ブロック (存在する場合) は、最後 (`try` ブロックおよび一致する `catch` ブロックの後) に実行されます。 `finally` ブロックは、例外がスローされたかどうかや、例外の種類に一致する `catch` ブロックが見つかったかどうかにかかわらず、常に実行されます。  
  
 `finally` ブロックは、ランタイム内のガベージ コレクターによってオブジェクトがファイナライズされるのを待つことなく、ファイル ストリーム、データベース接続、グラフィックス ハンドルなどのリソースを解放するために使用できます。 詳細については、「[using ステートメント](../../language-reference/keywords/using-statement.md)」を参照してください。  
  
 次の例では、`finally` ブロックで開かれたファイルを閉じるために `try` ブロックが使用されています。 ファイルを閉じる前に、ファイル ハンドルの状態が確認されています。 `try` ブロックがファイルを開けなかった場合は、ファイル ハンドルの値が `null` のままになり、`finally` ブロックはファイルを閉じようとしません。 `try` ブロックでファイルが正常に開かれた場合は、開かれたファイルを `finally` ブロックが閉じます。  
  
 [!code-csharp[csProgGuideExceptions#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#11)]  
  
## <a name="c-language-specification"></a>C# 言語仕様  

詳細については、「[C# 言語仕様](~/_csharplang/spec/exceptions.md)」の[例外](~/_csharplang/spec/statements.md#the-try-statement)と [try ステートメント](/dotnet/csharp/language-reference/language-specification/introduction)に関するセクションを参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。
  
## <a name="see-also"></a>参照

- [C# リファレンス](../../language-reference/index.md)
- [C# プログラミングガイド](../index.md)
- [例外と例外処理](./index.md)
- [try-catch](../../language-reference/keywords/try-catch.md)
- [try-finally](../../language-reference/keywords/try-finally.md)
- [try-catch-finally](../../language-reference/keywords/try-catch-finally.md)
- [using ステートメント](../../language-reference/keywords/using-statement.md)
