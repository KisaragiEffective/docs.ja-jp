---
title: <include>
ms.date: 07/20/2015
helpviewer_keywords:
- include XML tag
- <include> XML tag
ms.assetid: ba8e9173-82cd-460b-8938-a075a2dfb36d
ms.openlocfilehash: 2f2bebfd06d4614f05cb66834cc5bef40524ce3b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348457"
---
# <a name="include-visual-basic"></a>\<include> (Visual Basic)
は、ソースコード内の型とメンバーを記述する別のファイルを参照します。  
  
## <a name="syntax"></a>構文  
  
```xml  
<include file="filename" path="tagpath[@name='id']" />  
```  
  
## <a name="parameters"></a>パラメーター  
 `filename`  
 必須。 文書を含むファイルの名前。 ファイル名をパスで修飾することができます。 `filename` を二重引用符 ("") で囲みます。  
  
 `tagpath`  
 必須。 タグ `filename` につながる `name` 内のタグのパス。 パスは二重引用符 ("") で囲みます。  
  
 `name`  
 必須。 コメントの前にあるタグの名前指定子。 `Name` には `id`があります。  
  
 `id`  
 必須。 コメントの前に配置するタグの ID。 ID は単一引用符 (' ') で囲みます。  
  
## <a name="remarks"></a>コメント  
 `<include>` タグを使用して、ソースコード内の型とメンバーを記述する別のファイル内のコメントを参照します。 これは文書化のコメントをソース コード ファイル内に直接配置する方法の代替です。  
  
 `<include>` タグでは、W3C 勧告『 XML Path Language (XPath) バージョン1.0 が使用されています。 `<include>` の使用方法をカスタマイズする方法の詳細については、「<https://www.w3.org/TR/xpath>」を参照してください。  
  
## <a name="example"></a>例  
 この例では、`<include>` タグを使用して、`commentFile.xml`という名前のファイルからメンバードキュメントコメントをインポートします。  
  
 [!code-vb[VbVbcnXmlDocComments#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#4)]  
  
 `commentFile.xml` の形式は次のとおりです。  
  
```xml  
<Docs>  
<Members name="Open">  
<summary>Opens a file.</summary>  
<param name="filename">File name to open.</param>  
</Members>  
<Members name="Close">  
<summary>Closes a file.</summary>  
<param name="filename">File name to close.</param>  
</Members>  
</Docs>  
```  
  
## <a name="see-also"></a>参照

- [XML のコメント用タグ](../../../visual-basic/language-reference/xmldoc/index.md)
