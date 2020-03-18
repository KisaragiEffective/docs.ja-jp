---
title: 構造体とその他のプログラミング要素
ms.date: 07/20/2015
helpviewer_keywords:
- structures [Visual Basic], arrays
- procedures [Visual Basic], structures as arguments to
- objects [Visual Basic], structure elements
- arrays [Visual Basic], structure elements
- nested structures [Visual Basic]
ms.assetid: 0f849313-ccd2-4c9a-acb9-69de6751c088
ms.openlocfilehash: 309d0e5214897675e1758bd98b964392b379ca1b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346120"
---
# <a name="structures-and-other-programming-elements-visual-basic"></a>構造体およびその他のプログラミング要素 (Visual Basic)
構造体は、配列、オブジェクト、およびプロシージャと組み合わせて使用することも、相互に使用することもできます。 相互作用は、これらの要素が個別に使用するのと同じ構文を使用します。  
  
> [!NOTE]
> 構造体の宣言で構造体の要素を初期化することはできません。 値は、構造体型として宣言されている変数の要素にのみ割り当てることができます。  
  
## <a name="structures-and-arrays"></a>構造体と配列  
 構造体には、配列を1つ以上の要素として含めることができます。 これを次の例に示します。  
  
```vb  
Public Structure systemInfo  
    Public cPU As String  
    Public memory As Long  
    Public diskDrives() As String  
    Public purchaseDate As Date  
End Structure   
```  
  
 構造体内の配列の値には、オブジェクトのプロパティにアクセスするのと同じ方法でアクセスします。 これを次の例に示します。  
  
```vb  
Dim mySystem As systemInfo  
ReDim mySystem.diskDrives(3)  
mySystem.diskDrives(0) = "1.44 MB"  
```  
  
 構造体の配列を宣言することもできます。 これを次の例に示します。  
  
```vb  
Dim allSystems(100) As systemInfo  
```  
  
 このデータアーキテクチャのコンポーネントにアクセスするには、同じ規則に従います。 これを次の例に示します。  
  
```vb  
ReDim allSystems(5).diskDrives(3)  
allSystems(5).CPU = "386SX"  
allSystems(5).diskDrives(2) = "100M SCSI"  
```  
  
## <a name="structures-and-objects"></a>構造体とオブジェクト  
 構造体には、オブジェクトを1つ以上の要素として含めることができます。 これを次の例に示します。  
  
```vb  
Protected Structure userInput  
    Public userName As String  
    Public inputForm As System.Windows.Forms.Form  
    Public userFileNumber As Integer  
End Structure  
```  
  
 `Object`ではなく、このような宣言では特定のオブジェクトクラスを使用する必要があります。  
  
## <a name="structures-and-procedures"></a>構造体とプロシージャ  
 プロシージャ引数として構造体を渡すことができます。 これを次の例に示します。  
  
```vb  
Public currentCPUName As String = "700MHz Pentium compatible"  
Public currentMemorySize As Long = 256  
Public Sub fillSystem(ByRef someSystem As systemInfo)  
    someSystem.cPU = currentCPUName  
    someSystem.memory = currentMemorySize  
    someSystem.purchaseDate = Now  
End Sub  
```  
  
 前の例では、*参照によって*構造体を渡しています。これにより、プロシージャは、呼び出し元のコードで変更が有効になるように要素を変更できます。 このような変更に対して構造体を保護する場合は、値で渡します。  
  
 `Function` プロシージャから構造体を返すこともできます。 これを次の例に示します。  
  
```vb  
Dim allSystems(100) As systemInfo  
Function findByDate(ByVal searchDate As Date) As systemInfo  
    Dim i As Integer  
    For i = 1 To 100  
        If allSystems(i).purchaseDate = searchDate Then Return allSystems(i)  
    Next i  
   ' Process error: system with desired purchase date not found.  
End Function  
```  
  
## <a name="structures-within-structures"></a>構造体内の構造体  
 構造体には他の構造体を含めることができます。 これを次の例に示します。  
  
```vb  
Public Structure driveInfo  
    Public type As String  
    Public size As Long  
End Structure  
Public Structure systemInfo  
    Public cPU As String  
    Public memory As Long  
    Public diskDrives() As driveInfo  
    Public purchaseDate As Date  
End Structure  
```  
  
```vb  
Dim allSystems(100) As systemInfo  
ReDim allSystems(1).diskDrives(3)  
allSystems(1).diskDrives(0).type = "Floppy"  
```  
  
 また、この方法を使用して、あるモジュールで定義されている構造体を、別のモジュールで定義されている構造体内にカプセル化することもできます。  
  
 構造体には、任意の深さの他の構造体を含めることができます。  
  
## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [基本データ型](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)
- [複合データ型](../../../../visual-basic/programming-guide/language-features/data-types/composite-data-types.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [構造体](../../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [方法 : 構造体を宣言する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [構造体変数](../../../../visual-basic/programming-guide/language-features/data-types/structure-variables.md)
- [構造体とクラス](../../../../visual-basic/programming-guide/language-features/data-types/structures-and-classes.md)
- [Structure ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)
