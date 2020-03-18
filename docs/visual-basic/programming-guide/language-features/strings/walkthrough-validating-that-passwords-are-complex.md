---
title: パスワードの複雑さの検証
ms.date: 07/20/2015
helpviewer_keywords:
- String data type [Visual Basic], validation
ms.assetid: 5d9a918f-6c1f-41a3-a019-b5c2b8ce0381
ms.openlocfilehash: 49e6f79c13c94a3f2f6891b259c4bb2bec54ae6f
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75344521"
---
# <a name="walkthrough-validating-that-passwords-are-complex-visual-basic"></a>チュートリアル: パスワードの複雑さの検証 (Visual Basic)
このメソッドは、強力なパスワードの特性を確認し、パスワードが失敗したことを確認する情報を含む文字列パラメーターを更新します。  
  
 セキュリティで保護されたシステムでは、パスワードを使用してユーザーを承認できます。 ただし、パスワードは、承認されていないユーザーが推測するのを困難にする必要があります。 攻撃者は*辞書攻撃*プログラムを使用できます。このプログラムでは、ディクショナリ内のすべての単語 (または異なる言語の複数の辞書) を反復処理し、単語がユーザーのパスワードとして機能するかどうかをテストします。 "ヤンキース" や "" などの脆弱なパスワードは、すぐに推測できます。 "?" などの強力なパスワード'L1N3vaFiNdMeyeP@sSWerd! ' は、推測される可能性が非常に低くなっています。 パスワードで保護されたシステムでは、ユーザーが強力なパスワードを選択できるようにする必要があります。  
  
 強力なパスワードは、大文字、小文字、数字、および特殊文字を組み合わせたもので、単語ではありません。 この例では、複雑さを検証する方法を示します。  
  
## <a name="example"></a>使用例  
  
### <a name="code"></a>コード  
 [!code-vb[VbVbcnRegEx#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnRegEx/VB/Class1.vb#1)]  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 そのパスワードを含む文字列を渡して、このメソッドを呼び出します。  
  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Text.RegularExpressions> 名前空間のメンバーへのアクセス許可。 コード内でメンバー名を完全修飾していない場合は、`Imports` ステートメントを追加します。 詳細については、「[Imports ステートメント (.NET 名前空間および型)](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)」を参照してください。  
  
## <a name="security"></a>セキュリティ  
 ネットワーク経由でパスワードを移動する場合は、セキュリティで保護された方法でデータを転送する必要があります。 詳細については、「 [ASP.NET Web Application Security](https://docs.microsoft.com/previous-versions/aspnet/330a99hc(v=vs.100))」を参照してください。
  
 さらに複雑なチェックを追加することで、`ValidatePassword` 関数の精度を向上させることができます。  
  
- パスワードとその部分文字列を、ユーザーの名前、ユーザー id、およびアプリケーション定義の辞書と比較します。 また、比較を実行するときに、視覚的に似た文字を同等として扱います。 たとえば、"l" および "e" という文字は、数字の "1" と "3" に相当するものとして扱います。  
  
- 大文字が1つしかない場合は、それがパスワードの最初の文字ではないことを確認します。  
  
- パスワードの最後の2文字が文字であることを確認します。  
  
- すべての記号がキーボードの先頭行から入力されている場合は、パスワードを許可しないでください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Text.RegularExpressions.Regex>
- [ASP.NET Web アプリケーションのセキュリティ](https://docs.microsoft.com/previous-versions/aspnet/330a99hc(v=vs.100))
