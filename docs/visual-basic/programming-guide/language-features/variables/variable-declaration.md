---
title: 変数宣言
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], declaring
- member variables [Visual Basic], declarations
- Dim statement [Visual Basic], variable declaration
- declaring variables [Visual Basic]
- variables [Visual Basic], scope
- variables [Visual Basic], data types
- data types [Visual Basic], variable declarations
- lifetime [Visual Basic], variables
- variables [Visual Basic], lifetime
- access levels [Visual Basic], variables
- scope [Visual Basic], declaration statements
- variables [Visual Basic], access level
- local variables [Visual Basic], declarations
- scope [Visual Basic], variables
ms.assetid: d8f10226-92b1-480f-9f53-df377b2d7e15
ms.openlocfilehash: b89773e9527af0d65cde53b61654f2511f5c8dde
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351759"
---
# <a name="variable-declaration-in-visual-basic"></a>Visual Basic での変数宣言
変数を宣言して、その名前と特性を指定します。 変数の宣言ステートメントは、 [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)です。 その場所と内容によって、変数の特性が決まります。  
  
 変数の名前付け規則と考慮事項については、「[宣言された要素の名前](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。  
  
## <a name="declaration-levels"></a>宣言レベル  
  
### <a name="local-and-member-variables"></a>ローカル変数とメンバー変数  
 *ローカル変数*は、プロシージャ内で宣言されています。 *メンバー変数*は Visual Basic 型のメンバーです。これは、モジュールレベルで、クラス、構造体、またはモジュールの内部で宣言されますが、そのクラス、構造体、またはモジュールの内部のプロシージャ内では宣言されません。  
  
### <a name="shared-and-instance-variables"></a>共有変数とインスタンス変数  
 クラスまたは構造体では、メンバー変数のカテゴリは、そのメンバーが共有されているかどうかによって異なります。 [Shared](../../../../visual-basic/language-reference/modifiers/shared.md)キーワードを使用して宣言されている場合、これは*共有変数*であり、クラスまたは構造体のすべてのインスタンス間で共有される1つのコピーに存在します。  
  
 それ以外の場合は、*インスタンス変数*になり、クラスまたは構造体のインスタンスごとに個別のコピーが作成されます。 インスタンス変数の特定のコピーは、それが作成されたクラスまたは構造体のインスタンスでのみ使用できます。 これは、クラスまたは構造体の他のインスタンスのインスタンス変数のコピーとは無関係です。  
  
## <a name="declaring-data-type"></a>データ型の宣言  
 宣言ステートメントの As 句を使用する[と](../../../../visual-basic/language-reference/statements/as-clause.md)、宣言する変数のデータ型またはオブジェクト型を定義できます。 変数には、次のいずれかの型を指定できます。  
  
- `Boolean`、`Long`、`Decimal` などの基本データ型  
  
- 配列や構造体などの複合データ型  
  
- アプリケーション内または別のアプリケーションで定義されているオブジェクトの種類 (クラス)。  
  
- <xref:System.Windows.Forms.Label> や <xref:System.Windows.Forms.TextBox> などの .NET Framework クラス。  
  
- <xref:System.IComparable> や <xref:System.IDisposable> などのインターフェイス型  
  
 1つのステートメントで複数の変数を宣言しても、データ型を繰り返す必要はありません。 次のステートメントでは、変数 `i`、`j`、および `k` を型 `Integer`、`l` として `m` として宣言し、`Long`として `x` します。`y``Single`  
  
```vb  
Dim i, j, k As Integer  
' All three variables in the preceding statement are declared as Integer.  
Dim l, m As Long, x, y As Single  
' In the preceding statement, l and m are Long, x and y are Single.  
```  
  
 データ型の詳細については、「[データ型](../../../../visual-basic/programming-guide/language-features/data-types/index.md)」を参照してください。 オブジェクトの詳細については、「[オブジェクトとクラス](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)」および「[コンポーネントを使用したプログラミング](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/0ffkdtkf(v=vs.120))」を参照してください。  
  
## <a name="local-type-inference"></a>ローカル型の推論  
 *型の推定*は、`As` 句を使用せずに宣言されたローカル変数のデータ型を決定するために使用されます。 コンパイラは、初期化式の型から変数の型を推測します。 これにより、型を明示的に指定せずに変数を宣言できます。 次の例では、`num1` と `num2` の両方が整数として厳密に型指定されています。  
  
 [!code-vb[VbVbalrTypeInference#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#1)]  
  
 ローカル型の推論を使用する場合は、`Option Infer` を `On`に設定する必要があります。 詳細については、「[ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)」と「[Option Infer ステートメント](../../../../visual-basic/language-reference/statements/option-infer-statement.md)」を参照してください。  
  
## <a name="characteristics-of-declared-variables"></a>宣言された変数の特性  
 変数の*有効*期間は、その期間内に使用できる期間です。 一般に、変数は、その変数を宣言する要素 (プロシージャやクラスなど) が引き続き存在する限り存在します。 変数に含まれる要素の有効期間が過ぎても既存の変数を続行する必要がない場合は、宣言で特別な操作を行う必要はありません。 変数が、それを含む要素よりも長く存在する必要がある場合は、`Dim` ステートメントに `Static` または `Shared` キーワードを含めることができます。 詳細については、「 [Visual Basic の有効期間](../../../../visual-basic/programming-guide/language-features/declared-elements/lifetime.md)」を参照してください。  
  
 変数の*スコープ*は、その名前を修飾せずに参照できるすべてのコードのセットです。 変数のスコープは、宣言されている場所によって決まります。 特定の地域にあるコードは、その領域で定義された変数を使用できますが、名前を修飾する必要はありません。 詳細については、「[Visual Basic におけるスコープ](../../../../visual-basic/programming-guide/language-features/declared-elements/scope.md)」を参照してください。  
  
 変数の*アクセスレベル*は、それにアクセスするためのアクセス許可を持つコードの範囲です。 これは、`Dim` ステートメントで使用するアクセス修飾子 ( [Public](../../../../visual-basic/language-reference/modifiers/public.md) 、 [Private](../../../../visual-basic/language-reference/modifiers/private.md)など) によって決定されます。 詳細については、「[Visual Basic でのアクセス レベル](../../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [方法 : 新しい変数を作成する](../../../../visual-basic/programming-guide/language-features/variables/how-to-create-a-new-variable.md)
- [方法 : 変数に値を格納する、および変数から値を取得する](../../../../visual-basic/programming-guide/language-features/variables/how-to-move-data-into-and-out-of-a-variable.md)
- [データの種類](../../../../visual-basic/language-reference/data-types/index.md)
- [Protected](../../../../visual-basic/language-reference/modifiers/protected.md)
- [Friend](../../../../visual-basic/language-reference/modifiers/friend.md)
- [Static](../../../../visual-basic/language-reference/modifiers/static.md)
- [宣言された要素の特性](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-characteristics.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [Option Infer ステートメント](../../../../visual-basic/language-reference/statements/option-infer-statement.md)
