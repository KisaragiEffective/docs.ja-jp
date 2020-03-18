---
title: My.Forms と My.WebServices が提供する既定のオブジェクト インスタンス
ms.date: 07/20/2015
helpviewer_keywords:
- My.WebServices object [Visual Basic], developing applications
- My.Forms object [Visual Basic], developing applications
- rapid application development (RAD), My.Forms
- rapid application development (RAD), My.WebServices
ms.assetid: de930027-9108-4f0c-b97c-5e7db4d6ef79
ms.openlocfilehash: d06df4bd023892429b2aaefdd624398a6546d06d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74330215"
---
# <a name="default-object-instances-provided-by-myforms-and-mywebservices-visual-basic"></a>My.Forms および My.WebServices が提供する既定のオブジェクト インスタンス (Visual Basic)

My.forms オブジェクトと[My](../../../visual-basic/language-reference/objects/my-webservices-object.md) Web Web サービス[オブジェクトは、](../../../visual-basic/language-reference/objects/my-forms-object.md)アプリケーションで使用されるフォーム、データソース、および XML Web サービスへのアクセスを提供します。 これらの操作は、これらの各オブジェクトの*既定のインスタンス*のコレクションを提供することによって行います。  
  
## <a name="default-instances"></a>既定のインスタンス  

 既定のインスタンスは、ランタイムによって提供されるクラスのインスタンスであり、`Dim` および `New` ステートメントを使用して宣言およびインスタンス化する必要はありません。 次の例では、`Form1`と呼ばれる <xref:System.Windows.Forms.Form> クラスのインスタンスを宣言およびインスタンス化し、`My.Forms`を通じてこの <xref:System.Windows.Forms.Form> クラスの既定のインスタンスを取得できるようになった方法を示します。  
  
 [!code-vb[VbVbcnMy#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#5)]  
  
 [!code-vb[VbVbcnMy#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#6)]  
  
 `My.Forms` オブジェクトは、プロジェクトに存在するすべての `Form` クラスの既定のインスタンスのコレクションを返します。 同様に、`My.WebServices` は、アプリケーションで参照を作成したすべての Web サービスに対して、プロキシクラスの既定のインスタンスを提供します。  
  
## <a name="see-also"></a>参照

- [My.Forms オブジェクト](../../../visual-basic/language-reference/objects/my-forms-object.md)
- [My.WebServices オブジェクト](../../../visual-basic/language-reference/objects/my-webservices-object.md)
- [プロジェクトの種類に応じた My の機能](../../../visual-basic/developing-apps/development-with-my/how-my-depends-on-project-type.md)
