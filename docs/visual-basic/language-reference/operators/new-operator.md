---
title: New 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.new
- vb.NewConstraint
helpviewer_keywords:
- constraints, Visual Basic generic types
- generics [Visual Basic], constraints
- constraints, New keyword [Visual Basic]
- New constraint
- New keyword [Visual Basic]
ms.assetid: d7d566d7-fe0e-4336-91f7-641a542de4d0
ms.openlocfilehash: 27b5b4516ef729045036c36fedc24b6c576a4f61
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348314"
---
# <a name="new-operator-visual-basic"></a>New 演算子 (Visual Basic)

では、新しいオブジェクトインスタンスを作成したり、型パラメーターにコンストラクター制約を指定したり、クラスコンストラクターとして `Sub` プロシージャを識別したりするための `New` 句が導入されています。

## <a name="remarks"></a>コメント

宣言または代入ステートメントでは、`New` 句で、インスタンスを作成できる定義済みのクラスを指定する必要があります。 これは、クラスが、呼び出し元のコードがアクセスできる1つ以上のコンストラクターを公開する必要があることを意味します。

`New` 句は、宣言ステートメントまたは代入ステートメントで使用できます。 ステートメントを実行すると、指定したクラスの適切なコンストラクターが呼び出され、指定した引数が渡されます。 次の例は、2つのコンストラクターを持つ `Customer` クラスのインスタンスを作成することによってこれを示しています。1つはパラメーターを取らず、もう1つは文字列パラメーターを受け取ります。

[!code-vb[VbVbalrKeywords#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class6.vb#11)]

配列はクラスであるため、次の例に示すように、`New` 新しい配列インスタンスを作成できます。

[!code-vb[VbVbalrKeywords#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class6.vb#12)]

新しいインスタンスを作成するためのメモリが不足している場合、共通言語ランタイム (CLR) は <xref:System.OutOfMemoryException> エラーをスローします。

> [!NOTE]
> `New` キーワードは、指定された型がアクセス可能なパラメーターなしのコンストラクターを公開する必要があることを指定するために、型パラメーターリストでも使用されます。 型パラメーターと制約の詳細については、「 [Type List](../statements/type-list.md)」を参照してください。

クラスのコンストラクタープロシージャを作成するには、`Sub` プロシージャの名前を `New` キーワードに設定します。 詳細については、「[オブジェクトの有効期間: オブジェクトの作成方法と破棄方法](../../programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)」を参照してください。

キーワード `New` は次のコンテキストで使用できます。

- [Dim ステートメント](../statements/dim-statement.md)
- [Of](../statements/of-clause.md)
- [Sub ステートメント](../statements/sub-statement.md)

## <a name="see-also"></a>関連項目

- <xref:System.OutOfMemoryException>
- [キーワード](../keywords/index.md)
- [型リスト](../statements/type-list.md)
- [Visual Basic におけるジェネリック型](../../programming-guide/language-features/data-types/generic-types.md)
- [オブジェクトの有効期間 : オブジェクトの作成と破棄](../../programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)
