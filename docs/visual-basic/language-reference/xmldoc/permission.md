---
title: <permission>
ms.date: 07/20/2015
helpviewer_keywords:
- <permission> XML tag
- permission XML tag
ms.assetid: 0edf0500-5cd7-49c0-9255-64c48f972b77
ms.openlocfilehash: 71b00b669804e644d1171480192b9d55455bdf53
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352264"
---
# <a name="permission-visual-basic"></a>\<のアクセス許可 > (Visual Basic)
メンバーに必要な権限を指定します。  
  
## <a name="syntax"></a>構文  
  
```xml  
<permission cref="member">description</permission>  
```  
  
## <a name="parameters"></a>パラメーター  
 `member`  
 現在のコンパイル環境からの呼び出しに利用できる、メンバーまたはフィールドへの参照。 コンパイラは、指定されたコード要素が存在し、出力の XML で `member` が正規要素名に変換されることを確認します。 `member` を引用符 ("") で囲みます。  
  
 `description`  
 メンバーへのアクセスの説明です。  
  
## <a name="remarks"></a>コメント  
 `<permission>` タグを使用して、メンバーのアクセスを文書化します。 <xref:System.Security.PermissionSet> クラスを使用して、メンバーへのアクセスを指定します。  
  
 コンパイル時に [-doc](../../../visual-basic/reference/command-line-compiler/doc.md) を指定して、ドキュメント コメントをファイルに出力します。  
  
## <a name="example"></a>例  
 この例では、`<permission>` タグを使用して、`ReadFile` メソッドで <xref:System.Security.Permissions.FileIOPermission> が必要であることを説明します。  
  
 [!code-vb[VbVbcnXmlDocComments#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#7)]  
  
## <a name="see-also"></a>関連項目

- [XML のコメント用タグ](../../../visual-basic/language-reference/xmldoc/index.md)
