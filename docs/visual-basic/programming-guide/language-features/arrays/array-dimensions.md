---
title: 配列の次元
ms.date: 07/20/2015
helpviewer_keywords:
- dimensions, arrays
- arrays [Visual Basic], dimensions
- arrays [Visual Basic], rectangular
- arrays [Visual Basic], rank
- rectangular arrays
- ranking, arrays
ms.assetid: 385e911b-18c1-4e98-9924-c6d279101dd9
ms.openlocfilehash: 12e983ae62fa9f9ea762d434ffe5b73873a4a2e8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351908"
---
# <a name="array-dimensions-in-visual-basic"></a>Visual Basic における配列のディメンション

*ディメンション*は、配列の要素の指定を変更できる方向です。 月の各日の売上合計を保持する配列には、1つのディメンション (その月の日付) があります。 月の各日の売上合計を部門別に保持する配列には、2つのディメンション (部署番号とその月の日) があります。 配列が持つ次元の数を*ランク*と呼びます。

> [!NOTE]
> <xref:System.Array.Rank%2A> プロパティを使用して、配列の次元数を確認できます。

## <a name="working-with-dimensions"></a>ディメンションの操作

配列の要素を指定するには、各次元の*インデックス*または*添字*を指定します。 要素は、インデックス0からそのディメンションの最大インデックスまで、各ディメンションに沿って連続しています。

次の図は、異なるランクを持つ配列の概念構造を示しています。 図の各要素は、それにアクセスするインデックス値を示しています。 たとえば、インデックス `(1, 0)`を指定することにより、2次元配列の2番目の行の最初の要素にアクセスできます。

![1次元配列を示す図。](./media/array-dimensions/one-dimensional-array.gif)

![2次元配列を示す図。](./media/array-dimensions/two-dimensional-array.gif)

![3次元配列を示す図。](./media/array-dimensions/three-dimensional-array.gif)

### <a name="one-dimension"></a>1つのディメンション

多くの配列には、各年齢の人の数など、ディメンションが1つしかありません。 要素を指定する唯一の要件は、その要素がカウントを保持する期間です。 したがって、このような配列では、インデックスを1つだけ使用します。 次の例では、0 ~ 120 の年齢の年齢カウントの*1 次元配列*を保持する変数を宣言しています。

```vb
Dim ageCounts(120) As UInteger
```

### <a name="two-dimensions"></a>2つのディメンション

一部の配列には、キャンパス上の各建物の各フロアのオフィス数など、2つの次元があります。 要素の指定には、ビル番号とフロアの両方が必要です。各要素には、建物と床の組み合わせの数が保持されます。 そのため、このような配列では2つのインデックスを使用します。 次の例では、オフィス数が 0 ~ 40 で、床が 0 ~ 5 の*2 次元配列*を保持する変数を宣言しています。

```vb
Dim officeCounts(40, 5) As Byte
```

2次元配列は、*四角形配列*とも呼ばれます。

### <a name="three-dimensions"></a>3次元

3次元空間の値など、いくつかの配列には3つの次元があります。 このような配列では、3つのインデックスが使用されます。この場合、物理空間の x、y、z 座標を表します。 次の例では、3次元ボリュームのさまざまな点で、航空温度の*3 次元配列*を保持する変数を宣言しています。

```vb
Dim airTemperatures(99, 99, 24) As Single
```

### <a name="more-than-three-dimensions"></a>3次元以上

配列の次元は最大で32次元ですが、少なくとも3つを超えることはほとんどありません。

> [!NOTE]
> 配列にディメンションを追加すると、配列に必要なストレージの合計容量が大幅に増加するので、多次元配列を使用することを考慮してください。

## <a name="using-different-dimensions"></a>異なる次元の使用

今月の各日の売上高を追跡するとします。 次の例に示すように、31個の要素を持つ1次元配列を、その月の各日に1つ宣言できます。

```vb
Dim salesAmounts(30) As Double
```

次に、1か月のすべての日についてだけでなく、すべての月についても同じ情報を追跡するとします。 次の例に示すように、12行 (月) と31列 (曜日) を含む2次元配列を宣言できます。

```vb
Dim salesAmounts(11, 30) As Double
```

ここで、配列に複数の年の情報を保持することにしたとします。 5年間の売上高を追跡する場合は、次の例に示すように、5つのレイヤー、12行、および31列を含む3次元配列を宣言できます。

```vb
Dim salesAmounts(4, 11, 30) As Double
```

各インデックスは0から最大値まで変化するため、`salesAmounts` の各次元は、その次元で必要な長さより1つの値として宣言されます。 また、新しいディメンションごとに配列のサイズが増加することにも注意してください。 前の例の3つのサイズは、それぞれ31、372、および1860要素です。

> [!NOTE]
> `Dim` ステートメントまたは `New` 句を使用せずに配列を作成できます。 たとえば、<xref:System.Array.CreateInstance%2A> メソッドを呼び出すことができます。または、別のコンポーネントが、この方法で作成された配列をコードに渡すことができます。 このような配列は、0以外の下限を持つことができます。 <xref:System.Array.GetLowerBound%2A> メソッドまたは `LBound` 関数を使用すると、ディメンションの下限をいつでもテストできます。

## <a name="see-also"></a>参照

- [配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
- [配列のトラブルシューティング](../../../../visual-basic/programming-guide/language-features/arrays/troubleshooting-arrays.md)
