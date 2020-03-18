---
title: バリアント ジェネリック インターフェイスの作成
ms.date: 07/20/2015
ms.assetid: d4037dd2-dfe9-4811-9150-93d4e8b20113
ms.openlocfilehash: 74362b9d9effab028bebb9e9ecf72ac0111366d3
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347066"
---
# <a name="creating-variant-generic-interfaces-visual-basic"></a>バリアントジェネリックインターフェイスの作成 (Visual Basic)

インターフェイスのジェネリック型パラメーターは、共変または反変として宣言できます。 "*共変性*" により、インターフェイス メソッドの戻り値の型の派生を、ジェネリック型パラメーターで定義されている型よりも強くすることができます。 "*反変性*" により、インターフェイス メソッドの引数の型の派生を、ジェネリック パラメーターで指定されている型よりも弱くすることができます。 共変または反変のジェネリック型パラメーターを持つジェネリック インターフェイスは、"*バリアント*" と呼ばれます。

> [!NOTE]
> .NET Framework 4 では、既存のいくつかのジェネリック インターフェイスに対して、変性のサポートが導入されています。 .NET Framework 内のバリアントインターフェイスの一覧については、「[ジェネリックインターフェイスの分散 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)」を参照してください。

## <a name="declaring-variant-generic-interfaces"></a>バリアント ジェネリック インターフェイスの宣言

バリアント ジェネリック インターフェイスは、ジェネリック型パラメーターの `in` キーワードと `out` キーワードを使用して宣言できます。

> [!IMPORTANT]
> Visual Basic の `ByRef` パラメーターをバリアントにすることはできません。 また、値の型も変性をサポートしていません。

ジェネリック型パラメーターを共変として宣言するには、`out` キーワードを使用します。 共変の型は、次の条件を満たす必要があります。

- 型がインターフェイス メソッドの戻り値の型としてのみ使用され、メソッド引数の型として使用されない。 この例を以下に示します。ここでは、型 `R` が共変として宣言されています。

    ```vb
    Interface ICovariant(Of Out R)
        Function GetSomething() As R
        ' The following statement generates a compiler error.
        ' Sub SetSomething(ByVal sampleArg As R)
    End Interface
    ```

    この規則には例外が 1 つあります。 反変の汎用デリゲートをメソッド パラメーターとして使用する場合は、型をデリゲートのジェネリック型パラメーターとして使用できます。 次の例では、型 `R` によって示します。 詳細については、「[デリゲートの分散 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md) 」および「 [Func と Action 汎用デリゲートの分散の使用 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)」を参照してください。

    ```vb
    Interface ICovariant(Of Out R)
        Sub DoSomething(ByVal callback As Action(Of R))
    End Interface
    ```

- 型がインターフェイス メソッドのジェネリック制約として使用されない。 これを次のコードに示します。

    ```vb
    Interface ICovariant(Of Out R)
        ' The following statement generates a compiler error
        ' because you can use only contravariant or invariant types
        ' in generic constraints.
        ' Sub DoSomething(Of T As R)()
    End Interface
    ```

ジェネリック型パラメーターを反変として宣言するには、`in` キーワードを使用します。 反変の型は、メソッド引数の型としてのみ使用でき、インターフェイス メソッドの戻り値の型として使用できません。 また、反変の型はジェネリック制約にも使用できます。 次のコードでは、反変のインターフェイスを宣言し、そのメソッドの 1 つにジェネリック制約を使用する方法を示します。

```vb
Interface IContravariant(Of In A)
    Sub SetSomething(ByVal sampleArg As A)
    Sub DoSomething(Of T As A)()
    ' The following statement generates a compiler error.
    ' Function GetSomething() As A
End Interface
```

次のコード例に示すように、同一インターフェイス内の異なる型パラメーターで、共変性と反変性の両方をサポートすることもできます。

```vb
Interface IVariant(Of Out R, In A)
    Function GetSomething() As R
    Sub SetSomething(ByVal sampleArg As A)
    Function GetSetSomething(ByVal sampleArg As A) As R
End Interface
```

Visual Basic では、デリゲート型を指定せずに、バリアントインターフェイスでイベントを宣言することはできません。 また、バリアントインターフェイスは、入れ子になったクラス、列挙型、または構造体を持つことはできませんが、入れ子になったインターフェイスを持つことができます。 これを次のコードに示します。

```vb
Interface ICovariant(Of Out R)
    ' The following statement generates a compiler error.
    ' Event SampleEvent()
    ' The following statement specifies the delegate type and
    ' does not generate an error.
    Event AnotherEvent As EventHandler

    ' The following statements generate compiler errors,
    ' because a variant interface cannot have
    ' nested enums, classes, or structures.

    'Enum SampleEnum : test : End Enum
    'Class SampleClass : End Class
    'Structure SampleStructure : Dim value As Integer : End Structure

    ' Variant interfaces can have nested interfaces.
    Interface INested : End Interface
End Interface
```

## <a name="implementing-variant-generic-interfaces"></a>バリアント ジェネリック インターフェイスの実装

バリアント ジェネリック インターフェイスをクラスに実装する場合は、インバリアント インターフェイスに使用するのと同じ構文を使用します。 次のコード例では、共変のインターフェイスをジェネリック クラスに実装する方法を示します。

```vb
Interface ICovariant(Of Out R)
    Function GetSomething() As R
End Interface

Class SampleImplementation(Of R)
    Implements ICovariant(Of R)
    Public Function GetSomething() As R _
    Implements ICovariant(Of R).GetSomething
        ' Some code.
    End Function
End Class
```

バリアント インターフェイスを実装するクラスは不変です。 次に例を示します。

```vb
 The interface is covariant.
Dim ibutton As ICovariant(Of Button) =
    New SampleImplementation(Of Button)
Dim iobj As ICovariant(Of Object) = ibutton

' The class is invariant.
Dim button As SampleImplementation(Of Button) =
    New SampleImplementation(Of Button)
' The following statement generates a compiler error
' because classes are invariant.
' Dim obj As SampleImplementation(Of Object) = button
```

## <a name="extending-variant-generic-interfaces"></a>バリアント ジェネリック インターフェイスの拡張

バリアント ジェネリック インターフェイスを拡張するときは、`in` キーワードと `out` キーワードを使用して、派生インターフェイスで変性をサポートするかどうかを明示的に指定する必要があります。 コンパイラでは、拡張されているインターフェイスからの変性の推論は行われません。 たとえば、次のようなインターフェイスがあるとします。

```vb
Interface ICovariant(Of Out T)
End Interface

Interface IInvariant(Of T)
    Inherits ICovariant(Of T)
End Interface

Interface IExtCovariant(Of Out T)
    Inherits ICovariant(Of T)
End Interface
```

`Invariant(Of T)` インターフェイスでは、ジェネリック型パラメーター `T` は不変であり、`IExtCovariant (Of Out T)`では、型パラメーターは共変ですが、どちらのインターフェイスも同じインターフェイスを拡張します。 これと同じ規則が、反変のジェネリック型パラメーターにも当てはまります。

拡張インターフェイスのジェネリック型パラメーター `T` が不変であれば、ジェネリック型パラメーター `T` が共変のインターフェイスと反変のインターフェイスの両方を拡張する 1 つのインターフェイスを作成できます。 これを次のコード例に図示します。

```vb
Interface ICovariant(Of Out T)
End Interface

Interface IContravariant(Of In T)
End Interface

Interface IInvariant(Of T)
    Inherits ICovariant(Of T), IContravariant(Of T)
End Interface
```

ただし、一方のインターフェイスでジェネリック型パラメーター `T` が共変として宣言されている場合は、それを拡張インターフェイスで反変として宣言することはできません。その逆についても同様です。 これを次のコード例に図示します。

```vb
Interface ICovariant(Of Out T)
End Interface

' The following statements generate a compiler error.
' Interface ICoContraVariant(Of In T)
'     Inherits ICovariant(Of T)
' End Interface
```

### <a name="avoiding-ambiguity"></a>あいまいさの回避

バリアント ジェネリック インターフェイスの実装時には、変性によってあいまいさが発生することがあります。 このような状況は回避する必要があります。

たとえば、同一のバリアント ジェネリック インターフェイスを、異なるジェネリック型パラメーターを使用して 1 つのクラスに明示的に実装すると、あいまいさが発生する可能性があります。 この場合、コンパイラでエラーは生成されませんが、実行時にどのインターフェイスの実装が選択されるかは明確ではありません。 これにより、コードで特定しにくいバグが発生する可能性があります。 次のようなコード例があるとします。

> [!NOTE]
> `Option Strict Off`を使用すると、あいまいなインターフェイス実装がある場合に、Visual Basic によってコンパイラの警告が生成されます。 `Option Strict On`では、Visual Basic によってコンパイラエラーが生成されます。

```vb
' Simple class hierarchy.
Class Animal
End Class

Class Cat
    Inherits Animal
End Class

Class Dog
    Inherits Animal
End Class

' This class introduces ambiguity
' because IEnumerable(Of Out T) is covariant.
Class Pets
    Implements IEnumerable(Of Cat), IEnumerable(Of Dog)

    Public Function GetEnumerator() As IEnumerator(Of Cat) _
        Implements IEnumerable(Of Cat).GetEnumerator
        Console.WriteLine("Cat")
        ' Some code.
    End Function

    Public Function GetEnumerator1() As IEnumerator(Of Dog) _
        Implements IEnumerable(Of Dog).GetEnumerator
        Console.WriteLine("Dog")
        ' Some code.
    End Function

    Public Function GetEnumerator2() As IEnumerator _
        Implements IEnumerable.GetEnumerator
        ' Some code.
    End Function
End Class

Sub Main()
    Dim pets As IEnumerable(Of Animal) = New Pets()
    pets.GetEnumerator()
End Sub
```

この例では、`pets.GetEnumerator` メソッドで `Cat` と `Dog` がどのように選択されるかが明らかではありません。 そのため、コードで問題が発生する可能性があります。

## <a name="see-also"></a>参照

- [ジェネリック インターフェイスの分散 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)
- [Func および Action 汎用デリゲートでの分散の使用 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)
