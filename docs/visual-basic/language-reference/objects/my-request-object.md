---
title: My.Request オブジェクト
ms.date: 07/20/2015
f1_keywords:
- My.MyWebExtension.Request
- My.Request
helpviewer_keywords:
- My.Request object
ms.assetid: 93d5f0e2-6b60-4a2c-8652-d90216f6ad10
ms.openlocfilehash: 22329bc501c9bb75b1336dd5384ab5b23a98ac21
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350675"
---
# <a name="myrequest-object"></a>My.Request オブジェクト
要求されたページの <xref:System.Web.HttpRequest> オブジェクトを取得します。  
  
## <a name="remarks"></a>コメント  
 `My.Request` オブジェクトには、現在の HTTP 要求に関する情報が含まれています。  
  
 `My.Request` オブジェクトは、ASP.NET アプリケーションでのみ使うことができます。  
  
## <a name="example"></a>例  
 次の例では、`My.Request` オブジェクトからヘッダーコレクションを取得し、`My.Response` オブジェクトを使用して ASP.NET ページに書き込みます。  
  
 [!code-aspx-vb[VbVbalrMyWeb#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWeb/VB/Default.aspx#1)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Web.HttpRequest>
- [My.Response オブジェクト](../../../visual-basic/language-reference/objects/my-response-object.md)
