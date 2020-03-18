---
title: DebugView プロパティで使用される構文
description: 式ツリーを文字列で表現する目的で DebugView プロパティにより使用される特別な構文について説明します。
author: zspitz
ms.author: wiwagn
ms.date: 05/22/2019
ms.topic: reference
helpviewer_keywords:
- expression trees
- debugview
ms.openlocfilehash: 98ceba37aa226fab68ae1c1028e2a1139b3b8e7e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346876"
---
# <a name="debugview-syntax"></a>`DebugView` の構文

`DebugView` プロパティ (デバッグ時にのみ利用可能) により、式ツリーが文字列でレンダリングされます。 構文の大部分はかなりわかりやすいです。特別なケースについて以降のセクションで説明します。

各例には、`DebugView`を含むコメントブロックが続きます。

## <a name="parameterexpression"></a>ParameterExpression

<xref:System.Linq.Expressions.ParameterExpression?displayProperty=nameWithType> 変数名は、先頭に記号 "$" を付けて表示されます。

パラメーターに名前がない場合、`$var1` や `$var2` など、自動的に生成された名前が割り当てられます。

### <a name="examples"></a>使用例

```vb
Dim numParam As ParameterExpression = Expression.Parameter(GetType(Integer), "num")
'
'    $num
'

Dim numParam As ParameterExpression = Expression.Parameter(GetType(Integer))
'
'    $var1
'
```

## <a name="constantexpressions"></a>ConstantExpressions

整数値、文字列、および <xref:System.Linq.Expressions.ConstantExpression?displayProperty=nameWithType> を表す `null` オブジェクトの場合、定数の値が表示されます。

数値型によっては、次のような値にサフィックスが追加されます。

| 型 | Keyword | Suffix |
|--|--|--|
| <xref:System.UInt32> | [UInteger](../../../language-reference/data-types/uinteger-data-type.md) | U |
| <xref:System.Int64> | [Long](../../../language-reference/data-types/long-data-type.md) | L |
| <xref:System.UInt64> | [ULong](../../../language-reference/data-types/ulong-data-type.md) | UL |
| <xref:System.Double> | [Double](../../../language-reference/data-types/double-data-type.md) | D |
| <xref:System.Single> | [Single](../../../language-reference/data-types/single-data-type.md) | F |
| <xref:System.Decimal> | [Decimal](../../../language-reference/data-types/decimal-data-type.md) | M |

### <a name="examples"></a>使用例

```vb
Dim num as Integer = 10
Dim expr As ConstantExpression = Expression.Constant(num)
'
'    10
'

Dim num As Double = 10
Dim expr As ConstantExpression = Expression.Constant(num)
'
'    10D
'
```

## <a name="blockexpression"></a>BlockExpression

<xref:System.Linq.Expressions.BlockExpression?displayProperty=nameWithType> オブジェクトの型がブロック内の最後の式の型と異なる場合、その型が山かっこ (`<` と `>`) 内に表示されます。 それ以外の場合、<xref:System.Linq.Expressions.BlockExpression> オブジェクトの型は表示されません。

### <a name="examples"></a>使用例

```vb
Dim block As BlockExpression = Expression.Block(Expression.Constant("test"))
'
'    .Block() {
'        "test"
'    }
'

Dim block As BlockExpression = Expression.Block(
    GetType(Object),
    Expression.Constant("test")
)
'
'    .Block<System.Object>() {
'        "test"
'    }
'
```

## <a name="lambdaexpression"></a>LambdaExpression

<xref:System.Linq.Expressions.LambdaExpression?displayProperty=nameWithType> オブジェクトは、デリゲート型と共に表示されます。

ラムダ式に名前がない場合、`#Lambda1` や `#Lambda2` など、自動的に生成された名前が割り当てられます。

### <a name="examples"></a>使用例

```vb
Dim lambda As LambdaExpression = Expression.Lambda(Of Func(Of Integer))(
    Expression.Constant(1)
)
'
'    .Lambda #Lambda1<System.Func'1[System.Int32]>() {
'        1
'    }
'

Dim lambda As LambdaExpression = Expression.Lambda(Of Func(Of Integer))(
    Expression.Constant(1),
    "SampleLambda",
    Nothing
)
'
'    .Lambda #SampleLambda<System.Func'1[System.Int32]>() {
'        1
'    }
'
```

## <a name="labelexpression"></a>LabelExpression

<xref:System.Linq.Expressions.LabelExpression?displayProperty=nameWithType> オブジェクトの既定値を指定した場合、その値が <xref:System.Linq.Expressions.LabelTarget?displayProperty=nameWithType> オブジェクトの前に表示されます。

`.Label` トークンは、ラベルの開始を示します。 `.LabelTarget` トークンは、ジャンプ先のターゲットを示します。

ラベルに名前がない場合、`#Label1` や `#Label2` など、自動的に生成された名前が割り当てられます。

### <a name="examples"></a>使用例

```vb
Dim target As LabelTarget = Expression.Label(GetType(Integer), "SampleLabel")
Dim label1 As BlockExpression = Expression.Block(
    Expression.Goto(target, Expression.Constant(0)),
    Expression.Label(target, Expression.Constant(-1))
)
'
'    .Block() {
'        .Goto SampleLabel { 0 };
'        .Label
'            -1
'        .LabelTarget SampleLabel:
'    }
'

Dim target As LabelTarget = Expression.Label()
Dim block As BlockExpression = Expression.Block(
    Expression.Goto(target),
    Expression.Label(target)
)
'
'    .Block() {
'        .Goto #Label1 { };
'        .Label
'        .LabelTarget #Label1:
'    }
'
```

## <a name="checked-operators"></a>checked 演算子

checked 演算子は、演算子の前に `#` 記号が付く形式で表示されます。 たとえば、checked 加算演算子は `#+` と表示されます。

### <a name="examples"></a>使用例

```vb
Dim expr As Expression = Expression.AddChecked(
    Expression.Constant(1),
    Expression.Constant(2)
)
'
'     1 #+ 2
'

Dim expr As Expression = Expression.ConvertChecked(
    Expression.Constant(10.0),
    GetType(Integer)
)
'
'    #(System.Int32)10D
'
```
