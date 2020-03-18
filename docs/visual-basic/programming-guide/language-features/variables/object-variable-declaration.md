---
title: オブジェクト変数の宣言
ms.date: 07/20/2015
helpviewer_keywords:
- early binding [Visual Basic]
- declarations [Visual Basic], class
- classes [Visual Basic], declaring
- binding [Visual Basic], late and early
- object variables [Visual Basic], declaring
- variables [Visual Basic], object
- declaring variables [Visual Basic], object variables
- declaring classes [Visual Basic]
- late binding [Visual Basic]
ms.assetid: 2a5a41a3-1aa8-4236-b1f0-2382af7bf715
ms.openlocfilehash: d1964e3768124dde1deeabfada9006ff5a59def0
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351812"
---
# <a name="object-variable-declaration-visual-basic"></a>オブジェクト変数の宣言 (Visual Basic)
通常の宣言ステートメントを使用して、オブジェクト変数を宣言します。 データ型については、`Object` (つまり、[Object データ型](../../../../visual-basic/language-reference/data-types/object-data-type.md)) またはオブジェクトの作成元となるより具体的なクラスを指定します。  
  
 変数を `Object` として宣言することは、<xref:System.Object?displayProperty=nameWithType>として宣言することと同じです。  
  
 特定のオブジェクトクラスを使用して変数を宣言すると、そのクラスによって公開されているすべてのメソッドとプロパティ、およびそのクラスが継承するクラスにアクセスできます。 <xref:System.Object>を使用して変数を宣言すると、遅延バインディングを許可するように `Option Strict Off` 有効にしない限り、<xref:System.Object> クラスのメンバーにのみアクセスできます。  
  
## <a name="declaration-syntax"></a>宣言の構文  
 オブジェクト変数を宣言するには、次の構文を使用します。  
  
```vb  
Dim variablename As [New] { objectclass | Object }  
```  
  
 宣言に [Public](../../../../visual-basic/language-reference/modifiers/public.md)、 [Protected](../../../../visual-basic/language-reference/modifiers/protected.md)、 [Friend](../../../../visual-basic/language-reference/modifiers/friend.md)、`Protected Friend`、 [Private](../../../../visual-basic/language-reference/modifiers/private.md)、 [Shared](../../../../visual-basic/language-reference/modifiers/shared.md)、または [Static](../../../../visual-basic/language-reference/modifiers/static.md)を指定することもできます。 有効な宣言の例を次に示します。  
  
```vb  
Private objA As Object  
Static objB As System.Windows.Forms.Label  
Dim objC As System.OperatingSystem  
```  
  
## <a name="late-binding-and-early-binding"></a>遅延バインディングと事前バインディング  
 場合によっては、コードが実行されるまで特定のクラスが不明になることがあります。 この場合は、`Object` データ型を使用して、オブジェクト変数を宣言する必要があります。 これにより、任意の型のオブジェクトへの一般的な参照が作成され、特定のクラスが実行時に割り当てられます。 これは、*遅延バインディング*と呼ばれます。 遅延バインディングでは、追加の実行時間が必要です。 また、コードは、最後に割り当てられたクラスのメソッドとプロパティに限定されます。 これにより、コードが別のクラスのメンバーにアクセスしようとした場合に、実行時エラーが発生する可能性があります。  
  
 コンパイル時に特定のクラスがわかっている場合は、そのクラスのオブジェクト変数を宣言する必要があります。 これは、*事前バインディング*と呼ばれます。 事前バインディングを使用すると、パフォーマンスが向上し、特定のクラスのすべてのメソッドとプロパティにコードからアクセスできるようになります。 前の例の宣言では、変数 `objA` がクラス <xref:System.Windows.Forms.Label?displayProperty=nameWithType>のオブジェクトのみを使用する場合、その宣言に `As System.Windows.Forms.Label` を指定する必要があります。  
  
### <a name="advantages-of-early-binding"></a>事前バインディングの利点  
 オブジェクト変数を特定のクラスとして宣言すると、いくつかの利点があります。  
  
- 自動型チェック  
  
- 特定のクラスのすべてのメンバーへのアクセスが保証されます。  
  
- コードエディターでの Microsoft IntelliSense のサポート  
  
- コードの読みやすさの向上  
  
- コード内のエラーの数を減らす  
  
- 実行時ではなくコンパイル時に検出されたエラー  
  
- より高速なコード実行  
  
## <a name="access-to-object-variable-members"></a>オブジェクト変数のメンバーへのアクセス  
 `Option Strict` が `On`されている場合、オブジェクト変数は、宣言したクラスのメソッドとプロパティにのみアクセスできます。 これを次の例に示します。  
  
```vb  
' Option statements must precede all other source file lines.  
Option Strict On  
' Imports statement must precede all declarations in the source file.  
Imports System.Windows.Forms  
Public Sub accessMembers()  
    Dim p As Object  
    Dim q As System.Windows.Forms.Label  
    p = New System.Windows.Forms.Label  
    q = New System.Windows.Forms.Label  
    Dim j, k As Integer  
    ' The following statement generates a compiler ERROR.  
    j = p.Left  
    ' The following statement retrieves the left edge of the label in pixels.  
    k = q.Left  
End Sub  
```  
  
 この例の場合、 `p` で使用できるのは <xref:System.Object> クラス自体のメンバーのみです。 `Left` プロパティは含まれません。 一方、 `q` は、 <xref:System.Windows.Forms.Label>型として宣言されているため、 <xref:System.Windows.Forms.Label> 名前空間の <xref:System.Windows.Forms> クラスのすべてのメソッドとプロパティを使用できます。  
  
## <a name="flexibility-of-object-variables"></a>オブジェクト変数の柔軟性  
 継承階層内のオブジェクトを操作する場合は、オブジェクト変数の宣言に使用するクラスを選択できます。 この選択を行うには、オブジェクトの割り当ての柔軟性と、クラスのメンバーへのアクセスとのバランスを取る必要があります。 たとえば、<xref:System.Windows.Forms.Form?displayProperty=nameWithType> クラスにつながる継承階層を考えてみます。  
  
 <xref:System.Object>  
  
 &nbsp;&nbsp;<xref:System.MarshalByRefObject>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;<xref:System.ComponentModel.Component>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.Windows.Forms.Control>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.Windows.Forms.ScrollableControl>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.Windows.Forms.ContainerControl>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.Windows.Forms.Form>  
  
 たとえば、アプリケーションで `specialForm`というフォームクラスが定義されているとします。このクラスは、クラス <xref:System.Windows.Forms.Form>から継承されます。 次の例に示すように、特に `specialForm`を参照するオブジェクト変数を宣言できます。  
  
```vb  
Public Class specialForm  
    Inherits System.Windows.Forms.Form  
    ' Insert code defining methods and properties of specialForm.  
End Class  
Dim nextForm As New specialForm  
```  
  
 前の例の宣言では、変数 `nextForm` を `specialForm`クラスのオブジェクトに限定していますが、`specialForm` のすべてのメソッドとプロパティを `nextForm`に使用できるだけでなく、`specialForm` が継承するすべてのクラスのすべてのメンバーも使用できます。  
  
 次の例に示すように <xref:System.Windows.Forms.Form>型として宣言することで、オブジェクト変数をより一般的なものにすることができます。  
  
```vb  
Dim anyForm As System.Windows.Forms.Form  
```  
  
 前の例の宣言を使用すると、アプリケーションの任意のフォームを `anyForm`に割り当てることができます。 ただし、`anyForm` はクラス <xref:System.Windows.Forms.Form>のすべてのメンバーにアクセスできますが、`specialForm`などの特定のフォームに対して定義されている追加のメソッドやプロパティを使用することはできません。  
  
 基底クラスのすべてのメンバーは派生クラスで使用できますが、派生クラスの追加メンバーを基底クラスで使用することはできません。  
  
## <a name="see-also"></a>参照

- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [オブジェクト変数の代入](../../../../visual-basic/programming-guide/language-features/variables/object-variable-assignment.md)
- [オブジェクト変数の値](../../../../visual-basic/programming-guide/language-features/variables/object-variable-values.md)
- [方法: オブジェクト変数を宣言し、その変数にオブジェクトを割り当てる Visual Basic](../../../../visual-basic/programming-guide/language-features/variables/how-to-declare-an-object-variable-and-assign-an-object-to-it.md)
- [方法: オブジェクトのメンバーにアクセスする](../../../../visual-basic/programming-guide/language-features/variables/how-to-access-members-of-an-object.md)
- [New 演算子](../../../../visual-basic/language-reference/operators/new-operator.md)
- [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
