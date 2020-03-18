---
title: Of 句
ms.date: 07/20/2015
f1_keywords:
- Of
- vb.Of
- vb.of
helpviewer_keywords:
- Of keyword [Visual Basic]
- arguments [Visual Basic], data types
- constraints, Visual Basic generic types
- generic parameters
- generics [Visual Basic], constraints
- parameters [Visual Basic], type
- types [Visual Basic], generic
- parameters [Visual Basic], generic
- type parameters
- data type arguments
ms.assetid: 0db8f65c-65af-4089-ab7f-6fcfecb60444
ms.openlocfilehash: d88c43efe858d6b81b7d8d2470b234ff5d40632a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353839"
---
# <a name="of-clause-visual-basic"></a>Of 句 (Visual Basic)
では、*ジェネリック*クラス、構造体、インターフェイス、デリゲート、またはプロシージャの*型パラメーター*を識別する `Of` 句が導入されています。 ジェネリック型の詳細については、「[Visual Basic におけるジェネリック型](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)」を参照してください。  
  
## <a name="using-the-of-keyword"></a>Of キーワードの使用  
 次のコード例では、`Of` キーワードを使用して、2つの型パラメーターを受け取るクラスのアウトラインを定義します。 `keyType` パラメーターは <xref:System.IComparable> インターフェイスによって*制限*されます。つまり、コンシューマー側のコードは <xref:System.IComparable>を実装する型引数を指定する必要があります。 これは、`add` プロシージャが <xref:System.IComparable.CompareTo%2A?displayProperty=nameWithType> メソッドを呼び出すことができるようにするために必要です。 制約の詳細については、「[型リスト](../../../visual-basic/language-reference/statements/type-list.md)」をご覧ください。  
  
```vb  
Public Class Dictionary(Of entryType, keyType As IComparable)  
    Public Sub add(ByVal e As entryType, ByVal k As keyType)  
        Dim dk As keyType  
        If k.CompareTo(dk) = 0 Then  
        End If  
    End Sub  
    Public Function find(ByVal k As keyType) As entryType  
    End Function  
End Class  
```  
  
 前のクラス定義を完了すると、そこからさまざまな `dictionary` クラスを構築できます。 `entryType` するために指定する型は、クラスに保持されているエントリの種類と、各エントリに関連付けられているキーの種類を決定 `keyType` ます。 制約があるため、<xref:System.IComparable>を実装する型を `keyType` するには、を指定する必要があります。  
  
 次のコード例では、`String` エントリを保持し、`Integer` キーをそれぞれに関連付けるオブジェクトを作成します。 `Integer` は <xref:System.IComparable> を実装するため、`keyType`の制約を満たすことになります。  
  
```vb  
Dim d As New dictionary(Of String, Integer)  
```  
  
 キーワード `Of` は次のコンテキストで使用できます。  
  
 [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)  
  
 [Delegate ステートメント](../../../visual-basic/language-reference/statements/delegate-statement.md)  
  
 [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)  
  
 [Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md)  
  
 [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)  
  
 [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)  
  
## <a name="see-also"></a>参照

- <xref:System.IComparable>
- [型リスト](../../../visual-basic/language-reference/statements/type-list.md)
- [Visual Basic におけるジェネリック型](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [In](../../../visual-basic/language-reference/modifiers/in-generic-modifier.md)
- [Out](../../../visual-basic/language-reference/modifiers/out-generic-modifier.md)
