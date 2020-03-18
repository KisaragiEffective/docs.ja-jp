---
title: 冗語構文
description: F#プログラミング言語における verbose 構文と簡易構文の違いについて説明します。
ms.date: 05/16/2016
ms.openlocfilehash: 575585b201acc1366980cfc5cf523c4117259084
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73421179"
---
# <a name="verbose-syntax"></a>冗語構文

言語のさまざまな構成要素で使用できる構文には、 *verbose 構文*と*簡易構文*の2つの形式があります。 F# Verbose 構文は一般的に使用されるものではありませんが、インデントの影響が少なくなるという利点があります。 軽量の構文は短く、インデントを使用して、`begin`、`end`、`in`などの追加のキーワードではなく、コンストラクトの開始と終了を通知します。 既定の構文は、簡易構文です。 このトピックでは、軽量F#構文が有効になっていない場合のコンストラクトの構文について説明します。 Verbose 構文は常に有効になっているため、簡易構文を有効にした場合でも、一部のコンストラクトに対して verbose 構文を使用できます。 `#light "off"` ディレクティブを使用して、軽量構文を無効にすることができます。

## <a name="table-of-constructs"></a>構造体の表

次の表は、2つの形式のF#間に違いがあるコンテキストでの言語構成要素の簡易構文と詳細な構文を示しています。 この表では、山かっこ (&lt;&gt;) は、ユーザーが指定した構文要素を囲みます。 これらのコンストラクトで使用される構文の詳細については、各言語構成要素のドキュメントを参照してください。

<table>
<tr>
<th>言語構成要素</th>
<th>簡易構文</th>
<th>Verbose 構文</th>
</tr>
<tr>
<td>
複合式
</td>
<td>

```xml
<expression1>
<expression2>
```

</td><td>

```fsharp
<expression1>; <expression2>
```

</td>
</tr>
<tr><td>

入れ子になった `let` バインド

</td><td>

```fsharp
let f x =
    let a = 1
    let b = 2
    x + a + b
```

</td><td>

```fsharp
let f x =
    let a = 1 in
    let b = 2 in
    x + a + b
```

</td>
</tr>
<tr><td>
コードブロック
</td><td>

```fsharp
(
    <expression1>
    <expression2>
)
```

</td><td>

```fsharp
begin
    <expression1>;
    <expression2>;
end
```

</td>
</tr>
<tr><td>
`for...do`
</td><td>

```fsharp
for counter = start to finish do
    ...
```

</td><td>

```fsharp
for counter = start to finish do
    ...
done
```

</td>
</tr>
<tr><td>
`while...do`
</td><td>

```fsharp
while <condition> do
    ...
```

</td><td>

```fsharp
while <condition> do
    ...
done
```

</td>
</tr>
<tr><td>
`for...in`
</td><td>

```fsharp
for var in start .. finish do
    ...
```

</td><td>

```fsharp
for var in start .. finish do
    ...
done
```

</td>
</tr>
<tr><td>
`do`
</td><td>

```fsharp
do
    ...
```

</td><td>

```fsharp
do
    ...
in
```

</td>
</tr>
<tr><td>録音
</td><td>

```fsharp
type <record-name> =
    {
        <field-declarations>
    }
    <value-or-member-definitions>
```

</td><td>

```fsharp
type <record-name> =
    {
        <field-declarations>
    }
    with
        <value-or-member-definitions>
    end
```

</td>
</tr>
<tr><td>クラス
</td><td>

```fsharp
type <class-name>(<params>) =
    ...
```

</td><td>

```fsharp
type <class-name>(<params>) =
    class
        ...
    end
```

</td>
</tr>
<tr><td>構造体</td><td>

```fsharp
[<StructAttribute>]
type <structure-name> =
    ...
```

</td><td>

```fsharp
type <structure-name> =
    struct
        ...
    end
```

</td>
</tr>
<tr><td>判別共用体</td><td>

```fsharp
type <union-name> =
    | ...
    | ...
    ...
    <value-or-member definitions>
```

</td><td>

```fsharp
type <union-name> =
    | ...
    | ...
    ...
    with
        <value-or-member-definitions>
    end
```

</td>
</tr>
<tr><td>interface</td><td>

```fsharp
type <interface-name> =
    ...
```

</td><td>

```fsharp
type <interface-name> =
    interface
        ...
    end
```

</td>
</tr>
<tr><td>オブジェクト式</td><td>

```fsharp
{ new <type-name>
    with
        <value-or-member-definitions>
        <interface-implementations>
}
```

</td><td>

```fsharp
{ new <type-name>
    with
        <value-or-member-definitions>
    end
    <interface-implementations>
}
```

</td>
</tr>
<tr><td>インターフェイスの実装</td><td>

```fsharp
interface <interface-name>
    with
        <value-or-member-definitions>
```

</td><td>

```fsharp
interface <interface-name>
    with
        <value-or-member-definitions>
    end
```

</td>
</tr>
<tr><td>型拡張</td><td>

```fsharp
type <type-name>
    with
        <value-or-member-definitions>
```

</td><td>

```fsharp
type <type-name>
    with
        <value-or-member-definitions>
    end
```

</td>
</tr>
<tr><td>name</td><td>

```fsharp
module <module-name> =
    ...
```

</td><td>

```fsharp
module <module-name> =
    begin
        ...
    end
```

</td>
</tr>
</table>

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](index.md)
- [コンパイラ ディレクティブ](compiler-directives.md)
- [コードのフォーマットに関するガイドライン](../style-guide/formatting.md)
