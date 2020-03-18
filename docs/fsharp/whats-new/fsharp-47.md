---
title: F# 4.7 の新機能- F#ガイド
description: 4\.7 でF#利用可能な新機能の概要を説明します。
ms.date: 11/27/2019
ms.openlocfilehash: 203b258466cb9f1f50215ecf8884e92e7e86416b
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74644122"
---
# <a name="whats-new-in-f-47"></a>4\.7 のF#新機能

F#4.7 では、言語にF#複数の機能強化が追加されています。

## <a name="get-started"></a>作業開始

F#4.7 は、すべての .NET Core ディストリビューションと Visual Studio ツールで使用できます。 詳細につい[ては、 F# ](../get-started/index.md) 「」を参照してください。

## <a name="language-version"></a>言語バージョン

4\.7 F#コンパイラでは、プロジェクトファイルのプロパティを使用して有効な言語バージョンを設定する機能が導入されています。

```xml
<PropertyGroup>
    <LangVersion>preview</LangVersion>
</PropertyGroup>
```

`4.6`、`4.7`、`latest`、および `preview`の値に設定できます。 既定値は、 `latest`です。

`preview`に設定した場合は、コンパイラによってF#実装されているすべてのプレビュー機能がアクティブ化されます。

## <a name="implicit-yields"></a>暗黙的な生成

型を推論できる配列、リスト、シーケンス、または計算式に `yield` キーワードを適用する必要がなくなりました。 次の例では、両方の式でF# 4.7 より前の各エントリに対して `yield` ステートメントが必要でした。

```fsharp
let s = seq { 1; 2; 3; 4; 5 }

let daysOfWeek includeWeekend =
    [ 
        "Monday"
        "Tuesday"
        "Wednesday"
        "Thursday"
        "Friday"
        if includeWeekend then 
            "Saturday"
            "Sunday"
    ] 
```

1つの `yield` キーワードを導入する場合は、他のすべての項目にも `yield` 適用する必要があります。

暗黙的な生成は、シーケンスを平坦化するように `yield!` を使用する式で使用されている場合はアクティブ化されません。 このような場合は、今日の `yield` を引き続き使用する必要があります。

## <a name="wildcard-identifiers"></a>ワイルドカード識別子

クラスF#を使用するコードでは、自己識別子は常にメンバー宣言で明示的である必要があります。 しかし、自己識別子が使用されていない場合は、無名の自己識別子を示すために、2つのアンダースコアを使用することが従来の慣例でした。 1つのアンダースコアを使用できるようになりました。

```fsharp
type C() =
    member _.M() = ()
```

これは、`for` ループにも適用されます。

```fsharp
for _ in 1..10 do printfn "Hello!"
```

## <a name="indentation-relaxations"></a>インデントリラクゼーション

F# 4.7 より前の場合、プライマリコンストラクターと静的メンバー引数のインデント要件には過剰なインデントが必要でした。 ここでは、インデントスコープを1つだけ必要とします。

```fsharp
type OffsideCheck(a:int,
    b:int, c:int,
    d:int) = class end

type C() =
    static member M(a:int,
        b:int, c:int,
        d:int) = 1
```
