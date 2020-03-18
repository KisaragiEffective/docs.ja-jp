---
title: '方法 : プロシージャを作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- Visual Basic code, reusing
- procedure declarations
- procedures [Visual Basic], about procedures
ms.assetid: 4f779247-0b50-47e8-9e5c-ab5cf39ac0d2
ms.openlocfilehash: a831814c18f97991fca8067f1c9c8e491da1b665
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344907"
---
# <a name="how-to-create-a-procedure-visual-basic"></a>方法: プロシージャを作成する (Visual Basic)

プロシージャは、開始宣言ステートメント (`Sub` または `Function`) と終了宣言ステートメント (`End Sub` または `End Function`) の間で囲みます。 すべてのプロシージャのコードは、これらのステートメントの間にあります。

 プロシージャに別のプロシージャを含めることはできません。そのため、プロシージャの開始と終了のステートメントを他のプロシージャの外側に指定する必要があります。

 異なる場所で同じタスクを実行するコードがある場合は、そのタスクを1回のプロシージャとして記述し、コード内のさまざまな場所から呼び出すことができます。

### <a name="to-create-a-procedure-that-does-not-return-a-value"></a>値を返さないプロシージャを作成するには

1. 他のプロシージャの外部では、`Sub` ステートメントを使用し、その後に `End Sub` ステートメントを使用します。

2. `Sub` ステートメントで、`Sub` キーワードの後にプロシージャの名前を入力し、パラメーターリストをかっこで囲んで指定します。

3. プロシージャのコードステートメントを `Sub` と `End Sub` ステートメントの間に配置します。

### <a name="to-create-a-procedure-that-returns-a-value"></a>値を返すプロシージャを作成するには

1. 他のプロシージャの外部では、`Function` ステートメントを使用し、その後に `End Function` ステートメントを使用します。

2. `Function` ステートメントで、`Function` キーワードの後にプロシージャの名前を入力します。次に、かっこで囲まれたパラメーターリストを入力し、戻り値のデータ型を指定する `As` 句を指定します。

3. プロシージャのコードステートメントを `Function` と `End Function` ステートメントの間に配置します。

4. `Return` ステートメントを使用して、呼び出し元のコードに値を返します。

### <a name="to-connect-your-new-procedure-with-the-old-repetitive-blocks-of-code"></a>新しいプロシージャを古い繰り返しのコードブロックに接続するには

1. 古いコードがアクセスできる場所に新しいプロシージャを定義していることを確認します。

2. 繰り返し実行されるコードブロックで、繰り返し発生するタスクを実行するステートメントを、`Sub` または `Function` プロシージャを呼び出す1つのステートメントに置き換えます。

3. プロシージャが値を返す `Function` である場合は、呼び出し元のステートメントが戻り値を持つアクション (変数に格納するなど) を実行していることを確認してください。そうでない場合、値は失われます。

## <a name="example"></a>例

 次の `Function` プロシージャは、他の2つの辺の値を指定して、直角三角形の最長の辺 (斜辺) を計算します。

 [!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]

## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](index.md)
- [Sub プロシージャ](sub-procedures.md)
- [Function プロシージャ](function-procedures.md)
- [Property プロシージャ](property-procedures.md)
- [演算子プロシージャ](operator-procedures.md)
- [プロシージャのパラメーターと引数](procedure-parameters-and-arguments.md)
- [再帰プロシージャ](recursive-procedures.md)
- [プロシージャのオーバーロード](procedure-overloading.md)
- [クラスとオブジェクト](../objects-and-classes/index.md)
- [オブジェクト指向プログラミング (Visual Basic)](../../concepts/object-oriented-programming.md)
