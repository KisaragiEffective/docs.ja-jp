---
title: My.Response オブジェクト
ms.date: 07/20/2015
f1_keywords:
- My.MyWebExtension.Response
- My.Response
helpviewer_keywords:
- My.Response object
ms.assetid: 626359bc-3165-40b4-bfaf-2c610e26eb5b
ms.openlocfilehash: 522814ad48fb7548032b8a37779bb3ff6ca62413
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350658"
---
# <a name="myresponse-object"></a>My.Response オブジェクト
<xref:System.Web.UI.Page>に関連付けられている <xref:System.Web.HttpResponse> オブジェクトを取得します。 このオブジェクトでは、HTTP 応答データをクライアントに送信し、その応答に関する情報を含めることができます。  
  
## <a name="remarks"></a>コメント  
 `My.Response` オブジェクトには、ページに関連付けられている現在の <xref:System.Web.HttpResponse> オブジェクトが含まれています。  
  
 `My.Response` オブジェクトは、ASP.NET アプリケーションでのみ使用できます。  
  
## <a name="example"></a>例  
 次の例では、`My.Request` オブジェクトからヘッダーコレクションを取得し、`My.Response` オブジェクトを使用して ASP.NET ページに書き込みます。  
  
 [!code-aspx-vb[VbVbalrMyWeb#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWeb/VB/Default.aspx#1)]  
  
## <a name="see-also"></a>参照

- <xref:System.Web.HttpResponse>
- [My.Request オブジェクト](../../../visual-basic/language-reference/objects/my-request-object.md)
