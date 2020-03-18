---
title: 型リスト
ms.date: 07/20/2015
f1_keywords:
- StructureConstraint
- vb.StructureConstraint
- ClassConstraint
- vb.ClassConstraint
helpviewer_keywords:
- class constraint
- constraints, Visual Basic generic types
- generic parameters
- generics [Visual Basic], constraints
- generics [Visual Basic], type list
- structure constraint
- constraints, in type parameters
- generics [Visual Basic], generic types
- parameters [Visual Basic], type
- constraints, Structure keyword
- type parameters [Visual Basic], constraints
- types [Visual Basic], generic
- parameters [Visual Basic], generic
- generics [Visual Basic], type parameters
- type parameters
- constraints, Class keyword
ms.assetid: 56db947a-2ae8-40f2-a70a-960764e9d0db
ms.openlocfilehash: 3f2aaa65293ed2bcc6c8eeb4bd77e49907d93425
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352774"
---
# <a name="type-list-visual-basic"></a>型リスト (Visual Basic)

*ジェネリック*プログラミング要素の*型パラメーター*を指定します。 複数のパラメーターはコンマで区切られます。 1つの型パラメーターの構文を次に示します。

## <a name="syntax"></a>構文

```vb
[genericmodifier] typename [ As constraintlist ]
```

## <a name="parts"></a>指定項目

|用語|Definition|
|---|---|
|`genericmodifier`|省略可。 は、ジェネリックインターフェイスおよびデリゲートでのみ使用できます。 [In](../../../visual-basic/language-reference/modifiers/in-generic-modifier.md)キーワードを使用し[て、型](../../../visual-basic/language-reference/modifiers/out-generic-modifier.md)を共変として宣言することができます。 [共変性と反変性](../../programming-guide/concepts/covariance-contravariance/index.md)を参照してください。|
|`typename`|必須。 型パラメーターの名前。 これは、対応する型引数によって提供される定義済みの型に置き換えられるプレースホルダーです。|
|`constraintlist`|省略可。 `typename`に指定できるデータ型を制限する要件の一覧。 複数の制約がある場合は、それらを中かっこ (`{ }`) で囲み、コンマで区切ります。 制約リストは [As](../../../visual-basic/language-reference/statements/as-clause.md) キーワードを使用して導入する必要があります。 `As` は、リストの先頭で1回だけ使用します。|

## <a name="remarks"></a>コメント

すべてのジェネリックプログラミング要素は、少なくとも1つの型パラメーターを受け取る必要があります。 型パラメーターは、ジェネリック型のインスタンスを作成するときにクライアントコードによって指定される特定の型 (構築された*要素*) のプレースホルダーです。 ジェネリッククラス、構造体、インターフェイス、プロシージャ、またはデリゲートを定義できます。

ジェネリック型を定義する場合の詳細については、「[Visual Basic におけるジェネリック型](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)」を参照してください。 型パラメーター名の詳細については、「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。

## <a name="rules"></a>ルール

- **Parentheses.** 型パラメータのリストを指定する場合は、リストをかっこで囲み、[Of](../../../visual-basic/language-reference/statements/of-clause.md) キーワードを使って特定する必要があります。 `Of` はリストの先頭に一度だけ記述します。 `Of` は、リストの先頭に一度だけ記述します。

- **制約。** ** 型パラメータに対する制約のリストには、次の項目を任意の組み合わせで定義できます。

  - 任意の数のインターフェイス。 指定された型は、このリスト内のすべてのインターフェイスを実装する必要があります。

  - クラスは1つだけです。 指定された型は、そのクラスから継承する必要があります。

  - `New` キーワード。 指定された型は、ジェネリック型がアクセスできるパラメーターなしのコンストラクターを公開する必要があります。 これは、1つまたは複数のインターフェイスによって型パラメーターを制約する場合に便利です。 インターフェイスを実装する型は、必ずしもコンストラクターを公開するわけではありません。また、コンストラクターのアクセスレベルによっては、ジェネリック型内のコードがアクセスできない場合があります。

  - `Class` キーワードまたは `Structure` キーワード。 `Class` キーワードは、ジェネリック型パラメーターを制約して、その型引数が参照型であることを要求します (文字列、配列、デリゲート、クラスから作成されたオブジェクトなど)。 `Structure` キーワードは、構造体、列挙型、基本データ型など、型引数が値型であることを要求するジェネリック型パラメーターを制約します。 `Class` と `Structure` を同じ `constraintlist`に含めることはできません。

  指定された型は、`constraintlist`に含まれるすべての要件を満たしている必要があります。

  型パラメーターごとの制約は、他の型パラメーターに対する制約とは無関係です。

## <a name="behavior"></a>動作

- **コンパイル時の代入。** ジェネリックプログラミング要素から構築された型を作成する場合は、型パラメーターごとに定義済みの型を指定します。 Visual Basic コンパイラは、ジェネリック要素内で `typename` が発生するたびに、指定された型を置き換えます。

- **制約の省略。** 型パラメーターに対して制約を指定しない場合、コードは、その型パラメーターの [Object データ型](../../../visual-basic/language-reference/data-types/object-data-type.md)でサポートされている操作とメンバーに制限されます。

## <a name="example"></a>例

次の例は、ジェネリックディクショナリクラスのスケルトン定義を示しています。これには、新しいエントリをディクショナリに追加するスケルトン関数も含まれます。

[!code-vb[VbVbalrStatements#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#3)]

## <a name="example"></a>例

`dictionary` はジェネリックであるため、このメソッドを使用するコードは、それぞれが同じ機能を持ち、別のデータ型に対して動作する、さまざまなオブジェクトを作成できます。 次の例は、`String` エントリと `Integer` キーを持つ `dictionary` オブジェクトを作成するコード行を示しています。

[!code-vb[VbVbalrStatements#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#4)]

## <a name="example"></a>例

次の例は、前の例で生成された同等のスケルトン定義を示しています。

[!code-vb[VbVbalrStatements#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#5)]

## <a name="see-also"></a>参照

- [Of](../../../visual-basic/language-reference/statements/of-clause.md)
- [New 演算子](../../../visual-basic/language-reference/operators/new-operator.md)
- [Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
- [Object データ型](../../../visual-basic/language-reference/data-types/object-data-type.md)
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
- [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)
- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)
- [方法 : ジェネリック クラスを使用する](../../../visual-basic/programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
- [共変性と反変性](../../programming-guide/concepts/covariance-contravariance/index.md)
- [In](../../../visual-basic/language-reference/modifiers/in-generic-modifier.md)
- [Out](../../../visual-basic/language-reference/modifiers/out-generic-modifier.md)
