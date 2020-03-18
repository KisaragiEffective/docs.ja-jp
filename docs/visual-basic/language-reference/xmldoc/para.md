---
title: <para>
ms.date: 07/20/2015
helpviewer_keywords:
- <para> XML tag
- para XML tag
ms.assetid: a3a18b6c-6416-4358-94ec-37b22675fd37
ms.openlocfilehash: 8f28ecc33eea99150509bb4bade047489b4b826b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352300"
---
# <a name="para-visual-basic"></a>\<para> (Visual Basic)
コンテンツが段落として書式設定されることを指定します。  
  
## <a name="syntax"></a>構文  
  
```xml  
<para>content</para>  
```  
  
## <a name="parameters"></a>パラメーター  
 `content`  
 段落のテキストです。  
  
## <a name="remarks"></a>コメント  
 `<para>` タグは、 [\<summary>](../../../visual-basic/language-reference/xmldoc/summary.md)、 [\<remarks>](../../../visual-basic/language-reference/xmldoc/remarks.md)、 [\<returns](../../../visual-basic/language-reference/xmldoc/returns.md)> など、タグ内で使用するためのものであり、テキストに構造を追加することができます。  
  
 コンパイル時に [-doc](../../../visual-basic/reference/command-line-compiler/doc.md) を指定して、ドキュメント コメントをファイルに出力します。  
  
## <a name="example"></a>例  
 この例では、`<para>` タグを使用して、`UpdateRecord` メソッドの解説セクションを2つの段落に分割します。  
  
 [!code-vb[VbVbcnXmlDocComments#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>参照

- [XML のコメント用タグ](../../../visual-basic/language-reference/xmldoc/index.md)
