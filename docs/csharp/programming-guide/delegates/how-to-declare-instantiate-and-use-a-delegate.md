---
title: デリゲートを宣言し、インスタンス化して使用する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [C#], declaring and instantiating
ms.assetid: 61c4895f-f785-48f8-8bfe-db73b411c4ae
ms.openlocfilehash: 7ac1d736e19c4dcf1c8408db944505c399762778
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712365"
---
# <a name="how-to-declare-instantiate-and-use-a-delegate-c-programming-guide"></a>デリゲートを宣言し、インスタンス化して使用する方法 (C# プログラミング ガイド)
C# 1.0 以降では、次の例に示すようにデリゲートを宣言できます。  
  
 [!code-csharp[csProgGuideDelegates#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#13)]  
  
 [!code-csharp[csProgGuideDelegates#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#14)]  
  
 C# 2.0 では、次の例に示すように、上記の宣言をより簡単に記述できます。  
  
 [!code-csharp[csProgGuideDelegates#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#32)]  
  
 C# 2.0 以降では、次の例に示すように、匿名メソッドを使用して[デリゲート](../../language-reference/builtin-types/reference-types.md)の宣言と初期化を行うこともできます。  
  
 [!code-csharp[csProgGuideDelegates#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#15)]  
  
 C# 3.0 以降では、次の例に示すように、ラムダ式を使用してデリゲートの宣言とインスタンス化を行うこともできます。  
  
 [!code-csharp[csProgGuideDelegates#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#31)]  
  
 詳しくは、「[ラムダ式](../statements-expressions-operators/lambda-expressions.md)」をご覧ください。  
  
 次の例で、デリゲートの宣言、インスタンス化、および使用の方法について説明します。 `BookDB` クラスにより、書籍のデータベースを保持する書店データベースをカプセル化します。 これは、データベース内にあるすべてのペーパーバックを検索し、各ペーパーバックに対してデリゲートを呼び出す `ProcessPaperbackBooks` メソッドを公開します。 使用する `delegate` 型には `ProcessBookDelegate` という名前を付けます。 `Test` クラスでは、このクラスを使用してペーパーバックのタイトルと平均価格を出力します。  
  
 デリゲートを使用することで、書店データベースとクライアント コードの機能を適切に分離しやすくなります。 クライアント コードからは、書籍の格納方法や書店コードでのペーパーバックの検索方法はわかりません。 書店コードからは、検索後にペーパーバックに対して行われる処理はわかりません。  
  
## <a name="example"></a>例  
 [!code-csharp[csProgGuideDelegates#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#12)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
  
- デリゲートを宣言する。  
  
     次のステートメントで、新しいデリゲート型を宣言します。  
  
     [!code-csharp[csProgGuideDelegates#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#16)]  
  
     各デリゲート型は、引数の数と型、およびデリゲートでカプセル化可能なメソッドの戻り値の型を示します。 引数型または戻り値型のセットが新しく必要になった場合は、常に新しいデリゲート型を宣言する必要があります。  
  
- デリゲートをインスタンス化する。  
  
     デリゲート型の宣言後、デリゲート オブジェクトを作成して特定のメソッドに関連付ける必要があります。 先の例では、次の例に示すように `PrintTitle` メソッドを `ProcessPaperbackBooks` メソッドに渡すことでこの操作を行っています。  
  
     [!code-csharp[csProgGuideDelegates#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#17)]  
  
     これにより、[静的](../../language-reference/keywords/static.md) メソッド `Test.PrintTitle` に関連付けられた新しいデリゲート オブジェクトが作成されます。 同様に、次の例で示すようにオブジェクト `AddBookToTotal` の非静的メソッド `totaller` も渡しています。  
  
     [!code-csharp[csProgGuideDelegates#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#18)]  
  
     どちらの場合でも、新しいデリゲート オブジェクトが `ProcessPaperbackBooks` メソッドに渡されます。  
  
     デリゲート オブジェクトは不変であるため、関連付けられているメソッドを、デリゲートの作成後に変更することはできません。  
  
- デリゲートを呼び出す。  
  
     デリゲート オブジェクトを作成したら、通常は、そのデリゲートを呼び出す別のコードにデリゲート オブジェクトを渡します。 デリゲート オブジェクトを呼び出すときには、デリゲートに渡す引数を、デリゲート オブジェクト名の後ろにかっこで囲んで付けます。 デリゲートの呼び出しの例を次に示します。  
  
     [!code-csharp[csProgGuideDelegates#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#19)]  
  
     デリゲートは、この例に示すように同期的に呼び出すことも、`BeginInvoke` メソッドおよび `EndInvoke` メソッドを使用して非同期的に呼び出すこともできます。  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [イベント](../events/index.md)
- [デリゲート](./index.md)
