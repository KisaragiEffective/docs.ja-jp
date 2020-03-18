---
title: メソッド
description: F#メソッドが、オブジェクトと型の機能および動作を公開および実装するために使用される型に関連付けられた関数であることを説明します。
ms.date: 11/04/2019
ms.openlocfilehash: 6f5ae76ea450b07763eb58d0c95b18b30f634551
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976645"
---
# <a name="methods"></a>メソッド

*メソッド*は、型に関連付けられている関数です。 オブジェクト指向プログラミングでは、メソッドを使用して、オブジェクトと型の機能および動作を公開および実装します。

## <a name="syntax"></a>構文

```fsharp
// Instance method definition.
[ attributes ]
member [inline] self-identifier.method-name parameter-list [ : return-type ] =
    method-body

// Static method definition.
[ attributes ]
static member [inline] method-name parameter-list [ : return-type ] =
    method-body

// Abstract method declaration or virtual dispatch slot.
[ attributes ]
abstract member method-name : type-signature

// Virtual method declaration and default implementation.
[ attributes ]
abstract member method-name : type-signature
[ attributes ]
default self-identifier.method-name parameter-list [ : return-type ] =
    method-body

// Override of inherited virtual method.
[ attributes ]
override self-identifier.method-name parameter-list [ : return-type ] =
    method-body

// Optional and DefaultParameterValue attributes on input parameters
[ attributes ]
[ modifier ] member [inline] self-identifier.method-name ([<Optional; DefaultParameterValue( default-value )>] input) [ : return-type ]
```

## <a name="remarks"></a>Remarks

前の構文では、さまざまな形式のメソッド宣言と定義を確認できます。 メソッド本体が長い場合、改行は等号 (=) に従い、メソッド本体全体がインデントされます。

属性は、任意のメソッド宣言に適用できます。 これらは、メソッド定義の構文の前にあり、通常は別の行に表示されます。 詳細については、「[属性](../attributes.md)」を参照してください。

メソッドは `inline`としてマークできます。 `inline` の詳細については、「[Inline Functions](../functions/inline-functions.md)」(インライン関数) を参照してください。

非インラインメソッドは、型の中で再帰的に使用できます。`rec` キーワードを明示的に使用する必要はありません。

## <a name="instance-methods"></a>インスタンスメソッド

インスタンスメソッドは、`member` キーワードと*自己識別子*を使用して宣言し、その後にピリオド (.) とメソッド名とパラメーターを指定して宣言します。 `let` バインドの場合と同様に、*パラメーターリスト*はパターンにすることができます。 通常、メソッドのパラメーターは、他の .NET Framework 言語で作成されたときにメソッドがF#に表示されるように、組形式でかっこで囲みます。 ただし、カリー化形式 (パラメーターはスペースで区切られています) も一般的であり、他のパターンもサポートされています。

次の例は、非抽象インスタンスメソッドの定義と使用方法を示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3401.fs)]

インスタンスメソッド内では、let バインドを使用して定義されたフィールドにアクセスするために、自己識別子を使用しないでください。 他のメンバーやプロパティにアクセスするときは、自己識別子を使用します。

## <a name="static-methods"></a>静的メソッド

キーワード `static` は、インスタンスを使用せずにメソッドを呼び出すことができ、オブジェクトインスタンスに関連付けられていないことを指定するために使用されます。 それ以外の場合、メソッドはインスタンスメソッドです。

次のセクションの例では、`let` キーワード、`member` キーワードで宣言されたプロパティメンバー、および `static` キーワードで宣言された静的メソッドを使用して宣言されたフィールドを示します。

次の例は、静的メソッドの定義と使用方法を示しています。 これらのメソッド定義は、前のセクションの `SomeType` クラスにあるものとします。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3402.fs)]

## <a name="abstract-and-virtual-methods"></a>抽象メソッドと仮想メソッド

キーワード `abstract` は、メソッドに仮想ディスパッチスロットがあり、クラスに定義がない可能性があることを示します。 *仮想ディスパッチスロット*は、オブジェクト指向の型の仮想関数呼び出しを検索するために実行時に使用される、内部的に管理された関数のテーブル内のエントリです。 仮想ディスパッチ機構は、オブジェクト指向プログラミングの重要な機能である*ポリモーフィズム*を実装するメカニズムです。 定義のない抽象メソッドを1つ以上持つクラスは*抽象クラス*であるため、そのクラスのインスタンスを作成することはできません。 抽象クラスの詳細については、「[抽象クラス](../abstract-classes.md)」を参照してください。

抽象メソッドの宣言には、メソッドの本体は含まれません。 代わりに、メソッドの名前の後にコロン (:)およびメソッドの型シグネチャ。 メソッドの型シグネチャは、パラメーター名がない場合を除き、Visual Studio Code エディターでメソッド名の上にマウスポインターを置くと IntelliSense によって表示されるものと同じです。 型シグネチャは、対話的に作業しているときにインタープリター fsi.exe によっても表示されます。 メソッドの型シグネチャは、適切な区切り記号を使用して、パラメーターの型と戻り値の型を一覧表示することによって作成されます。 カリー化されたパラメーターは `->` で区切られ、組のパラメーターは `*`で区切られます。 戻り値は常に、引数と `->` 記号で区切られます。 かっこを使用すると、関数の型がパラメーターである場合や、組が2つのパラメーターとしてではなく1つのパラメーターとして扱われるタイミングを指定する場合など、複雑なパラメーターをグループ化できます。

また、このトピックの構文ブロックに示すように、クラスに定義を追加し、`default` キーワードを使用して、抽象メソッドの既定の定義を指定することもできます。 同じクラス内に定義を持つ抽象メソッドは、他の .NET Framework 言語の仮想メソッドに相当します。 定義が存在するかどうかにかかわらず、`abstract` キーワードによって、クラスの仮想関数テーブルに新しいディスパッチスロットが作成されます。

基底クラスが抽象メソッドを実装するかどうかに関係なく、派生クラスは抽象メソッドの実装を提供できます。 派生クラスで抽象メソッドを実装するには、派生クラスで同じ名前とシグネチャを持つメソッドを定義します。ただし、`override` または `default` キーワードを使用し、メソッド本体を指定します。 キーワード `override` と `default` は、まったく同じことを意味します。 新しいメソッドが基底クラスの実装をオーバーライドする場合は、`override` を使用します。元の抽象宣言と同じクラスに実装を作成する場合は、`default` を使用します。 基底クラスで abstract として宣言されたメソッドを実装するメソッドでは、`abstract` キーワードを使用しないでください。

次の例は、.NET Framework 仮想メソッドに相当する既定の実装を持つ `Rotate` 抽象メソッドを示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3403.fs)]

基底クラスのメソッドをオーバーライドする派生クラスの例を次に示します。 この場合、オーバーライドは、メソッドが何も実行しないように動作を変更します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3404.fs)]

## <a name="overloaded-methods"></a>オーバーロードされたメソッド

オーバーロードされたメソッドは、指定された型の名前が同じで、引数が異なるメソッドです。 でF#は、通常、オーバーロードされたメソッドの代わりに、省略可能な引数が使用されます。 ただし、オーバーロードされたメソッドは、引数がカリー化形式ではなくタプル形式である場合に、その言語で使用できます。

## <a name="optional-arguments"></a>省略可能な引数

F# 4.1 以降では、メソッドに既定のパラメーター値を持つ省略可能な引数を指定することもできます。  これは、コードとのC#相互運用を容易にするためのものです。  構文の例を次に示します。

```fsharp
// A class with a method M, which takes in an optional integer argument.
type C() =
    _.M([<Optional; DefaultParameterValue(12)>] i) = i + 1
```

`DefaultParameterValue` に渡される値は入力型と一致する必要があることに注意してください。  上のサンプルでは、これは `int`です。  整数以外の値を `DefaultParameterValue` に渡そうとすると、コンパイルエラーが発生します。

## <a name="example-properties-and-methods"></a>例: プロパティとメソッド

次の例には、フィールド、プライベート関数、プロパティ、および静的メソッドの例を含む型が含まれています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3406.fs)]

## <a name="see-also"></a>関連項目

- [メンバー](index.md)
