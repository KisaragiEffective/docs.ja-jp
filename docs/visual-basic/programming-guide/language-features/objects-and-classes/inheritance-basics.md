---
title: 継承の基本
ms.date: 07/20/2015
helpviewer_keywords:
- derived classes [Visual Basic], inheritance
- MyClass keyword [Visual Basic], using
- MyBase keyword [Visual Basic], using
- Inherits statement [Visual Basic], inheritance
- overriding, Overridable keyword
- MustInherit keyword [Visual Basic], using
- Overrides keyword [Visual Basic], using
- inheritance
- MustInherit classes [Visual Basic]
- MustOverride keyword [Visual Basic], using
- classes [Visual Basic], derived
- NotInheritable keyword [Visual Basic], using
- base classes [Visual Basic], extending properties and methods [Visual Basic]
- NotOverridable keyword [Visual Basic], using
- base classes [Visual Basic], inheritance
- abstract classes [Visual Basic], inheritance
- overriding, Overrides keyword
ms.assetid: dfc8deba-f5b3-4d1d-a937-7cb826446fc5
ms.openlocfilehash: 89fcf2a14d8938d536aa72628218242811baa1a2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350819"
---
# <a name="inheritance-basics-visual-basic"></a>継承の基本 (Visual Basic)

`Inherits` ステートメントは、*基底クラス*と呼ばれる既存のクラスに基づいて、*派生クラス*と呼ばれる新しいクラスを宣言するために使用されます。 派生クラスは、基底クラスで定義されているプロパティ、メソッド、イベント、フィールド、および定数を継承し、拡張することができます。 次のセクションでは、継承に関するいくつかの規則と、クラスの継承方法や継承方法を変更するために使用できる修飾子について説明します。

- 既定では、`NotInheritable` キーワードでマークされていない限り、すべてのクラスが継承可能になります。 クラスは、プロジェクト内の他のクラス、またはプロジェクトが参照する他のアセンブリのクラスから継承できます。

- 複数の継承を許可する言語とは異なり、Visual Basic ではクラスでの単一継承のみが許可されます。つまり、派生クラスは基底クラスを1つだけ持つことができます。 クラスでは複数の継承は許可されていませんが、クラスは複数のインターフェイスを実装できます。これにより、同じ終了を効果的に実現できます。

- 基底クラスで制限された項目を公開しないようにするには、派生クラスのアクセスの種類がその基本クラスよりも制限以上である必要があります。 たとえば、`Public` クラスは、`Friend` または `Private` クラスを継承できません。また、`Friend` クラスは `Private` クラスを継承できません。

## <a name="inheritance-modifiers"></a>継承修飾子

Visual Basic では、継承をサポートするために、次のクラスレベルのステートメントと修飾子が導入されています。

- `Inherits` ステートメント-基本クラスを指定します。

- `NotInheritable` 修飾子: プログラマがクラスを基底クラスとして使用するのを防ぎます。

- `MustInherit` modifier: クラスが基底クラスとしてのみ使用されることを指定します。 `MustInherit` クラスのインスタンスを直接作成することはできません。派生クラスの基底クラスのインスタンスとしてのみ作成できます。 ( C++やなどの他のプログラミング言語C#では、*抽象クラス*という用語を使用して、このようなクラスを記述します)。

## <a name="overriding-properties-and-methods-in-derived-classes"></a>派生クラスのプロパティとメソッドのオーバーライド

既定では、派生クラスは、基本クラスからプロパティとメソッドを継承します。 継承されたプロパティまたはメソッドが派生クラスで動作が異なる場合は、*オーバーライド*できます。 つまり、派生クラスでメソッドの新しい実装を定義できます。 プロパティやメソッドのオーバーライド方法を制御するには、次の修飾子を使用します。

- `Overridable`: クラス内のプロパティまたはメソッドを派生クラスでオーバーライドできます。

- `Overrides`-基本クラスで定義されている `Overridable` のプロパティまたはメソッドをオーバーライドします。

- `NotOverridable`-継承クラスでプロパティまたはメソッドがオーバーライドされるのを防ぎます。 既定では、`Public` メソッドは `NotOverridable`です。

- `MustOverride`-派生クラスでプロパティまたはメソッドをオーバーライドする必要があります。 `MustOverride` キーワードを使用する場合、メソッド定義は、`Sub`、`Function`、または `Property` ステートメントだけで構成されます。 他のステートメントは許可されません。具体的には、`End Sub` または `End Function` ステートメントはありません。 `MustOverride` メソッドは `MustInherit` クラスで宣言する必要があります。

給与支払いを処理するクラスを定義するとします。 一般的な週の給与支払いを計算する `RunPayroll` 方法を含む汎用 `Payroll` クラスを定義できます。 その後、より特殊化された `BonusPayroll` クラスの基底クラスとして `Payroll` を使用できます。これは、従業員のボーナスを配布するときに使用できます。

`BonusPayroll` クラスは、基本 `Payroll` クラスで定義されている `PayEmployee` メソッドを継承し、オーバーライドできます。

次の例では、基底クラス、`Payroll,` および派生クラスを定義しています。 `BonusPayroll`は、継承されたメソッドの `PayEmployee`をオーバーライドします。 プロシージャ `RunPayroll`は、`Payroll` オブジェクトと `BonusPayroll` オブジェクトを作成し、両方のオブジェクトの `PayEmployee` メソッドを実行する関数 `Pay`に渡します。

[!code-vb[VbVbalrOOP#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#28)]

## <a name="the-mybase-keyword"></a>MyBase キーワード

`MyBase` キーワードは、クラスの現在のインスタンスの基底クラスを参照するオブジェクト変数のように動作します。 `MyBase` は、派生クラスでオーバーライドまたはシャドウされる基底クラスのメンバーにアクセスするためによく使用されます。 特に、`MyBase.New` は、派生クラスのコンストラクターから基底クラスのコンストラクターを明示的に呼び出すために使用されます。

たとえば、基底クラスから継承されたメソッドをオーバーライドする派生クラスを設計するとします。 オーバーライドされたメソッドは、基底クラスのメソッドを呼び出し、次のコードフラグメントに示すように戻り値を変更できます。

[!code-vb[VbVbalrOOP#109](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#109)]

`MyBase`の使用に関する制限事項を次に示します。

- `MyBase` は、直接の基底クラスとその継承されたメンバーを参照します。 クラスの `Private` メンバーにアクセスするために使用することはできません。

- `MyBase` は、実際のオブジェクトではなく、キーワードです。 `MyBase` を変数に割り当てたり、プロシージャに渡したり、`Is` の比較で使用したりすることはできません。

- `MyBase` 修飾されたメソッドは、直接の基底クラスで定義されている必要はありません。代わりに、間接的に継承された基底クラスで定義することができます。 `MyBase` によって修飾された参照を正しくコンパイルするために、一部の基底クラスには、呼び出しに表示されるパラメーターの名前と型に一致するメソッドが含まれている必要があります。

- `MyBase` を使用して `MustOverride` 基底クラスのメソッドを呼び出すことはできません。

- `MyBase` を使用して自身を修飾することはできません。 したがって、次のコードは無効です。

  `MyBase.MyBase.BtnOK_Click()`

- `MyBase` をモジュールで使用することはできません。

- 基底クラスが別のアセンブリにある場合、`Friend` としてマークされている基底クラスのメンバーにアクセスするために `MyBase` を使用することはできません。

詳細およびその他の例については、「[方法: 派生クラスによって非表示](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-access-a-variable-hidden-by-a-derived-class.md)にされている変数にアクセスする」を参照してください。

## <a name="the-myclass-keyword"></a>MyClass キーワード

`MyClass` キーワードは、もともと実装されているクラスの現在のインスタンスを参照するオブジェクト変数のように動作します。 `MyClass` は `Me`に似ていますが、`MyClass` のすべてのメソッドとプロパティ呼び出しは、メソッドまたはプロパティが[NotOverridable](../../../../visual-basic/language-reference/modifiers/notoverridable.md)であるかのように扱われます。 したがって、メソッドまたはプロパティは、派生クラスでオーバーライドしても影響を受けません。

- `MyClass` は、実際のオブジェクトではなく、キーワードです。 `MyClass` を変数に割り当てたり、プロシージャに渡したり、`Is` の比較で使用したりすることはできません。

- `MyClass` は、含んでいるクラスとその継承メンバーを参照します。

- `MyClass` は、`Shared` メンバーの修飾子として使用できます。

- `MyClass` は `Shared` メソッド内では使用できませんが、インスタンスメソッド内で使用して、クラスの共有メンバーにアクセスすることができます。

- `MyClass` は、標準モジュールでは使用できません。

- `MyClass` を使用すると、基底クラスで定義されていて、そのクラスで提供されるメソッドの実装を持たないメソッドを修飾できます。 このような参照は、`MyBase.`*メソッド*と同じ意味を持ちます。

次の例では、`Me` と `MyClass`を比較します。

```vb
Class baseClass
    Public Overridable Sub testMethod()
        MsgBox("Base class string")
    End Sub
    Public Sub useMe()
        ' The following call uses the calling class's method, even if
        ' that method is an override.
        Me.testMethod()
    End Sub
    Public Sub useMyClass()
        ' The following call uses this instance's method and not any
        ' override.
        MyClass.testMethod()
    End Sub
End Class
Class derivedClass : Inherits baseClass
    Public Overrides Sub testMethod()
        MsgBox("Derived class string")
    End Sub
End Class
Class testClasses
    Sub startHere()
        Dim testObj As derivedClass = New derivedClass()
        ' The following call displays "Derived class string".
        testObj.useMe()
        ' The following call displays "Base class string".
        testObj.useMyClass()
    End Sub
End Class
```

`derivedClass` が `testMethod`をオーバーライドする場合でも、`useMyClass` の `MyClass` キーワードは、をオーバーライドした場合の影響を nullifies、コンパイラは基本クラスバージョンの `testMethod`の呼び出しを解決します。

## <a name="see-also"></a>参照

- [Inherits ステートメント](../../../../visual-basic/language-reference/statements/inherits-statement.md)
- [Me、My、MyBase、および MyClass](../../../../visual-basic/programming-guide/program-structure/me-my-mybase-and-myclass.md)
