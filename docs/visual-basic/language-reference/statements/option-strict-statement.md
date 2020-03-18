---
title: Option Strict Statement
ms.date: 07/20/2015
f1_keywords:
- vb.Strict
- vb.OptionStrict
helpviewer_keywords:
- Strict keyword [Visual Basic]
- Option Strict statement [Visual Basic]
- conversions [Visual Basic], implicit
- late binding [Visual Basic]
- implicit conversions [Visual Basic]
ms.assetid: 5883e0c1-a920-4274-8e46-b0ff047eaee5
ms.openlocfilehash: a002526a107fdc6e8e02890d11db94a3d224c94b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353758"
---
# <a name="option-strict-statement"></a>Option Strict Statement
暗黙的なデータ型変換を拡大変換のみに制限し、遅延バインディングを許可しません。また、`Object` 型になる暗黙の型指定を許可しません。  
  
## <a name="syntax"></a>構文  
  
```vb  
Option Strict { On | Off }  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`On`|省略可。 `Option Strict` チェックを有効にします。|  
|`Off`|省略可。 `Option Strict` チェックを無効にします。|  
  
## <a name="remarks"></a>コメント  
 `Option Strict On` または `Option Strict` がファイルに含まれている場合、次のような状況によってコンパイル時エラーが発生します。  
  
- 暗黙的な縮小変換  
  
- 遅延バインディング  
  
- 結果が `Object` 型となる暗黙の型指定  
  
> [!NOTE]
> [[コンパイル] ページ (プロジェクトデザイナー)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)で設定できる警告構成 (Visual Basic) には、コンパイル時エラーの原因となる3つの条件に対応する3つの設定があります。 これらの設定の使用方法の詳細については、このトピックの「 [IDE で警告構成を設定するに](../../../visual-basic/language-reference/statements/option-strict-statement.md#conditions)は」を参照してください。  
  
 関連付けられている IDE 設定でこれらのエラーまたは警告をオンにしている場合でも、`Option Strict Off` ステートメントは、3つの条件すべてについてエラーおよび警告チェックをオフにします。 `Option Strict On` ステートメントは、関連する IDE 設定でこれらのエラーまたは警告をオフにするように指定している場合でも、3つの条件すべてについてエラーおよび警告チェックをオンにします。  
  
 使用する場合、`Option Strict` ステートメントは、ファイル内の他のコードステートメントの前に記述する必要があります。  
  
 `Option Strict` を `On`に設定すると、Visual Basic では、すべてのプログラミング要素にデータ型が指定されているかどうかがチェックされます。 データ型は、明示的に指定することも、ローカル型推論を使用して指定することもできます。 次の理由から、すべてのプログラミング要素にデータ型を指定することをお勧めします。  
  
- これにより、変数とパラメーターの IntelliSense サポートが有効になります。 これにより、コードを入力するときに、プロパティとその他のメンバーを表示できます。  
  
- これにより、コンパイラが型チェックを実行できるようになります。 型チェックを使用すると、型変換エラーのために実行時に失敗する可能性があるステートメントを見つけることができます。 また、これらのメソッドをサポートしていないオブジェクトに対するメソッドの呼び出しも識別します。  
  
- これにより、コードの実行速度が向上します。 その理由の1つは、プログラミング要素のデータ型を指定しない場合、Visual Basic コンパイラによって `Object` 型に割り当てられることです。 コンパイルされたコードは、`Object` とその他のデータ型との間で変換を行う必要があります。これにより、パフォーマンスが低下します。  
  
## <a name="implicit-narrowing-conversion-errors"></a>暗黙的な縮小変換エラー  
 縮小変換する暗黙的なデータ型変換がある場合は、暗黙的な縮小変換エラーが発生します。  
  
 Visual Basic は、さまざまなデータ型を他のデータ型に変換できます。 データが失われる可能性があるのは、あるデータ型の値が、精度が低いか、容量が小さいデータ型に変換されたときです。 このような縮小変換が失敗した場合、実行時エラーが発生します。 `Option Strict` によって、これらの縮小変換のコンパイル時通知が確実に行われるため、回避できます。 詳細については、「[暗黙の型変換と明示的な型変換](../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)」および「[拡大変換と縮小変換](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)」を参照してください。  
  
 エラーを発生させる可能性のある変換には、式で発生する暗黙的な変換が含まれます。 詳細については、次のトピックを参照してください。  
  
- [+ 演算子](../../../visual-basic/language-reference/operators/addition-operator.md)  
  
- [+= 演算子](../../../visual-basic/language-reference/operators/addition-assignment-operator.md)  
  
- [\ 演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/integer-division-operator.md)  
  
- [/= 演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/floating-point-division-assignment-operator.md)  
  
- [Char データ型](../../../visual-basic/language-reference/data-types/char-data-type.md)  
  
 [& 演算子](../../../visual-basic/language-reference/operators/concatenation-operator.md)を使用して文字列を連結する場合、文字列へのすべての変換は拡大されていると見なされます。 したがって、`Option Strict` がオンの場合でも、これらの変換では、暗黙的な縮小変換エラーは生成されません。  
  
 対応するパラメーターとは異なるデータ型の引数を持つメソッドを呼び出すと、`Option Strict` がオンになっていると、縮小変換によってコンパイル時エラーが発生します。 拡大変換または明示的な変換を使用することで、コンパイル時エラーを回避できます。  
  
 `For Each…Next` コレクション内の要素からループ制御変数への変換では、コンパイル時に暗黙的な縮小変換エラーが抑制されます。 これは、`Option Strict` がオンになっている場合でも発生します。 詳細については、「[For Each...Next ステートメント](../../../visual-basic/language-reference/statements/for-each-next-statement.md)」の「縮小変換」セクションを参照してください。  
  
## <a name="late-binding-errors"></a>遅延バインディングエラー  
 `Object` 型として宣言された変数のプロパティまたはメソッドにオブジェクトを代入する場合は、そのオブジェクトは遅延バインディングされます。 詳細については、「[事前バインディングと遅延バインディング](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)」を参照してください。  
  
## <a name="implicit-object-type-errors"></a>暗黙的なオブジェクト型のエラー  
 適切な型が宣言された変数を推論できない場合は暗黙的なオブジェクトの型エラーが発生するため、`Object` の型が推論されます。 これは主に、`Dim` 句を使用せず、`As` をオフにして、`Option Infer` ステートメントを使用して変数を宣言した場合に発生します。 詳細については、「[Option Infer ステートメント](../../../visual-basic/language-reference/statements/option-infer-statement.md)」および「[Visual Basic 言語仕様](../../../visual-basic/reference/language-specification/index.md)」を参照してください。  
  
 メソッドパラメーターの場合、`Option Strict` がオフの場合、`As` 句は省略可能です。 ただし、いずれかのパラメーターが `As` 句を使用する場合は、すべてのパラメーターで使用する必要があります。 `Option Strict` が on の場合は、すべてのパラメーター定義に対して `As` 句が必要です。  
  
 `As` 句を使用せずに変数を宣言し、それを `Nothing`に設定すると、変数の型は `Object`になります。 この場合、`Option Strict` がオンで `Option Infer` がオンになっていると、コンパイル時エラーは発生しません。 この例としては、`Dim something = Nothing`があります。  
  
### <a name="default-data-types-and-values"></a>既定のデータ型と値  
 次の表では、 [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)でデータ型と初期化子を指定するさまざまな組み合わせの結果について説明します。  
  
|データ型が指定されているか|初期化子が指定されているか|例|結果|  
|---|---|---|---|  
|いいえ|いいえ|`Dim qty`|`Option Strict` がオフ (既定値) の場合、変数は `Nothing` に設定されます。<br /><br /> `Option Strict` がオンの場合、コンパイル時エラーが発生します。|  
|いいえ|はい|`Dim qty = 5`|`Option Infer` がオン (既定値) の場合、変数は初期化子のデータ型になります。 「[ローカル型の推定](../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)」を参照してください。<br /><br /> `Option Infer` がオフで、`Option Strict` がオフの場合、変数は `Object` のデータ型になります。<br /><br /> `Option Infer` がオフで、`Option Strict` がオンの場合、コンパイル時エラーが発生します。|  
|はい|いいえ|`Dim qty As Integer`|変数は、データ型の既定値に初期化されます。 詳細については、「[Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)」を参照してください。|  
|はい|はい|`Dim qty  As Integer = 5`|初期化子のデータ型を指定したデータ型に変換できない場合は、コンパイル時エラーが発生します。|  
  
## <a name="when-an-option-strict-statement-is-not-present"></a>Option Strict ステートメントが存在しない場合  
 ソースコードに `Option Strict` ステートメントが含まれていない場合、[[コンパイル] ページの [プロジェクトデザイナー] (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)の**Option strict**設定が使用されます。 [**コンパイル] ページ**には、エラーを生成する条件をさらに制御するための設定があります。  
  
 コマンドラインコンパイラを使用している場合は、 [-optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md)コンパイラオプションを使用して `Option Strict`の設定を指定できます。  
  
### <a name="to-set-option-strict-in-the-ide"></a>IDE で Option Strict を設定するには  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。  
  
2. **[コンパイル]** タブで、 **[Option Strict]** ボックスの値を設定します。  
  
### <a name="conditions"></a>IDE で警告の構成を設定するには  
 `Option Strict` ステートメントの代わりに [[コンパイル] ページ、プロジェクトデザイナー (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)を使用すると、エラーを生成する条件をさらに制御できます。 [**コンパイル] ページ**の **[警告の構成]** セクションには、`Option Strict` がオンのときにコンパイル時にエラーが発生する3つの条件に対応する設定があります。 これらの設定を次に示します。  
  
- **暗黙的な変換**  
  
- **遅延バインディング、呼び出しが実行時に失敗する可能性があります**  
  
- **暗黙的な型、オブジェクトと見なされます**  
  
 **[Option Strict]** を **[オン]** に設定すると、これら 3 つの警告の構成設定のすべてが **[エラー]** に設定されます。 **[Option Strict]** を **[オフ]** に設定すると、3 つの設定すべてが **[なし]** に設定されます。  
  
 各警告の構成設定を個別に **[なし]** 、 **[警告]** 、または **[エラー]** に変更することができます。 3 つの警告の構成設定がすべて **[エラー]** に設定されている場合、`On` ボックスに `Option strict` が表示されます。 3 つすべてが **[なし]** に設定されている場合、このボックスには `Off` が表示されます。 これらの設定のその他の組み合わせに対しては、 **(カスタム)** が表示されます。  
  
### <a name="to-set-the-option-strict-default-setting-for-new-projects"></a>新しいプロジェクトの Option Strict default 設定を設定するには  
 プロジェクトを作成すると、 **[コンパイル]** タブの**option strict**設定が、 **[オプション]** ダイアログボックスの **[strict]** 設定に設定されます。  
  
 このダイアログボックスの `Option Strict` を設定するには、 **[ツール]** メニューの **[オプション]** をクリックします。 **[オプション]** ダイアログ ボックスの **[プロジェクトおよびソリューション]** を展開し、 **[VISUAL BASIC の既定値]** をクリックします。 [VB の既定**値**] の初期の既定の設定は、`Off`です。  
  
### <a name="to-set-option-strict-on-the-command-line"></a>コマンドラインで Option Strict を設定するには  
 **Vbc.exe**コマンドに[-optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md)コンパイラオプションを含めます。  
  
## <a name="example"></a>例  
 次の例は、縮小変換である暗黙の型変換によって発生するコンパイル時のエラーを示しています。 このカテゴリのエラーは、[**コンパイル] ページ**の**暗黙の変換**条件に対応しています。  
  
 [!code-vb[VbVbalrStatements#161](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#161)]  
  
## <a name="example"></a>例  
 次の例は、遅延バインディングによって発生するコンパイル時のエラーを示しています。 このカテゴリのエラーは、遅延バインディングに対応しています。 **コンパイル ページ**の **実行時に呼び出しが失敗する可能性**があります。  
  
 [!code-vb[VbVbalrStatements#162](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#162)]  
  
## <a name="example"></a>例  
 次の例は、暗黙的な型の `Object`で宣言された変数によって発生するエラーを示しています。 このカテゴリのエラーは、暗黙的な型に対応しています **。オブジェクト**は、**コンパイルページ**で条件を想定しています。  
  
 [!code-vb[VbVbalrStatements#163](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#163)]  
  
 [!code-vb[VbVbalrStatements#164](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#164)]  
  
## <a name="see-also"></a>参照

- [拡大変換と縮小変換](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [暗黙の型変換と明示的な型変換](../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [[コンパイル] ページ、プロジェクト デザイナー (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)
- [Option Explicit ステートメント](../../../visual-basic/language-reference/statements/option-explicit-statement.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [方法: オブジェクトのメンバーにアクセスする](../../../visual-basic/programming-guide/language-features/variables/how-to-access-members-of-an-object.md)
- [XML での埋め込み式](../../../visual-basic/programming-guide/language-features/xml/embedded-expressions-in-xml.md)
- [厳密でないデリゲート変換](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
- [Office ソリューションの遅延バインディング](/visualstudio/vsto/late-binding-in-office-solutions)
- [-optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md)
- [[Visual Basic の既定値] ([オプション] ダイアログ ボックス - [プロジェクト])](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
