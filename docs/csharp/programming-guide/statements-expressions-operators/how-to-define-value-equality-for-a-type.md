---
title: 型の値の等価性を定義する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- overriding Equals method [C#]
- object equivalence [C#]
- Equals method [C#], overriding
- value equality [C#]
- equivalence [C#]
ms.assetid: 4084581e-b931-498b-9534-cf7ef5b68690
ms.openlocfilehash: 140be18698a40be8f394b31fcd42b97d6685cb98
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79157092"
---
# <a name="how-to-define-value-equality-for-a-type-c-programming-guide"></a>型の値の等価性を定義する方法 (C# プログラミング ガイド)

クラスまたは構造体を定義する場合は、型に値の等価性 (同値) のカスタム定義を作成することが有用かどうかを判断します。 通常、値の等価性を実装するのは、その型のオブジェクトがある種のコレクションに追加されることが想定されている場合、または、そのオブジェクトの主な目的が一連のフィールドまたはプロパティを格納することである場合です。 値の等価性は、型のすべてのフィールドおよびプロパティの比較に基づいて定義できます。また、サブセットに基づいて定義することもできます。

いずれの場合も、クラスおよび構造体の両方について、等価性を保証する 5 つの条件に従って実装する必要があります (次のルールの場合、`x`、`y`、`z` が null ではないと想定しています)。  
  
1. `x.Equals(x)` は、`true` を返します。 これは再帰プロパティと呼ばれます。  
  
2. `x.Equals(y)` からは `y.Equals(x)` と同じ値が返されます。 これは対照プロパティと呼ばれます。  
  
3. `(x.Equals(y) && y.Equals(z))` で `true` が返される場合、`x.Equals(z)` で `true` が返されます。 これは推移的プロパティと呼ばれます。  
  
4. `x.Equals(y)` が連続して呼び出された場合は、x と y によって参照されるオブジェクトが変更されていない限り、同じ値を返します。  
  
5. 非 null 値は null と等しくありません。 ただし、CLR ではすべてのメソッド呼び出しで null がチェックされ、`NullReferenceException` 参照が null の場合、`this` がスローされます。 そのため、`x.Equals(y)` では、`x` が null のときに例外がスローされます。 それにより、`Equals` の引数に基づき、ルール 1 または 2 が破られます。

 構造体を定義すると、<xref:System.ValueType?displayProperty=nameWithType> メソッドの <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> オーバーライドから継承された値の等価性が既定で実装されます。 この実装では、リフレクションを使用して、型のフィールドとプロパティをすべて調べます。 この実装によって正しい結果が生成されますが、その型専用に記述したカスタム実装と比較すると、処理にかなり時間がかかります。  
  
 値の等価性に関する実装の詳細は、クラスと構造体で異なりますが、 等価性を実装するための基本的な手順については、両方とも同じです。  
  
1. [仮想](../../language-reference/keywords/virtual.md) <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> メソッドをオーバーライドします。 ほとんどの場合、`bool Equals( object obj )` の実装には、`Equals` インターフェイスの実装である型固有の <xref:System.IEquatable%601?displayProperty=nameWithType> メソッドを呼び出すだけで済みます (手順 2 を参照)。  
  
2. 型固有の <xref:System.IEquatable%601?displayProperty=nameWithType> メソッドを指定して、`Equals` インターフェイスを実装します。 ここで実際の等価性の比較を実行します。 たとえば、型のフィールドを 1 ～ 2 個だけ比較することで等価性を定義できます。 `Equals`から例外をスローしないでください。 クラスの場合に限り、このメソッドはクラスで宣言されているフィールドのみを調べます。 基底クラスに含まれるフィールドを調べるには、`base.Equals` を呼び出す必要があります (<xref:System.Object> から型が直接継承された場合は、この呼び出しを行わないでください。<xref:System.Object> の <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 実装では参照の等価性チェックが実行されるためです)。  
  
3. 推奨、ただし省略可能: [==](../../language-reference/operators/equality-operators.md#equality-operator-) 演算子および [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) 演算子をオーバーロードします。  
  
4. 値の等価性を持つ 2 つのオブジェクトによって同じハッシュ コードが生成されるように、<xref:System.Object.GetHashCode%2A?displayProperty=nameWithType> をオーバーライドします。  
  
5. 省略可能: "大なり" または "小なり" の定義をサポートするには、型に対して <xref:System.IComparable%601> インターフェイスを実装したうえで、[<=](../../language-reference/operators/comparison-operators.md#less-than-or-equal-operator-) 演算子および [>=](../../language-reference/operators/comparison-operators.md#greater-than-or-equal-operator-) 演算子をオーバーロードします。  
  
 次に示す最初の例は、クラスの実装です。 2 番目の例は、構造体の実装を示しています。  

## <a name="example"></a>例

 次の例は、クラス (参照型) で値の等価性を実装する方法を示しています。  
  
 [!code-csharp[csProgGuideStatements#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#19)]  
  
 クラス (参照型) の場合、両方の <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> メソッドの既定の実装で、参照の等価性の比較は実行されますが、値の等価性のチェックは実行されません。 実装が仮想メソッドをオーバーライドする場合、その目的は、仮想メソッドに値の等価性のセマンティクスを提供することです。  
  
 `==` 演算子と `!=` 演算子は、オーバーロードされなくてもクラスで使用できます。 ただし、既定の動作として参照の等価性のチェックが実行されます。 クラスで `Equals` メソッドをオーバーロードする場合は、`==` 演算子と `!=` 演算子をオーバーロードすることをお勧めしますが、必須ではありません。  

## <a name="example"></a>例

 次の例は、構造体 (値型) で値の等価性を実装する方法を示しています。  
  
 [!code-csharp[csProgGuideStatements#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#20)]  
  
 構造体の場合、<xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> (<xref:System.ValueType?displayProperty=nameWithType> でオーバーライドされるバージョン) の既定の実装で、リフレクションを使用して値の等価性のチェックが実行され、型のすべてのフィールドの値が比較されます。 実装が構造体の仮想 `Equals` メソッドをオーバーライドする場合、その目的は、値の等価性のチェックをより効率的に実行することと、オプションで、構造体のフィールドまたはプロパティの一部のサブセットに基づいて比較を行うことです。  
  
 [==](../../language-reference/operators/equality-operators.md#equality-operator-) 演算子および [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) 演算子は、構造体が明示的にその演算子をオーバーロードしない限り、その構造体を操作できません。  
  
## <a name="see-also"></a>参照

- [等価比較](equality-comparisons.md)
- [C# プログラミング ガイド](../index.md)
