---
title: Ansi
ms.date: 07/20/2015
f1_keywords:
- vb.Ansi
helpviewer_keywords:
- Declare statement [Visual Basic], marshaling strings [Visual Basic]
- ANSI, Visual Basic
- ANSI
ms.assetid: 4f1fa6ff-5557-41ab-b6da-90baf4c15917
ms.openlocfilehash: 0c38c2b81af7b4cb8fd1723853a09c5413f805af
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344737"
---
# <a name="ansi-visual-basic"></a>Ansi (Visual Basic)
宣言する外部プロシージャの名前に関係なく、すべての文字列を米国規格協会 (ANSI) (ANSI) 値にマーシャリング Visual Basic 必要があることを指定します。  
  
 プロジェクトの外部で定義されたプロシージャを呼び出すと、Visual Basic コンパイラは、プロシージャを正しく呼び出すために必要な情報にアクセスできません。 この情報には、プロシージャの配置場所、識別方法、呼び出し元のシーケンスと戻り値の型、および使用する文字列文字セットが含まれます。 [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)は、外部プロシージャへの参照を作成し、この必要な情報を提供します。  
  
 `Declare` ステートメントの `charsetmodifier` 部分では、外部プロシージャの呼び出し時に文字列をマーシャリングするための文字セット情報を指定します。 また、外部ファイルで外部プロシージャ名を検索 Visual Basic 方法にも影響します。 `Ansi` 修飾子は、Visual Basic がすべての文字列を ANSI 値にマーシャリングする必要があることを指定します。検索中は、プロシージャの名前を変更せずにプロシージャを検索する必要があります。  
  
 文字セット修飾子が指定されていない場合は、`Ansi` が既定値になります。  
  
## <a name="remarks"></a>コメント  
 このコンテキストでは、`Ansi` 修飾子を使用できます。  
  
 [Declare Statement](../../../visual-basic/language-reference/statements/declare-statement.md)  
  
## <a name="smart-device-developer-notes"></a>スマートデバイスの開発者向けメモ  
 このキーワードはサポートされていません。  
  
## <a name="see-also"></a>関連項目

- [Auto](../../../visual-basic/language-reference/modifiers/auto.md)
- [Unicode](../../../visual-basic/language-reference/modifiers/unicode.md)
- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
