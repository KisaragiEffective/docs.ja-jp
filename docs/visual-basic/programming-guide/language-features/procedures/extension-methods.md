---
title: 拡張メソッド
ms.date: 07/20/2015
f1_keywords:
- vb.ExtensionMethods
helpviewer_keywords:
- extending data types [Visual Basic]
- extension methods [Visual Basic]
ms.assetid: b8020aae-374d-46a9-bcb7-8cc2390b93b6
ms.openlocfilehash: a88756fce9137f89db1b6b8b007d528e98381830
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74341172"
---
# <a name="extension-methods-visual-basic"></a>拡張メソッド (Visual Basic)

拡張メソッドを使用すると、開発者は、新しい派生型を作成せずに既に定義されているデータ型にカスタム機能を追加できます。 拡張メソッドを使用すると、既存の型のインスタンスメソッドと同じように呼び出すことができるメソッドを作成できます。

## <a name="remarks"></a>コメント

拡張メソッドになるのは、`Sub` プロシージャと `Function` プロシージャだけです。 拡張プロパティ、拡張フィールド、拡張イベントを定義することはできません。 すべての拡張メソッドは、<xref:System.Runtime.CompilerServices?displayProperty=nameWithType> 名前空間から `<Extension>` 拡張属性でマークする必要があり、[Module](../../../language-reference/statements/module-statement.md)で定義する必要があります。 拡張メソッドがモジュール外で定義されている場合、Visual Basic コンパイラによってエラー [BC36551](../../../misc/bc36551.md)が生成されます。 "拡張メソッドはモジュール内でのみ定義できます"。

拡張メソッド定義の最初のパラメーターでは、そのメソッドが拡張するデータ型を指定します。 メソッドが実行されると、最初のパラメーターは、そのメソッドを呼び出すデータ型のインスタンスにバインディングされます。

`Extension` 属性は、Visual Basic [`Module`](../../../language-reference/statements/module-statement.md)、 [`Sub`](../../../language-reference/statements/sub-statement.md)、または[`Function`](../../../language-reference/statements/function-statement.md)にのみ適用できます。 `Class` または `Structure`に適用した場合、Visual Basic コンパイラによってエラー [BC36550](../../../language-reference/error-messages/extension-attribute-can-be-applied-only-to-module-sub-or-function-declarations.md)が生成されます。 "' Extension ' 属性は ' Module '、' Sub '、または ' Function ' 宣言にのみ適用できます。

## <a name="example"></a>例

`Print` データ型の <xref:System.String> 拡張を定義する例を次に示します。 このメソッドでは、`Console.WriteLine` を使用して文字列を表示します。 `Print` メソッドのパラメーター `aString` では、このメソッドによって <xref:System.String> クラスを拡張することを指定します。

[!code-vb[VbVbalrExtensionMethods#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/StringExtensions.vb#1)]

拡張メソッド定義に拡張属性 `<Extension()>` を設定している点に注目してください。 メソッドが定義されているモジュールに拡張属性を設定するかどうかは任意ですが、それぞれの拡張メソッドにはこの設定が必要です。 拡張属性にアクセスするには、<xref:System.Runtime.CompilerServices> をインポートする必要があります。

拡張メソッドはモジュール内でのみ宣言できます。 通常、拡張メソッドを定義するモジュールと拡張メソッドを呼び出すモジュールは、別々になります。 必要に応じて、拡張メソッドが含まれているモジュールをインポートすることによって、そのモジュールをスコープの中に入れます。 `Print` が含まれているモジュールをスコープの中に入れたら、引数を使用しない通常のインスタンス メソッド (`ToUpper` など) の場合と同じ要領でそのメソッドを呼び出すことができます。

[!code-vb[VbVbalrExtensionMethods#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class1.vb#2)]

次に取り上げる `PrintAndPunctuate` の例も <xref:System.String> の拡張ですが、今回は 2 つのパラメーターを定義します。 最初のパラメーター `aString` では、この拡張メソッドによって <xref:System.String> を拡張することを指定します。 2 番目のパラメーター `punc` では、メソッドの呼び出し時に引数として渡す区切り記号の文字列を指定します。 このメソッドでは、文字列の後にその区切り記号を表示します。

[!code-vb[VbVbalrExtensionMethods#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class2.vb#3)]

このメソッドを呼び出すときには、`punc` の引数として `example.PrintAndPunctuate(".")` を渡します。

`Print` と `PrintAndPunctuate` を定義して呼び出す例を次に示します。 拡張属性にアクセスできるようにするために、<xref:System.Runtime.CompilerServices> が定義モジュールにインポートされます。

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions

    <Extension()>
    Public Sub Print(aString As String)
        Console.WriteLine(aString)
    End Sub

    <Extension()>
    Public Sub PrintAndPunctuate(aString As String, punc As String)
        Console.WriteLine(aString & punc)
    End Sub
End Module
```

次に、拡張メソッドがスコープ内に取り込まれ、次のように呼び出されます。

```vb
Imports ConsoleApplication2.StringExtensions

Module Module1

    Sub Main()
        Dim example As String = "Example string"
        example.Print()

        example = "Hello"
        example.PrintAndPunctuate(".")
        example.PrintAndPunctuate("!!!!")
    End Sub
End Module
```

このような拡張メソッドを実行するための唯一の要件は、その拡張メソッドをスコープの中に組み入れておくことです。 拡張メソッドが含まれているモジュールがスコープの中に入っていれば、その拡張メソッドは IntelliSense からアクセスできるということであり、通常のインスタンス メソッドの場合と同じ要領で呼び出すことができます。

メソッドを呼び出すときに、最初のパラメーターの引数を渡していない点に注目してください。 前のメソッド定義のパラメーター `aString` が、メソッドを呼び出す `example` のインスタンスである `String` にバインディングされています。 コンパイラは、最初のパラメーターに渡す引数としてその `example` を使用します。

`Nothing` に設定されたオブジェクトに対して拡張メソッドが呼び出された場合、その拡張メソッドが実行されます。 これは、通常のインスタンス メソッドには適用されません。 拡張メソッドの `Nothing` は明示的にチェックできます。

## <a name="types-that-can-be-extended"></a>拡張できる型

拡張メソッドは、Visual Basic のパラメーター リストで記述できるほとんどの型で定義できます。以下に例を示します。

- クラス (参照型)
- 構造体 (値型)
- インターフェイス
- デリゲート
- ByRef 引数と ByVal 引数
- ジェネリック メソッド パラメーター
- 配列

最初のパラメーターでは、メソッドによって拡張するデータ型を指定するので、最初のパラメーターは必須であり、任意指定にすることはできません。 したがって、パラメーター リストの最初のパラメーターとして、`Optional` パラメーターと `ParamArray` パラメーターを記述することはできません。

拡張メソッドは遅延バインディングでは考慮されません。 次の例では、`anObject.PrintMe()` ステートメントで <xref:System.MissingMemberException> 例外が発生します。これは、2 番目の `PrintMe` 拡張メソッド定義を削除した場合に発生する例外と同じです。

[!code-vb[VbVbalrExtensionMethods#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class6.vb#9)]

## <a name="best-practices"></a>ベスト プラクティス

拡張メソッドは、既存の型を拡張するための便利で強力な手段になります。 それでも、適切に使用するにはいくつかの注意点があります。 ここで取り上げる注意点は、主にクラス ライブラリを作成するときに当てはまりますが、拡張メソッドを使用するアプリケーションであればどんなアプリケーションにも影響する可能性があります。

一般的に、自分で所有していない型に追加した拡張メソッドは、自分で制御できる型に追加した拡張メソッドよりも脆弱になります。 自分で所有していないクラスでは、拡張メソッドの動作に影響を及ぼしかねない事柄がいくつか発生する可能性があります。

- 呼び出し元ステートメントの引数との互換性があるシグネチャを持ったアクセス可能なインスタンス メンバーが存在し、引数からパラメーターへの縮小変換が不要な場合は、拡張メソッドよりもそのインスタンス メソッドの方が優先的に使用されます。 したがって、該当するインスタンス メソッドがいずれかの時点でクラスに追加されると、使用しなければならない既存の拡張メソッドにアクセスできなくなる可能性があります。

- 拡張メソッドの作成者の側では、その拡張メソッドよりも優先的に使用される可能性がある別の拡張メソッドを他のプログラマが作成する、という事態を防止できません。

- 拡張メソッドをそれ自身の名前空間に入れておけば、拡張メソッドの信頼性が向上します。 ライブラリを利用する側では、ライブラリの名前空間とそれ以外の部分を分けて、名前空間を組み込んだり除外したり取捨選択したりすることができます。

- クラスを拡張するよりもインターフェイスを拡張する方が安全です。インターフェイスまたはクラスを自分で所有していない場合は特にそういえます。 インターフェイスが変更されると、そのインターフェイスを実装するすべてのクラスが影響を受けます。 したがって、インターフェイスでメソッドが追加されたり変更されたりする可能性の方が低いといえます。 ただし、クラスが同じシグニチャの拡張メソッドを持つ 2 つのインターフェイスを実装する場合、どちらの拡張メソッドにもアクセスできません。

- できるだけ具体性の高い型を拡張するようにします。 型の階層の中で他の多くの型の派生元になっている型で拡張メソッドを選択すると、その拡張メソッドの動作に影響を及ぼしかねないインスタンス メソッドや他の拡張メソッドが組み込まれる可能性が高くなります。

## <a name="extension-methods-instance-methods-and-properties"></a>拡張メソッド、インスタンスメソッド、およびプロパティ

スコープ内のインスタンス メソッドが、呼び出し元ステートメントの引数と互換性があるシグネチャを持っている場合、拡張メソッドよりもそのインスタンス メソッドの方が優先的に使用されます。 この場合、より適合する拡張メソッドがあっても、インスタンス メソッドの方が優先されます。 次の例では、`ExampleClass` に、`ExampleMethod` 型のパラメーターを 1 つ持つ `Integer` という名前のインスタンス メソッドが含まれています。 拡張メソッド `ExampleMethod` は `ExampleClass` を拡張し、`Long` 型のパラメーターを 1 つ持ちます。

[!code-vb[VbVbalrExtensionMethods#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class4.vb#4)]

次のコードでは、`ExampleMethod` の最初の呼び出しで、拡張メソッドが呼び出されます。これは、`arg1` が `Long` であり、拡張メソッドの `Long` パラメーターとのみ互換性があるためです。 `ExampleMethod` の 2 回目の呼び出しでは、`Integer` 引数 `arg2` があるため、インスタンス メソッドが呼び出されます。

[!code-vb[VbVbalrExtensionMethods#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class4.vb#5)]

次は、2 つのメソッド間でパラメーターのデータ型が逆になっています。

[!code-vb[VbVbalrExtensionMethods#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class5.vb#6)]

今回は、`Main` 内のコードはどちらの場合でもインスタンス メソッドを呼び出します。 これは、`arg1` と `arg2` は `Long` へ拡大変換され、どちらの場合でも拡張メソッドよりインスタンス メソッドの方が優先されるためです。

[!code-vb[VbVbalrExtensionMethods#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class5.vb#7)]

つまり、既存のインスタンス メソッドの代わりに拡張メソッドを使用することはできません。 ただし、拡張メソッドとインスタンス メソッドの名前が同じでもシグネチャが競合しない場合は、両方のメソッドを使用できます。 たとえば、クラス `ExampleClass` に引数を使用しない `ExampleMethod` という名前のメソッドがあるとします。拡張メソッドの名前がそのメソッドと同じでもシグネチャが違えば、その拡張メソッドを使用することは可能です。次に例を示します。

[!code-vb[VbVbalrExtensionMethods#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Module3.vb#8)]

このコードの出力は次のようになります。

```console
Extension method
Instance method
```

プロパティの場合、状況はより単純です。拡張メソッドの名前がそのクラスのプロパティと同じである場合、その拡張メソッドは非表示になり、アクセスできません。

## <a name="extension-method-precedence"></a>拡張メソッドの優先順位

2 つの拡張メソッドのシグネチャが同じで、そのいずれもスコープに入っていてアクセスが可能な場合は、優先順位の高い方が呼び出されます。 拡張メソッドの優先順位は、メソッドをスコープに組み入れる方法に基づいています。 優先順位の高い方から低い方へと並べると、次のようになります。

1. 現在のモジュールの中で定義されている拡張メソッド。

2. 現在の名前空間の方がその親に相当する名前空間よりも優先順位が高ければ、現在の名前空間またはそのいずれかの親に相当する名前空間にあるデータ型の中で定義されている拡張メソッド。

3. 現在のファイルの型インポートの中で定義されている拡張メソッド。

4. 現在のファイルの名前空間インポートの中で定義されている拡張メソッド。

5. プロジェクト レベルの型インポートの中で定義されている拡張メソッド。

6. プロジェクト レベルの名前空間インポートの中で定義されている拡張メソッド。

優先順位を適用してもあいまいさが残る場合は、完全修飾名を使用して、呼び出すメソッドを指定できます。 先ほどの例の `Print` メソッドが `StringExtensions` という名前のモジュールで定義されていれば、完全修飾名は `StringExtensions.Print(example)` ではなく `example.Print()` になります。

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.CompilerServices>
- <xref:System.Runtime.CompilerServices.ExtensionAttribute>
- [拡張メソッド](../../../../csharp/programming-guide/classes-and-structs/extension-methods.md)
- [Module ステートメント](../../../language-reference/statements/module-statement.md)
- [プロシージャのパラメーターと引数](procedure-parameters-and-arguments.md)
- [省略可能なパラメーター](optional-parameters.md)
- [パラメーター配列](parameter-arrays.md)
- [属性の概要](../../concepts/attributes/index.md)
- [Visual Basic におけるスコープ](../declared-elements/scope.md)
