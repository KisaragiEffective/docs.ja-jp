---
title: 例外の使用 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- exception handling [C#], about exception handling
- exceptions [C#], about exceptions
ms.assetid: 71472c62-320a-470a-97d2-67995180389d
ms.openlocfilehash: 4012027dc1a9bd2543d0a4195360e5f7e0586fe1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75705263"
---
# <a name="using-exceptions-c-programming-guide"></a>例外の使用 (C# プログラミング ガイド)
C# では、例外と呼ばれるメカニズムを使用して、プログラムの実行時に発生したエラーがプログラムに伝えられます。 例外は、エラーが発生したコードによってスローされ、エラーを修正できるコードによってキャッチされます。 例外をスローできるのは、.NET Framework 共通言語ランタイム (CLR) か、プログラム内のコードです。 スローされた例外は、例外の `catch` ステートメントが見つかるまで呼び出し履歴をさかのぼります。 キャッチされない例外は、システムが提供する汎用の例外ハンドラーによって処理されます。このとき、ダイアログ ボックスが表示されます。  
  
 例外は、<xref:System.Exception> から派生したクラスによって表されます。 このクラスは例外の型を識別し、例外に関する詳細情報が含まれたプロパティを保持します。 例外をスローするには、例外の派生クラスのインスタンスを作成し、必要に応じて例外のプロパティを設定してから、`throw` キーワードを使用してオブジェクトをスローする必要があります。 次に例を示します。  
  
 [!code-csharp[csProgGuideExceptions#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#1)]  
  
 例外がスローされると、ランタイムは、現在のステートメントが `try` ブロック内に存在するかどうかを確認します。 存在する場合は、`catch` ブロックに関連付けられている `try` ブロックをチェックして、例外をキャッチできるかどうかを確認します。 通常は、この `Catch` ブロックによって例外の型が指定されます。`catch` ブロックの型が、例外または例外の基底クラスの型と一致する場合、`catch` ブロックはメソッドを処理できます。 次に例を示します。  
  
 [!code-csharp[csProgGuideExceptions#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#2)]  
  
 例外をスローするステートメントが `try` ブロックにない場合、または `try` ブロックにそのステートメントが含まれていても、対応する `catch` ブロックが存在しない場合は、ランタイムが呼び出し側のメソッドで `try` ステートメントと `catch` ブロックを探します。 続けて、呼び出し履歴で、対応する `catch` ブロックを探します。 `catch` ブロックが見つかり実行されると、その `catch` ブロックの後にある次のステートメントに制御が渡されます。  
  
 `try` ステートメントには、複数の `catch` ブロックを含めることができます。 例外を処理できる最初の `catch` ステートメントが実行され、その後の `catch` ステートメントは、対応していても無視されます。 そのため、catch ブロックは必ず特殊性が高いもの (または最も派生されたもの) から順に配置する必要があります。 次に例を示します。  
  
 [!code-csharp[csProgGuideExceptions#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#3)]  
  
 `catch` ブロックが実行される前に、ランタイムにより `finally` ブロックがチェックされます。 `Finally` ブロックを使用すると、中止された `try` ブロックによって残ることがある、あいまいな状態をクリーンアップできます。また、ランタイムのガベージ コレクターがオブジェクトを終了させるのを待たずに外部リソース (グラフィック ハンドル、データベース接続、ファイル ストリームなど) を解放することもできます。 次に例を示します。  
  
 [!code-csharp[csProgGuideExceptions#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#4)]  
  
 `WriteByte()` が例外をスローした場合は、`try` を呼び出さないと、ファイルを再度開こうとする 2 番目の `file.Close()` ブロックのコードが失敗し、ファイルはロックされたままになります。 例外がスローされても `finally` ブロックは実行されるため、上の例の `finally` ブロックではファイルを適切に閉じて、エラーを回避できます。  
  
 例外がスローされた後、対応する `catch` ブロックが呼び出し履歴に見つからない場合は、次のいずれかが発生します。  
  
- 例外がファイナライザーの内部で発生した場合、ファイナライザーは中止され、基本ファイナライザー (存在する場合) が呼び出されます。  
  
- 呼び出し履歴に静的コンストラクターまたは静的フィールド初期化子が含まれている場合は、<xref:System.TypeInitializationException> がスローされ、新しい例外の <xref:System.Exception.InnerException%2A> プロパティに元の例外が割り当てられます。  
  
- スレッドの開始位置に到達すると、スレッドは終了します。  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [例外と例外処理](./index.md)
