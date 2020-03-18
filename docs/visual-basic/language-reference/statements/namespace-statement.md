---
title: Namespace ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Namespace
helpviewer_keywords:
- namespaces [Visual Basic], root
- Namespace statement [Visual Basic]
- namespaces [Visual Basic], nested
- declaring namespaces [Visual Basic], syntax
- namespaces [Visual Basic], declaring
- root namespaces
- declarations [Visual Basic], namespaces
ms.assetid: a31fbd95-9ace-4c3d-bbb1-51222a2272b2
ms.openlocfilehash: 19207a42890640bd82ec547e53eb6d833668e4b5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74329644"
---
# <a name="namespace-statement"></a>Namespace ステートメント
名前空間の名前を宣言し、宣言に続くソースコードがその名前空間内でコンパイルされるようにします。  
  
## <a name="syntax"></a>構文  
  
```vb  
Namespace [Global.] { name | name.name }  
    [ componenttypes ]  
End Namespace  
```  
  
## <a name="parts"></a>指定項目  
 グローバル  
 省略可。 では、プロジェクトのルート名前空間から名前空間を定義できます。 [Visual Basic における名前空間](../../../visual-basic/programming-guide/program-structure/namespaces.md) を参照してください。  
  
 `name`  
 必須。 名前空間を識別する一意の名前。 有効な Visual Basic 識別子である必要があります。 詳細については、[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。  
  
 `componenttypes`  
 省略可。 名前空間を構成する要素。 これらには、列挙体、構造体、インターフェイス、クラス、モジュール、デリゲート、およびその他の名前空間が含まれますが、これらに限定されるわけではありません。  
  
 `End Namespace`  
 `Namespace` ブロックを終了します。  
  
## <a name="remarks"></a>コメント  
 名前空間は、組織のシステムとして使用されます。 これらのクラスは、他のプログラムやアプリケーションに公開されているプログラミング要素を分類して提示する手段を提供します。 名前空間は、クラスまたは構造体が意味を持つ*型*ではないことに注意してください。名前空間のデータ型を持つプログラミング要素を宣言することはできません。  
  
 `Namespace` ステートメントの後で宣言されたすべてのプログラミング要素は、その名前空間に属します。 Visual Basic は、`End Namespace` ステートメントまたは別の `Namespace` ステートメントが検出されるまで、最後に宣言された名前空間に要素をコンパイルし続けます。  
  
 名前空間が既に定義されている場合は、プロジェクトの外部でも、プログラミング要素を追加できます。 これを行うには、`Namespace` ステートメントを使用して Visual Basic を、その名前空間に要素をコンパイルするように指示します。  
  
 `Namespace` ステートメントは、ファイルまたは名前空間レベルでのみ使用できます。 つまり、名前空間の*宣言コンテキスト*は、ソースファイルまたは別の名前空間である必要があり、クラス、構造体、モジュール、インターフェイス、またはプロシージャにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。  
  
 1つの名前空間を別の名前空間内で宣言できます。 宣言できる入れ子のレベルに厳密な制限はありませんが、最も内側の名前空間で宣言されている要素に他のコードがアクセスする場合は、入れ子階層内のすべての名前空間名を含む修飾文字列を使用する必要があることに注意してください。  
  
## <a name="access-level"></a>アクセスレベル  
 名前空間は、`Public` アクセスレベルがあるかのように扱われます。 名前空間には、同じプロジェクト内の任意の場所のコード、プロジェクトを参照する他のプロジェクト、およびプロジェクトからビルドされた任意のアセンブリからアクセスできます。  
  
 名前空間レベルで宣言され、他の要素の内部には存在しないプログラミング要素は、`Public` または `Friend` アクセスを持つことができます。 指定されていない場合、このような要素のアクセスレベルは既定で `Friend` を使用します。 名前空間レベルで宣言できる要素には、クラス、構造体、モジュール、インターフェイス、列挙型、およびデリゲートが含まれます。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。  
  
## <a name="root-namespace"></a>ルート名前空間  
 プロジェクト内のすべての名前空間名は、*ルート名前空間*に基づいています。 Visual Studio では、プロジェクト内のすべてのコードで、既定のルート名前空間としてプロジェクト名が割り当てられます。 たとえば、プロジェクト名が `Payroll`である場合、そのプログラミング要素は `Payroll`名前空間に属します。 `Namespace funding`を宣言すると、その名前空間の完全な名前が `Payroll.funding`ます。  
  
 ジェネリックリストクラスの例のように、`Namespace` ステートメントで既存の名前空間を指定する場合は、ルート名前空間を null 値に設定できます。 これを行うには、 **[プロジェクト]** メニューの **[プロジェクトのプロパティ]** をクリックし、 **[ルート名前空間]** エントリをオフにして、ボックスが空になるようにします。 ジェネリックリストクラスの例でこれを実行しなかった場合、Visual Basic コンパイラは、プロジェクト `Payroll`内の新しい名前空間として `System.Collections.Generic` します。完全な名前は `Payroll.System.Collections.Generic`です。  
  
 または、`Global` キーワードを使用して、プロジェクトの外部で定義されている名前空間の要素を参照することもできます。 そうすることで、プロジェクト名をルート名前空間として保持できます。 これにより、プログラミング要素を既存の名前空間のものと誤ってマージする可能性が低くなります。 詳細については、「[Visual Basic における名前空間](../../../visual-basic/programming-guide/program-structure/namespaces.md)」の「完全修飾名の Global キーワード」セクションを参照してください。  
  
 `Global` キーワードは、Namespace ステートメントでも使用できます。 これにより、プロジェクトのルート名前空間から名前空間を定義できます。 詳細については、「[Visual Basic における名前空間](../../../visual-basic/programming-guide/program-structure/namespaces.md)」の「名前空間のステートメントでの Global キーワード」セクションを参照してください。  
  
 **トラブルシューティング** ルート名前空間は、名前空間名の予期しない連結につながる可能性があります。 プロジェクトの外部で定義されている名前空間への参照を作成した場合、Visual Basic コンパイラは、ルート名前空間の入れ子になった名前空間としてそれらを construe できます。 このような場合、コンパイラは、外部名前空間で既に定義されている型を認識しません。 これを回避するには、「ルート名前空間」で説明されているようにルート名前空間を null 値に設定するか、`Global` キーワードを使用して外部名前空間の要素にアクセスします。  
  
## <a name="attributes-and-modifiers"></a>属性と修飾子  
 名前空間に属性を適用することはできません。 属性は、アセンブリのメタデータに情報を提供します。これは、名前空間などのソース分類子には意味がありません。  
  
 アクセス修飾子またはプロシージャ修飾子、またはその他の修飾子を名前空間に適用することはできません。 型ではないため、これらの修飾子は意味がありません。  
  
## <a name="example"></a>例  
 次の例では、2つの名前空間を宣言します。1つは他方に入れ子になっています。  
  
 [!code-vb[VbVbalrStatements#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#43)]  
  
## <a name="example"></a>例  
 次の例では、1行に複数の入れ子になった名前空間を宣言しています。これは前の例と同じです。  
  
 [!code-vb[VbVbalrStatements#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#41)]  
  
## <a name="example"></a>例  
 次の例では、前の例で定義したクラスにアクセスします。  
  
 [!code-vb[VbVbalrStatements#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#42)]  
  
## <a name="example"></a>例  
 次の例では、新しいジェネリックリストクラスのスケルトンを定義し、それを <xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間に追加します。  
  
```vb  
Namespace System.Collections.Generic  
    Class specialSortedList(Of T)  
        Inherits List(Of T)  
        ' Insert code to define the special generic list class.  
    End Class  
End Namespace  
```  
  
## <a name="see-also"></a>参照

- [Imports ステートメント (.NET 名前空間および型)](../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)
- [宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)
- [Visual Basic における名前空間](../../../visual-basic/programming-guide/program-structure/namespaces.md)
