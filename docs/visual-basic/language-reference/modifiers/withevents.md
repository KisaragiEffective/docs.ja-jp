---
title: WithEvents
ms.date: 07/20/2015
f1_keywords:
- vb.WithEvents
- WithEvents
helpviewer_keywords:
- WithEvents keyword [Visual Basic]
ms.assetid: 19d461f5-d72f-4de9-8c1d-0a6650316990
ms.openlocfilehash: 2309c675b50a2025d73841a47fe8e30e7cecd522
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350748"
---
# <a name="withevents-visual-basic"></a>WithEvents (Visual Basic)
1つ以上の宣言されたメンバー変数が、イベントを発生させることができるクラスのインスタンスを参照することを指定します。

## <a name="remarks"></a>コメント

`WithEvents`を使用して変数が定義されている場合は、メソッドが `Handles` キーワードを使用して変数のイベントを処理するように、宣言によって指定できます。

`WithEvents` は、クラスレベルまたはモジュールレベルでのみ使用できます。 つまり、`WithEvents` 変数の宣言コンテキストは、クラスまたはモジュールである必要があり、ソースファイル、名前空間、構造体、またはプロシージャにすることはできません。

構造体メンバーで `WithEvents` を使用することはできません。

`WithEvents`では、配列ではなく個々の変数のみを宣言できます。

## <a name="rules"></a>ルール

**要素の型。** オブジェクト変数として `WithEvents` 変数を宣言して、クラスのインスタンスを受け入れるようにする必要があります。 ただし、`Object`として宣言することはできません。 これらのクラスは、イベントを発生させることができる特定のクラスとして宣言する必要があります。

`WithEvents` 修飾子は、次のコンテキストで使用できます: [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)

## <a name="example"></a>例

```vb
Dim WithEvents app As Application
```

## <a name="see-also"></a>参照

- [!](../../../visual-basic/language-reference/statements/handles-clause.md)
- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
- [イベント](../../../visual-basic/programming-guide/language-features/events/index.md)
