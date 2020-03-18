---
title: My.Application、My.Computer、および My.User でのタスクの実行
ms.date: 07/20/2015
helpviewer_keywords:
- My.Application object [Visual Basic], developing applications
- rapid application development (RAD), My.Application
- rapid application development (RAD), My.Computer
- rapid application development (RAD), My.User
- My.Computer object [Visual Basic], developing applications
- My.User object [Visual Basic], developing applications
ms.assetid: c8af61bd-4dd3-4a0f-9af5-795b594b240b
ms.openlocfilehash: fc9fd9093a3db4785bfc94719dbae9ec1d586050
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74329578"
---
# <a name="performing-tasks-with-myapplication-mycomputer-and-myuser-visual-basic"></a>My.Application、My.Computer、および My.User でのタスクの実行 (Visual Basic)

情報と一般的に使用される機能へのアクセスを提供する3つの中央 `My` オブジェクトは、`My.Application` (<xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>)、`My.Computer` (<xref:Microsoft.VisualBasic.Devices.Computer>)、および `My.User` (<xref:Microsoft.VisualBasic.ApplicationServices.User>) です。 これらのオブジェクトを使用して、現在のアプリケーション、アプリケーションがインストールされているコンピューター、またはアプリケーションの現在のユーザーに関連する情報にそれぞれアクセスできます。  
  
## <a name="myapplication-mycomputer-and-myuser"></a>[マイアプリケーション]、[マイコンピューター]、および [マイユーザー]  

 次の例は、`My`を使用して情報を取得する方法を示しています。  
  
 [!code-vb[VbVbcnMy#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#1)]  
  
 [!code-vb[VbVbcnMy#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#2)]  
  
 情報を取得するだけでなく、これら3つのオブジェクトを介して公開されるメンバーは、そのオブジェクトに関連するメソッドを実行することもできます。 たとえば、さまざまなメソッドにアクセスして、ファイルを操作したり、`My.Computer`を使用してレジストリを更新したりできます。  
  
 `My`を使用すると、ファイル i/o が大幅に簡単になり、ファイル、ディレクトリ、およびドライブを操作するためのさまざまなメソッドやプロパティが含まれます。 <xref:Microsoft.VisualBasic.FileIO.TextFieldParser> オブジェクトを使用すると、区切り記号または固定幅フィールドを持つ大規模な構造化ファイルから読み取ることができます。 この例では、`TextFieldParser` `reader` を開き、それを使用して `C:\TestFolder1\test1.txt`から読み取ります。  
  
 [!code-vb[VbVbalrTextFieldParser#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTextFieldParser/VB/Class1.vb#23)]  
  
 `My.Application` を使用すると、アプリケーションのカルチャを変更できます。 このメソッドを呼び出す方法を次の例に示します。  
  
 [!code-vb[VbVbcnMy#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.ApplicationServices.User>
- [プロジェクトの種類に応じた My の機能](../../../visual-basic/developing-apps/development-with-my/how-my-depends-on-project-type.md)
