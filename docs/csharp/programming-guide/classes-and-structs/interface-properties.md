---
title: インターフェイスのプロパティ - C# プログラミング ガイド
ms.date: 01/31/2020
helpviewer_keywords:
- properties [C#], on interfaces
- interfaces [C#], properties
ms.assetid: 6503e9ed-33d7-44ec-b4c1-cc16c084b795
ms.openlocfilehash: 5798b80526f34e923e2eaab43847b98f6c64e14b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77626621"
---
# <a name="interface-properties-c-programming-guide"></a>インターフェイスのプロパティ (C# プログラミング ガイド)

[interface](../../language-reference/keywords/interface.md) でプロパティを宣言することができます。 次の例では、インターフェイス プロパティ アクセサーが宣言されます。

[!code-csharp[DeclareProperties](~/samples/snippets/csharp/interfaces/properties.cs#DeclareInterfaceProperties)]

インターフェイス プロパティには通常、本体がありません。 プロパティが読み取り/書き込み、読み取り専用、書き込み専用のうちのどれであるかは、アクセサーによって示されます。 クラスや構造体の場合とは異なり、本体なしでアクセサーを宣言した場合、[自動実装プロパティ](auto-implemented-properties.md)は宣言されません。 C# 8.0 以降では、インターフェイスによって、プロパティを含む、メンバーの既定の実装を定義できます。 インターフェイスでプロパティの既定の実装を定義することはまれです。インターフェイスでは、インスタンス データ フィールドが定義されないことがあるためです。

## <a name="example"></a>例

この例では、インターフェイス `IEmployee` に読み取り/書き込み専用のプロパティ `Name` および読み取り専用のプロパティ `Counter` があります。 クラス `Employee` は `IEmployee` インターフェイスを実装し、これら 2 つのプロパティを使用します。 プログラムは、新しい従業員の名前と現在の従業員の数を読み込んで、従業員の名前と計算した従業員番号を表示します。

メンバーが宣言されているインターフェイスを参照するプロパティの完全修飾名を使用することができます。 次に例を示します。

[!code-csharp[ExplicitProperties](~/samples/snippets/csharp/interfaces/properties.cs#ExplicitImplementation)]

前の例では、[明示的なインターフェイス実装](../interfaces/explicit-interface-implementation.md)を確認できます。 たとえば、`Employee` クラスが 2 つのインターフェイス `ICitizen` と `IEmployee` を実装し、両方のインターフェイスが同じ `Name` プロパティを持っている場合、明示的なインターフェイス メンバーの実装が必要です。 つまり、次のプロパティの宣言があります。

[!code-csharp[ExplicitProperties](~/samples/snippets/csharp/interfaces/properties.cs#ExplicitImplementation)]

これは、`IEmployee` インターフェイスで `Name` プロパティを実装します。次の宣言があります。

[!code-csharp[ExplicitProperties](~/samples/snippets/csharp/interfaces/properties.cs#CitizenImplementation)]

これは、`ICitizen` インターフェイスで `Name` プロパティを実装します。

[!code-csharp[Example](~/samples/snippets/csharp/interfaces/properties.cs#PropertyExample)]
[!code-csharp[Example](~/samples/snippets/csharp/interfaces/properties.cs#UseProperty)]

**`210 Hazem Abolrous`**

## <a name="sample-output"></a>出力例

```console
Enter number of employees: 210
Enter the name of the new employee: Hazem Abolrous
The employee information:
Employee number: 211
Employee name: Hazem Abolrous
```

## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [プロパティ](./properties.md)
- [プロパティの使用](./using-properties.md)
- [プロパティとインデクサーの比較](../indexers/comparison-between-properties-and-indexers.md)
- [インデクサー](../indexers/index.md)
- [インターフェイス](../interfaces/index.md)
