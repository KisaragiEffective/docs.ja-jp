---
title: ポインターに関連する演算子 - C# リファレンス
description: ポインターを操作するときに使用できる C# の演算子について説明します。
ms.date: 05/20/2019
author: pkulikov
f1_keywords:
- ->_CSharpKeyword
helpviewer_keywords:
- pointer related operators [C#]
- address-of operator [C#]
- '& operator [C#]'
- pointer indirection operator [C#]
- dereference operator [C#]
- '* operator [C#]'
- pointer member access operator [C#]
- -> operator [C#]
- pointer element access [C#]
- '[] operator [C#]'
- pointer arithmetic [C#]
- pointer increment [C#]
- pointer decrement [C#]
- pointer comparison [C#]
ms.openlocfilehash: 22cd87a62c92dbfacd0b8ebbdd5cbafb92da73ca
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497360"
---
# <a name="pointer-related-operators-c-reference"></a>ポインターに関連する演算子 (C# リファレンス)

次の演算子を使って、ポインターを操作することができます。

- 単項 [`&` (アドレス取得)](#address-of-operator-) 演算子: 変数のアドレスを取得します
- 単項 [`*` (ポインター間接参照)](#pointer-indirection-operator-) 演算子: ポインターが指す位置にある変数を取得します
- [`->` (メンバー アクセス)](#pointer-member-access-operator--) および [`[]` (要素アクセス)](#pointer-element-access-operator-) 演算子
- 算術演算子 [`+`、`-`、`++`、`--`](#pointer-arithmetic-operators)
- 比較演算子 [`==`、`!=`、`<`、`>`、`<=`、`>=`](#pointer-comparison-operators)

ポインター型については、「[ポインター型](../unsafe-code.md#pointer-types)」をご覧ください。

> [!NOTE]
> ポインターに関するすべての操作には、[unsafe](../keywords/unsafe.md) コンテキストが必要です。 unsafe ブロックを含むコードは、[**AllowUnsafeBlocks**](../compiler-options/language.md#allowunsafeblocks) コンパイラ オプションを使ってコンパイルする必要があります。

## <a name="address-of-operator-amp"></a><a name="address-of-operator-"></a> アドレス取得演算子 &amp;

単項の `&` 演算子からは、そのオペランドのアドレスが返されます。

[!code-csharp[address of local](snippets/shared/PointerOperators.cs#AddressOf)]

`&` 演算子のオペランドは、固定変数である必要があります。 "*固定*" 変数とは、[ガベージ コレクター](../../../standard/garbage-collection/index.md)の操作によって影響を受けない記憶域の場所に存在する変数です。 前の例では、ローカル変数 `number` はスタックに存在するので固定変数です。 ガベージ コレクターによって影響を受ける可能性がある (たとえば再配置) 記憶域の場所にある変数は、"*移動可能*" 変数と呼ばれます。 オブジェクト フィールドや配列の要素は、移動可能変数の例です。 [`fixed` ステートメント](../keywords/fixed-statement.md)で移動可能変数を "固定" または "ピン留め" した場合は、移動可能変数のアドレスを取得できます。 取得したアドレスは、`fixed` ステートメントのブロック内でのみ有効です。 `fixed` ステートメントと `&` 演算子の使い方の例を次に示します。

[!code-csharp[address of fixed](snippets/shared/PointerOperators.cs#AddressOfFixed)]

定数または値のアドレスを取得できません。

固定変数と移動可能変数について詳しくは、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の「[Fixed and moveable variables (固定変数と移動可能変数)](~/_csharplang/spec/unsafe-code.md#fixed-and-moveable-variables)」セクションをご覧ください。

2 項 `&` 演算子では、ブール型オペランドの[論理 AND](boolean-logical-operators.md#logical-and-operator-) または整数オペランドの[ビットごとの論理 AND](bitwise-and-shift-operators.md#logical-and-operator-) が計算されます。

## <a name="pointer-indirection-operator-"></a>ポインター間接参照演算子 *

単項ポインター間接参照演算子 `*` では、オペランドが指し示す変数が取得されます。 逆参照演算子とも呼ばれます。 `*` 演算子のオペランドは、ポインター型である必要があります。

[!code-csharp[pointer indirection](snippets/shared/PointerOperators.cs#PointerIndirection)]

`void*` 型の式に `*` 演算子を適用することはできません。

2 項 `*` 演算子では、数値オペランドの[積](arithmetic-operators.md#multiplication-operator-)が計算されます。

## <a name="pointer-member-access-operator--"></a>ポインター メンバー アクセス演算子 ->

`->` 演算子では、[ポインターの間接参照](#pointer-indirection-operator-)と[メンバー アクセス](member-access-operators.md#member-access-expression-)が組み合わされます。 つまり、`x` が `T*` 型のポインターで、`y` が `T` 型のアクセス可能なメンバーである場合に、次のような形式の式を考えます

```csharp
x->y
```

上記の式は、次の式と同じです。

```csharp
(*x).y
```

`->` 演算子の使用例を次に示します。

[!code-csharp[pointer member access](snippets/shared/PointerOperators.cs#MemberAccess)]

`void*` 型の式に `->` 演算子を適用することはできません。

## <a name="pointer-element-access-operator-"></a>ポインター要素アクセス演算子 []

ポインター型の式 `p` の場合、`p[n]` の形式のポインター要素アクセスは、`*(p + n)` と評価されます。`n` は、`int`、`uint`、`long`、または `ulong` に暗黙的に変換できる型でなければなりません。 ポインターでの `+` 演算子の動作については、「[ポインターに対する整数値の加算または減算](#addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer)」セクションをご覧ください。

次の例では、ポインターと `[]` 演算子による配列要素へのアクセス方法を示します。

[!code-csharp[pointer element access](snippets/shared/PointerOperators.cs#ElementAccess)]

前の例では、[`stackalloc` 式](stackalloc.md)によってスタックにメモリ ブロックが割り当てられています。

> [!NOTE]
> ポインター要素アクセス演算子では、範囲外のエラーはチェックされません。

`void*` 型の式でポインター要素アクセスに `[]` を使うことはできません。

[配列要素またはインデクサー アクセス](member-access-operators.md#indexer-operator-)には `[]` 演算子を使用することもできます。

## <a name="pointer-arithmetic-operators"></a>ポインター算術演算子

ポインターで次の算術演算を実行できます。

- ポインターに整数値を加算する、またはポインターから整数値を減算する
- 2 個のポインターを減算する
- ポインターをインクリメントまたはデクリメントする

`void*` 型のポインターでこれらの演算を実行することはできません。

数値型でサポートされている算術演算については、「[算術演算子](arithmetic-operators.md)」をご覧ください。

### <a name="addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer"></a>ポインターへの整数値の加算またはポインターからの整数値の減算

`T*` 型のポインター `p` と、`int`、`uint`、`long`、または `ulong` に暗黙的に変換できる型の式`n` の場合、加算と減算は次のように定義されます。

- 式 `p + n` および `n + p` では、どちらの場合も、`p` によって与えられるアドレスに `n * sizeof(T)` を加算した結果である、`T*` 型のポインターが生成されます。
- 式 `p - n` では、`p` によって与えられるアドレスから `n * sizeof(T)` を減算した結果である、`T*` 型のポインターが生成されます。

[`sizeof` 演算子](sizeof.md)では、型のサイズ (バイト単位) が取得されます。

次の例では、ポインターでの `+` 演算子の使用方法を示します。

[!code-csharp[pointer addition](snippets/shared/PointerOperators.cs#AddNumber)]

### <a name="pointer-subtraction"></a>ポインターの減算

`T*` 型の 2 つのポインター `p1` と `p2` の場合、式 `p1 - p2` では、`p1` と `p2` によって指定されるアドレスの間の差を `sizeof(T)` によって除算した値が生成されます。 結果の型は `long` です。 つまり、`p1 - p2` は `((long)(p1) - (long)(p2)) / sizeof(T)` として計算されます。

ポインターの減算の例を次に示します。

[!code-csharp[pointer subtraction](snippets/shared/PointerOperators.cs#SubtractPointers)]

### <a name="pointer-increment-and-decrement"></a>ポインターのインクリメントとデクリメント

`++` インクリメント演算子では、ポインター オペランドに 1 が[加算](#addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer)されます。 `--` デクリメント演算子では、ポインター オペランドから 1 が[減算](#addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer)されます。

どちらの演算子も、後置 (`p++` および `p--`) と前置 (`++p` および `--p`) の 2 つの形式でサポートされます。 `p++` および `p--` の結果は、演算の "*前*" の `p` の値です。 `++p` および `--p` の結果は、演算の "*後*" の `p` の値です。

次の例では、後置と前置両方のインクリメント演算子の動作を示します。

[!code-csharp[pointer increment](snippets/shared/PointerOperators.cs#Increment)]

## <a name="pointer-comparison-operators"></a>ポインター比較演算子

`==`、`!=`、`<`、`>`、`<=`、`>=` 演算子を使って、`void*` を含む任意のポインター型のオペランドを比較できます。 これらの演算子では、2 つのオペランドによって指定されるアドレスが、符号なし整数のように比較されます。

他の型のオペランドに対するこれらの演算子の動作については、「[等値演算子](equality-operators.md)」および「[比較演算子](comparison-operators.md)」をご覧ください。

## <a name="operator-precedence"></a>演算子の優先順位

次のポインター関連演算子の一覧は、優先度が高い順に並べられています。

- 後置インクリメント `x++` およびデクリメント `x--` 演算子、`->` および `[]` 演算子
- 前置インクリメント `++x` およびデクリメント `--x` 演算子、`&` および `*` 演算子
- 加法 `+` および `-` 演算子
- 比較 `<`、`>`、`<=`、`>=` 演算子
- 等値 `==` および `!=` 演算子

演算子の優先順位によって定められた評価の順序を変更するには、かっこ `()` を使用します。

優先度順に並べられた C# 演算子の完全な一覧については、[C# 演算子](index.md)に関する記事の「[演算子の優先順位](index.md#operator-precedence)」セクションを参照してください。

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型では、ポインター関連演算子 `&`、`*`、`->`、`[]` をオーバーロードできません。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [固定変数と移動可能変数](~/_csharplang/spec/unsafe-code.md#fixed-and-moveable-variables)
- [アドレス取得演算子](~/_csharplang/spec/unsafe-code.md#the-address-of-operator)
- [ポインター間接参照](~/_csharplang/spec/unsafe-code.md#pointer-indirection)
- [ポインター メンバー アクセス](~/_csharplang/spec/unsafe-code.md#pointer-member-access)
- [ポインター要素アクセス](~/_csharplang/spec/unsafe-code.md#pointer-element-access)
- [ポインター算術](~/_csharplang/spec/unsafe-code.md#pointer-arithmetic)
- [ポインター インクリメントおよびデクリメント](~/_csharplang/spec/unsafe-code.md#pointer-increment-and-decrement)
- [ポインター比較](~/_csharplang/spec/unsafe-code.md#pointer-comparison)

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# の演算子と式](index.md)
- [ポインター型](../unsafe-code.md#pointer-types)
- [unsafe キーワード](../keywords/unsafe.md)
- [fixed キーワード](../keywords/fixed-statement.md)
- [stackalloc](stackalloc.md)
- [sizeof 演算子](sizeof.md)
