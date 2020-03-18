---
title: Dim ステートメント
ms.date: 05/12/2018
f1_keywords:
- vb.Dim
- Dim
helpviewer_keywords:
- Public keyword [Visual Basic], in Dim statement
- Dim statement [Visual Basic]
- fixed-length strings [Visual Basic], declaring
- variables [Visual Basic], declaring
- WithEvents keyword [Visual Basic], Dim statement
- dynamic arrays [Visual Basic], Dim statement
- variables [Visual Basic], initializing
- '{} braces'
- fields [Visual Basic], as member variables
- declarations [Visual Basic], dynamic arrays
- member variables [Visual Basic]
- default values [Visual Basic]
- data types [Visual Basic], assigning
- braces {}
- As keyword [Visual Basic], in Dim statement
- arrays [Visual Basic], declaring
- New keyword [Visual Basic], Dim statement
- To keyword [Visual Basic], in Dim statement
- storage [Visual Basic], allocating
- local variables [Visual Basic]
- declaration statements [Visual Basic]
- Dim statement [Visual Basic], syntax
- variables [Visual Basic], member and local
ms.assetid: fae3eca1-f0b2-4400-994b-7aa58a848448
ms.openlocfilehash: 1b0c3089c366c417af926c8c0703cea021674432
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744729"
---
# <a name="dim-statement-visual-basic"></a>Dim ステートメント (Visual Basic)

1つ以上の変数に対して記憶領域を宣言して割り当てます。

## <a name="syntax"></a>構文

```vb
[ <attributelist> ] [ accessmodifier ] [[ Shared ] [ Shadows ] | [ Static ]] [ ReadOnly ]
Dim [ WithEvents ] variablelist
```

## <a name="parts"></a>要素

- `attributelist`

  省略可能。 「[属性リスト](attribute-list.md)」を参照してください。

- `accessmodifier`

  省略可能。 以下のいずれかを指定できます。

  - [パブリック](../modifiers/public.md)

  - [Protected](../modifiers/protected.md)

  - [Friend](../modifiers/friend.md)

  - [プライベート](../modifiers/private.md)

  - [Protected Friend](../modifiers/protected-friend.md)

  - [Private Protected](../modifiers/private-protected.md)

  「 [Access levels in Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。

- `Shared`

  省略可能。 「[共有](../modifiers/shared.md)」を参照してください。

- `Shadows`

  省略可能。 「[シャドウ](../modifiers/shadows.md)」を参照してください。

- `Static`

  省略可能。 「[静的](../modifiers/static.md)」を参照してください。

- `ReadOnly`

  省略可能。 「 [ReadOnly](../modifiers/readonly.md)」を参照してください。

- `WithEvents`

  省略可能。 イベントを発生させるクラスのインスタンスを参照するオブジェクト変数であることを指定します。 「 [WithEvents](../modifiers/withevents.md)」を参照してください。

- `variablelist`

  必須。 このステートメントで宣言されている変数のリスト。

  `variable [ , variable ... ]`

  `variable` の構文と指定項目は次のとおりです。

  `variablename [ ( [ boundslist ] ) ] [ As [ New ] datatype [ With`{`[ .propertyname = propinitializer [ , ... ] ] } ] ] [ = initializer ]`

  |要素|[説明]|
  |---|---|
  |`variablename`|必須。 変数の名前。 「 [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。|
  |`boundslist`|省略可能。 配列変数の各次元の境界の一覧。|
  |`New`|省略可能。 `Dim` ステートメントの実行時に、クラスの新しいインスタンスを作成します。|
  |`datatype`|省略可能。 変数のデータ型。|
  |`With`|省略可能。 オブジェクト初期化子リストについて説明します。|
  |`propertyname`|省略可能。 インスタンスを作成しているクラスのプロパティの名前。|
  |`propinitializer`|`propertyname` = の後に指定する必要があります。 評価され、プロパティ名に割り当てられる式。|
  |`initializer`|`New` が指定されていない場合は省略可能です。 評価され、作成時に変数に割り当てられる式。|

## <a name="remarks"></a>解説

Visual Basic コンパイラは、`Dim` ステートメントを使用して、変数のデータ型やその他の情報 (変数にアクセスできるコードなど) を決定します。 次の例では、`Integer` 値を保持する変数を宣言しています。

```vb
Dim numberOfStudents As Integer
```

任意のデータ型を指定することも、列挙型、構造体、クラス、またはインターフェイスの名前を指定することもできます。

```vb
Dim finished As Boolean
Dim monitorBox As System.Windows.Forms.Form
```

参照型の場合は、`New` キーワードを使用して、データ型によって指定されたクラスまたは構造体の新しいインスタンスを作成します。 `New`を使用する場合、初期化子式は使用しません。 代わりに、変数を作成するクラスのコンストラクターに、必要に応じて引数を指定します。

```vb
Dim bottomLabel As New System.Windows.Forms.Label
```

プロシージャ、ブロック、クラス、構造体、またはモジュールで変数を宣言できます。 ソースファイル、名前空間、またはインターフェイスで変数を宣言することはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](declaration-contexts-and-default-access-levels.md)」を参照してください。

モジュールレベルで、プロシージャの外部で宣言されている変数は、*メンバー変数*または*フィールド*です。 メンバー変数は、クラス、構造体、またはモジュール全体でスコープ内にあります。 プロシージャレベルで宣言された変数は、*ローカル変数*です。 ローカル変数は、プロシージャまたはブロック内でのみスコープ内にあります。

次のアクセス修飾子は、プロシージャの外部で変数を宣言するために使用されます: `Public`、`Protected`、`Friend`、`Protected Friend`、および `Private`。 詳細については、「 [Visual Basic のアクセスレベル](../../programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。

`Dim` キーワードは省略可能で、通常、`Public`、`Protected`、`Friend`、`Protected Friend`、`Private`、`Shared`、`Shadows`、`Static`、`ReadOnly`、`WithEvents`のいずれかの修飾子を指定すると省略されます。

```vb
Public maximumAllowed As Double
Protected Friend currentUserName As String
Private salary As Decimal
Static runningTotal As Integer
```

`Option Explicit` が on (既定値) の場合、コンパイラは使用するすべての変数の宣言を必要とします。 詳細については、「 [Option Explicit ステートメント](option-explicit-statement.md)」を参照してください。

## <a name="specifying-an-initial-value"></a>初期値の指定

変数を作成するときに、変数に値を割り当てることができます。 値型の場合は、*初期化子*を使用して、変数に代入する式を指定します。 式は、コンパイル時に計算できる定数に評価される必要があります。

```vb
Dim quantity As Integer = 10
Dim message As String = "Just started"
```

初期化子が指定されていて、`As` 句でデータ型が指定されていない場合、*型の推定*は、初期化子からデータ型を推論するために使用されます。 次の例では、`num1` と `num2` の両方が整数として厳密に型指定されています。 2番目の宣言では、型の推定によって値3から型が推論されます。

```vb
' Use explicit typing.
Dim num1 As Integer = 3

' Use local type inference.
Dim num2 = 3
```

型の推定はプロシージャレベルで適用されます。 クラス、構造体、モジュール、またはインターフェイスのプロシージャの外側には適用されません。 型の推定の詳細については、「[オプション推論ステートメント](option-infer-statement.md)と[ローカル型の推論](../../programming-guide/language-features/variables/local-type-inference.md)」を参照してください。

データ型または初期化子が指定されていない場合の動作については、このトピックで後述する「[既定のデータ型と値](dim-statement.md#default)」を参照してください。

*オブジェクト初期化子*を使用して、名前付きの型と匿名型のインスタンスを宣言できます。 次のコードでは、`Student` クラスのインスタンスを作成し、オブジェクト初期化子を使用してプロパティを初期化します。

```vb
Dim student1 As New Student With {.First = "Michael",
                                  .Last = "Tucker"}
```

オブジェクト初期化子の詳細については、「[方法: オブジェクト初期化子を使用してオブジェクトを宣言](../../programming-guide/language-features/objects-and-classes/how-to-declare-an-object-by-using-an-object-initializer.md)する」、「[オブジェクト初期化子: 名前付きおよび匿名型](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)」、および「[匿名型](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)」を参照してください。

## <a name="declaring-multiple-variables"></a>複数の変数の宣言

1つの宣言ステートメントで複数の変数を宣言し、それぞれに変数名を指定し、各配列名をかっこで囲んで指定することができます。 複数の変数を指定するときは、コンマで区切ります。

```vb
Dim lastTime, nextTime, allTimes() As Date
```

1つの `As` 句で複数の変数を宣言する場合、その変数グループの初期化子を指定することはできません。

宣言する変数ごとに個別の `As` 句を使用して、異なる変数に異なるデータ型を指定できます。 各変数は、`variablename` 部分の後に出現する最初の `As` 句に指定されたデータ型を受け取ります。

```vb
Dim a, b, c As Single, x, y As Double, i As Integer
' a, b, and c are all Single; x and y are both Double
```

## <a name="arrays"></a>配列

複数の値を保持できる*配列*を保持する変数を宣言できます。 変数が配列を保持するように指定するには、その `variablename` の直後にかっこを付けます。 配列の詳細については、「[配列](../../programming-guide/language-features/arrays/index.md)」を参照してください。

配列の各次元の下限と上限を指定できます。 これを行うには、かっこ内に `boundslist` を含めます。 ディメンションごとに、`boundslist` は上限を指定し、必要に応じて下限を指定します。 下限は、指定したかどうかにかかわらず、常に0になります。 各インデックスは、0から上限値までの範囲内で異なる場合があります。

次の2つのステートメントは等価です。 各ステートメントは、21個の `Integer` 要素の配列を宣言します。 配列にアクセスする場合、インデックスは 0 ~ 20 で異なる場合があります。

```vb
Dim totals(20) As Integer
Dim totals(0 To 20) As Integer
```

次のステートメントは、`Double`型の2次元配列を宣言しています。 配列には、6つの列 (5 + 1) の4行 (3 + 1) があります。 上限は、ディメンションの長さではなく、インデックスの最大有効値を表します。 ディメンションの長さは、上限に1を加えた値です。

```vb
Dim matrix2(3, 5) As Double
```

配列は、1 ~ 32 の次元を持つことができます。

配列宣言では、すべての境界を空白のままにすることができます。 これを行うと、指定した次元の数が配列に含まれていますが、初期化されていません。 この値は、少なくともその要素の一部を初期化するまで `Nothing` の値を持ちます。 `Dim` ステートメントでは、すべてのディメンションに対して、またはディメンションなしの境界を指定する必要があります。

```vb
' Declare an array with blank array bounds.
Dim messages() As String
' Initialize the array.
ReDim messages(4)
```

配列に複数の次元がある場合は、ディメンションの数を示すために、かっこの間にコンマを含める必要があります。

```vb
Dim oneDimension(), twoDimensions(,), threeDimensions(,,) As Byte
```

配列の次元の1つを-1 に宣言することによって、*長さ0の配列*を宣言できます。 長さ0の配列を保持する変数には `Nothing`値がありません。 特定の共通言語ランタイム関数では、長さが0の配列が必要です。 このような配列にアクセスしようとすると、ランタイム例外が発生します。 詳細については、「[配列](../../programming-guide/language-features/arrays/index.md)」を参照してください。

配列リテラルを使用して、配列の値を初期化できます。 これを行うには、初期化値を中かっこ (`{}`) で囲みます。

```vb
Dim longArray() As Long = {0, 1, 2, 3}
```

多次元配列の場合、各次元の初期化は外側の次元で中かっこで囲まれます。 これらの要素は、行優先順で指定されます。

```vb
Dim twoDimensions(,) As Integer = {{0, 1, 2}, {10, 11, 12}}
```

配列リテラルの詳細については、「[配列](../../programming-guide/language-features/arrays/index.md)」を参照してください。

## <a name="default"></a>既定のデータ型と値

次の表では、`Dim` ステートメントのデータ型と初期化子を指定するさまざまな組み合わせの結果を示します。

|データ型が指定されているか|初期化子が指定されているか|例|結果|
|---|---|---|---|
|いいえ|いいえ|`Dim qty`|[Option Strict](option-strict-statement.md)がオフ (既定値) の場合、変数は `Nothing`に設定されます。<br /><br /> `Option Strict` がオンの場合、コンパイル時エラーが発生します。|
|いいえ|はい|`Dim qty = 5`|[オプションの推論](option-infer-statement.md)が on (既定値) の場合、変数は初期化子のデータ型を受け取ります。 「[ローカル型の推定](../../programming-guide/language-features/variables/local-type-inference.md)」を参照してください。<br /><br /> `Option Infer` がオフで、`Option Strict` がオフの場合、変数は `Object` のデータ型になります。<br /><br /> `Option Infer` がオフで、`Option Strict` がオンの場合、コンパイル時エラーが発生します。|
|はい|いいえ|`Dim qty As Integer`|変数は、データ型の既定値に初期化されます。 このセクションで後述する表を参照してください。|
|はい|はい|`Dim qty  As Integer = 5`|初期化子のデータ型を指定したデータ型に変換できない場合は、コンパイル時エラーが発生します。|

データ型を指定しても初期化子を指定しなかった場合は、Visual Basic 変数をそのデータ型の既定値に初期化します。 既定の初期化値を次の表に示します。

|データ型|既定値|
|---|---|
|すべての数値型 (`Byte` と `SByte`を含む)|0|
|`Char`|バイナリ0|
|すべての参照型 (`Object`、`String`、すべての配列を含む)|`Nothing`|
|`Boolean`|`False`|
|`Date`|12:00 年1月1日の午前 (午前 01/01/0001 12:00:00 時)|

構造体の各要素は、個別の変数であるかのように初期化されます。 配列の長さを宣言するが、その要素を初期化しない場合、各要素は個別の変数であるかのように初期化されます。

## <a name="static-local-variable-lifetime"></a>静的ローカル変数の有効期間

`Static` ローカル変数の有効期間は、宣言されているプロシージャよりも長くなります。 変数の有効期間の境界は、プロシージャが宣言されている場所と `Shared`かどうかによって異なります。

|プロシージャの宣言|変数の初期化|既存の変数の停止|
|---|---|---|
|モジュール内|プロシージャが初めて呼び出されたとき|プログラムの実行を停止したとき|
|クラスまたは構造体では、プロシージャは `Shared`|プロシージャが最初に呼び出されたときに、特定のインスタンスまたはクラスまたは構造体自体で呼び出されます。|プログラムの実行を停止したとき|
|クラスまたは構造体では、プロシージャは `Shared` ません。|特定のインスタンスで最初にプロシージャが呼び出されたとき|インスタンスがガベージコレクション (GC) 用に解放されたとき|

## <a name="attributes-and-modifiers"></a>属性と修飾子

属性は、ローカル変数ではなくメンバー変数にのみ適用できます。 属性は、アセンブリのメタデータに情報を提供します。これは、ローカル変数などの一時的なストレージには意味がありません。

モジュールレベルでは、`Static` 修飾子を使用してメンバー変数を宣言することはできません。 プロシージャレベルでは、`Shared`、`Shadows`、`ReadOnly`、`WithEvents`、または任意のアクセス修飾子を使用してローカル変数を宣言することはできません。

`accessmodifier`を指定することによって、変数にアクセスできるコードを指定できます。 クラスおよびモジュールメンバー変数 (プロシージャ以外) では、既定でプライベートアクセスが使用され、構造体メンバー変数は既定でパブリックアクセスになります。 アクセス修飾子を使用してこれらのアクセス レベルを調整できます。 ローカル変数 (プロシージャ内) では、アクセス修飾子を使用できません。

`WithEvents` は、プロシージャ内のローカル変数ではなく、メンバー変数に対してのみ指定できます。 `WithEvents`を指定する場合、変数のデータ型は、`Object`ではなく、特定のクラス型である必要があります。 `WithEvents`で配列を宣言することはできません。 イベントの詳細については、「 [events](../../programming-guide/language-features/events/index.md)」を参照してください。

> [!NOTE]
> クラス、構造体、またはモジュールの外部のコードでは、メンバー変数の名前を、そのクラス、構造体、またはモジュールの名前で修飾する必要があります。 プロシージャまたはブロックの外側のコードは、そのプロシージャまたはブロック内のローカル変数を参照できません。

## <a name="releasing-managed-resources"></a>マネージリソースの解放

.NET Framework ガベージコレクターは、追加のコーディングなしでマネージリソースを破棄します。 ただし、ガベージコレクターを待機するのではなく、マネージリソースを強制的に破棄することができます。

クラスが特に貴重なリソース (データベース接続やファイルハンドルなど) を保持している場合は、次のガベージコレクションが使用されなくなったクラスインスタンスをクリーンアップするまで待機することはできません。 クラスは、ガベージコレクションの前にリソースを解放する手段を提供するために、<xref:System.IDisposable> インターフェイスを実装できます。 このインターフェイスを実装するクラスは、重要なリソースを直ちに解放するために呼び出すことができる `Dispose` メソッドを公開します。

`Using` ステートメントは、リソースを取得し、一連のステートメントを実行して、リソースを破棄するプロセスを自動化します。 ただし、リソースは <xref:System.IDisposable> インターフェイスを実装する必要があります。 詳細については、「[Using ステートメント](using-statement.md)」を参照してください。

## <a name="example"></a>例

次の例では、さまざまなオプションを使用して `Dim` ステートメントを使用して変数を宣言しています。

[!code-vb[VbVbalrStatements#141](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class11.vb#141)]

## <a name="example"></a>例

次の例では、1から30までの素数を一覧表示します。 ローカル変数のスコープについては、「コードコメント」を参照してください。

[!code-vb[VbVbalrStatements#142](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class11.vb#142)]

## <a name="example"></a>例

次の例では、`speedValue` 変数がクラスレベルで宣言されています。 `Private` キーワードは、変数を宣言するために使用されます。 変数には、`Car` クラスの任意のプロシージャからアクセスできます。

[!code-vb[VbVbalrStatements#144](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class11.vb#144)]

[!code-vb[VbVbalrStatements#145](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class11.vb#145)]

## <a name="see-also"></a>参照

- [Const ステートメント](const-statement.md)
- [ReDim ステートメント](redim-statement.md)
- [Option Explicit ステートメント](option-explicit-statement.md)
- [Option Infer ステートメント](option-infer-statement.md)
- [Option Strict ステートメント](option-strict-statement.md)
- [[コンパイル] ページ、プロジェクト デザイナー (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)
- [変数宣言](../../programming-guide/language-features/variables/variable-declaration.md)
- [配列](../../programming-guide/language-features/arrays/index.md)
- [オブジェクト初期化子 : 名前付きの型と匿名型](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [匿名型](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [オブジェクト初期化子 : 名前付きの型と匿名型](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [方法 : オブジェクト初期化子を使用してオブジェクトを宣言する](../../programming-guide/language-features/objects-and-classes/how-to-declare-an-object-by-using-an-object-initializer.md)
- [ローカル型の推論](../../programming-guide/language-features/variables/local-type-inference.md)
