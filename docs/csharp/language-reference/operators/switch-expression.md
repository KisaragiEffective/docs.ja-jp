---
title: switch 式 - C# リファレンス
description: パターン マッチングに基づいてスイッチに似たセマンティクスを提供する C# の switch 式について説明します
ms.date: 04/16/2021
f1_keywords:
- switch_CSharpKeyword
helpviewer_keywords:
- switch expression [C#]
- pattern matching [C#]
ms.openlocfilehash: ce892598f364e4934613a797d9290fa385abd4c8
ms.sourcegitcommit: 23e2bc267872ce6ea22db4bf6478fe57bcba4bc8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2021
ms.locfileid: "107592267"
---
# <a name="switch-expression-c-reference"></a>switch 式 (C# リファレンス)

C# 8.0 以降は、`switch` 式を使用して、入力式とのパターン一致に基づいて候補の式のリストから 1 つの式を評価します。 ステートメントのコンテキストでの `switch` と同様のセマンティクスをサポートする `switch` ステートメントの詳細については、[`switch` ステートメント](../keywords/switch.md)の記事を参照してください。

次の例は、`switch` 式を示しており、オンライン マップでの視方向を表す [`enum`](../builtin-types/enum.md) の値を、対応する基本方位に変換しています。

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="SnippetBasicStructure":::

前のサンプルでは、`switch` 式の基本的な要素が示されています。

- `switch` キーワードが後に続く式。 上記の例では、`direction` メソッド パラメーターが該当します。
- コンマで区切った " *`switch` 式アーム*"。 各 `switch` 式アームには、"*パターン*"、省略可能な "[*ケース ガード*](#case-guards)"、`=>` トークン、"*式*" が含まれています。

上記の例では、`switch` 式に次のパターンが使用されています。

- [定数パターン](patterns.md#constant-pattern)が、`Direction` 列挙型の定義済みの値を処理するために使用されています。
- [破棄パターン](patterns.md#discard-pattern)が、`Direction` 列挙型の対応するメンバーを持たない任意の整数値 (たとえば `(Direction)10`) を処理するために使用されています。 これにより、`switch` 式が[網羅的](#non-exhaustive-switch-expressions)になります。

`switch` 式でサポートされるパターンの詳細については、「[パターン](patterns.md)」を参照してください。

`switch` 式の結果は、パターンが入力式と一致し、ケース ガード (存在する場合) が `true` と評価される、最初の `switch` 式アームの式の値です。 `switch` 式アームは、テキストの順番に評価されます。

上位の `switch` 式アームがすべての値と一致するため、下位の `switch` 式アームを選択できない場合、コンパイラでエラーが生成されます。

## <a name="case-guards"></a>ケース ガード

アームの式の評価の条件を指定するのに十分な表現がパターンにない場合があります。 このような場合は、ケース ガードを使用できます。 これは、パターンの一致と共に満たされる必要がある追加条件です。 次の例に示すように、パターンの後に続く `when` キーワードの後に、ケース ガードを指定します。

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="CaseGuardExample":::

前の例では、[var パターン](patterns.md#var-pattern)を入れ子にした[プロパティ パターン](patterns.md#property-pattern)を使用しています。

## <a name="non-exhaustive-switch-expressions"></a>網羅的でない switch 式

`switch` 式のどのパターンも入力値に一致しない場合、ランタイムで例外がスローされます。 .NET Core 3.0 以降のバージョンでは、例外は <xref:System.Runtime.CompilerServices.SwitchExpressionException?displayProperty=nameWithType> です。 .NET Framework では、例外は <xref:System.InvalidOperationException> です。 `switch` 式ですべての可能な入力値が処理されない場合、コンパイラで警告が生成されます。

> [!TIP]
> `switch` 式ですべての可能な入力値が確実に処理されるようにするには、[破棄パターン](patterns.md#discard-pattern)を使用した `switch` 式アームを指定します。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[機能提案メモ](~/_csharplang/proposals/csharp-8.0/patterns.md)の「[`switch` 式](~/_csharplang/proposals/csharp-8.0/patterns.md#switch-expression)」のセクションを参照してください。

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# の演算子と式](index.md)
- [パターン](patterns.md)
- [チュートリアル: パターン マッチングを使用して、型ドリブンおよびデータ ドリブンのアルゴリズムを構築する](../../tutorials/pattern-matching.md)
- [`switch` ステートメント](../keywords/switch.md)
