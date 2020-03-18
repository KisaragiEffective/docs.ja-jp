---
title: 名前付け規則
ms.date: 07/20/2015
helpviewer_keywords:
- names [Visual Basic], Visual Basic rules
- naming conventions
- naming conventions [Visual Basic], Visual Basic
- Visual Basic code, naming conventions
- conventions [Visual Basic], Visual Basic coding
- names [Visual Basic], naming conventions
- naming conventions [Visual Basic], classes
ms.assetid: 164949a4-2a7c-4736-9d82-9c3078e2e56c
ms.openlocfilehash: 98fdda2934c9df1b33f41b6e0442a39246efe168
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347320"
---
# <a name="visual-basic-naming-conventions"></a>Visual Basic の名前付け規則
Visual Basic アプリケーションの要素に名前を付けるときは、その名前の最初の文字は、英字またはアンダー スコアにする必要があります。 ただし、アンダースコアで始まる名前は、[言語への非依存性、および言語非依存コンポーネント](../../../standard/language-independence-and-language-independent-components.md)(CLS)に準拠していないことに注意してください。  
  
 次の提案が、名前付けに適用されます。  
  
- `FindLastRecord` と `RedrawMyForm`のように、名前に含まれる各単語の大文字を大文字にして開始します。  
  
- `InitNameArray` または `CloseDialog`のように、動詞を使用して関数名とメソッド名を開始します。  
  
- `EmployeeName` または `CarAccessory`のように、クラス、構造体、モジュール、およびプロパティの名前を名詞で開始します。  
  
- "I" というプレフィックスを使用してインターフェイス名を開始し、`IComponent`のように名詞または名詞句を続けます。または、`IPersistable`のように、インターフェイスの動作を記述する形容詞を使用します。 アンダー スコアは使用しないで、略語の使用も控えてください。 なぜなら略語は混乱を招くからです。  
  
- "`MouseEventHandler`" のように、イベントの種類と "`EventHandler`" サフィックスが記述されている名詞を使用して、イベントハンドラー名を開始します。  
  
- イベント引数クラスの名前には、"`EventArgs`" サフィックスを含めます。  
  
- イベントの概念が "before" または "after" の場合は、"`ControlAdd`" または "`ControlAdded`" のように、現在または過去の形でサフィックスを使用します。  
  
- 長い、または頻繁に使用される用語では、名前の長さを適切に維持するために略語を使用します。 たとえば、"Hypertext Markup Language"の代わりに"HTML"を使用します。 一般的に、32 文字を超える変数の名前は低解像度のモニターでは読むことが困難です。 また、略語はアプリケーション全体を通して一貫性のあるようにしてください。 プロジェクトの中で"HTML"と"Hypertext Markup Language"をランダムに切り替えることは混乱を招く可能性があります。  
  
- 外部スコープで使われている名前と同じものを、内部スコープの中で使用することは避けてください。 誤った変数がアクセスされた場合エラーが発生する可能性があります。 同名のキーワードと変数の間でコンフリクトが起きる場合、適切な種類のライブラリを使って先立ってキーワードを識別しなければなりません。 たとえば、`Date`という変数がある場合は、<xref:System.DateTime.Date%2A?displayProperty=nameWithType>を呼び出すことによってのみ、組み込みの `Date` 関数を使用できます。  
  
## <a name="see-also"></a>参照

- [コード内の要素名としてのキーワード](../../../visual-basic/programming-guide/program-structure/keywords-as-element-names-in-code.md)
- [Me、My、MyBase、および MyClass](../../../visual-basic/programming-guide/program-structure/me-my-mybase-and-myclass.md)
- [宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)
- [プログラム構造とコード規則](../../../visual-basic/programming-guide/program-structure/program-structure-and-code-conventions.md)
- [Visual Basic の言語リファレンス](../../../visual-basic/language-reference/index.md)
