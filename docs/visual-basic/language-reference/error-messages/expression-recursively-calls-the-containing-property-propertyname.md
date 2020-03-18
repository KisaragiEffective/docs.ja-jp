---
title: 式は、含んでいるプロパティ '<propertyname>' を再帰的に呼び出します。
ms.date: 07/20/2015
f1_keywords:
- vbc42026
- BC42026
helpviewer_keywords:
- BC42026
ms.assetid: 4fde9db6-3bf3-48dc-8e05-981bf08969da
ms.openlocfilehash: 42177f22e632e4a05b1f0b4d934f3e56ab9ff0f2
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71698567"
---
# <a name="expression-recursively-calls-the-containing-property-propertyname"></a>式は、含んでいるプロパティ '\<propertyname > ' を再帰的に呼び出します
プロパティ定義の `Set` プロシージャ内のステートメントは、プロパティの名前に値を格納します。  
  
 プロパティの値を保持するには、プロパティのコンテナーで `Private` 変数を定義し、`Get` と `Set` の両方のプロシージャで使用することをお勧めします。 `Set` プロシージャは、この `Private` 変数に入力値を格納する必要があります。  
  
 `Get` プロシージャは、`Function` プロシージャと同じように動作するので、`End Get` ステートメントを使用して、プロパティ名に値を割り当て、制御を返すことができます。 ただし、`Private` 変数を[Return ステートメント](../../../visual-basic/language-reference/statements/return-statement.md)の値として含めることをお勧めします。  
  
 `Set` プロシージャは、値を返さない `Sub` プロシージャと同じように動作します。 したがって、プロシージャまたはプロパティの名前は `Set` プロシージャ内で特別な意味を持たず、値を格納することはできません。  
  
 次の例では、このエラーの原因となる可能性がある方法を示し、その後に推奨される方法を示します。  
  
```vb  
Public Class illustrateProperties  
' The code in the following property causes this error.  
    Public Property badProp() As Char  
        Get  
            Dim charValue As Char  
            ' Insert code to update charValue.  
            badProp = charValue  
        End Get  
        Set(ByVal Value As Char)  
            ' The following statement causes this error.  
            badProp = Value  
            ' The value stored in the local variable badProp  
            ' is not used by the Get procedure in this property.  
        End Set  
    End Property  
' The following code uses the recommended approach.  
    Private propValue As Char  
    Public Property goodProp() As Char  
        Get  
            ' Insert code to update propValue.  
            Return propValue  
        End Get  
        Set(ByVal Value As Char)  
            propValue = Value  
        End Set  
    End Property  
End Class  
```  
  
 既定では、このメッセージは警告です。 警告を非表示にする方法や、警告をエラーとして扱う方法の詳細については、「 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)」を参照してください。  
  
 **エラー ID:** BC42026  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- 前の例に示されているように、推奨される方法を使用するようにプロパティ定義を書き直してください。  
  
## <a name="see-also"></a>参照

- [Property プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/property-procedures.md)
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)
- [Set ステートメント](../../../visual-basic/language-reference/statements/set-statement.md)
