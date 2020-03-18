---
title: '方法 : 2 つのオブジェクトが関連しているかどうかを決める'
ms.date: 07/20/2015
helpviewer_keywords:
- inheritance [Visual Basic], Visual Basic objects
- objects [Visual Basic], inheritance
- object variables [Visual Basic], determining relation
ms.assetid: da002e3f-6616-4bad-a229-f842d06652bb
ms.openlocfilehash: b3f5fc017166ba9cf28359db5de850c81b73bd69
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348630"
---
# <a name="how-to-determine-whether-two-objects-are-related-visual-basic"></a>方法: 2 つのオブジェクトが関連しているかどうかを判別する (Visual Basic)

2つのオブジェクトを比較して、作成元のクラス間のリレーションシップ (存在する場合) を判断できます。 <xref:System.Type?displayProperty=nameWithType> クラスの <xref:System.Type.IsInstanceOfType%2A> メソッドは、指定したクラスが現在のクラスから継承している場合、または現在の型が指定したクラスでサポートされているインターフェイスである場合に `True` を返します。

### <a name="to-determine-if-one-object-inherits-from-another-objects-class-or-interface"></a>あるオブジェクトが別のオブジェクトのクラスまたはインターフェイスから継承しているかどうかを確認するには

1. 基本型と考えられるオブジェクトで、<xref:System.Object.GetType%2A> メソッドを呼び出します。

2. <xref:System.Object.GetType%2A>によって返される <xref:System.Type?displayProperty=nameWithType> オブジェクトで、<xref:System.Type.IsInstanceOfType%2A> メソッドを呼び出します。

3. <xref:System.Type.IsInstanceOfType%2A>の引数リストで、派生型として考えられるオブジェクトを指定します。

    引数の型が <xref:System.Type?displayProperty=nameWithType> オブジェクト型から継承されている場合、<xref:System.Type.IsInstanceOfType%2A> は `True` を返します。

## <a name="example"></a>例
 次の例では、あるオブジェクトが、別のオブジェクトのクラスから派生したクラスを表しているかどうかを判断します。

```vb
Public Class baseClass
End Class
Public Class derivedClass : Inherits baseClass
End Class
Public Class testTheseClasses
    Public Sub seeIfRelated()
        Dim baseObj As Object = New baseClass()
        Dim derivedObj As Object = New derivedClass()
        Dim related As Boolean
        related = baseObj.GetType().IsInstanceOfType(derivedObj)
        MsgBox(CStr(related))
    End Sub
End Class
```

<xref:System.Type.IsInstanceOfType%2A>の呼び出しで、2つのオブジェクト変数が予期せずに配置されていることに注意してください。 想定される基本型を使用して <xref:System.Type?displayProperty=nameWithType> クラスを生成し、想定される派生型を引数として <xref:System.Type.IsInstanceOfType%2A> メソッドに渡します。

## <a name="see-also"></a>参照

- <xref:System.Object.GetType%2A>
- <xref:System.Type?displayProperty=nameWithType>
- <xref:System.Type.IsInstanceOfType%2A>
- [Object データ型](../../../../visual-basic/language-reference/data-types/object-data-type.md)
- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [オブジェクト変数の値](../../../../visual-basic/programming-guide/language-features/variables/object-variable-values.md)
- [方法: 2 つのオブジェクトが同一であるかどうか判別する](../../../../visual-basic/programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-identical.md)
