---
title: 固定サイズ バッファー - C# プログラミング ガイド
ms.date: 04/20/2018
helpviewer_keywords:
- fixed size buffers [C#]
- unsafe buffers [C#]
- unsafe code [C#], fixed size buffers
ms.openlocfilehash: 9005c425badc5a4ed74e6af3447e563daf61229e
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77627800"
---
# <a name="fixed-size-buffers-c-programming-guide"></a>固定サイズ バッファー (C# プログラミング ガイド)

C# では、[fixed](../../language-reference/keywords/fixed-statement.md) ステートメントを使って、データの構造体に固定サイズの配列を持ったバッファーを作成することができます。 固定サイズのバッファーは、他の言語またはプラットフォームのデータ ソースと相互運用するメソッドを作成するときに便利です。 この固定配列には、標準的な構造体メンバーで許容されている属性または修飾子であれば、何でも適用することができます。 ただし配列の型は `bool`、`byte`、`char`、`short`、`int`、`long`、`sbyte`、`ushort`、`uint`、`ulong`、`float`、`double` のいずれかに該当する必要があり、それが唯一の制限となります。

```csharp
private fixed char name[30];
```

## <a name="remarks"></a>Remarks

セーフ コードでは、配列を含む C# 構造体に配列要素が含まれません。 この場合、構造体には、配列の要素ではなく、その参照が格納されます。 [unsafe](../../language-reference/keywords/unsafe.md) のコード ブロックで使われている [struct](../../language-reference/builtin-types/struct.md) に、固定サイズの配列を埋め込むことができます。

次の `struct` のサイズは 8 バイトです。 `pathName` 配列は参照です。

[!code-csharp[Struct with embedded array](../../../../samples/snippets/csharp/keywords/FixedKeywordExamples.cs#6)]

アンセーフ コードでは、`struct` に埋め込み配列を含めることができます。 以下の例の `fixedBuffer` 配列は固定サイズです。 `fixed` ステートメントを使用して、先頭要素へのポインターを確立します。 このポインターを使用して配列の要素にアクセスします。 `fixed` ステートメントによって、`fixedBuffer` インスタンス フィールドがメモリ内の特定の位置に固定されます。

[!code-csharp[Struct with embedded inline array](../../../../samples/snippets/csharp/keywords/FixedKeywordExamples.cs#7)]

要素数 128 の `char` 配列のサイズは 256 バイトです。 固定サイズの [char](../../language-reference/builtin-types/char.md) 型バッファーは、エンコーディングに関係なく常に、1 文字あたり 2 バイトを消費します。 これは、char 型のバッファーが、`CharSet = CharSet.Auto` または `CharSet = CharSet.Ansi` で API メソッドや構造体にマーシャリングされたときにも当てはまります。 詳細については、「<xref:System.Runtime.InteropServices.CharSet>」を参照してください。

上記の例は、固定せずに `fixed` フィールドにアクセスする方法を示しています。この方法は C# 7.3 以降から使用できます。

一般的な固定サイズの配列としては、他にも [bool](../../language-reference/builtin-types/bool.md) 配列があります。 `bool` 配列内の要素のサイズは常に 1 バイトです。 `bool` 配列は、ビット配列やバッファーの作成には適していません。

> [!NOTE]
> C# コンパイラおよび共通言語ランタイム (CLR) は、[stackalloc](../../language-reference/operators/stackalloc.md) を使って作成されたメモリを除き、バッファー オーバーランのセキュリティ チェックを実行しません。 その他のアンセーフ コードと同様、十分な注意が必要です。

アンセーフ バッファーは、次の点で通常の配列とは異なります。

- アンセーフ バッファーの使用は、unsafe コンテキストに限られます。
- アンセーフ バッファーは常にベクタ (1 次元配列) です。
- 配列の宣言には要素数を指定する必要があります (例: `char id[8]`)。 `char id[]` は使用できません。
- アンセーフ バッファーは、unsafe コンテキストで構造体のインスタンス フィールドとしてのみ使用できます。

## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [アンセーフ コードとポインター](index.md)
- [fixed ステートメント](../../language-reference/keywords/fixed-statement.md)
- [相互運用性](../interop/index.md)
