---
title: Object データ型
ms.date: 07/20/2015
f1_keywords:
- vb.Object
- vb.Variant
helpviewer_keywords:
- object variables [Visual Basic], Object type
- data types [Visual Basic], assigning
- Object data type
- Object data type [Visual Basic], reference
ms.assetid: 61ea4a7c-3b3d-48d4-adc4-eacfa91779b2
ms.openlocfilehash: 2ccb9b69b865c259d078ed9642d63c7f83514756
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343966"
---
# <a name="object-data-type"></a>Object データ型

オブジェクトを参照するアドレスを保持します。 任意の参照型 (文字列、配列、クラス、またはインターフェイス) を `Object` 変数に割り当てることができます。 `Object` 変数は、任意の値型 (数値、`Boolean`、`Char`、`Date`、構造体、または列挙型) のデータを参照することもできます。

## <a name="remarks"></a>コメント

`Object` のデータ型は、アプリケーションで認識される任意のデータ型のデータを指すことができます。 コンパイル時に変数が指している可能性のあるデータ型を把握していない場合は、`Object` を使用します。

`Object` の既定値は `Nothing` (null 参照) です。

## <a name="data-types"></a>データ型

任意のデータ型の変数、定数、または式を `Object` 変数に割り当てることができます。 `Object` 変数で現在参照されているデータ型を特定するには、<xref:System.Type?displayProperty=nameWithType> クラスの <xref:System.Type.GetTypeCode%2A> メソッドを使用します。 次に例を示します。

```vb
Dim myObject As Object
' Suppose myObject has now had something assigned to it.
Dim datTyp As Integer
datTyp = Type.GetTypeCode(myObject.GetType())
```

`Object` データ型は参照型です。 ただし、Visual Basic は、値型のデータを参照するときに、`Object` 変数を値型として扱います。

## <a name="storage"></a>ストレージ

参照先のデータ型にかかわらず、`Object` 変数にはデータ値自体は含まれませんが、値へのポインターが含まれます。 コンピューターのメモリでは常に4バイトを使用しますが、変数の値を表すデータのストレージは含まれません。 ポインターを使用してデータを検索するコードのため、`Object` 値型を保持する変数は、明示的に型指定された変数よりもアクセスが多少遅くなります。

## <a name="programming-tips"></a>プログラミングのヒント

- **相互運用に関する考慮事項。** .NET Framework 用に作成されていないコンポーネント (オートメーションや COM オブジェクトなど) とやり取りする場合は、他の環境のポインター型が Visual Basic `Object` 型と互換性がないことに注意してください。

- **パフォーマンス。** `Object` 型で宣言する変数は、任意のオブジェクトへの参照を格納するのに十分な柔軟性があります。 ただし、このような変数に対してメソッドまたはプロパティを呼び出すと、常に*遅延バインディング*(実行時) が発生します。 (コンパイル時に)*事前バインディング*を強制的に実行し、パフォーマンスを向上させるには、特定のクラス名を持つ変数を宣言するか、特定のデータ型にキャストします。

  オブジェクト変数を宣言するときは、一般化された `Object` 型ではなく、<xref:System.OperatingSystem>などの特定のクラス型を使用してみてください。 また、<xref:System.Windows.Forms.Control>ではなく <xref:System.Windows.Forms.TextBox> など、使用可能な最も具体的なクラスを使用して、そのプロパティとメソッドにアクセスできるようにする必要があります。 通常は、**オブジェクトブラウザー**の**クラス**の一覧を使用して、使用可能なクラス名を検索できます。

- **広げ.** すべてのデータ型とすべての参照型は、`Object` データ型に拡大変換されます。 これは、<xref:System.OverflowException?displayProperty=nameWithType> エラーが発生することなく、任意の型を `Object` に変換できることを意味します。

  ただし、値型と `Object`の間で変換を行う場合、Visual Basic によって、*ボックス*化と*ボックス化解除*と呼ばれる操作が実行され、実行速度が低下します。

- **文字を入力します。** `Object` には、リテラルの型文字または識別子の型文字がありません。

- **フレームワークの種類。** .NET Framework 内の対応する型は、<xref:System.Object?displayProperty=nameWithType> クラスです。

## <a name="example"></a>例

次の例は、オブジェクトインスタンスを指す `Object` 変数を示しています。

```vb
Dim objDb As Object
Dim myCollection As New Collection()
' Suppose myCollection has now been populated.
objDb = myCollection.Item(1)
```

## <a name="see-also"></a>関連項目

- <xref:System.Object>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
- [方法: 2 つのオブジェクトが関連しているかどうかを決める](../../../visual-basic/programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-related.md)
- [方法: 2 つのオブジェクトが同一であるかどうか判別する](../../../visual-basic/programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-identical.md)
