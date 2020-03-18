---
title: オブジェクトとクラス
ms.date: 07/20/2015
helpviewer_keywords:
- classes [Visual Basic]
- objects [Visual Basic]
ms.assetid: c68c5752-1006-46e1-975a-6717b62a42fc
ms.openlocfilehash: 589b0b362cc25fd10e2780fd541cf9f7cfb546a9
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75344640"
---
# <a name="objects-and-classes-in-visual-basic"></a>Visual Basic のオブジェクトとクラス

"*オブジェクト*" は、1 つの単位として扱うことができるコードとデータの組み合わせです。 オブジェクトは、コントロールやフォームのように、アプリケーションの一部になることができます。 アプリケーション全体も、オブジェクトになることができます。

Visual Basic でアプリケーションを作成する場合は、常にオブジェクトを操作します。 Visual Basic によって提供されるオブジェクト (コントロール、フォーム、データアクセスオブジェクトなど) を使用できます。 また、Visual Basic アプリケーション内の他のアプリケーションのオブジェクトを使用することもできます。 独自のオブジェクトを作成し、それらのプロパティとメソッドを追加で定義することもできます。 オブジェクトはプログラムの作成済みの構成要素として機能し、コードを一度記述すれば、何度も再利用できます。

このトピックでは、オブジェクトの詳細について説明します。

## <a name="objects-and-classes"></a>オブジェクトとクラス

Visual Basic 内の各オブジェクトは、*クラス*によって定義されます。 クラスは、オブジェクトの変数、プロパティ、プロシージャ、およびイベントを記述します。 オブジェクトはクラスのインスタンスです。クラスを定義したら、必要な数のオブジェクトを作成することができます。

オブジェクトとそのクラス間の関係を理解するために、クッキーの抜き型とクッキーを考えてみましょう。 クッキーの抜き型はクラスです。 それは、クッキーの特徴 (大きさや形など) を定義します。 クラスを使用して、オブジェクトを作成します。 オブジェクトはクッキーです。

メンバーにアクセスする前に、オブジェクトを作成する必要があります。

### <a name="to-create-an-object-from-a-class"></a>クラスからオブジェクトを作成するには

1. オブジェクトの作成元となるクラスを決定します。

2. [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)を記述して、クラス インスタンスを割り当てることができる変数を作成します。 変数は、目的のクラスの型にする必要があります。

   ```vb
   Dim nextCustomer As customer
   ```

3. [New 演算子](../../../../visual-basic/language-reference/operators/new-operator.md)キーワードを追加して、変数をクラスの新しいインスタンスに初期化します。

   ```vb
   Dim nextCustomer As New customer
   ```

4. これで、オブジェクト変数を使用してクラスのメンバーにアクセスできるようになりました。

   ```vb
   nextCustomer.accountNumber = lastAccountNumber + 1
   ```

> [!NOTE]
> 可能であれば、変数は、変数に割り当てる予定のクラスの型で宣言する必要があります。 これは、*事前バインディング*と呼ばれます。 コンパイル時にクラスの型を把握していない場合は、変数を [Object データ型](../../../../visual-basic/language-reference/data-types/object-data-type.md)で宣言することで、*遅延バインディング*を呼び出すことができます。 ただし、遅延バインディングでは、パフォーマンスが低下し、実行時のオブジェクトのメンバーへのアクセスが制限される可能性があります。 詳細については、「[オブジェクト変数の宣言](../../../../visual-basic/programming-guide/language-features/variables/object-variable-declaration.md)」を参照してください。

### <a name="multiple-instances"></a>複数インスタンス

多くの場合、クラスから新しく作成されたオブジェクトは互いに同一です。 ただし、個々のオブジェクトとして存在した後、それぞれの変数とプロパティは、他のインスタンスとは無関係に変更することができます。 たとえば、次の 3 つのチェック ボックスをフォームに追加する場合、各チェック ボックスのオブジェクトは、<xref:System.Windows.Forms.CheckBox> クラスのインスタンスになります。 個々の <xref:System.Windows.Forms.CheckBox> オブジェクトは、クラスによって定義された特性と機能 (プロパティ、変数、プロシージャ、およびイベント) の共通セットを共有します。 ただし、それぞれが独自の名前を持ち、別々に有効または無効にすることができ、フォームの異なる場所に配置することができます。

## <a name="object-members"></a>オブジェクトのメンバー

オブジェクトはアプリケーションの要素であり、クラスの "*インスタンス*" を表しています。 フィールド、プロパティ、メソッド、およびイベントは、オブジェクトの構成要素であり、オブジェクトの*メンバー*を構成します。

### <a name="member-access"></a>メンバー アクセス

オブジェクトのメンバーにアクセスするには、オブジェクト変数の名前、ピリオド (`.`)、およびメンバーの名前をこの順序で指定します。 <xref:System.Windows.Forms.Label> オブジェクトの <xref:System.Windows.Forms.Control.Text%2A> プロパティを設定する例を次に示します。

```vb
warningLabel.Text = "Data not saved"
```

#### <a name="intellisense-listing-of-members"></a>メンバーの IntelliSense の一覧

IntelliSense は、[メンバーの一覧] オプションが呼び出されたとき (たとえばメンバー アクセス演算子としてピリオド (`.`) が入力されたとき) に、クラスのメンバーを一覧表示します。 ピリオドに続けて、そのクラスのインスタンスとして宣言された変数の名前を入力すると、IntelliSense は、すべてのインスタンス メンバーを表示しますが、共有メンバーは表示しません。 クラス名に続けてピリオドを入力すると、IntelliSense は、すべての共有メンバーを表示し、インスタンス メンバーは表示しません。 詳細については、「[IntelliSense の使用](/visualstudio/ide/using-intellisense)」を参照してください。

### <a name="fields-and-properties"></a>フィールドとプロパティ

"*フィールド*" と "*プロパティ*" は、オブジェクトに格納されている情報を表します。 代入ステートメントによる値の取得と設定は、プロシージャでローカル変数の取得と設定を行うのと同じ方法で実行します。 次の例では <xref:System.Windows.Forms.Control.Width%2A> プロパティを取得し、<xref:System.Windows.Forms.Label> オブジェクトの <xref:System.Windows.Forms.Control.ForeColor%2A> プロパティを設定しています。

```vb
Dim warningWidth As Integer = warningLabel.Width
warningLabel.ForeColor = System.Drawing.Color.Red
```

フィールドは、"*メンバー変数*" とも呼ばれます。

次の場合は、プロパティ プロシージャを使用します。

- 値を設定または取得するタイミングと方法を制御する必要がある。

- プロパティに、検証する必要がある適切に定義された一連の値がある。

- 値を設定することで、オブジェクトの状態をはっきりと変更する (例: `IsVisible` プロパティ)。

- プロパティを設定することで、他の内部変数またはその他のプロパティの値を変更する。

- プロパティを設定または取得する前に、一連の手順を実行する必要がある。

次の場合は、フィールドを使用します。

- 値は自己検証型である。 たとえば、`Boolean` 変数に `True` または `False` の値が割り当てられた場合は、エラーまたはデータの自動変換が発生します。

- データ型によってサポートされている範囲の値はすべて有効である。 これは、`Single` 型または `Double` 型の多くのプロパティに当てはまります。

- プロパティは `String` データ型であり、文字列のサイズまたは値に制限がない。

- 詳細については、「[プロパティ プロシージャ](../../../../visual-basic/programming-guide/language-features/procedures/property-procedures.md)」を参照してください。

### <a name="methods"></a>メソッド

"*メソッド*" は、オブジェクトが実行できる処理です。 たとえば、<xref:System.Windows.Forms.ComboBox.ObjectCollection.Add%2A> は、コンボ ボックスに新しいエントリを追加する <xref:System.Windows.Forms.ComboBox> オブジェクトのメソッドです。

次の例は、<xref:System.Windows.Forms.Timer> オブジェクトの <xref:System.Windows.Forms.Timer.Start%2A> メソッドを示しています。

```vb
Dim safetyTimer As New System.Windows.Forms.Timer
safetyTimer.Start()
```

メソッドは、オブジェクトによって公開される "*プロシージャ*" です。

詳細については、「[プロシージャ](../../../../visual-basic/programming-guide/language-features/procedures/index.md)」を参照してください。

### <a name="events"></a>Events

イベントは、マウスのクリックやキーの押下などのオブジェクトによって認識される操作であり、それに応答するコードを記述できます。 イベントは、ユーザーの操作またはプログラム コードの結果として発生する可能性がありますが、システムによって発生する場合もあります。 イベントを通知するコードを、イベントを "*発生させる*" と言い、イベントに応答するコードを、イベントを "*処理する*" と言います。

オブジェクトによって発生する独自のカスタム イベントを作成し、その他のオブジェクトで処理することもできます。 詳細については、「[イベント](../../../../visual-basic/programming-guide/language-features/events/index.md)」を参照してください。

### <a name="instance-members-and-shared-members"></a>インスタンス メンバーと共有メンバー

クラスからオブジェクトを作成した結果が、そのクラスのインスタンスになります。 [Shared](../../../../visual-basic/language-reference/modifiers/shared.md) キーワードなしで宣言されたメンバーは、"*インスタンス メンバー*" になり、特定のインスタンスにのみ属します。 あるインスタンスのインスタンス メンバーは、同じクラスの別のインスタンスに同じメンバーがあっても互いに独立しています。 たとえばインスタンス メンバー変数は、インスタンスごとに異なる値を持つことができます。

`Shared` キーワード付きで宣言されたメンバーは "*共有メンバー*" になり、全体がクラスに属し、特定のインスタンスには属しません。 共有メンバーは、作成するクラスのインスタンスの数に関係なく 1 度だけ存在します。これは、インスタンスを作成していない場合でも同じです。 たとえば共有メンバー変数は、クラスにアクセスできるすべてのコードが使用できる 1 つの値のみを持っています。

#### <a name="accessing-nonshared-members"></a>非共有メンバーへのアクセス

##### <a name="to-access-a-nonshared-member-of-an-object"></a>オブジェクトの非共有メンバーにアクセスするには

1. オブジェクトがクラスから作成され、オブジェクト変数に割り当てられていることを確認してください。

   ```vb
   Dim secondForm As New System.Windows.Forms.Form
   ```

2. メンバーにアクセスするステートメントで、てオブジェクト変数名に続けて、"*メンバー アクセス演算子"*  (`.`) とメンバー名を指定します。

   ```vb
   secondForm.Show()
   ```

#### <a name="accessing-shared-members"></a>共有メンバーへのアクセス

##### <a name="to-access-a-shared-member-of-an-object"></a>オブジェクトの共有メンバーにアクセスするには

- クラス名に続けて、"*メンバー アクセス演算子*" (`.`) とメンバー名を指定します。 オブジェクトの `Shared` メンバーは、常にクラス名から直接アクセスする必要があります。

   ```vb
   MsgBox("This computer is called " & Environment.MachineName)
   ```

- 既にクラスからオブジェクトを作成している場合は、オブジェクト変数を使用して `Shared` メンバーにアクセスすることもできます。

### <a name="differences-between-classes-and-modules"></a>クラスとモジュールの違い

クラスとモジュールの主な違いは、クラスはオブジェクトとしてインスタンス化できますが、標準モジュールではできないことです。 標準モジュールのデータは 1 つしかないため、プログラムのある部分が標準モジュールのパブリック変数を変更した場合、その後、プログラムの他の部分がその変数を読み取ると、変更後の値が取得されます。 これに対し、オブジェクト データは、インスタンス化されたオブジェクトごとに個別に存在します。 別の相違点は、標準モジュールとは異なり、クラスはインターフェイスを実装できることです。

> [!NOTE]
> `Shared` 修飾子は、クラス メンバーに適用された場合は、クラスの特定のインスタンスではなく、クラス自体に関連付けられます。 メンバーへのアクセスは、モジュール メンバーへのアクセス方法と同じように、クラス名を使用して直接行います。

さらに、クラスとモジュールは、メンバーに対して異なるスコープを使用します。 クラス内に定義されているメンバーのスコープはクラスの特定のインスタンス内に限られ、オブジェクトの有効期間のみ存在します。 クラスの外部からクラス メンバーにアクセスするには、"*オブジェクト*.*メンバー*" 形式の完全修飾名を使用する必要があります。

その一方で、モジュール内に宣言されているメンバーは、既定ではパブリックにアクセスすることができ、モジュールにアクセスできるすべてのコードからアクセスできます。 つまり、標準モジュール内の変数は、プロジェクト内の任意の場所から参照でき、プログラムが実行されている間は存在するため、実質的にはグローバル変数です。

## <a name="reusing-classes-and-objects"></a>クラスとオブジェクトの再利用

オブジェクトを使用すると、変数とプロシージャを 1 度宣言した後、必要に応じていつでもそれらを再利用できます。 たとえば、スペル チェック機能をアプリケーションに追加する場合は、スペル チェック機能を提供するためのすべての変数とサポート機能を定義することができます。 スペル チェック機能をクラスとして作成した場合は、コンパイルされたアセンブリへの参照を追加することで、他のアプリケーションで再利用できます。 さらに、誰かが既に開発したスペル チェック機能のクラスを使用することで、作業の手間を省くことができます。

.NET Framework には、使用可能なコンポーネントの例が多数用意されています。 次の例では、<xref:System> 名前空間に <xref:System.TimeZone> クラスが使用されています。 <xref:System.TimeZone> は、現在のコンピューター システムのタイム ゾーンに関する情報を取得できるメンバーを提供します。

```vb
Public Sub examineTimeZone()
    Dim tz As System.TimeZone = System.TimeZone.CurrentTimeZone
    Dim s As String = "Current time zone is "
    s &= CStr(tz.GetUtcOffset(Now).Hours) & " hours and "
    s &= CStr(tz.GetUtcOffset(Now).Minutes) & " minutes "
    s &= "different from UTC (coordinated universal time)"
    s &= vbCrLf & "and is currently "
    If tz.IsDaylightSavingTime(Now) = False Then s &= "not "
    s &= "on ""summer time""."
    MsgBox(s)
End Sub
```

上記の例では、最初の [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)が <xref:System.TimeZone> 型のオブジェクト変数を宣言し、<xref:System.TimeZone.CurrentTimeZone%2A> プロパティによって返された <xref:System.TimeZone> オブジェクトに割り当てています。

## <a name="relationships-among-objects"></a>オブジェクト間の関係

オブジェクトは、いくつかの方法で相互に関連付けることができます。 リレーションシップの主要な種類は、"*階層*" と "*含有*" です。

### <a name="hierarchical-relationship"></a>階層関係

より基礎的なクラスから派生したクラスは、"*階層関係*" があると言います。 クラスの階層は、より一般的なクラスのサブタイプである項目を記述する場合に便利です。

次の例では、通常の <xref:System.Windows.Forms.Button> と同じように機能しますが、前景と背景の色を逆転させるメソッドも公開する、特殊な <xref:System.Windows.Forms.Button> を定義します。

#### <a name="to-define-a-class-is-derived-from-an-already-existing-class"></a>既に存在するクラスから派生するクラスを定義するのには

1. [Class ステートメント](../../../../visual-basic/language-reference/statements/class-statement.md)を使用して、必要なオブジェクトの作成元となるクラスを定義します。

   ```vb
   Public Class reversibleButton
   ```

   クラスのコードの最後の行の後に、必ず `End Class` ステートメントを指定してください。 統合開発環境 (IDE) では、既定により、`Class` ステートメントを入力すると、`End Class` が自動的に生成されます。

2. `Class` ステートメントの直後に、[Inherits ステートメント](../../../../visual-basic/language-reference/statements/inherits-statement.md)を記述します。 新しいクラスの派生元クラスを指定します。

   ```vb
   Inherits System.Windows.Forms.Button
   ```

   新しいクラスは、基本クラスによって定義されたすべてのメンバーを継承します。

3. 派生クラスで公開される追加のメンバー用のコードを追加します。 たとえば、`reverseColors` メソッドを追加すると、派生クラスは次のようになります。

   ```vb
   Public Class reversibleButton
       Inherits System.Windows.Forms.Button
           Public Sub reverseColors()
               Dim saveColor As System.Drawing.Color = Me.BackColor
               Me.BackColor = Me.ForeColor
               Me.ForeColor = saveColor
          End Sub
   End Class
   ```

   `reversibleButton` クラスからオブジェクトを作成すると、オブジェクトは、<xref:System.Windows.Forms.Button> クラスのすべてのメンバーだけでなく、`reverseColors` メソッドと`reversibleButton` に定義したその他の新しいメンバーにもアクセスできます。

派生クラスは、派生元のクラスからメンバーを継承するため、クラス階層が深くなるにつれて、クラスをより複雑にすることができます。 詳細については、「[継承の基本](../../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)」を参照してください。

### <a name="compile-the-code"></a>コードのコンパイル

コンパイラが、新しいクラスの派生元となるクラスにアクセスできることを確認します。 これは、上記の例のように名前を完全に修飾するか、名前空間を [Imports ステートメント (.NET 名前空間と型)](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md) で識別することを意味する場合があります。 クラスが別のプロジェクト内にある場合は、そのプロジェクトへの参照の追加が必要になることがあります。 詳細については、「[プロジェクト内の参照の管理](/visualstudio/ide/managing-references-in-a-project)」を参照してください。

### <a name="containment-relationship"></a>含有リレーションシップ

オブジェクトを関連付けることのできる別の方法は、"*含有関係*" にすることです。 コンテナー オブジェクトは、その他のオブジェクトを論理的にカプセル化します。 たとえば、<xref:System.OperatingSystem> オブジェクトには、その <xref:System.OperatingSystem.Version%2A> プロパティによって返される <xref:System.Version> オブジェクトが論理的に含まれています。 コンテナー オブジェクトが物理的にその他のオブジェクトを含んでいるわけではないことに注意してください。

#### <a name="collections"></a>コレクション

特別な種類のオブジェクトの含有として、"*コレクション*" と表現されるものがあります。 コレクションは、列挙できる、類似したオブジェクトのグループです。 Visual Basic の [For Each...Next ステートメント](../../../../visual-basic/language-reference/statements/for-each-next-statement.md)には、コレクションの項目を反復処理するための特別な構文があります。 さらに、多くの場合、コレクションでは、<xref:Microsoft.VisualBasic.Collection.Item%2A> を使用して、その要素を、インデックスまたは一意の文字列の関連付けによって取得することができます。 コレクションは、インデックスなしで項目を削除または追加できるため、配列よりも簡単に使用できます。 簡単に使用できるため、多くの場合、コレクションは、フォームとコントロールを格納するために使用されます。

## <a name="related-topics"></a>関連トピック

[チュートリアル: クラスの定義](../../../../visual-basic/programming-guide/language-features/objects-and-classes/walkthrough-defining-classes.md)\
クラスを作成する方法を手順を追って説明します。

オーバーロードされた[プロパティとメソッド](../../../../visual-basic/programming-guide/language-features/objects-and-classes/overloaded-properties-and-methods.md)\
オーバーロードされたプロパティとメソッド

[継承の基本](../../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)\
継承修飾子、メソッドとプロパティのオーバーライド、MyClass、および MyBase について説明します。

[オブジェクトの有効期間: オブジェクトの作成と破棄](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)\
クラス インスタンスの作成と破棄について説明します。

[匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)\
匿名型を作成して使用する方法について説明します。匿名型を使用すると、データ型のクラス定義を記述せずにオブジェクトを作成できます。

[オブジェクト初期化子 : 名前付きの型と匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)\
1 つの式で名前付きの型と匿名型のインスタンスを作成するために使用する、オブジェクト初期化子について説明します。

[方法: 匿名型の宣言でプロパティの名前と型を推論する](../../../../visual-basic/programming-guide/language-features/objects-and-classes/how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)\
匿名型で宣言されたプロパティの名前と型を推論する方法について説明します。 推論の成功例と失敗例を示します。
