---
title: Declare ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Declare
- vb.Lib
- vb.Any
helpviewer_keywords:
- Lib keyword [Visual Basic]
- declaring procedures [Visual Basic], Declare statement
- functions [Visual Basic], function procedures
- declarations [Visual Basic], procedures
- procedures [Visual Basic], declaration
- procedures [Visual Basic], external
- Alias keyword [Visual Basic]
- external references [Visual Basic], Visual Basic
- DLLs, declaring procedures
- Declare statement [Visual Basic]
- declarations [Visual Basic], external
- Visual Basic code, Function procedures
- As keyword [Visual Basic], in Declare statement
- resources [Visual Basic], declaring
- Public keyword [Visual Basic], Declare statement
- platform invoke, Visual Basic external references
- Sub procedures [Visual Basic], declarations
- APIs, declaring API functions
- Visual Basic code, Sub procedures
- Function procedures [Visual Basic], declaring
ms.assetid: d3f21fb0-b804-4c99-97ed-583b23894cf1
ms.openlocfilehash: 48a36e3ecdef40810ea7a3194e85b5b646154331
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354083"
---
# <a name="declare-statement"></a>Declare ステートメント

外部ファイルに実装されているプロシージャへの参照を宣言します。

## <a name="syntax"></a>構文

```vb
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ Overloads ] _
Declare [ charsetmodifier ] [ Sub ] name Lib "libname" _
[ Alias "aliasname" ] [ ([ parameterlist ]) ]
' -or-
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ Overloads ] _
Declare [ charsetmodifier ] [ Function ] name Lib "libname" _
[ Alias "aliasname" ] [ ([ parameterlist ]) ] [ As returntype ]
```

## <a name="parts"></a>指定項目

|用語|Definition|
|---|---|
|`attributelist`|省略可。 「[属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)」を参照してください。|
|`accessmodifier`|省略可。 次のいずれかになります。<br /><br /> -   [Public](../../../visual-basic/language-reference/modifiers/public.md)<br />-   [Protected](../../../visual-basic/language-reference/modifiers/protected.md)<br />-   [Friend](../../../visual-basic/language-reference/modifiers/friend.md)<br />-   [Private](../../../visual-basic/language-reference/modifiers/private.md)<br />- [Protected Friend](../../language-reference/modifiers/protected-friend.md)<br />- [Private Protected](../../language-reference/modifiers/private-protected.md)<br /><br /> 「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。|
|`Shadows`|省略可。 「[Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)」を参照してください。|
|`charsetmodifier`|省略可。 文字セットとファイル検索情報を指定します。 次のいずれかになります。<br /><br /> -   [Ansi](../../../visual-basic/language-reference/modifiers/ansi.md) (既定値)<br />-   [Unicode](../../../visual-basic/language-reference/modifiers/unicode.md)<br />-   [Auto](../../../visual-basic/language-reference/modifiers/auto.md)|
|`Sub`|省略可能ですが、`Sub` または `Function` のいずれかが表示されている必要があります。 外部プロシージャが値を返さないことを示します。|
|`Function`|省略可能ですが、`Sub` または `Function` のいずれかが表示されている必要があります。 外部プロシージャが値を返すことを示します。|
|`name`|必須。 この外部参照の名前です。 詳細については、「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。|
|`Lib`|必須。 では、外部プロシージャを含む外部ファイル (DLL またはコードリソース) を識別する `Lib` 句が導入されています。|
|`libname`|必須。 宣言されたプロシージャを含むファイルの名前。|
|`Alias`|省略可。 宣言されているプロシージャが、`name`で指定された名前によってファイル内で識別されないことを示します。 Id は `aliasname`で指定します。|
|`aliasname`|`Alias` キーワードを使用する場合は必須です。 次の2つの方法のいずれかでプロシージャを識別する文字列。<br /><br /> ファイル内のプロシージャのエントリポイント名 (引用符 (`""`))<br /><br /> または<br /><br /> ファイル内のプロシージャのエントリポイントの序数を指定する整数の後にシャープ記号 (`#`)|
|`parameterlist`|プロシージャがパラメーターを受け取る場合は必須です。 「[パラメーターリスト](../../../visual-basic/language-reference/statements/parameter-list.md)」を参照してください。|
|`returntype`|`Function` が指定され、`Option Strict` が `On`場合は必須です。 プロシージャによって返される値のデータ型。|

## <a name="remarks"></a>コメント

場合によっては、プロジェクト外のファイル (DLL やコードリソースなど) で定義されたプロシージャを呼び出す必要があります。 この場合、プロシージャが配置されている場所、その識別方法、呼び出し元のシーケンスと戻り値の型、および使用する文字列文字セットなど、プロシージャを正しく呼び出すために必要な情報には、Visual Basic コンパイラがアクセスできません。 `Declare` ステートメントは、外部プロシージャへの参照を作成し、この必要な情報を提供します。

`Declare` は、モジュール レベルでのみ使用できます。 つまり、外部参照の*宣言コンテキスト*は、クラス、構造体、またはモジュールである必要があり、ソースファイル、名前空間、インターフェイス、プロシージャ、またはブロックにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。

外部参照では、既定で[Public](../../../visual-basic/language-reference/modifiers/public.md)アクセスが使用されます。 アクセス修飾子を使用してこれらのアクセス レベルを調整できます。

## <a name="rules"></a>ルール

- **アトリビュート.** 外部参照に属性を適用できます。 適用した属性は、プロジェクト内でのみ有効になり、外部ファイルには反映されません。

- **ド.** 外部プロシージャは暗黙的に[Shared](../../../visual-basic/language-reference/modifiers/shared.md)されます。 外部参照を宣言するときに `Shared` キーワードを使用することはできません。また、共有ステータスを変更することもできません。

  外部プロシージャは、オーバーライド、インターフェイスメンバーの実装、またはイベントの処理に関与することはできません。 したがって、`Implements`ステートメントで `Overrides`、`Overridable`、`NotOverridable`、`MustOverride`、`Handles`、または `Declare` キーワードを使用することはできません。

- **外部プロシージャ名。** 外部ファイル (`aliasname`) 内のプロシージャのエントリポイント名と同じ名前 (`name`) をこの外部参照に与える必要はありません。 `Alias` 句を使用して、エントリポイント名を指定できます。 これは、外部プロシージャの名前が Visual Basic 予約済みの修飾子、変数、プロシージャ、または同じスコープ内のその他のプログラミング要素と同じである場合に便利です。

  > [!NOTE]
  > ほとんどの Dll のエントリポイント名は大文字と小文字が区別されます。

- **外部プロシージャ番号。** または、`Alias` 句を使用して、外部ファイルのエクスポートテーブル内のエントリポイントの序数を指定することもできます。 これを行うには、番号記号 (`#`) を使用して `aliasname` を開始します。 これは、Visual Basic で外部プロシージャ名に使用できない文字がある場合や、外部ファイルによってプロシージャが名前なしでエクスポートされた場合に便利です。

## <a name="data-type-rules"></a>データ型ルール

- **パラメーターのデータ型。** `Option Strict` が `On`場合は `parameterlist`で各パラメーターのデータ型を指定する必要があります。 任意のデータ型、または列挙体、構造体、クラス、またはインターフェイスの名前を指定できます。 `parameterlist`内では、`As` 句を使用して、各パラメーターに渡される引数のデータ型を指定します。

  > [!NOTE]
  > .NET Framework に対して外部プロシージャが記述されていない場合は、データ型が対応していることに注意する必要があります。 たとえば、`Integer` パラメーター (Visual Basic 6.0 で16ビット) を使用して Visual Basic 6.0 プロシージャへの外部参照を宣言する場合は、`Declare` の16ビット整数型であるため、対応する引数を Visual Basic ステートメントで `Short` として指定する必要があります。 同様に、`Long` のデータ幅は Visual Basic 6.0 で異なり、`Date` の実装方法も異なります。

- **戻り値のデータ型。** 外部プロシージャが `Function` で `Option Strict` が `On`場合は、呼び出し元のコードに返される値のデータ型を指定する必要があります。 任意のデータ型、または列挙体、構造体、クラス、またはインターフェイスの名前を指定できます。

  > [!NOTE]
  > Visual Basic コンパイラでは、データ型が外部プロシージャのデータ型と互換性があるかどうかは検証されません。 不一致がある場合は、実行時に共通言語ランタイムによって <xref:System.Runtime.InteropServices.MarshalDirectiveException> 例外が生成されます。

- **既定のデータ型。** `Option Strict` が `Off`、`parameterlist`でパラメーターのデータ型を指定していない場合、Visual Basic コンパイラは対応する引数を[Object データ型](../../../visual-basic/language-reference/data-types/object-data-type.md)に変換します。 同様に、`returntype`を指定しない場合、コンパイラは戻り値のデータ型を `Object`します。

  > [!NOTE]
  > 別のプラットフォームに記述されている可能性がある外部プロシージャを扱うため、データ型についての想定を作成したり、既定値を許可したりすることは危険です。 すべてのパラメーターのデータ型と戻り値 (存在する場合) を指定する方が、はるかに安全です。 これにより、コードの読みやすさも向上します。

## <a name="behavior"></a>動作

- **検索.** 外部参照は、そのクラス、構造体、またはモジュール全体でスコープ内にあります。

- **最短.** 外部参照の有効期間は、宣言されているクラス、構造体、またはモジュールと同じです。

- **外部プロシージャの呼び出し。** 外部プロシージャは、`Function` または `Sub` プロシージャを呼び出すのと同じ方法で、値を返す場合は式で使用するか、値を返さない場合は [Call ステートメント](../../../visual-basic/language-reference/statements/call-statement.md)で指定することによって呼び出します。

  引数は、`Declare` ステートメントの `parameterlist` で指定されたとおりに、外部プロシージャに渡すことができます。 パラメーターが外部ファイル内で最初に宣言された方法を考慮しないでください。 同様に、戻り値がある場合は、`Declare` ステートメントの `returntype` で指定されたとおりに使用します。

- **文字セット。** 外部プロシージャを呼び出すときに Visual Basic が文字列をマーシャリングする方法 `charsetmodifier` でを指定できます。 `Ansi` 修飾子は、すべての文字列を ANSI 値にマーシャリングするように Visual Basic に指示し、`Unicode` 修飾子は、すべての文字列を Unicode 値にマーシャリングするように指示します。 `Auto` 修飾子は、外部参照 `name`に基づいて .NET Framework 規則に従って文字列をマーシャリングするように Visual Basic に指示します。または、指定されている場合は `aliasname` します。 既定値は `Ansi` です。

  また、外部ファイル内で外部プロシージャを検索 Visual Basic 方法も指定 `charsetmodifier` ます。 検索中に名前を変更することなく、直接 Visual Basic を検索するように、両方を `Ansi` および `Unicode` ます。 `Auto` は、次のように、ランタイムプラットフォームの基本文字セットを決定するために Visual Basic に指示します。また、場合によっては外部プロシージャ名を変更します。

  - Windows 95、Windows 98、または Windows Millennium Edition などの ANSI プラットフォームでは、最初に名前を変更せずに外部プロシージャを検索します。 失敗した場合は、外部プロシージャ名の末尾に "A" を追加して、もう一度確認します。

  - Windows NT、Windows 2000、Windows XP などの Unicode プラットフォームでは、最初に名前を変更せずに外部プロシージャを検索します。 失敗した場合は、外部プロシージャ名の末尾に "W" を追加して、もう一度確認します。

- **しくみ.** Visual Basic は、.NET Framework *platform invoke* (PInvoke) メカニズムを使用して、外部プロシージャを解決し、アクセスします。 `Declare` ステートメントと <xref:System.Runtime.InteropServices.DllImportAttribute> クラスは、どちらもこのメカニズムを自動的に使用します。 PInvoke に関する知識は必要ありません。 詳細については、「[チュートリアル: Windows api の呼び出し](../../../visual-basic/programming-guide/com-interop/walkthrough-calling-windows-apis.md)」を参照してください。

> [!IMPORTANT]
> 外部プロシージャが共通言語ランタイム (CLR) の外部で実行されている場合は、*アンマネージコード*です。 このようなプロシージャ (Windows API 関数や COM メソッドなど) を呼び出すと、アプリケーションがセキュリティ上のリスクにさらされる可能性があります。 詳細については、「[アンマネージコードの安全なコーディングのガイドライン](../../../framework/security/secure-coding-guidelines-for-unmanaged-code.md)」を参照してください。

## <a name="example"></a>例

次の例では、現在のユーザー名を返す `Function` プロシージャへの外部参照を宣言しています。 次に、`getUser` プロシージャの一部として、外部プロシージャ `GetUserNameA` を呼び出します。

[!code-vb[VbVbalrStatements#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#15)]

## <a name="example"></a>例

<xref:System.Runtime.InteropServices.DllImportAttribute> には、アンマネージコードで関数を使用する別の方法が用意されています。 次の例では、`Declare` ステートメントを使用せずにインポートされた関数を宣言します。

[!code-vb[VbVbalrStatements#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#16)]

[!code-vb[VbVbalrStatements#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#1)]

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.ErrObject.LastDllError%2A>
- [Imports ステートメント (.NET 名前空間および型)](../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)
- [AddressOf 演算子](../../../visual-basic/language-reference/operators/addressof-operator.md)
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)
- [パラメーター リスト](../../../visual-basic/language-reference/statements/parameter-list.md)
- [Call ステートメント](../../../visual-basic/language-reference/statements/call-statement.md)
- [チュートリアル : Windows API の呼び出し](../../../visual-basic/programming-guide/com-interop/walkthrough-calling-windows-apis.md)
