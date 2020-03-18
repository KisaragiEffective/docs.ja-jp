---
title: スコープ
ms.date: 07/20/2015
helpviewer_keywords:
- module scope [Visual Basic]
- scope [Visual Basic], levels
- module level
- procedures [Visual Basic], scope
- declared elements [Visual Basic], scope
- namespaces [Visual Basic], scope
- scope [Visual Basic], declared elements
- scope [Visual Basic], about scope
- levels of scope [Visual Basic]
- block scope [Visual Basic]
- scope [Visual Basic], Visual Basic
- procedure scope [Visual Basic]
ms.assetid: 208106fe-79c9-4eec-93c6-55f08548895f
ms.openlocfilehash: 37fcfa897accb23e9c8c56407ce4ebd956b39c4d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345287"
---
# <a name="scope-in-visual-basic"></a>Visual Basic におけるスコープ

宣言された要素の*スコープ*は、その名前を修飾したり、[Imports ステートメント (.NET 名前空間および型)](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)を通じて使用可能にすることなく参照できるすべてのコードのセットです。 要素は、次のいずれかのレベルでスコープを持つことができます。

|Level|説明|
|-----------|-----------------|
|ブロック スコープ|宣言されているコードブロック内でのみ使用可能|
|プロシージャスコープ|宣言されているプロシージャ内のすべてのコードで使用できます。|
|モジュールのスコープ|宣言されているモジュール、クラス、または構造体内のすべてのコードで使用できます。|
|名前空間のスコープ|宣言されている名前空間のすべてのコードで使用可能|

最も狭い (ブロック) から最も幅の広い (名前空間) までの範囲の処理レベル。最も*狭いスコープ*は、要素を修飾なしで参照できる最小のコードセットを意味します。 詳細については、このページの「範囲のレベル」を参照してください。

## <a name="specifying-scope-and-defining-variables"></a>スコープの指定と変数の定義

要素のスコープは、宣言するときに指定します。 スコープは、次の要因に依存する場合があります。

- 要素を宣言する領域 (ブロック、プロシージャ、モジュール、クラス、または構造体)

- 要素の宣言を含む名前空間

- 要素に対して宣言するアクセスレベル

同じ名前でスコープが異なる変数を定義する場合は、慎重に行う必要があります。これは、予期しない結果につながる可能性があるためです。 詳細については、「[宣言された要素の参照](../../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)」を参照してください。

## <a name="levels-of-scope"></a>スコープのレベル

プログラミング要素は、宣言する領域全体で使用できます。 同じリージョン内のすべてのコードは、その名前を修飾せずに要素を参照できます。

### <a name="block-scope"></a>ブロックスコープ

ブロックは、次のように、宣言ステートメントの開始と終了に囲まれた一連のステートメントです。

- `Do` および `Loop`

- `For` [`Each`] と `Next`

- `If` および `End If`

- `Select` および `End Select`

- `SyncLock` および `End SyncLock`

- `Try` および `End Try`

- `While` および `End While`

- `With` および `End With`

ブロック内で変数を宣言すると、そのブロック内でのみ変数を使用できます。 次の例では、整数変数 `cube` のスコープが `If` と `End If`間のブロックであり、ブロックから実行が渡されたときに `cube` を参照することはできません。

```vb
If n < 1291 Then
    Dim cube As Integer
    cube = n ^ 3
End If
```

> [!NOTE]
> 変数のスコープがブロックに限定されている場合でも、その有効期間はプロシージャ全体の有効期間になります。 プロシージャの実行中にブロックを複数回入力した場合、各ブロック変数の前の値が保持されます。 このような場合に予期しない結果が生じないようにするには、ブロックの先頭でブロック変数を初期化することをお勧めします。

### <a name="procedure-scope"></a>プロシージャスコープ

プロシージャ内で宣言された要素は、そのプロシージャの外部では使用できません。 宣言を含むプロシージャだけが使用できます。 このレベルの変数は、*ローカル変数*とも呼ばれます。 これらの宣言は、 [Static](../../../../visual-basic/language-reference/modifiers/static.md)キーワードの有無にかかわらず、 [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)を使用して宣言します。

プロシージャとブロックのスコープは密接に関連しています。 プロシージャ内で変数を宣言し、そのプロシージャ内のブロックの外側で変数を宣言した場合、その変数はブロックスコープを持つと考えることができます。ブロックはプロシージャ全体です。

> [!NOTE]
> すべてのローカル要素は `Static` 変数であっても、それらが表示されるプロシージャに対してプライベートです。 プロシージャ内で[Public](../../../../visual-basic/language-reference/modifiers/public.md)キーワードを使用して要素を宣言することはできません。

### <a name="module-scope"></a>モジュールのスコープ

便宜上、単項*モジュールレベル*はモジュール、クラス、および構造体にも同様に適用されます。 このレベルで要素を宣言するには、宣言ステートメントをプロシージャまたはブロックの外側に配置し、モジュール、クラス、または構造体の内部に配置します。

モジュールレベルで宣言を行うと、選択したアクセスレベルによってスコープが決まります。 モジュール、クラス、または構造体を含む名前空間は、スコープにも影響します。

[Private](../../../../visual-basic/language-reference/modifiers/private.md) アクセスレベルを宣言する要素は、そのモジュール内のすべてのプロシージャで使用できますが、別のモジュール内のコードには使用できません。 アクセスレベルのキーワードを使用しない場合、モジュールレベルの `Dim` ステートメントは既定で `Private` になります。 ただし、`Dim` ステートメントで `Private` キーワードを使用すると、スコープとアクセスレベルをより明確にすることができます。

次の例では、モジュールで定義されているすべてのプロシージャが文字列変数 `strMsg`を参照できます。 2番目のプロシージャを呼び出すと、`strMsg` 文字列変数の内容がダイアログボックスに表示されます。

```vb
' Put the following declaration at module level (not in any procedure).
Private strMsg As String
' Put the following Sub procedure in the same module.
Sub initializePrivateVariable()
    strMsg = "This variable cannot be used outside this module."
End Sub
' Put the following Sub procedure in the same module.
Sub usePrivateVariable()
    MsgBox(strMsg)
End Sub
```

### <a name="namespace-scope"></a>名前空間のスコープ

[Friend](../../../../visual-basic/language-reference/modifiers/friend.md)または[Public](../../../../visual-basic/language-reference/modifiers/public.md)キーワードを使用してモジュールレベルで要素を宣言すると、その要素が宣言されている名前空間全体のすべてのプロシージャで使用できるようになります。 前の例を次のように変更すると、`strMsg` 文字列変数は、宣言の名前空間内の任意の場所でコードによって参照できます。

```vb
' Include this declaration at module level (not inside any procedure).
Public strMsg As String
```

名前空間スコープには入れ子になった名前空間が含まれます。 名前空間内から使用できる要素は、その名前空間内で入れ子になっている名前空間内からも使用できます。

プロジェクトに [Namespace ステートメント](../../../../visual-basic/language-reference/statements/namespace-statement.md)が含まれていない場合、プロジェクト内のすべてのものが同じ名前空間にあります。 この場合、名前空間のスコープはプロジェクトスコープと考えることができます。 モジュール、クラス、または構造体内の `Public` 要素は、そのプロジェクトを参照するすべてのプロジェクトでも使用できます。

## <a name="choice-of-scope"></a>スコープの選択

変数を宣言するときは、そのスコープを選択するときに、次の点に注意する必要があります。

### <a name="advantages-of-local-variables"></a>ローカル変数の利点

ローカル変数は、次の理由により、任意の種類の一時的な計算に適しています。

- **名前の競合の回避。** ローカル変数名が競合する可能性はありません。 たとえば、`intTemp`という変数を含むいくつかの異なるプロシージャを作成できます。 各 `intTemp` がローカル変数として宣言されている限り、各プロシージャは独自のバージョンの `intTemp`のみを認識します。 1つのプロシージャでは、他のプロシージャの `intTemp` 変数に影響を与えることなく、ローカル `intTemp` の値を変更できます。

- **メモリ使用量。** ローカル変数は、プロシージャの実行中にのみメモリを消費します。 メモリは、プロシージャが呼び出し元のコードに戻ったときに解放されます。 これに対し、[Shared](../../../../visual-basic/language-reference/modifiers/shared.md)変数と[Static](../../../../visual-basic/language-reference/modifiers/static.md)変数は、アプリケーションが実行を停止するまでメモリリソースを消費するため、必要な場合にのみ使用します。 インスタンス*変数*は、インスタンスが存在している間はメモリを消費するため、ローカル変数よりも効率的ではありませんが、`Shared` または `Static` 変数よりも効率的です。

### <a name="minimizing-scope"></a>スコープの最小化

一般に、変数または定数を宣言するときは、可能な限り範囲を絞り込むことをお勧めします (ブロックスコープが最も狭い)。 これにより、メモリを節約し、コードが間違った変数を誤って参照する可能性を最小限に抑えることができます。 同様に、プロシージャ呼び出し間で値を保持する必要がある場合にのみ、変数を[Static](../../../../visual-basic/language-reference/modifiers/static.md)に宣言する必要があります。

## <a name="see-also"></a>参照

- [宣言された要素の特性](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-characteristics.md)
- [方法: 変数のスコープを制御する](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-control-the-scope-of-a-variable.md)
- [Visual Basic の有効期間](../../../../visual-basic/programming-guide/language-features/declared-elements/lifetime.md)
- [Visual Basic でのアクセス レベル](../../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
- [宣言された要素の参照](../../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
