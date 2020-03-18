---
title: dynamic 型の使用 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- dynamic [C#], about dynamic type
- dynamic type [C#]
ms.assetid: 3828989d-c967-4a51-b948-857ebc8fdf26
ms.openlocfilehash: c5ac5b3692266010f0be8672ef744baaa32e6a03
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75711858"
---
# <a name="using-type-dynamic-c-programming-guide"></a>dynamic 型の使用 (C# プログラミング ガイド)

C# 4 では、`dynamic` という新しい型が導入されています。 この型は静的な型ですが、`dynamic` 型のオブジェクトは静的な型チェックをバイパスします。 ほとんどの場合、`object` 型を使用する場合と同様に機能します。 コンパイル時には、`dynamic` として型指定された要素はあらゆる操作をサポートすると見なされます。 したがって、オブジェクトが COM API、IronPython などの動的言語、HTML ドキュメント オブジェクト モデル (DOM)、リフレクション、プログラムの他の場所のいずれから値を取得するのかを考慮する必要はありません。 ただし、コードが無効な場合には、実行時にエラーが検出されます。

たとえば、次のコードの `exampleMethod1` インスタンス メソッドにパラメーターが 1 つしかない場合、`ec.exampleMethod1(10, 4)` メソッドへの最初の呼び出しは引数を 2 つ含むため、コンパイラはこの呼び出しを無効と認識します。 この呼び出しではコンパイラ エラーが発生します。 `dynamic_ec.exampleMethod1(10, 4)` メソッドの 2 番目の呼び出しは、`dynamic_ec` の型が `dynamic` であるため、コンパイラによってチェックされません。 そのため、コンパイラ エラーは報告されません。 ただし、このエラーがいつまでも検出されないということではありません。 実行時に検出されて実行時例外が発生します。

[!code-csharp[CsProgGuideTypes#50](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#50)]

[!code-csharp[CsProgGuideTypes#56](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#56)]

これらの例におけるコンパイラの役割は、`dynamic` として型指定されたオブジェクトまたは式に対して各ステートメントが何を実行しようとしているかについて、情報をまとめてパッケージ化することです。 格納された情報が実行時に調べられ、無効なステートメントがある場合は実行時例外が発生します。

ほとんどの動的操作は、結果自体が `dynamic` です。 たとえば、次の例では `testSum` を使用している箇所にマウス ポインターを置くと、IntelliSense によって型が **(ローカル変数) dynamic testSum** と表示されます。

[!code-csharp[CsProgGuideTypes#51](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#51)]

結果が `dynamic` ではない操作を次に示します。

* `dynamic` から別の型への変換。
* `dynamic` 型の引数を含むコンストラクターの呼び出し。

たとえば、次の宣言の `testInstance` の型は、`ExampleClass` ではなく、`dynamic` です。

[!code-csharp[CsProgGuideTypes#52](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#52)]

次の「変換」のセクションに変換例を示します。

## <a name="conversions"></a>コンバージョン

動的オブジェクトとその他の型との変換は簡単です。 そのため、開発者は動的な動作と動的でない動作を切り替えることができます。

次の例に示すように、任意のオブジェクトを動的な型に暗黙的に変換できます。

[!code-csharp[CsProgGuideTypes#53](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#53)]

逆に、`dynamic` 型の任意の式に暗黙的な変換を動的に適用できます。

[!code-csharp[CsProgGuideTypes#54](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#54)]

## <a name="overload-resolution-with-arguments-of-type-dynamic"></a>dynamic 型の引数を使用したオーバーロードの解決

メソッド呼び出しの 1 つ以上の引数が `dynamic` 型を指定している場合、またはメソッド呼び出しの受信側が `dynamic` 型である場合は、コンパイル時ではなく実行時にオーバーロードの解決が実行されます。 次の例では、唯一のアクセス可能な `exampleMethod2` メソッドが文字列引数を受け取るように定義されている場合、`d1` を引数として送信すると、コンパイラ エラーは発生しませんが、実行時例外が発生します。 `d1` の実行時の型は `int` であり、`exampleMethod2` には文字列が必要であるため、オーバーロードの解決は実行時に失敗します。

[!code-csharp[CsProgGuideTypes#55](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#55)]

## <a name="dynamic-language-runtime"></a>動的言語ランタイム

動的言語ランタイム (DLR) は、.NET Framework 4 の新しい API です。 DLR は、C# の `dynamic` 型だけでなく、IronPython や IronRuby などの動的プログラミング言語の実装もサポートするインフラストラクチャを提供します。 DLR の詳細については、「[動的言語ランタイムの概要](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)」を参照してください。

## <a name="com-interop"></a>COM 相互運用

C# 4 には、Office オートメーション API などの COM API との相互運用エクスペリエンスを強化する複数の機能があります。 この機能強化には、`dynamic` 型の使用、および[名前付き引数と省略可能な引数](../classes-and-structs/named-and-optional-arguments.md)の使用が含まれます。

多くの COM メソッドでは、型を `object` と指定することによって、引数の型と戻り値の型にバリエーションを持たせることができます。 このためには、C# で厳密に型指定された変数と連携できるように値の明示的なキャストが必要でした。 [-link (C# コンパイラ オプション)](../../language-reference/compiler-options/link-compiler-option.md) オプションを使用してコンパイルする場合、`dynamic` 型が導入されて COM シグネチャの `object` のオカレンスを `dynamic` 型と同様に処理できるようになったため、ほとんどのキャストを回避できます。 たとえば、Microsoft Office Excel スプレッドシートのセルに `dynamic` 型を使用してアクセスする方法と、`dynamic` 型を使用しないでアクセスする方法の対比を次のステートメントに示します。

[!code-csharp[csOfficeWalkthrough#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#12)]

[!code-csharp[csOfficeWalkthrough#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#13)]

## <a name="related-topics"></a>関連トピック

|タイトル|[説明]|
|-----------|-----------------|
|[dynamic](../../language-reference/builtin-types/reference-types.md)|`dynamic` キーワードの使用法について説明します。|
|[動的言語ランタイムの概要](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)|DLR の概要について説明します。DLR は動的言語の一連のサービスを共通言語ランタイム (CLR) に追加するランタイム環境です。|
|[チュートリアル: 動的オブジェクトの作成と使用](walkthrough-creating-and-using-dynamic-objects.md)|動的なカスタム オブジェクト、および `IronPython` ライブラリにアクセスするプロジェクトを作成するための詳細な手順について説明します。|
|[C# の機能を使用して Office 相互運用オブジェクトにアクセスする方法](../interop/how-to-access-office-onterop-objects.md)|名前付き引数と省略可能な引数、`dynamic` 型、および Office API オブジェクトへのアクセスを簡単にするその他の強化機能を使用するプロジェクトを作成する方法について説明します。|
