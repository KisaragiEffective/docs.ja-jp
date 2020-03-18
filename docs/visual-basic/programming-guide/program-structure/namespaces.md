---
title: 名前空間
ms.date: 07/20/2015
f1_keywords:
- vb.global
helpviewer_keywords:
- assemblies [Visual Basic], namespaces
- name collisions
- Global keyword [Visual Basic]
- namespace pollution
- names [Visual Basic], avoiding conflicts
- naming conflicts [Visual Basic], namespaces
- namespaces [Visual Basic], assemblies
- Visual Basic code, namespaces
- fully qualified names [Visual Basic]
- naming conventions [Visual Basic], naming conflicts
- namespaces
ms.assetid: cffac744-ab8c-4f1f-ba50-732c22ab4b88
ms.openlocfilehash: ec892167f30a7ded739dc188ab4096cb3a5d154c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347331"
---
# <a name="namespaces-in-visual-basic"></a>Visual Basic における名前空間
アセンブリ内で定義されているオブジェクトは、名前空間によって編成されています。 アセンブリには複数の名前空間を含めることができます。さらに、名前空間の中に他の名前空間を含めることもできます。 名前空間を使用するとあいまいさがなくなるため、クラス ライブラリを使用する場合など、多数のオブジェクトを使用する場合に参照が簡単になります。  
  
 たとえば、.NET Framework は <xref:System.Windows.Forms?displayProperty=nameWithType> 名前空間の <xref:System.Windows.Forms.ListBox> クラスを定義します。 次のコードは、このクラスの完全修飾名を使用して変数を宣言する方法を示しています。  
  
 [!code-vb[VbVbalrApplication#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#6)]  
  
## <a name="avoiding-name-collisions"></a>名前の競合の回避
 .NET Framework 名前空間は、*名前空間の汚染*と呼ばれる問題に対処します。このような場合、クラスライブラリの開発者は、別のライブラリで類似した名前を使用することによって妨げられます。 このような既存コンポーネントとの競合は、 *名前の競合*とも呼ばれます。  
  
 たとえば、 `ListBox`という名前の新しいクラスを作成した場合、プロジェクト内ではこのクラスを修飾子を付けずに使用できます。 ただし、同じプロジェクトで .NET Framework <xref:System.Windows.Forms.ListBox> クラスを使用する場合は、参照を一意にするために完全修飾参照を使用する必要があります。 参照が一意でない場合、Visual Basic によって、名前があいまいであることを示すエラーが生成されます。 次のコード例では、これらのオブジェクトを宣言する方法を示しています。  
  
 [!code-vb[VbVbalrApplication#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#7)]  
  
 次の図は、2つの名前空間階層を示しています。どちらも `ListBox`という名前のオブジェクトを含んでいます。  
  
 ![2つの名前空間階層を示すスクリーンショット。](./media/namespaces/visual-basic-namespace-hierarchy.gif)  
  
 既定では、Visual Basic で作成するすべての実行可能ファイルには、プロジェクトと同じ名前の名前空間が含まれます。 たとえば、 `ListBoxProject`という名前のプロジェクト内でオブジェクトを定義した場合、実行可能ファイル ListBoxProject.exe には `ListBoxProject`という名前空間が含まれます。  
  
 複数のアセンブリで同じ名前空間を使用することができます。 Visual Basic は、これらを1つの名前のセットとして扱います。 たとえば、 `SomeNameSpace` というアセンブリの `Assemb1`という名前空間のクラスを定義した後に、 `Assemb2`というアセンブリの同じ名前空間のクラスを定義できます。  
  
## <a name="fully-qualified-names"></a>完全修飾名  
 完全修飾名は、オブジェクトが定義されている名前空間の名前で始まるオブジェクト参照です。 他のプロジェクトで定義されているオブジェクトを使用するには、 **[プロジェクト]** メニューの **[参照の追加]** をクリックしてそのクラスへの参照を作成し、コード内でそのオブジェクトの完全修飾名を使用します。 次のコードは、別のプロジェクトの名前空間のオブジェクトを使用して完全修飾名を使用する方法を示しています。  
  
 [!code-vb[VbVbalrApplication#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#8)]  
  
 完全修飾名を使用すると、どのオブジェクトを使用するかをコンパイラが認識できるため、名前の競合を防止できます。 ただし、名前自体が長くなるため、使いにくくなります。 この問題を回避するには、 `Imports` ステートメントを使って *エイリアス*を定義します。エイリアスとは、完全修飾名の代わりに使用できる短い名前です。 たとえば、次のコード例では、2 つの完全修飾名に対してエイリアスを作成し、作成したエイリアスを使って 2 つのオブジェクトを定義しています。  
  
 [!code-vb[VbVbalrApplication#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#9)]  
  
 [!code-vb[VbVbalrApplication#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#10)]  
  
 エイリアスを指定せずに `Imports` ステートメントを使用すると、インポートした名前空間のすべての名前を修飾子を付けずに使用できます。ただし、それらの名前がプロジェクト内で一意であることが必要です。 プロジェクトに、同じ名前の複数の項目を持つ名前空間をインポートする `Imports` ステートメントがある場合は、それらの名前を使用するときに完全修飾名を使用する必要があります。 たとえば、プロジェクトに次の 2 つの `Imports` ステートメントがあるとします。  
  
 [!code-vb[VbVbalrApplication#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#11)]  
  
 完全修飾なしで `Class1` を使用しようとすると、Visual Basic `Class1` 名前があいまいであることを示すエラーが生成されます。  
  
## <a name="namespace-level-statements"></a>Namespace Level ステートメント  
 名前空間内では、モジュール、インターフェイス、クラス、デリゲート、列挙体、構造体、他の名前空間などの項目を定義できます。 プロパティ、プロシージャ、変数、イベントなどの項目を名前空間のレベルで定義することはできません。 これらの項目は、モジュール、構造体、クラスなどのコンテナー内で宣言する必要があります。  
  
## <a name="global-keyword-in-fully-qualified-names"></a>完全修飾名の Global キーワード  
 入れ子になった階層構造の名前空間を定義すると、その階層構造の内部にあるコードが .NET Framework の <xref:System?displayProperty=nameWithType> 名前空間にアクセスできない場合があります。 次の例は、 `SpecialSpace.System` 名前空間から <xref:System?displayProperty=nameWithType>にアクセスできない階層構造を示しています。  
  
```vb  
Namespace SpecialSpace  
    Namespace System  
        Class abc  
            Function getValue() As System.Int32  
                Dim n As System.Int32  
                Return n  
            End Function  
        End Class  
    End Namespace  
End Namespace  
```  
  
 この場合、 <xref:System.Int32?displayProperty=nameWithType>に `SpecialSpace.System` が定義されていないため、Visual Basic コンパイラは `Int32`への参照を正しく解決できません。 `Global` キーワードを使用すると、修飾チェーンを .NET Framework クラス ライブラリの最も外側のレベルで開始できます。 これにより、クラス ライブラリの <xref:System?displayProperty=nameWithType> 名前空間や、その他のあらゆる名前空間を指定できるようになります。 これを次の例に示します。  
  
```vb  
Namespace SpecialSpace  
    Namespace System  
        Class abc  
            Function getValue() As Global.System.Int32  
                Dim n As Global.System.Int32  
                Return n  
            End Function  
        End Class  
    End Namespace  
End Namespace  
```  
  
 `Global` を使用して、 <xref:Microsoft.VisualBasic?displayProperty=nameWithType>などの他のルート レベルの名前空間、およびプロジェクトに関連する任意の名前空間にアクセスできます。  
  
## <a name="global-keyword-in-namespace-statements"></a>Namespace ステートメントでの Global キーワード  
 また、 [Namespace ステートメント](../../../visual-basic/language-reference/statements/namespace-statement.md)で `Global` キーワードを使用することもできます。 これにより、プロジェクトのルート名前空間から名前空間を定義できます。  
  
 プロジェクト内のすべての名前空間は、プロジェクトのルート名前空間に基づいています。  Visual Studio では、プロジェクト内のすべてのコードで、既定のルート名前空間としてプロジェクト名が割り当てられます。 たとえば、プロジェクト名が `ConsoleApplication1`である場合、そのプログラミング要素は `ConsoleApplication1`名前空間に属します。 `Namespace Magnetosphere`を宣言すると、プロジェクトの `Magnetosphere` への参照は `ConsoleApplication1.Magnetosphere`にアクセスします。  
  
 次の例では、プロジェクトのルート名前空間から名前空間を宣言するために `Global` キーワードを使用しています。  
  
 [!code-vb[VbVbalrApplication#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/module1.vb#22)]  
  
 名前空間宣言では、 `Global` を別の名前空間に入れ子にすることはできません。  
  
 [Application Page, Project Designer (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic) を使用して、プロジェクトの **ルート名前空間** を表示および変更できます。  新しいプロジェクトの場合、 **ルート名前空間** の既定値は、プロジェクト名です。 `Global` を最上位レベルの名前空間にするには、 **ルート名前空間** のエントリを消去して、ボックスを空にします。 **ルート名前空間** を消去すると、名前空間の宣言で `Global` キーワードの必要がなくなります。  
  
 `Namespace` ステートメントで .NET framework の名前空間にもなっている名前を宣言する場合、 `Global` キーワードが完全修飾名で使用されていない場合、.NET Framework 名前空間は使用できなくなります。 `Global` キーワードを使用せずに、.NET Framework 名前空間へのアクセスを有効にするには、 `Global` キーワードを `Namespace` ステートメントに含めます。  
  
 次の例では、 `Global` 名前空間の宣言で、 `System.Text` キーワードを使用しています。  
  
 名前空間の宣言に `Global` キーワードが含まれていない場合、 <xref:System.Text.StringBuilder> にアクセスするには、 `Global.System.Text.StringBuilder`を指定する必要があります。 `ConsoleApplication1`という名前のプロジェクトで、 `System.Text` キーワードが使用されていない場合、 `ConsoleApplication1.System.Text` を参照すると、 `Global` にアクセスすることになります。  
  
 [!code-vb[VbVbalrApplication#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/module1.vb#21)]  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ListBox>
- <xref:System.Windows.Forms?displayProperty=nameWithType>
- [.NET のアセンブリ](../../../standard/assembly/index.md)
- [参照と Imports ステートメント](references-and-the-imports-statement.md)
- [Imports ステートメント (.NET 名前空間および型)](../../language-reference/statements/imports-statement-net-namespace-and-type.md)
- [Writing Code in Office Solutions](/visualstudio/vsto/writing-code-in-office-solutions)
