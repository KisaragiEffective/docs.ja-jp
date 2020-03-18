---
title: '[自動]'
ms.date: 07/20/2015
f1_keywords:
- vb.Auto
helpviewer_keywords:
- Auto keyword [Visual Basic], external references
- Declare statement [Visual Basic], marshaling strings
- Auto keyword [Visual Basic]
- Auto keyword [Visual Basic], marshaling strings
ms.assetid: bf79ba95-a62c-48a5-916f-0ac7a52c13ec
ms.openlocfilehash: 7ea46e5f8b882bb986f23e792b240bad0c5be7a5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351617"
---
# <a name="auto-visual-basic"></a>Auto (Visual Basic)
宣言する外部プロシージャの外部名に基づいて、.NET Framework ルールに従って Visual Basic が文字列をマーシャリングする必要があることを指定します。  
  
 プロジェクトの外部で定義されたプロシージャを呼び出すと、Visual Basic コンパイラは、プロシージャを正しく呼び出すために必要な情報にアクセスできません。 この情報には、プロシージャの配置場所、識別方法、呼び出し元のシーケンスと戻り値の型、および使用する文字列文字セットが含まれます。 [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)は、外部プロシージャへの参照を作成し、この必要な情報を提供します。  
  
 `Declare` ステートメントの `charsetmodifier` 部分では、外部プロシージャの呼び出し時に文字列をマーシャリングするための文字セット情報を指定します。 また、外部ファイルで外部プロシージャ名を検索 Visual Basic 方法にも影響します。 `Auto` 修飾子は、Visual Basic が .NET Framework 規則に従って文字列をマーシャリングする必要があること、およびランタイムプラットフォームの基本文字セットを決定し、最初の検索が失敗した場合に外部プロシージャ名を変更する必要があることを指定します。 詳細については、「[Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)」の「文字セット」を参照してください。  
  
 文字セット修飾子が指定されていない場合は、`Ansi` が既定値になります。  
  
## <a name="remarks"></a>コメント  
 このコンテキストでは、`Auto` 修飾子を使用できます。  
  
 [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)  
  
## <a name="smart-device-developer-notes"></a>スマートデバイスの開発者向けメモ  
 このキーワードはサポートされていません。  
  
## <a name="see-also"></a>参照

- [Ansi](../../../visual-basic/language-reference/modifiers/ansi.md)
- [Unicode](../../../visual-basic/language-reference/modifiers/unicode.md)
- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
