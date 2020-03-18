---
title: C# 7.3 の新機能
description: C# 7.3 の新機能の概要
ms.date: 05/16/2018
ms.openlocfilehash: ba4cea302d91b395e88940d087fcaed306920840
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74204552"
---
# <a name="whats-new-in-c-73"></a>C# 7.3 の新機能

C# 7.3 リリースには 2 つの主要なテーマがあります。 1 つ目のテーマは、アンセーフ コードと同様のパフォーマンスをセーフ コードで確保するための機能の提供です。 2 つ目のテーマは、既存の機能のインクリメンタルな改善の提供です。 また、新しいコンパイラ オプションがこのリリースで追加されました。

次の新機能は、セーフ コードのパフォーマンス向上のテーマをサポートします。

- ピン留めを使用せずに fixed フィールドにアクセスできます。
- `ref` ローカル変数を再割り当てできます。
- `stackalloc` 配列で初期化子を使用できます。
- パターンをサポートする型と共に `fixed` ステートメントを使用できます。
- 追加のジェネリック制約を使用できます。

既存の機能が次のように強化されました。

- タプル型を使用して `==` と `!=` をテストできます。
- 式の変数をより多くの場所で使用できます。
- 自動実装プロパティのバッキング フィールドに属性をアタッチできます。
- 引数が `in` によって異なる場合のメソッド解決が改善されました。
- オーバーロードの解決のあいまいなケースが削減されました。

新しいコンパイラ オプションは次のとおりです。

- `-publicsign`: オープン ソース ソフトウェア (OSS) のアセンブリの署名を可能にします。
- `-pathmap`: ソース ディレクトリのマッピングを提供します。

この記事の残りの部分では、それぞれの機能強化の詳細とリンクを示します。 `dotnet try` グローバル ツールを使って、これらの機能をご自身の環境で調べることができます。

1. [dotnet try](https://github.com/dotnet/try/blob/master/README.md#setup) グローバル ツールをインストールします。
1. [dotnet/try-samples](https://github.com/dotnet/try-samples) リポジトリを複製します。
1. 現在のディレクトリを、*try-samples* リポジトリの *csharp7* サブディレクトリに設定します。
1. `dotnet try` を実行します。

## <a name="enabling-more-efficient-safe-code"></a>セーフ コードをより効率的にする

アンセーフ コードと同様のパフォーマンスを確保した C# コードを安全に記述できるようにする必要があります。 セーフ コードは、バッファー オーバーラン、ストレイ ポインター、その他のメモリ アクセス エラーなどのエラーを回避します。 ここで説明する新機能は、検証可能なセーフ コードの機能を拡張します。 安全なコンストラクトを使用してより多くのコードを記述するようにしてください。 以下に示す機能によって、コードの記述が容易になります。

### <a name="indexing-fixed-fields-does-not-require-pinning"></a>ピン留めが不要な `fixed` フィールドのインデックス付け

たとえば、次の構造体があるとします。

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}
```

以前のバージョンの C# では、`myFixedField` の一部であるいずれかの整数にアクセスするために変数のピン留めが必要でした。 今では、次のコードは、変数 `p` を別の `fixed` ステートメントの内部にピン留めせずに、コンパイルされます。

```csharp
class C
{
    static S s = new S();

    unsafe public void M()
    {
        int p = s.myFixedField[5];
    }
}
```

変数 `p` は `myFixedField` の 1 個の要素にアクセスします。 個別の `int*` 変数を宣言する必要はありません。 `unsafe` コンテキストは引き続き必要です。 以前のバージョンの C# では、2 番目の固定ポインターを宣言する必要があります。

```csharp
class C
{
    static S s = new S();

    unsafe public void M()
    {
        fixed (int* ptr = s.myFixedField)
        {
            int p = ptr[5];
        }
    }
}
```

詳細については、[`fixed` ステートメント](../language-reference/keywords/fixed-statement.md)に関する記事を参照してください。

### <a name="ref-local-variables-may-be-reassigned"></a>再割り当て可能な `ref` ローカル変数

初期化後に別のインスタンスを参照するために、`ref` ローカル変数を再割り当てできるようになりました。 次のコードがコンパイルされます。

```csharp
ref VeryLargeStruct refLocal = ref veryLargeStruct; // initialization
refLocal = ref anotherVeryLargeStruct; // reassigned, refLocal refers to different storage.
```

詳しくは、[`ref` 戻り値と `ref` ローカル変数](../programming-guide/classes-and-structs/ref-returns.md)に関する記事と [`foreach`](../language-reference/keywords/foreach-in.md) に関する記事を参照してください。

### <a name="stackalloc-arrays-support-initializers"></a>`stackalloc` 配列による初期化子のサポート

配列を初期化する際に、配列内の要素の値を指定することが可能でした。

```csharp
var arr = new int[3] {1, 2, 3};
var arr2 = new int[] {1, 2, 3};
```

現在では、`stackalloc` で宣言された配列に同じ構文を適用できます。

```csharp
int* pArr = stackalloc int[3] {1, 2, 3};
int* pArr2 = stackalloc int[] {1, 2, 3};
Span<int> arr = stackalloc [] {1, 2, 3};
```

詳細については、「[`stackalloc` 演算子](../language-reference/operators/stackalloc.md)」の記事を参照してください。

### <a name="more-types-support-the-fixed-statement"></a>`fixed` ステートメントをサポートする型の増加

`fixed` ステートメントは、限られた一連の型をサポートしていました。 C# 7.3 以降では、`GetPinnableReference()` または `ref T` を返す `ref readonly T` メソッドを格納する型として `fixed` を使用できます。 この機能の追加により、`fixed` を <xref:System.Span%601?displayProperty=nameWithType> および関連する型と共に使用できます。

詳しくは、言語リファレンスの [`fixed` ステートメント](../language-reference/keywords/fixed-statement.md)に関する記事を参照してください。

### <a name="enhanced-generic-constraints"></a>ジェネリック制約の拡張

型パラメーターの基底クラスの制約として、<xref:System.Enum?displayProperty=nameWithType> 型または <xref:System.Delegate?displayProperty=nameWithType> 型を指定できるようになりました。

また、新しい `unmanaged` 制約を使用して、型パラメーターが null 非許容で[アンマネージ型](../language-reference/builtin-types/unmanaged-types.md)である必要があることを指定することもできます。

詳しくは、[`where` ジェネリック制約](../language-reference/keywords/where-generic-type-constraint.md)および[型パラメーターの制約](../programming-guide/generics/constraints-on-type-parameters.md)に関する記事を参照してください。

これらの制約を既存の型に追加することは、[互換性のない変更](version-update-considerations.md#incompatible-changes)です。 クローズ ジェネリック型は、これらの新しい制約を満たさなくなります。

## <a name="make-existing-features-better"></a>既存の機能の改善

2 つ目のテーマは、言語の機能の改善の提供です。 以下に示す機能によって、C# を記述する際の生産性が向上します。

### <a name="tuples-support--and-"></a>タプルによる `==` と `!=` のサポート

C# のタプル型で `==` と `!=` がサポートされるようになりました。 詳細については、[タプル](../tuples.md#equality-and-tuples)に関する記事の[等値](../tuples.md)について説明したセクションを参照してください。

### <a name="attach-attributes-to-the-backing-fields-for-auto-implemented-properties"></a>自動実装プロパティのバッキング フィールドへの属性のアタッチ

次の構文がサポートされるようになりました。

```csharp
[field: SomeThingAboutFieldAttribute]
public int SomeProperty { get; set; }
```

属性 `SomeThingAboutFieldAttribute` は、`SomeProperty` のコンパイラによって生成されるバッキング フィールドに適用されます。 詳しくは、C# プログラミング ガイドの「[属性](../programming-guide/concepts/attributes/index.md)」を参照してください。

### <a name="in-method-overload-resolution-tiebreaker"></a>`in` メソッドのオーバーロードを解決するタイブレーカー

`in` 引数修飾子が追加されると、次の 2 つのメソッドがあいまいさの原因となっていました。

```csharp
static void M(S arg);
static void M(in S arg);
```

現在では、値による (前述の例の最初の方法) オーバーロードが readonly 参照によるオーバーロードよりも優れています。 readonly 参照引数を使用してバージョンを呼び出すには、メソッドの呼び出し時に `in` 修飾子を含める必要があります。

> [!NOTE]
> これはバグの修正として実装されました。 これにより、言語バージョンが "7.2" に設定されている場合でもあいまいさが発生することはありません。

詳しくは、[`in` パラメーター修飾子](../language-reference/keywords/in-parameter-modifier.md)に関する記事を参照してください。

### <a name="extend-expression-variables-in-initializers"></a>初期化子における式の変数の拡張

`out` 変数の宣言を許可するために C# 7.0 に追加された構文が、フィールド初期化子、プロパティ初期化子、コンストラクター初期化子、およびクエリ句を含めるように拡張されました。 これにより、次の例に示すようなコードを使用できるようになります。

```csharp
public class B
{
   public B(int i, out int j)
   {
      j = i;
   }
}

public class D : B
{
   public D(int i) : base(i, out var j)
   {
      Console.WriteLine($"The value of 'j' is {j}");
   }
}
```

### <a name="improved-overload-candidates"></a>オーバーロード候補の改善

すべてのリリースにおいて、あいまいなメソッド呼び出しに対する "明らかな" 選択肢が存在する状況に対応するようにオーバーロードの解決ルールが更新されました。 このリリースでは、コンパイラが明らかな選択肢を選択できるようにする 3 つの新しいルールが追加されています。

1. インスタンス メンバーと静的メンバーの両方がメソッド グループに含まれている場合は、インスタンス レシーバーまたはコンテキストを指定せずにメソッドが呼び出されると、コンパイラがインスタンス メンバーを破棄します。 インスタンス レシーバーを使用してメソッドが呼び出された場合、コンパイラは静的メンバーを破棄します。 レシーバーがない場合、コンパイラは静的メンバーだけを静的コンテキストに含めます。それ以外の場合は、静的メンバーとインスタンス メンバーの両方を含めます。 レシーバーがあいまいなインスタンスまたは型である場合、コンパイラは両方のメンバーを含めます。 暗黙的な `this` インスタンス レシーバーを使用できない静的コンテキストには、`this` が定義されないメンバー (静的メンバーなど) の本体および `this` を使用できない場所 (フィールド初期化子やコンストラクター初期化子など) が含まれます。
1. 型引数が制約を満たしていない複数のジェネリック メソッドがメソッド グループに含まれている場合、そのグループのメンバーは候補セットから削除されます。
1. メソッド グループの変換では、戻り値の型がデリゲートの戻り値の型と一致しない候補メソッドが候補セットから削除されます。

適切なメソッドがわかっている場合には、あいまいなメソッドのオーバーロードに対するコンパイラ エラーが少なくなることを認識できるため、この変更点にのみ注意してください。

## <a name="new-compiler-options"></a>新しいコンパイラ オプション

新しいコンパイラ オプションでは、C# プログラムの新しいビルドと DevOps のシナリオがサポートされます。

### <a name="public-or-open-source-signing"></a>公開署名またはオープン ソース署名

`-publicsign` コンパイラ オプションは、公開キーを使用してアセンブリに署名するようにコンパイラに指示します。 アセンブリは署名済みとしてマークされますが、署名は公開キーから取得されます。 このオプションでは、公開キーを使用するオープン ソース プロジェクトから署名済みのアセンブリをビルドできます。

詳しくは、[-publicsign コンパイラ オプション](../language-reference/compiler-options/publicsign-compiler-option.md)の記事を参照してください。

### <a name="pathmap"></a>pathmap

`-pathmap` コンパイラ オプションは、ビルド環境からのソース パスをマップ済みのソース パスに置き換えるようにコンパイラに指示します。 `-pathmap` オプションは、コンパイラが記述した PDB ファイルまたは <xref:System.Runtime.CompilerServices.CallerFilePathAttribute> のソース パスを制御します。

詳しくは、[-pathmap コンパイラ オプション](../language-reference/compiler-options/pathmap-compiler-option.md)の記事を参照してください。
