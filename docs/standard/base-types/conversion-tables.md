---
title: .NET の型変換の表
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- widening conversions
- narrowing conversions
- type conversion, table
- converting types, narrowing conversions
- converting types, widening conversions
- base types, converting
- tables [.NET Framework], type conversions
- data types [.NET Framework], converting
ms.assetid: 0ea65c59-85eb-4a52-94ca-c36d3bd13058
ms.openlocfilehash: aa1ef8397338af949bd147fd3252b2d9ecaf53ef
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73103890"
---
# <a name="type-conversion-tables-in-net"></a>.NET の型変換の表
拡大変換は、1 つの型の値が、サイズが同じかそれ以上の別の型に変換されるときに発生します。 縮小変換は、1 つの型の値が、サイズがより小さい別の型の値に変換されるときに発生します。 このトピックの表は、両方の種類の変換の動作を示しています。  
  
## <a name="widening-conversions"></a>拡大変換  
 次の表は、情報を失うことなく実行できる拡大変換を示しています。  
  
|[種類]|データ損失なしに次の型に変換可能|  
|----------|-------------------------------------------|  
|<xref:System.Byte>|<xref:System.UInt16>, <xref:System.Int16>, <xref:System.UInt32>, <xref:System.Int32>, <xref:System.UInt64>, <xref:System.Int64>, <xref:System.Single>, <xref:System.Double>, <xref:System.Decimal>|  
|<xref:System.SByte>|<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.Single>、<xref:System.Double>、<xref:System.Decimal>|  
|<xref:System.Int16>|<xref:System.Int32>, <xref:System.Int64>, <xref:System.Single>, <xref:System.Double>, <xref:System.Decimal>|  
|<xref:System.UInt16>|<xref:System.UInt32>､<xref:System.Int32>、<xref:System.UInt64>、<xref:System.Int64>、<xref:System.Single>、<xref:System.Double>、<xref:System.Decimal>|  
|<xref:System.Char>|<xref:System.UInt16>, <xref:System.UInt32>, <xref:System.Int32>, <xref:System.UInt64>, <xref:System.Int64>, <xref:System.Single>, <xref:System.Double>, <xref:System.Decimal>|  
|<xref:System.Int32>|<xref:System.Int64>、 <xref:System.Double>、 <xref:System.Decimal>|  
|<xref:System.UInt32>|<xref:System.Int64>、<xref:System.UInt64>、<xref:System.Double>, <xref:System.Decimal>|  
|<xref:System.Int64>|<xref:System.Decimal>|  
|<xref:System.UInt64>|<xref:System.Decimal>|  
|<xref:System.Single>|<xref:System.Double>|  
  
 一部の <xref:System.Single> または <xref:System.Double> への拡大変換では、精度が損なわれる可能性があります。 次の表は、情報が失われる可能性のある拡大変換を示しています。  
  
|[種類]|次の型に変換可能|  
|----------|-------------------------|  
|<xref:System.Int32>|<xref:System.Single>|  
|<xref:System.UInt32>|<xref:System.Single>|  
|<xref:System.Int64>|<xref:System.Single>, <xref:System.Double>|  
|<xref:System.UInt64>|<xref:System.Single>, <xref:System.Double>|  
|<xref:System.Decimal>|<xref:System.Single>, <xref:System.Double>|  
  
## <a name="narrowing-conversions"></a>縮小変換  
 <xref:System.Single> または <xref:System.Double> への縮小変換では、情報が失われる可能性があります。 ターゲット型がソースの大きさを正確に表現できない場合、結果の型は定数 `PositiveInfinity` または `NegativeInfinity` に設定されます。 `PositiveInfinity` は、正の数を 0 で除算した結果であり、<xref:System.Single> または <xref:System.Double> の値が `MaxValue` フィールドの値を超える場合にも返されます。 `NegativeInfinity` は、負の数を 0 で除算した結果であり、<xref:System.Single> または <xref:System.Double> の値が `MinValue` フィールドの値を下回る場合にも返されます。 <xref:System.Double> から <xref:System.Single> への変換は、`PositiveInfinity` または `NegativeInfinity` になる場合があります。  
  
 その他のデータ型についても、縮小変換によって情報が失われる可能性があります。 ただし、変換される型の値が、ターゲット型の <xref:System.OverflowException> フィールドと `MaxValue` フィールドで指定された範囲外にある場合は、`MinValue` がスローされます。また、ターゲット型の値がその `MaxValue` または `MinValue` を超えないことを確認するために、ランタイムによって変換がチェックされます。 <xref:System.Convert?displayProperty=nameWithType> クラスを使用して実行される変換は、常にこの方法でチェックされます。  
  
 次の表は、<xref:System.OverflowException> を使用して <xref:System.Convert?displayProperty=nameWithType> をスローする変換、または、変換される型の値が結果の型の定義済みの範囲外にあるかどうかのチェックを行うすべての変換を示しています。  
  
|[種類]|次の型に変換可能|  
|----------|-------------------------|  
|<xref:System.Byte>|<xref:System.SByte>|  
|<xref:System.SByte>|<xref:System.Byte>、<xref:System.UInt16>、<xref:System.UInt32>, <xref:System.UInt64>|  
|<xref:System.Int16>|<xref:System.Byte>、 <xref:System.SByte>、 <xref:System.UInt16>|  
|<xref:System.UInt16>|<xref:System.Byte>、 <xref:System.SByte>、 <xref:System.Int16>|  
|<xref:System.Int32>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>,<xref:System.UInt32>|  
|<xref:System.UInt32>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>|  
|<xref:System.Int64>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>,<xref:System.UInt32>,<xref:System.UInt64>|  
|<xref:System.UInt64>|<xref:System.Byte>､<xref:System.SByte>、<xref:System.Int16>、<xref:System.UInt16>、<xref:System.Int32>、<xref:System.UInt32>、<xref:System.Int64>|  
|<xref:System.Decimal>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>, <xref:System.UInt32>, <xref:System.Int64>, <xref:System.UInt64>|  
|<xref:System.Single>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>, <xref:System.UInt32>, <xref:System.Int64>, <xref:System.UInt64>|  
|<xref:System.Double>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>, <xref:System.UInt32>, <xref:System.Int64>, <xref:System.UInt64>|  
  
## <a name="see-also"></a>参照

- <xref:System.Convert?displayProperty=nameWithType>
- [.NET での型変換](../../../docs/standard/base-types/type-conversion.md)
