---
title: '方法 : オブジェクト変数がインスタンスを参照しないようにする'
ms.date: 07/20/2015
helpviewer_keywords:
- Nothing keyword [Visual Basic], variable assignment
- object variables [Visual Basic], null reference
ms.assetid: e6d30578-bdae-4142-a3ac-a10697bf696a
ms.openlocfilehash: 320dadb61c12f3339c5328dcef31c41503892c56
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352897"
---
# <a name="how-to-make-an-object-variable-not-refer-to-any-instance-visual-basic"></a>方法: オブジェクト変数がインスタンスを参照しないようにする (Visual Basic)
オブジェクト変数を[Nothing](../../../../visual-basic/language-reference/nothing.md)に設定することによって、オブジェクトのインスタンスの関連付けを解除できます。  
  
### <a name="to-disassociate-an-object-variable-from-any-object-instance"></a>オブジェクト変数とオブジェクトインスタンスの関連付けを解除するには  
  
- 代入ステートメントで `Nothing` に変数を設定します。  
  
    ```vb  
    ' Assume account is a defined class  
    Dim currentAccount As account  
    currentAccount = Nothing  
    ```  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 コードが `Nothing`に設定されているオブジェクト変数のメンバーにアクセスしようとすると、<xref:System.NullReferenceException> が発生します。 オブジェクト変数を頻繁に `Nothing` に設定した場合、または変数が初期化されていない可能性がある場合は、メンバーアクセスを `Try...Catch...Finally` ブロックで囲むことをお勧めします。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  
 機密データを含むオブジェクトに対してオブジェクト変数を使用する場合は、これらのオブジェクトのいずれかをアクティブに処理していないときに、変数を `Nothing` に設定できます。 これにより、悪意のあるコードがデータにアクセスする可能性が低くなります。  
  
## <a name="see-also"></a>関連項目

- <xref:System.NullReferenceException>
- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [オブジェクト変数の代入](../../../../visual-basic/programming-guide/language-features/variables/object-variable-assignment.md)
- [Nothing](../../../../visual-basic/language-reference/nothing.md)
- [Try...Catch...Finally ステートメント](../../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)
