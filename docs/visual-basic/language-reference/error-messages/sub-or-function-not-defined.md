---
title: Sub または Function が定義されていません。
ms.date: 07/20/2015
f1_keywords:
- vbrID35
ms.assetid: 661fdb90-ee7d-40ce-b30b-5e7267bd957a
ms.openlocfilehash: 8b81460eccb6be8baa2ea7bc68d0f80c9d16398e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349577"
---
# <a name="sub-or-function-not-defined-visual-basic"></a>Sub または Function が定義されていません。(Visual Basic)
呼び出されるには、`Sub` または `Function` が定義されている必要があります。 このエラーでは以下の原因が考えられます。  
  
- プロシージャ名のスペルが間違っています。  
  
- **[参照]** ダイアログボックスでそのプロジェクトへの参照を明示的に追加せずに、別のプロジェクトからプロシージャを呼び出そうとしています。  
  
- 呼び出し元のプロシージャからは参照できないプロシージャを指定します。  
  
- 指定されたライブラリまたはコードリソースにない Windows ダイナミックリンクライブラリ (DLL) ルーチンまたは Macintosh コードリソースルーチンを宣言しています。  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
1. プロシージャ名のスペルが正しいことを確認してください。  
  
2. **[参照]** ダイアログボックスで、呼び出すプロシージャを含むプロジェクトの名前を検索します。 表示されない場合は、 **[参照]** ボタンをクリックして検索します。 プロジェクト名の左側にあるチェックボックスをオンにして、[ **OK]** をクリックします。  
  
3. ルーチンの名前を確認します。  
  
## <a name="see-also"></a>参照

- [エラーの種類](../../../visual-basic/programming-guide/language-features/error-types.md)
- [プロジェクト内の参照の管理](/visualstudio/ide/managing-references-in-a-project)
- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
