---
title: Protected
ms.date: 07/20/2015
f1_keywords:
- vb.Protected
helpviewer_keywords:
- Protected Friend keyword combination
- Protected keyword [Visual Basic], and Friend
- Protected keyword [Visual Basic], syntax
- Protected access modifier
- Protected keyword [Visual Basic]
ms.assetid: 74ad3d56-309f-49d2-b60c-1d0157d010e8
ms.openlocfilehash: 740c998b8a6ccc6798bce37e9b08e408dac7c17d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351299"
---
# <a name="protected-visual-basic"></a>Protected (Visual Basic)

1つ以上の宣言されたプログラミング要素が、独自のクラス内または派生クラスからのみアクセス可能であることを指定するメンバーアクセス修飾子。

## <a name="remarks"></a>コメント

場合によっては、クラスで宣言されたプログラミング要素が機微なデータまたは制限付きコードを含んでいて、要素へのアクセスを制限する必要があります。 ただし、クラスが継承可能であり、派生クラスの階層を想定している場合は、これらの派生クラスがデータまたはコードにアクセスするために必要な場合があります。 このような場合は、基本クラスとすべての派生クラスから要素にアクセスできるようにする必要があります。 このようにして、要素へのアクセスを制限するには、`Protected`で宣言します。

> [!NOTE]
> `Protected` アクセス修飾子は、次の2つの修飾子と組み合わせることができます。
>
> - [Protected Friend](protected-friend.md)修飾子は、クラス内、派生クラス、およびクラスが定義されている同じアセンブリから、クラスメンバーにアクセスできるようにします。
> - [Private Protected](private-protected.md)修飾子は、派生型でクラスメンバーにアクセスできるようにしますが、それを含むアセンブリ内でのみ使用できます。

## <a name="rules"></a>ルール

**宣言コンテキスト。** `Protected` はクラスレベルでのみ使用できます。 つまり、`Protected` 要素の宣言コンテキストはクラスである必要があり、ソースファイル、名前空間、インターフェイス、モジュール、構造体、またはプロシージャにすることはできません。

## <a name="behavior"></a>動作

- **アクセスレベル。** クラス内のすべてのコードは、その要素にアクセスできます。 基底クラスから派生したクラスのコードは、基底クラスのすべての `Protected` 要素にアクセスできます。 これは、派生のすべての世代に当てはまります。 これは、クラスが基底クラスの基底クラスの要素 `Protected` にアクセスできることを意味します。

     保護されたアクセスは、スーパーセットまたはフレンドアクセスのサブセットではありません。

- **アクセス修飾子。** アクセスレベルを指定するキーワードは、*アクセス修飾子*と呼ばれます。 アクセス修飾子の比較については、「[Visual Basic のアクセスレベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。

`Protected` 修飾子は、次のコンテキストで使用できます。

- [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)

- [Const ステートメント](../../../visual-basic/language-reference/statements/const-statement.md)

- [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)

- [Delegate ステートメント](../../../visual-basic/language-reference/statements/delegate-statement.md)

- [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)

- [Enum ステートメント](../../../visual-basic/language-reference/statements/enum-statement.md)

- [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)

- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)

- [Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md)

- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)

- [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)

- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)

## <a name="see-also"></a>参照

- [Public](../../../visual-basic/language-reference/modifiers/public.md)
- [Friend](../../../visual-basic/language-reference/modifiers/friend.md)
- [Private](../../../visual-basic/language-reference/modifiers/private.md)
- [Private Protected](private-protected.md)
- [Protected Friend](protected-friend.md)
- [Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
- [Visual Basic におけるプロシージャ](../../../visual-basic/programming-guide/language-features/procedures/index.md)
- [構造体](../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [クラスとオブジェクト](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
