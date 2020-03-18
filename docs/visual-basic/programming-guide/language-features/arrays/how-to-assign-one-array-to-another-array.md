---
title: '方法 : 配列を別の配列に代入する'
ms.date: 07/20/2015
helpviewer_keywords:
- covariance, arrays
- arrays [Visual Basic], assigning
- arrays [Visual Basic], covariance
ms.assetid: 1ae89ea5-f292-4282-bcfc-e9b06b37fbd5
ms.openlocfilehash: be5337e36c2cc7ad9f9b32182b8575ac66bb4a50
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351890"
---
# <a name="how-to-assign-one-array-to-another-array-visual-basic"></a>方法: 配列を別の配列に代入する (Visual Basic)

配列はオブジェクトであるため、他の種類のオブジェクトと同様に代入ステートメントで使用できます。 配列変数は、配列要素とランクおよび長さの情報を構成するデータへのポインターを保持し、割り当てによってこのポインターのみがコピーされます。

### <a name="to-assign-one-array-to-another-array"></a>1つの配列を別の配列に割り当てるには

1. 2つの配列のランク (次元の数) と互換性のある要素のデータ型が同じであることを確認します。

2. 標準の代入ステートメントを使用して、コピー元の配列をコピー先の配列に代入します。 配列名のいずれかをかっこで囲んでください。

    ```vb
    Dim formArray() As System.Windows.Forms.Form
    Dim controlArray() As System.Windows.Forms.Control
    controlArray = formArray
    ```

1つの配列を別の配列に割り当てる場合は、次の規則が適用されます。

- **ランクが等しいこと。** コピー先の配列のランク (次元数) は、ソース配列と同じである必要があります。

  2つの配列のランクが等しい場合、次元は同じである必要はありません。 指定されたディメンション内の要素の数は、割り当て時に変更される可能性があります。

- **要素の型。** 両方の配列に*参照型*の要素が含まれているか、両方の配列が*値の型*の要素を持っている必要があります。 詳細については、「[値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)」を参照してください。

  - 両方の配列に値型の要素がある場合、要素のデータ型はまったく同じである必要があります。 唯一の例外は、`Enum` 要素の配列を、その `Enum`の基本型の配列に割り当てることができることです。

  - 両方の配列に参照型の要素がある場合、ソース要素の型は、変換先の要素の型から派生する必要があります。 この場合、2つの配列は、要素と同じ継承関係を持ちます。 これは、*配列の共変性*と呼ばれます。

上記の規則に違反すると、コンパイラはエラーを報告します。たとえば、データ型に互換性がない場合や、ランクが等しくない場合などです。 コードにエラー処理を追加して、割り当てを試行する前に配列に互換性があることを確認できます。 例外がスローされないようにする場合は、 [TryCast Operator](../../../../visual-basic/language-reference/operators/trycast-operator.md)キーワードを使用することもできます。

## <a name="see-also"></a>関連項目

- [配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
- [配列のトラブルシューティング](../../../../visual-basic/programming-guide/language-features/arrays/troubleshooting-arrays.md)
- [Enum ステートメント](../../../../visual-basic/language-reference/statements/enum-statement.md)
- [配列変換](../../../../visual-basic/programming-guide/language-features/data-types/array-conversions.md)
