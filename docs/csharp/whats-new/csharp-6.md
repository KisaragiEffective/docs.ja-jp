---
title: C# 6 の新機能 - C# ガイド
description: C# バージョン 6 の新機能について説明します
ms.date: 12/12/2018
ms.openlocfilehash: da40b4c9d4af0094fdd907c542e971ba55086e0f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398155"
---
# <a name="whats-new-in-c-6"></a>C# 6 の新機能

C# の 6.0 リリースには、開発者の生産性を向上させる多くの機能が含まれています。 これらの機能がもたらす全体的な効果として、より読みやすく簡潔なコードを記述できるようになりました。 一般的なベスト プラクティスを多数反映することで、構文に使用される形式的な記述が減っています。 形式的な記述が減ったことで、設計の意図が理解しやすくなっています。 これらの機能を十分に学習すると、生産性が向上し、より読みやすいコードを記述できるようになります。 言語のコンストラクトよりも機能により集中することができます。

この記事の残りの部分では、これらの各機能の概要と、各機能をより詳しく学習するためのリンクを提供します。 チュートリアル セクションの [C# 6 の対話型探索](../tutorials/exploration/csharp-6.yml)で、機能を探究することもできます。

## <a name="read-only-auto-properties"></a>読み取り専用の自動プロパティ

*読み取り専用の自動プロパティ*を使用すると、より簡潔な構文で不変型を作成できます。 自動プロパティは、get アクセサーのみを使用して宣言します。

[!code-csharp[ReadOnlyAutoProperty](../../../samples/snippets/csharp/new-in-6/newcode.cs#ReadOnlyAutoProperty)]

`FirstName` プロパティと `LastName` プロパティは、同じクラスのコンストラクターの本体内でのみ設定できます。

[!code-csharp[ReadOnlyAutoPropertyConstructor](../../../samples/snippets/csharp/new-in-6/newcode.cs#ReadOnlyAutoPropertyConstructor)]

別のメソッドで `LastName` を設定しようとすると、コンパイル エラー `CS0200` が生成されます。

```csharp
public class Student
{
    public string LastName { get;  }

    public void ChangeName(string newLastName)
    {
        // Generates CS0200: Property or indexer cannot be assigned to -- it is read only
        LastName = newLastName;
    }
}
```

この機能を使用すれば、不変型を作成したり、より簡潔で便利な自動プロパティ構文を使用するための、本格的な言語サポートを実現できます。

この構文を追加してもアクセス可能なメソッドが削除されない場合は、[バイナリ互換性がある変更](version-update-considerations.md#binary-compatible-changes)です。

## <a name="auto-property-initializers"></a>自動プロパティ初期化子

*自動プロパティ初期化子*を使用すると、プロパティ宣言の一部として自動プロパティの初期値を宣言できます。

[!code-csharp[Initialization](../../../samples/snippets/csharp/new-in-6/newcode.cs#Initialization)]

`Grades` メンバーは宣言された場所で初期化されます。 これにより、初期化を厳密に 1 回だけ実行しやすくなります。 初期化がプロパティ宣言の一部になることで、ストレージ割り当てが `Student` オブジェクトのパブリック インターフェイスと同じであることを示しやすくなっています。

## <a name="expression-bodied-function-members"></a>式形式の関数メンバー

記述する多くのメンバーは、単一の式の可能性がある 1 つのステートメントです。 代わりに式形式のメンバーを記述します。 この方法は、メソッドと読み取り専用プロパティに有効です。 たとえば、`ToString()` のオーバーライドはその好例です。

[!code-csharp[ToStringExpressionMember](../../../samples/snippets/csharp/new-in-6/newcode.cs#ToStringExpressionMember)]

読み取り専用プロパティには、次の構文を使用することもできます。

[!code-csharp[FullNameExpressionMember](../../../samples/snippets/csharp/new-in-6/newcode.cs#FullNameExpressionMember)]

既存のメンバーを式のようなメンバーに変更することは、[バイナリ互換性がある変更](version-update-considerations.md#binary-compatible-changes)です。

## <a name="using-static"></a>using static

*using static* 拡張機能を使用すると、1 つのクラスの静的メソッドをインポートすることができます。 具体的には、使用するクラスを次のように指定します。

[!code-csharp[UsingStaticMath](../../../samples/snippets/csharp/new-in-6/newcode.cs#UsingStaticMath)]

<xref:System.Math> にはいずれのインスタンス メソッドも含まれていません。 また `using static` は、静的メソッドとインスタンス メソッドの両方を持つクラスの静的クラスをインポートするためにも使用できます。 特に便利な例の 1 つが <xref:System.String> です。

[!code-csharp[UsingStatic](../../../samples/snippets/csharp/new-in-6/newcode.cs#UsingStatic)]

> [!NOTE]
> static using ステートメントでは、完全修飾クラス名 (`System.String`) 使用する必要があります。  代わりに `string` キーワードを使用することはできません。

`static using` ステートメントからインポートすると、拡張メソッドは、拡張メソッドの呼び出し構文を使用して呼び出された場合にのみ、スコープ内に含められます。 静的メソッドとして呼び出された場合には含められません。 これは、LINQ クエリで多く見受けられることになるでしょう。 LINQ パターンは、<xref:System.Linq.Enumerable> または <xref:System.Linq.Queryable> をインポートすることによってインポートできます。

[!code-csharp[UsingStaticLinq](../../../samples/snippets/csharp/new-in-6/newcode.cs#usingStaticLinq)]

拡張メソッドは通常、拡張メソッドの呼び出し式を使用して呼び出します。 まれなケースですが、静的メソッドの呼び出し構文を使用して拡張メソッドを呼び出す場合は、クラス名を追加することであいまいさを解決します。

`static using` ディレクティブは、入れ子にされた型もインポートします。 これにより、入れ子にされた型を修飾なしで参照できます。

## <a name="null-conditional-operators"></a>Null 条件演算子

*null 条件演算子*を使用すれば、これらの null チェックをより簡単で円滑なものにすることができます。 メンバー アクセス `.` を `?.` に置き換えます。

[!code-csharp[NullConditional](../../../samples/snippets/csharp/new-in-6/program.cs#NullConditional)]

上記の例では、person オブジェクトが `first` の場合に、変数 `null` に `null` が割り当てられます。 それ以外の場合には、`FirstName` プロパティの値が割り当てられます。 特に重要なのは、`?.` を使用した場合、変数 `NullReferenceException` が `person` の場合、このコード行では `null` が生成されないということです。 代わりに、処理がショートサーキットされ、`null` が返されます。 null 条件演算子は配列アクセスまたはインデクサー アクセスにも使用できます。 インデックス式の `[]` を `?[]` に置き換えます。

次の式では、`string` の値に関係なく、`person` が返されます。 このコンストラクトは、いずれかのプロパティが  *の場合に既定値を割り当てる目的で、* null 結合`null`演算子と共によく使用されます。 式がショート サーキットされると、返された `null` 値が式全体に一致するように入力されます。

[!code-csharp[NullCoalescing](../../../samples/snippets/csharp/new-in-6/program.cs#NullCoalescing)]

`?.` を使用してメソッドを条件付きで呼び出すこともできます。 null 条件演算子を使用したメンバー関数の最も一般的な用途は、`null` の可能性があるデリゲート (またはイベント ハンドラー) を安全に呼び出すことです。  `Invoke` 演算子を使用してデリゲートの `?.` メソッドを呼び出して、メンバーにアクセスします。 例については、[デリゲート パターン](../delegates-patterns.md#handling-null-delegates)に関する記事でご覧になれます。

`?.` 演算子のルールでは、、演算子の左側が 1 回だけ評価されることが保証されています。 これによって、イベント ハンドラーを使用した次の例のように、多くの表現方法が可能になります。

```csharp
// preferred in C# 6:
this.SomethingHappened?.Invoke(this, eventArgs);
```

左辺が 1 回だけ評価されることを保証し、また `?.` の左辺で、メソッドの呼び出しなどの任意の式を使用できるようにします。

## <a name="string-interpolation"></a>文字列補間

C# 6 では、新しい[文字列補間](../language-reference/tokens/interpolated.md)機能を使用し、文字列に式を埋め込むことができます。 文字列の前に `$` を付けて、序数の代わりに `{` と `}` の間に式を使用するだけです。

[!code-csharp[stringInterpolation](../../../samples/snippets/csharp/new-in-6/newcode.cs#FullNameExpressionMember)]

この例では、置換される式にプロパティを使用しています。 任意の式を使用できます。 たとえば、補間の一部として生徒の評価点の平均を計算することもできます。

[!code-csharp[stringInterpolationFormat](../../../samples/snippets/csharp/new-in-6/newcode.cs#stringInterpolationFormat)]

上記のコード行では、`Grades.Average()` の値が、小数点以下 2 桁の浮動小数点数として書式設定されます。

しかし場合によっては、生成された文字列を特定のカルチャで書式設定する必要が生じる場合もあるでしょう。 文字列補間によって生成されたオブジェクトは暗黙的に <xref:System.FormattableString?displayProperty=nameWithType> に変換できることを利用します。 <xref:System.FormattableString> インスタンスには、複合書式文字列と、それらを文字列に変換する前の式評価の結果が含まれます。 <xref:System.FormattableString.ToString(System.IFormatProvider)?displayProperty=nameWithType> メソッドを使用し、文字列の書式設定時にカルチャを指定します。 次の例は、ドイツ (de-DE) のカルチャを使用して文字列を生成します (既定では、ドイツのカルチャでは小数点区切り文字に ',' 文字が使用され、桁区切り記号に '.' 文字が使用されます)。

```csharp
FormattableString str = $"Average grade is {s.Grades.Average()}";
var gradeStr = str.ToString(new System.Globalization.CultureInfo("de-DE"));
```

文字列補間を開始するには、[C# における文字列補間](../tutorials/exploration/interpolated-strings.yml)の対話型チュートリアル、[文字列補間](../language-reference/tokens/interpolated.md)に関する記事、および「[C# における文字列補間](../tutorials/string-interpolation.md)」のチュートリアルを参照してください。

## <a name="exception-filters"></a>例外フィルター

*例外フィルター*は、特定の catch 句がいつ適用されるのかを決定する句です。 例外フィルターに使用されている式が `true` と評価された場合、catch 句は例外に対して通常の処理を実行します。 式が `false` と評価された場合、`catch` 句はスキップされます。 用途としては、例外に関する情報を調べて、`catch` 句で例外を処理できるかどうかを確認するという使い方があります。

[!code-csharp[ExceptionFilter](../../../samples/snippets/csharp/new-in-6/NetworkClient.cs#ExceptionFilter)]

## <a name="the-nameof-expression"></a>`nameof` 式

[nameof](../language-reference/operators/nameof.md) 式では、シンボルの名前が評価されます。 この式は、変数、プロパティ、またはメンバー フィールドの名前が必要なときに便利です。 `nameof` の用途として特に一般的なものの 1 つは、例外の原因となったシンボルの名前を取得することです。

[!code-csharp[nameof](../../../samples/snippets/csharp/new-in-6/NewCode.cs#UsingStaticString)]

また、`INotifyPropertyChanged` インターフェイスを実装する XAML ベースのアプリケーションでも使用されます。

[!code-csharp[nameofNotify](../../../samples/snippets/csharp/new-in-6/viewmodel.cs#nameofNotify)]

## <a name="await-in-catch-and-finally-blocks"></a>Catch ブロックと Finally ブロックでの Await

C# 5 では、`await` 式を配置できる位置について、いくつかの制限がありました。 C# 6 では、`await` を `catch` 式や `finally` 式で使用できるようになりました。 次に示すのは、ロギングのシナリオで特によく使用されるコードです。

[!code-csharp[AwaitFinally](../../../samples/snippets/csharp/new-in-6/NetworkClient.cs#AwaitFinally)]

`await` 句と `catch` 句の内部に `finally` のサポートを追加するための実装詳細では、その動作と、同期コードの動作との整合性が保証されています。 `catch` または `finally` 句で実行されたコードがエラーをスローした場合、プログラムは次の包含ブロック内から適切な `catch` 句を検索します。 現行の例外があった場合、その例外は失われます。 `catch` 句や `finally` 句で待機中の式についても、これと同じことが起こります。つまり、適切な `catch` が検索され、現行の例外がある場合は、その例外が失われます。  

> [!NOTE]
> この動作を理由に、`catch` 句と `finally` 句は慎重に記述することが推奨されています (新しい例外が導入されないようにしてください)。

## <a name="initialize-associative-collections-using-indexers"></a>インデクサーを使用して関連コレクションを初期化する

*インデックス初期化子*は、インデックスの使用によってコレクション初期化子の一貫性を高める 2 つの機能のうちの 1 つです。 C# の以前のリリースでは、キーと値のペアを中かっこで囲むことで  *を含めたシーケンス スタイルのコレクションで*コレクション初期化子<xref:System.Collections.Generic.Dictionary%602>を使用できました。

[!code-csharp[ListInitializer](../../../samples/snippets/csharp/new-in-6/initializers.cs#CollectionInitializer)]

それらを <xref:System.Collections.Generic.Dictionary%602> コレクションおよびアクセス可能な `Add` メソッドが複数の引数を受け取るその他の型と共に使用できます。 新しい構文は、インデックスを使用したコレクションへの割り当てをサポートしています。

[!code-csharp[DictionaryInitializer](../../../samples/snippets/csharp/new-in-6/initializers.cs#DictionaryInitializer)]

この機能が加わったことにより、いくつかのバージョンでシーケンス コンテナーに使用されていたものと同様の構文を使用して、連想コンテナーを初期化できるようになりました。

## <a name="extension-add-methods-in-collection-initializers"></a>コレクション初期化子内の拡張 `Add` メソッド

コレクション初期化を使いやすくするもう 1 つの機能として、 *メソッドの*拡張メソッド`Add`を使用できるようになりました。 この機能は、Visual Basic との同等性を確保するために追加されました。 この機能は、新しい項目を意味的に追加するために、カスタム コレクション クラスで別の名前のメソッドを使用している場合に特に便利です。

## <a name="improved-overload-resolution"></a>オーバーロード解決の改善

最後に説明するのは、言われなければ気付かないかもしれない機能です。 以前のバージョンの C# コンパイラでは、ラムダ式を使用した一部のメソッド呼び出しがあいまいと判断されるコンストラクトがありました。 たとえば、次のメソッドがあったとします。

[!code-csharp[AsyncMethod](../../../samples/snippets/csharp/new-in-6/overloads.cs#AsyncMethod)]

以前のバージョンの C# では、メソッド グループ構文を使用してこのメソッドを呼び出すと、エラーが発生していました。

[!code-csharp[MethodGroup](../../../samples/snippets/csharp/new-in-6/overloads.cs#MethodGroup)]

以前のコンパイラでは、`Task.Run(Action)` と `Task.Run(Func<Task>())` を正しく区別することができませんでした。 以前のバージョンでは、引数としてラムダ式を使用する必要がありました。

[!code-csharp[Lambda](../../../samples/snippets/csharp/new-in-6/overloads.cs#Lambda)]

C# 6 の コンパイラは、`Task.Run(Func<Task>())` のほうが適切であるということを正しく判断できます。

### <a name="deterministic-compiler-output"></a>決定論的コンパイラの出力

`-deterministic` オプションが、同じソース ファイルの連続するコンパイルで、バイト単位で同じ出力アセンブリを生成するようにコンパイラに指示します。

既定では、すべてのコンパイルでは、コンパイルごとに一意な出力が生成されます。 コンパイラは、タイムスタンプを追加し、ランダムな数から生成された GUID を追加します。 ビルド間の一貫性を確保するために、バイト単位で出力を比較する場合は、このオプションを使用します。

詳細については、[-deterministic コンパイラ オプション](../language-reference/compiler-options/deterministic-compiler-option.md)の記事を参照してください。
