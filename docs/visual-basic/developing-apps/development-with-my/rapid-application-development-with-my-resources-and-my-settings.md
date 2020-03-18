---
title: My.Resources と My.Settings による Rapid Application Development
ms.date: 07/20/2015
helpviewer_keywords:
- My.Settings object [Visual Basic], developing applications
- rapid application development (RAD), My.Resources
- rapid application development (RAD), My.Settings
- My.Resources object [Visual Basic], developing applications
ms.assetid: 68284ab1-b685-4814-a2a4-01ae40445ff8
ms.openlocfilehash: ce9a5bf76ba3132f58aa40227a145d8b5bf1591d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349264"
---
# <a name="rapid-application-development-with-myresources-and-mysettings-visual-basic"></a>My.Resources と My.Settings による Rapid Application Development (Visual Basic)

`My.Resources` オブジェクトは、アプリケーションのリソースへのアクセスを提供し、アプリケーションのリソースを動的に取得できるようにします。  
  
## <a name="retrieving-resources"></a>リソースの取得  

 `My.Resources` オブジェクトを使用して、オーディオファイル、アイコン、画像、文字列などのさまざまなリソースを取得できます。 たとえば、アプリケーションのカルチャ固有のリソースファイルにアクセスできます。 次の例では、フォームのアイコンを、アプリケーションのリソースファイルに格納されている `Form1Icon` という名前のアイコンに設定します。  
  
 [!code-vb[VbVbcnMy#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#7)]  
  
 `My.Resources` オブジェクトは、グローバルリソースのみを公開します。 フォームに関連付けられたリソースファイルへのアクセスは提供されません。 フォームリソースには、フォームからアクセスする必要があります。  
  
 同様に、`My.Settings` オブジェクトを使用すると、アプリケーションの設定にアクセスして、アプリケーションのプロパティ設定やその他の情報を動的に格納および取得することができます。 詳細については、「 [My.resources オブジェクト](../../../visual-basic/language-reference/objects/my-resources-object.md)」と「 [my.settings オブジェクト](../../../visual-basic/language-reference/objects/my-settings-object.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [My.Resources オブジェクト](../../../visual-basic/language-reference/objects/my-resources-object.md)
- [My.Settings オブジェクト](../../../visual-basic/language-reference/objects/my-settings-object.md)
- [Accessing Application Settings](../../../visual-basic/developing-apps/programming/app-settings/index.md)
