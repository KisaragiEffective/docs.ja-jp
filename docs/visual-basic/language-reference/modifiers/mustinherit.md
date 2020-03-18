---
title: MustInherit
ms.date: 07/20/2015
f1_keywords:
- MustInherit
- vb.MustInherit
helpviewer_keywords:
- classes [Visual Basic], abstract
- MustInherit classes [Visual Basic], MustInherit keyword
- abstract classes [Visual Basic], MustInherit class
- MustInherit keyword [Visual Basic]
ms.assetid: b8f05185-90e3-4dd7-adc2-90d852fab5b4
ms.openlocfilehash: 30befaaf194d78d26a57f29c59bf0a603e9f07a3
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351496"
---
# <a name="mustinherit-visual-basic"></a>MustInherit (Visual Basic)
クラスを基底クラスとしてのみ使用でき、そこからオブジェクトを直接作成することはできないことを指定します。  
  
## <a name="remarks"></a>コメント  
 *基底クラス*(*抽象クラス*とも呼ばれます) の目的は、このクラスから派生したすべてのクラスに共通の機能を定義することです。 これにより、派生クラスで共通の要素を再定義する必要がなくなります。 場合によっては、この共通機能は使用可能なオブジェクトを作成するのに十分ではないため、各派生クラスは不足している機能を定義します。 このような場合は、コンシューマー側のコードで派生クラスからのみオブジェクトを作成する必要があります。 これを適用するには、基本クラスで `MustInherit` を使用します。  
  
 `MustInherit` クラスのもう1つの用途は、変数を関連クラスのセットに制限することです。 基本クラスを定義し、そこからすべての関連クラスを派生させることができます。 基底クラスは、すべての派生クラスに共通する機能を提供する必要はありませんが、変数に値を代入するためのフィルターとして使用できます。 コンシューマーコードが基底クラスとして変数を宣言している場合、Visual Basic によって、派生クラスのいずれかからその変数にオブジェクトのみを割り当てることができます。  
  
 .NET Framework は、<xref:System.Array>、<xref:System.Enum>、<xref:System.ValueType>の複数の `MustInherit` クラスを定義します。 <xref:System.ValueType> は、変数を制限する基底クラスの例です。 すべての値型は <xref:System.ValueType>から派生します。 変数を <xref:System.ValueType>として宣言する場合は、その変数に値型のみを割り当てることができます。  
  
## <a name="rules"></a>ルール  
  
- **宣言コンテキスト。** `MustInherit` は、`Class` ステートメントでのみ使用できます。  
  
- **結合された修飾子。** 同じ宣言内の `NotInheritable` と共に `MustInherit` を指定することはできません。  
  
## <a name="example"></a>例  
 強制継承と強制オーバーライドの両方の例を次に示します。 基本クラス `shape` は `acrossLine`変数を定義します。 クラス `circle` および `square` は `shape`から派生します。 `acrossLine`の定義を継承しますが、その計算は図形の種類によって異なるため、関数 `area` を定義する必要があります。  
  
 [!code-vb[VbVbalrKeywords#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#2)]  
  
 `shape1` を宣言して `shape`型で `shape2` することができます。 ただし、`shape` からオブジェクトを作成することはできません。これは、関数 `area` の機能がなく、`MustInherit`としてマークされているためです。  
  
 これらは `shape`として宣言されているため、`shape1` と `shape2` の変数は、派生クラス `circle` および `square`のオブジェクトに限定されます。 Visual Basic では、これらの変数に他のオブジェクトを割り当てることはできません。これにより、高いレベルのタイプセーフが得られます。  
  
## <a name="usage"></a>使用法  
 このコンテキストでは、`MustInherit` 修飾子を使用できます。  
  
 [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)  
  
## <a name="see-also"></a>参照

- [Inherits ステートメント](../../../visual-basic/language-reference/statements/inherits-statement.md)
- [NotInheritable](../../../visual-basic/language-reference/modifiers/notinheritable.md)
- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
- [継承の基本](../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)
