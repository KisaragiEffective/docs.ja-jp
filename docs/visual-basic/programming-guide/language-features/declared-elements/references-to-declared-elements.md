---
title: 宣言された要素の参照
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic]
- references [Visual Basic], declared elements
- qualified names [Visual Basic]
ms.assetid: d6301709-f4cc-4b7a-b8ba-80898f14ab46
ms.openlocfilehash: a6477a9f0abaf8eb9176f4f6ab2a920af6c8f500
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345304"
---
# <a name="references-to-declared-elements-visual-basic"></a>宣言された要素の参照 (Visual Basic)
宣言された要素をコードが参照する場合、Visual Basic コンパイラは参照内の名前を、その名前の適切な宣言に一致させる必要があります。 複数の要素が同じ名前で宣言されている場合は、その名前を*修飾*することによって、これらの要素のうちどれを参照するかを制御できます。  
  
 コンパイラは、名前の参照を最も*狭いスコープ*の名前宣言と照合しようとします。 これは、参照を行うコードから開始し、それを含んでいる要素のレベルを通じて外部で動作することを意味します。  
  
 次の例では、同じ名前を持つ2つの変数への参照を示します。 この例では、`totalCount`という名前の2つの変数を、モジュール `container`のさまざまなレベルのスコープで宣言しています。 プロシージャ `showCount` が修飾なしで `totalCount` 表示される場合、Visual Basic コンパイラは、最も狭いスコープ (つまり、`showCount`内のローカル宣言) への参照を解決します。 親モジュール `container`で `totalCount` を修飾すると、コンパイラは、より広いスコープの宣言への参照を解決します。  
  
```vb  
' Assume these two modules are both in the same assembly.  
Module container  
    Public totalCount As Integer = 1  
    Public Sub showCount()  
        Dim totalCount As Integer = 6000  
        ' The following statement displays the local totalCount (6000).  
        MsgBox("Unqualified totalCount is " & CStr(totalCount))  
        ' The following statement displays the module's totalCount (1).  
        MsgBox("container.totalCount is " & CStr(container.totalCount))  
    End Sub  
End Module  
Module callingModule  
    Public Sub displayCount()  
        container.showCount()  
        ' The following statement displays the containing module's totalCount (1).  
        MsgBox("container.totalCount is " & CStr(container.totalCount))  
    End Sub  
End Module  
```  
  
## <a name="qualifying-an-element-name"></a>要素名を修飾する  
 この検索プロセスをオーバーライドして、より広範なスコープで宣言された名前を指定する場合は、より広いスコープのコンテナー要素を使用して名前を*修飾*する必要があります。 場合によっては、コンテナー要素を修飾する必要がある場合もあります。  
  
 名前を修飾すると、source ステートメントの前に、ターゲット要素が定義されている場所を識別する情報が含まれることを意味します。 この情報は、*修飾文字列*と呼ばれます。 1つ以上の名前空間、およびモジュール、クラス、または構造体を含めることができます。  
  
 修飾文字列では、ターゲット要素を含むモジュール、クラス、または構造体を明確に指定する必要があります。 コンテナーは、別の親要素 (通常は名前空間) に配置される場合があります。 修飾文字列に複数の要素を含めることが必要になる場合があります。  
  
#### <a name="to-access-a-declared-element-by-qualifying-its-name"></a>宣言された要素に名前を修飾してアクセスするには  
  
1. 要素が定義されている場所を確認します。 これには、名前空間、または名前空間の階層が含まれます。 最下位レベルの名前空間内では、要素はモジュール、クラス、または構造体に含まれている必要があります。  
  
    ```vb  
    ' Assume the following hierarchy exists outside your code.  
    Namespace outerSpace  
        Namespace innerSpace  
            Module holdsTotals  
                Public Structure totals  
                    Public thisTotal As Integer  
                    Public Shared grandTotal As Long  
                End Structure  
            End Module  
        End Namespace  
    End Namespace  
    ```  
  
2. ターゲット要素の場所に基づいて、修飾パスを決定します。 最上位レベルの名前空間から開始し、最下位レベルの名前空間に進み、target 要素を含むモジュール、クラス、または構造体で終了します。 パス内の各要素には、その後に続く要素が含まれている必要があります。  
  
     `outerSpace` → `innerSpace` → `holdsTotals` → `totals`  
  
3. ターゲット要素の修飾文字列を準備します。 パス内のすべての要素の後にピリオド (`.`) を配置します。 アプリケーションは、修飾文字列内のすべての要素にアクセスできる必要があります。  
  
    ```vb  
    outerSpace.innerSpace.holdsTotals.totals.  
    ```  
  
4. 通常の方法で、ターゲット要素を参照する式または代入ステートメントを記述します。  
  
    ```vb  
    grandTotal = 9000  
    ```  
  
5. ターゲット要素名の前に修飾文字列を指定します。 名前は、要素が含まれているモジュール、クラス、または構造体の後のピリオド (`.`) の直後に記述する必要があります。  
  
    ```vb  
    ' Assume the following module is part of your code.  
    Module accessGrandTotal  
        Public Sub setGrandTotal()  
            outerSpace.innerSpace.holdsTotals.totals.grandTotal = 9000  
        End Sub  
    End Module  
    ```  
  
6. コンパイラは、修飾文字列を使用して、ターゲット要素参照と一致させる明確で明確な宣言を検索します。  
  
 また、アプリケーションが同じ名前を持つ複数のプログラミング要素にアクセスできる場合は、名前参照を修飾する必要があります。 たとえば、<xref:System.Windows.Forms> と <xref:System.Web.UI.WebControls> の名前空間にはどちらも `Label` クラス (<xref:System.Windows.Forms.Label?displayProperty=nameWithType> および <xref:System.Web.UI.WebControls.Label?displayProperty=nameWithType>) が含まれています。 アプリケーションで両方を使用する場合、または独自の `Label` クラスを定義する場合は、異なる `Label` オブジェクトを区別する必要があります。 変数宣言に、名前空間またはインポートエイリアスを含めます。 次の例では、インポートエイリアスを使用します。  
  
```vb  
' The following statement must precede all your declarations.  
Imports win = System.Windows.Forms, web = System.Web.UI.WebControls  
' The following statement references the Windows.Forms.Label class.  
Dim winLabel As New win.Label()  
```  
  
## <a name="members-of-other-containing-elements"></a>他のコンテナー要素のメンバー  
 別のクラスまたは構造体の非共有メンバーを使用する場合は、まず、クラスまたは構造体のインスタンスを指す変数または式を使用して、メンバー名を修飾する必要があります。 次の例では、`demoClass` は `class1`という名前のクラスのインスタンスです。  
  
```vb  
Dim demoClass As class1 = New class1()  
demoClass.someSub[(argumentlist)]  
```  
  
 クラス名自体を使用して、[Shared](../../../../visual-basic/language-reference/modifiers/shared.md) されていないメンバーを修飾することはできません。 まず、オブジェクト変数にインスタンスを作成し (この場合は `demoClass`)、変数名でインスタンスを参照する必要があります。  
  
 クラスまたは構造体に `Shared` メンバーが含まれている場合は、クラスまたは構造体の名前を使用するか、インスタンスを指す変数または式を使用して、そのメンバーを修飾できます。  
  
 モジュールには個別のインスタンスがなく、すべてのメンバーが既定で `Shared` されます。 そのため、モジュール名を持つモジュールメンバーを修飾します。  
  
 次の例では、モジュールメンバープロシージャへの修飾参照を示します。 この例では、2つの `Sub` プロシージャを、プロジェクト内の異なるモジュールに2つの名前付き `perform`として宣言しています。 それぞれのモジュール内で修飾子を指定せずにそれぞれを指定できますが、他の場所から参照されている場合は修飾する必要があります。 `module3` の最後の参照は `perform`を修飾しないため、コンパイラはその参照を解決できません。  
  
```vb  
' Assume these three modules are all in the same assembly.  
Module module1  
    Public Sub perform()  
        MsgBox("module1.perform() now returning")  
    End Sub  
End Module  
Module module2  
    Public Sub perform()  
        MsgBox("module2.perform() now returning")  
    End Sub  
    Public Sub doSomething()  
        ' The following statement calls perform in module2, the active module.  
        perform()  
        ' The following statement calls perform in module1.  
        module1.perform()  
    End Sub  
End Module  
Module module3  
    Public Sub callPerform()  
        ' The following statement calls perform in module1.  
        module1.perform()  
        ' The following statement makes an unresolvable name reference  
        ' and therefore generates a COMPILER ERROR.  
        perform() ' INVALID statement  
    End Sub  
End Module  
```  
  
## <a name="references-to-projects"></a>プロジェクトへの参照  
 別のプロジェクトで定義されている [Public](../../../../visual-basic/language-reference/modifiers/public.md) 要素を使用するには、最初にそのプロジェクトのアセンブリまたはタイプライブラリへの*参照*を設定する必要があります。 参照を設定するには、 **[プロジェクト]** メニューの **[参照の追加]** をクリックするか、 [-reference (Visual Basic)](../../../../visual-basic/reference/command-line-compiler/reference.md)コマンドラインコンパイラオプションを使用します。  
  
 たとえば、.NET Framework の XML オブジェクトモデルを使用できます。 <xref:System.Xml> 名前空間への参照を設定した場合は、<xref:System.Xml.XmlDocument>などの任意のクラスを宣言して使用できます。 次の例では、<xref:System.Xml.XmlDocument>を使用します。  
  
```vb  
' Assume this project has a reference to System.Xml  
' The following statement creates xDoc as an XML document object.  
Dim xDoc As System.Xml.XmlDocument  
```  
  
## <a name="importing-containing-elements"></a>インポート (含まれる要素を)  
 [Imports ステートメント (.NET 名前空間と型)](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)を使用して、使用するモジュールまたはクラスを含む名前空間を*インポート*できます。 これにより、インポートされた名前空間で定義されている要素の名前を完全修飾せずに参照できます。 次の例では、前の例を書き直して <xref:System.Xml> 名前空間をインポートします。  
  
```vb  
' Assume this project has a reference to System.Xml  
' The following statement must precede all your declarations.  
Imports System.Xml  
' The following statement creates xDoc as an XML document object.  
Dim xDoc As XmlDocument  
```  
  
 さらに、`Imports` ステートメントでは、インポートされた各名前空間の*インポートエイリアス*を定義できます。 これにより、ソースコードを短く、読みやすくすることができます。 次の例では、前の例を書き直して、<xref:System.Xml> 名前空間のエイリアスとして `xD` を使用します。  
  
```vb  
' Assume this project has a reference to System.Xml  
' The following statement must precede all your declarations.  
Imports xD = System.Xml  
' The following statement creates xDoc as an XML document object.  
Dim xDoc As xD.XmlDocument  
```  
  
 `Imports` ステートメントでは、他のプロジェクトの要素をアプリケーションで使用することはできません。 つまり、参照の設定は行われません。 名前空間をインポートすると、その名前空間で定義されている名前を修飾する必要がなくなります。  
  
 また、`Imports` ステートメントを使用して、モジュール、クラス、構造体、および列挙型をインポートすることもできます。 その後、このようなインポートされた要素のメンバーを修飾なしで使用できます。 ただし、クラスや構造体の非共有メンバーは、クラスまたは構造体のインスタンスに評価される変数または式を使用して、常に修飾する必要があります。  
  
## <a name="naming-guidelines"></a>名前付けガイドライン  
 同じ名前を持つ2つ以上のプログラミング要素を定義すると、コンパイラがその名前への参照を解決しようとしたときに、*名前のあいまい*さが発生する可能性があります。 スコープ内に複数の定義が含まれている場合、またはスコープ内に定義が存在しない場合、参照は解決不可能なになります。 例については、このヘルプページの「修飾参照の例」を参照してください。  
  
 すべての要素に一意の名前を付けることで、名前のあいまいさを回避できます。 その後、名前を名前空間、モジュール、またはクラスで修飾することなく、任意の要素を参照できます。 間違った要素が誤って参照される可能性を低くすることもできます。  
  
## <a name="shadowing"></a>シャドウ  
 2 つのプログラミング要素が同じ名前を共有している場合、そのうちの 1 つを非表示にしたり、もう一方に*シャドウ*を付けることができます。 シャドウされた要素を参照することはできません。代わりに、シャドウされた要素名を使用するコードでは、Visual Basic コンパイラによってシャドウ要素に解決されます。 例の詳細については、「[Visual Basic におけるシャドウ](../../../../visual-basic/programming-guide/language-features/declared-elements/shadowing.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [宣言された要素の名前](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)
- [宣言された要素の特性](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-characteristics.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
- [変数](../../../../visual-basic/programming-guide/language-features/variables/index.md)
- [Imports ステートメント (.NET 名前空間および型)](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)
- [New 演算子](../../../../visual-basic/language-reference/operators/new-operator.md)
- [Public](../../../../visual-basic/language-reference/modifiers/public.md)
