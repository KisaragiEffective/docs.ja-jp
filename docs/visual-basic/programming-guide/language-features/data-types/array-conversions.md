---
title: 配列の変換
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [Visual Basic], converting type
- type conversion [Visual Basic], arrays
- conversions [Visual Basic], type
- arrays [Visual Basic], data types
- conversions [Visual Basic], data type
- object arrays [Visual Basic], converting type
- data type conversion [Visual Basic], array conversions
- conversions [Visual Basic], array types
- object arrays
ms.assetid: fceff7d2-a1b7-44c7-b9aa-8bd831d8a444
ms.openlocfilehash: 622ebe8a77f2dfbeb35e0408be48622d93d409c6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345858"
---
# <a name="array-conversions-visual-basic"></a>配列の変換 (Visual Basic)
次の条件を満たしている場合は、配列型を別の配列型に変換できます。  
  
- **ランクが等しい。** 2 つの配列のランクは同じである必要があります。つまり、次元数が同じである必要があります。 ただし、それぞれの次元の長さは同じである必要はありません。  
  
- **要素のデータ型。** 両方の配列の要素のデータ型は、参照型である必要があります。 少なくとも 1 つの値型が関係しているため、`Integer` 配列を `Long` 配列に変換したり、`Object` 配列に変換したりすることはできません。 詳細については、「[値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)」を参照してください。  
  
- **変換性。 ** 2 つの配列の要素型の間で、拡大または縮小のいずれかの変換を実行できる必要があります。 この要件を満たさない例として、`String` 配列と <xref:System.Attribute?displayProperty=nameWithType>から派生したクラスの配列との間での変換が試行されます。 これらの2つの型は共通していません。これらの型の間にはどのような種類の変換もありません。  
  
 ある配列型から別の配列型への変換は、それぞれの要素の変換が拡大または縮小のどちらであるかによって、拡大または縮小されます。 詳細については、「[拡大変換と縮小変換](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)」を参照してください。  
  
## <a name="conversion-to-an-object-array"></a>オブジェクト配列への変換  
 初期化せずに `Object` 配列を宣言すると、初期化されていない限り、その要素の型は `Object` ます。 特定のクラスの配列に設定すると、そのクラスの型が使用されます。 ただし、基になる型はまだ `Object`であり、その後、関連のないクラスの別の配列に設定できます。 すべてのクラスは `Object`から派生するため、任意のクラスから他のクラスに配列の要素型を変更できます。  
  
 次の例では `student` と `String`型の間に変換は存在しませんが、両方とも `Object`から派生しているので、すべての代入が有効です。  
  
```vb  
' Assume student has already been defined as a class.  
Dim testArray() As Object  
' testArray is still an Object array at this point.  
Dim names() As String = New String(3) {"Name0", "Name1", "Name2", "Name3"}  
testArray = New student(3) {}  
' testArray is now of type student().  
testArray = names  
' testArray is now a String array.  
```  
  
### <a name="underlying-type-of-an-array"></a>配列の基になる型  
 最初に特定のクラスを使用して配列を宣言した場合、その基になる要素型がそのクラスになります。 後で別のクラスの配列に設定する場合は、2つのクラス間の変換が必要です。  
  
 次の例では、`students` は `student` 配列です。 `String` と `student`の間に変換が存在しないため、最後のステートメントは失敗します。  
  
```vb  
Dim students() As student  
Dim names() As String = New String(3) {"Name0", "Name1", "Name2", "Name3"}  
students = New Student(3) {}  
' The following statement fails at compile time.  
students = names  
```  
  
## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [Visual Basic における型変換](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
- [暗黙の型変換と明示的な型変換](../../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [文字列とその他の型との変換](../../../../visual-basic/programming-guide/language-features/data-types/conversions-between-strings-and-other-types.md)
- [方法: Visual Basic でオブジェクトを別の型に変換する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-convert-an-object-to-another-type.md)
- [データの種類](../../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
