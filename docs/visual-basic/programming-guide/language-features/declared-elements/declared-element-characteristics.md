---
title: 宣言された要素の特性
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic], lifetime
- access levels, declared elements
- declared elements [Visual Basic], scope
- visibility [Visual Basic], declared elements
- elements [Visual Basic], programming
- scope [Visual Basic], declared elements
- lifetime [Visual Basic], declared elements
- declared elements [Visual Basic], access level
- data types [Visual Basic], declared elements
- declared elements [Visual Basic], visibility
ms.assetid: 1bc40fb8-b67c-4428-90a4-76b630ae2583
ms.openlocfilehash: 4e03cd28fed5e0ae109337739251c11a0ff3424a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74331626"
---
# <a name="declared-element-characteristics-visual-basic"></a>宣言された要素の特性 (Visual Basic)
宣言された要素の*特性*は、コードと対話する方法に影響する要素の側面です。 宣言されたすべての要素には、次の1つ以上の特性が関連付けられています。  
  
- *データ型* - 要素が保持できる値と、その値を格納する方法。 詳細については、「[データ型の概要(Visual Basic)](../../../../visual-basic/language-reference/data-types/index.md)」を参照してください。 
  
- *有効期間* - 要素を使用できるようになるまでの実行時間。 詳細については、「[Visual Basic の有効期間](../../../../visual-basic/programming-guide/language-features/declared-elements/lifetime.md)」を参照してください。  
  
- *スコープ* - 名前を修飾せずに要素を参照できるすべてのコードのセット。 詳細については、「[方法: 変数のスコープを制御する](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-control-the-scope-of-a-variable.md)」を参照してください。  
  
- *アクセスレベル* - 要素を使用するためのコードのアクセス許可。 詳細については、「[方法: 変数の可用性を制御する](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-control-the-availability-of-a-variable.md)」を参照してください。  
  
## <a name="characteristics-of-the-elements"></a>要素の特性  
 次の表は、宣言された要素と、それぞれに適用される特性を示しています。  
  
|要素|データ型|有効期間|スコープ<sup>1</sup>|アクセスレベル|  
|-------------|---------------|--------------|------------------------|------------------|  
|変数|はい|はい|はい|はい|  
|定数|はい|いいえ|はい|はい|  
|列挙値|はい|いいえ|はい|はい|  
|構造体|いいえ|いいえ|はい|はい|  
|プロパティ|はい|はい|はい|はい|  
|メソッド|いいえ|はい|はい|はい|  
|プロシージャ (`Sub` または `Function`)|いいえ|はい|はい|はい|  
|プロシージャ パラメーター|はい|はい|はい|いいえ|  
|関数の戻り値|はい|はい|はい|いいえ|  
|演算子|はい|いいえ|はい|はい|  
|インターフェイス|いいえ|いいえ|はい|はい|  
|クラス|いいえ|いいえ|はい|はい|  
|イベント|いいえ|いいえ|はい|はい|  
|デリゲート|いいえ|いいえ|はい|はい|  
  
 <sup>1</sup>スコープは、*可視性*と呼ばれることもあります。  
  
## <a name="see-also"></a>関連項目

- [宣言された要素](../../../../visual-basic/programming-guide/language-features/declared-elements/index.md)
- [宣言された要素の名前](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)
- [宣言された要素の参照](../../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [Visual Basic の有効期間](../../../../visual-basic/programming-guide/language-features/declared-elements/lifetime.md)
- [Visual Basic におけるスコープ](../../../../visual-basic/programming-guide/language-features/declared-elements/scope.md)
- [Visual Basic のアクセスレベル](../../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
