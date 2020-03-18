---
title: デリゲートの使用 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [C#], how to use
ms.assetid: 99a2fc27-a32e-4a34-921c-e65497520eec
ms.openlocfilehash: dcc73aba738d6296a44c48aad8b66cd6fc7f4a7b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77448440"
---
# <a name="using-delegates-c-programming-guide"></a>デリゲートの使用 (C# プログラミング ガイド)

[デリゲート](../../language-reference/builtin-types/reference-types.md)は、C および C++ の関数ポインターのようなメソッドを安全にカプセル化する型です。 ただし、C 関数ポインターとは異なり、デリゲートはオブジェクト指向で、タイプ セーフで、安全です。 デリゲートの型は、デリゲートの名前によって定義されます。 次の例では、引数として`Del`文字列[を受け取り、](../../language-reference/builtin-types/reference-types.md)void[ を返すメソッドをカプセル化できる ](../../language-reference/builtin-types/void.md) という名前のデリゲートを宣言しています。

[!code-csharp[csProgGuideDelegates#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#21)]

デリゲート オブジェクトは、通常、デリゲートがラップするメソッドの名前を指定して構成されるか、[匿名関数](../statements-expressions-operators/anonymous-functions.md)を使用して構成されます。 デリゲートがインスタンス化されると、デリゲートに対するメソッドの呼び出しが、デリゲートからメソッドに渡されます。 呼び出し元によってデリゲートに渡されるパラメーターはメソッドに渡され、戻り値がある場合は、デリゲートによってメソッドから呼び出し元に返されます。 これは、デリゲートの呼び出しと呼ばれます。 インスタンス化されたデリゲートは、ラップされたメソッドそのものであるかのように呼び出すことができます。 次に例を示します。

[!code-csharp[csProgGuideDelegates#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#22)]  

[!code-csharp[csProgGuideDelegates#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#23)]

デリゲート型は、.NET Framework の <xref:System.Delegate> クラスから派生しています。 デリゲート型は [sealed](../../language-reference/keywords/sealed.md) (派生できません) であり、<xref:System.Delegate> からカスタム クラスを派生することはできません。 インスタンス化されたデリゲートはオブジェクトであるため、パラメーターとして渡したり、プロパティに割り当てたりすることができます。 これにより、メソッドは、パラメーターとしてデリゲートを受け入れ、後でデリゲートを呼び出すことができます。 これは非同期のコールバックと呼ばれ、長いプロセスの完了時に呼び出し元に通知する一般的な方法です。 デリゲートをこの方法で使用する場合、デリゲートを使用するコードは、使用されるメソッドの実装について認識している必要はありません。 機能は、カプセル化インターフェイスが提供する機能に似ています。

コールバックの別の一般的な使用方法は、カスタム比較メソッドの定義およびそのデリゲートの並べ替えメソッドへの引き渡しです。 これにより、呼び出し元のコードを並べ替えアルゴリズムの一部にすることができます。 次の例のメソッドは `Del` 型をパラメーターとして使用しています。

[!code-csharp[csProgGuideDelegates#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#24)]

上記で作成したデリゲートをメソッドに渡すことができます。

[!code-csharp[csProgGuideDelegates#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#25)]

次の出力がコンソールに表示されます。

```console
The number is: 3
```

抽象化としてデリゲートを使用する場合、`MethodWithCallback` がコンソールを直接呼び出す必要はありません。コンソールに留意して設計する必要はありません。 `MethodWithCallback` は文字列を準備し、文字列を別のメソッドに渡すだけです。 これは、デリゲート メソッドが任意の数のパラメーターを使用できるため、特に強力です。

インスタンス メソッドをラップするようにデリゲートが構築された場合、デリゲートはインスタンスとメソッドの両方を参照します。 デリゲートはラップするメソッド以外のインスタンス型を認識しないため、デリゲート シグネチャと一致するオブジェクトにメソッドがある限り、どの型のオブジェクトでも参照できます。 静的メソッドをラップするようにデリゲートが構築された場合、デリゲートはそのメソッドのみを参照します。 次に宣言の例を示します。

[!code-csharp[csProgGuideDelegates#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#26)]

以前に示した静的な `DelegateMethod` と共に、`Del` インスタンスによってラップできるメソッドが 3 つあります。

デリゲートは、呼び出されたときに複数のメソッドを呼び出すことができます。 これはマルチキャスティングと呼ばれます。 デリゲートのメソッドの一覧 (呼び出しリスト) に追加のメソッドを追加するには、加算演算子または加算代入演算子 ('+' または '+=') を使用して 2 つのデリゲートを追加する必要があります。 次に例を示します。

[!code-csharp[csProgGuideDelegates#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#27)]

この時点で、`allMethodsDelegate` には、呼び出しリスト内の 3 つのメソッド (`Method1`、`Method2`、`DelegateMethod`) が含まれています。 元の 3 つのデリゲートである `d1`、`d2`、および `d3` は変更されません。 `allMethodsDelegate` が呼び出されると、すべての 3 つのメソッドが順に呼び出されます。 デリゲートで参照パラメーターを使用する場合、参照は、3 つのメソッドに順番に渡され、1 つのメソッドによって行われた変更は、次のメソッドに示されます。 いずれかのメソッドがメソッド内でキャッチされない例外をスローした場合、デリゲートの呼び出し元に例外が渡され、呼び出しリスト内の後続のメソッドは呼び出されません。 デリゲートに戻り値や out パラメーターがある場合、デリゲートは戻り値と最後に呼び出されたメソッドのパラメーターを返します。 呼び出しリストからメソッドを削除するには、[減算演算子または減算代入演算子](../../language-reference/operators/subtraction-operator.md) (`-` または `-=`) を使います。 次に例を示します。

[!code-csharp[csProgGuideDelegates#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#28)]

デリゲート型が `System.Delegate` から派生しているため、そのクラスで定義されるメソッドとプロパティはデリゲートで呼び出すことができます。 たとえば、デリゲートの呼び出しリスト内のメソッドの数を検索するには、次のように記述します。

[!code-csharp[csProgGuideDelegates#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#29)]

呼び出しリストに複数のメソッドがあるデリゲートは、<xref:System.MulticastDelegate> のサブクラスである `System.Delegate` から派生します。 上記のコードでは、両方のクラスが `GetInvocationList` をサポートしているため、いずれの場合も機能します。

マルチキャスト デリゲートは、イベント処理で広く使用されます。 イベント ソース オブジェクトはイベントを受け取るように登録されている受信者オブジェクトにイベント通知を送信します。 イベントを登録するには、受信者は、イベントを処理するようにデザインされたメソッドを作成し、そのメソッドのデリゲートを作成してイベント ソースにデリゲートを渡します。 イベントが発生すると、ソースがデリゲートを呼び出します。 デリゲートは、受信者のイベント処理メソッドを呼び出し、イベント データを配信します。 特定のイベントのデリゲート型は、イベント ソースによって定義されます。 詳細については、「[イベント (C# プログラミング ガイド)](../events/index.md)」を参照してください。

コンパイル時に割り当てられた 2 つの異なる型のデリゲートを比較すると、コンパイル エラーが発生します。 デリゲート インスタンスが静的な `System.Delegate` 型の場合は、比較できますが、実行時に false が返されます。 次に例を示します。

[!code-csharp[csProgGuideDelegates#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#30)]

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [デリゲート](./index.md)
- [デリゲートの変性の使用](../concepts/covariance-contravariance/using-variance-in-delegates.md)
- [デリゲートの変性](../concepts/covariance-contravariance/variance-in-delegates.md)
- [Func および Action 汎用デリゲートでの変性の使用](../concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)
- [イベント](../events/index.md)
