---
title: '方法: ファイル名とパスを検証する'
ms.date: 07/20/2015
helpviewer_keywords:
- file names [Visual Basic], validating
- strings [Visual Basic], validating
- Boolean values [Visual Basic]
- paths [Visual Basic], validating
ms.assetid: f673462d-57b7-4120-b13a-6a7592f7ab2c
ms.openlocfilehash: cc4d275d469860aa19c45ca0fe0401b709b42d82
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344363"
---
# <a name="how-to-validate-file-names-and-paths-in-visual-basic"></a>方法 : Visual Basic でファイル名とパスを検証する
この例では、文字列がファイル名またはパスを表すかどうかを示す `Boolean` 値を返します。 検証では、ファイルシステムで許可されていない文字が名前に含まれているかどうかを確認します。  
  
## <a name="example"></a>例  
 [!code-vb[VbVbcnRegEx#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnRegEx/VB/Class1.vb#4)]  
  
 この例では、名前のコロンが誤って配置されているか、名前のないディレクトリがあるか、または名前の長さがシステムで定義されている最大長を超えていないかを確認しません。 また、指定した名前のファイルシステムリソースにアクセスするためのアクセス許可がアプリケーションにあるかどうかも確認しません。  
  
## <a name="see-also"></a>関連項目

- <xref:System.IO.Path.GetInvalidPathChars%2A>
- [Visual Basic における文字列の検証](../../../../visual-basic/programming-guide/language-features/strings/validating-strings.md)
