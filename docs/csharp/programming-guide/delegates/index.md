---
title: デリゲート - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, delegates
- delegates [C#]
ms.assetid: 97de039b-c76b-4b9c-a27d-8c1e1c8d93da
ms.openlocfilehash: c0f28716926d4c9d74cde58fd00e27d1fdfa7ce1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75705367"
---
# <a name="delegates-c-programming-guide"></a>デリゲート (C# プログラミング ガイド)
[デリゲート](../../language-reference/builtin-types/reference-types.md)は、特定のパラメーター リストおよび戻り値の型を使用して、メソッドへの参照を表す型です。 デリゲートをインスタンス化するときは、互換性のあるシグネチャと戻り値の型を持つ任意のメソッドにそのインスタンスを関連付けることができます。 メソッドは、デリゲート インスタンスを使用して起動する (呼び出す) ことができます。  
  
 デリゲートは、他のメソッドへの引数としてメソッドを渡すために使用されます。 イベント ハンドラーは、デリゲートを介して呼び出されるメソッドにすぎません。 カスタム メソッドを作成して、特定のイベントの発生時に、作成したメソッドが Windows コントロールなどのクラスから呼び出されるようにできます。 次の例にデリゲート宣言を示します。  
  
 [!code-csharp[csProgGuideDelegates#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#20)]  
  
 デリゲート型と一致するアクセス可能なクラスまたは構造体のメソッドはすべて、デリゲートに代入できます。 メソッドは、静的メソッドとインスタンス メソッドのいずれかにできます。 このため、メソッド呼び出しをプログラムによって変更でき、また新しいコードを既存のクラスに接続することもできます。  
  
> [!NOTE]
> メソッドのオーバーロードのコンテキストでは、メソッドのシグネチャに戻り値は含まれません。 しかしデリゲートのコンテキストでは、シグネチャに戻り値が含まれます。 つまり、メソッドにはデリゲートと同じ戻り値の型が必要です。  
  
 このようにメソッドをパラメーターとして参照できるため、デリゲートはコールバック メソッドを定義するのに最適です。 たとえば、2 つのオブジェクトを比較するメソッドへの参照は、並べ替えのアルゴリズムへの引数として渡すことができます。 比較を行うコードは独立したプロシージャであるため、並べ替えのアルゴリズムはより一般的な方法で記述できます。  
  
## <a name="delegates-overview"></a>デリゲートの概要  
 デリゲートには、次の特徴があります。  
  
- デリゲートは C++ 関数ポインターと似ていますが、デリゲートは完全なオブジェクト指向です。また、メンバー関数への C++ ポインターとは異なり、デリゲートではオブジェクト インスタンスとメソッドの両方をカプセル化します。
  
- デリゲートを使用すると、メソッドをパラメーターとして渡すことができます。  
  
- デリゲートは、コールバック メソッドを定義するのに使用できます。  
  
- デリゲートは連結でき、たとえば、複数のメソッドを 1 つのイベントで呼び出すことができます。  
  
- メソッドは、デリゲート型に正確に一致する必要がありません。 詳細については、「[デリゲートの分散の使用](../concepts/covariance-contravariance/using-variance-in-delegates.md)」を参照してください。  
  
- C# バージョン 2.0 では[匿名メソッド](../../language-reference/operators/delegate-operator.md)の概念が導入されました。これを使用すると、別個に定義されたメソッドの代わりに、コード ブロックをパラメーターとして渡すことができます。 C# 3.0 ではラムダ式が導入され、インライン コード ブロックをより簡潔に記述できるようになりました。 匿名メソッドと (特定のコンテキストにおける) ラムダ式はどちらも、デリゲート型にコンパイルされます。 これらの機能は総称して、匿名関数と呼ばれるようになりました。 ラムダ式について詳しくは、「[ラムダ式](../statements-expressions-operators/lambda-expressions.md)」をご覧ください。
  
## <a name="in-this-section"></a>このセクションの内容  
  
- [デリゲートの使用](./using-delegates.md)  
  
- [インターフェイス (c# プログラミング ガイド) ではなくデリゲートを使用する場合](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ms173173(v=vs.100))  
  
- [名前付きメソッドを使用したデリゲートと匿名メソッドを使用したデリゲート](./delegates-with-named-vs-anonymous-methods.md)  
  
- [デリゲートの変性の使用](../concepts/covariance-contravariance/using-variance-in-delegates.md)  
  
- [デリゲートを結合する方法 (マルチキャスト デリゲート)](./how-to-combine-delegates-multicast-delegates.md)  
  
- [デリゲートを宣言し、インスタンス化して、使用する方法](./how-to-declare-instantiate-and-use-a-delegate.md)

## <a name="c-language-specification"></a>C# 言語仕様  

詳細については、「[C# 言語の仕様](/dotnet/csharp/language-reference/language-specification/introduction)」の「[デリゲート](~/_csharplang/spec/delegates.md)」を参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。
  
## <a name="featured-book-chapters"></a>参考書籍の該当する章  
 「[Delegates, Events, and Lambda Expressions (デリゲート、イベント、およびラムダ式)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518994%28v=orm.10%29)」(『[C# 3.0 Cookbook, Third Edition: More than 250 solutions for C# 3.0 programmers (C# 3.0 クックブック (第 3 版): C# 3.0 プログラマ向けの 250 以上のソリューション)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518995%28v=orm.10%29)』)  
  
 「[デリゲートとイベント](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652490%28v=orm.10%29)」(『[Learning C# 3.0: Master the fundamentals of C# 3.0 (C# 3.0 の学習: C# 3.0 の基礎を習得)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652493%28v=orm.10%29)』)  
  
## <a name="see-also"></a>関連項目

- <xref:System.Delegate>
- [C# プログラミング ガイド](../index.md)
- [イベント](../events/index.md)
