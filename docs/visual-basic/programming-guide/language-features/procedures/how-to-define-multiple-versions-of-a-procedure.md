---
title: '方法 : プロシージャの複数のバージョンを定義する'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
- procedure overloading [Visual Basic], multiple versions
ms.assetid: 71ccdd66-1b00-4b66-bee4-6926c0d696f4
ms.openlocfilehash: e8ed9a6356b7177b2c029a9280d0790a93676653
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347598"
---
# <a name="how-to-define-multiple-versions-of-a-procedure-visual-basic"></a>方法: プロシージャの複数のバージョンを定義する (Visual Basic)
複数のバージョンでプロシージャを定義*するには*、各バージョンに対して同じ名前を使用し、異なるパラメーターリストを使用します。 オーバーロードの目的は、プロシージャの名前を区別せずに、密接に関連する複数のバージョンを定義することです。  
  
 詳細については、「 [Procedure Overloading](./procedure-overloading.md)」を参照してください。  
  
### <a name="to-define-multiple-versions-of-a-procedure"></a>プロシージャの複数のバージョンを定義するには  
  
1. 定義するプロシージャの各バージョンに対して、`Sub` または `Function` 宣言ステートメントを記述します。 すべての宣言で同じプロシージャ名を使用します。  
  
2. [オーバーロード](../../../../visual-basic/language-reference/modifiers/overloads.md)キーワードを使用して、各宣言内の `Sub` または `Function` キーワードの前に配置します。 必要に応じて、宣言で `Overloads` を省略できますが、宣言のいずれかに含める場合は、すべての宣言に含める必要があります。  
  
3. 各宣言ステートメントの後にプロシージャコードを記述し、呼び出し元のコードがそのバージョンのパラメーターリストに一致する引数を指定した場合に処理します。 呼び出し元のコードがどのパラメーターを指定したかをテストする必要はありません。 Visual Basic は、プロシージャの一致するバージョンに制御を渡します。  
  
4. 必要に応じて、`End Sub` または `End Function` ステートメントを使用して、プロシージャの各バージョンを終了します。  
  
## <a name="example"></a>使用例  
 次の例では、顧客の残高に対してトランザクションを投稿するための `Sub` プロシージャを定義します。 この例では、`Overloads` キーワードを使用して2つのバージョンのプロシージャを定義しています。1つは顧客を名前で受け取り、もう1つはアカウント番号で指定します。  
  
 [!code-vb[VbVbcnProcedures#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#72)]  
  
 呼び出し元のコードは、`String` または `Integer`として顧客 id を取得し、どちらの場合も同じ呼び出しステートメントを使用できます。  
  
 これらのバージョンの `post` プロシージャを呼び出す方法については、「[方法: オーバーロードされたプロシージャを呼び出す](./how-to-call-an-overloaded-procedure.md)」を参照してください。  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 オーバーロードされた各バージョンのプロシージャ名が同じで、パラメーターリストが異なることを確認してください。  
  
## <a name="see-also"></a>関連項目

- [手順](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [プロシージャのトラブルシューティング](./troubleshooting-procedures.md)
- [方法 : 省略可能なパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [方法 : 不特定数のパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [プロシージャのオーバーロードに関する注意事項](./considerations-in-overloading-procedures.md)
- [オーバーロードの解決](./overload-resolution.md)
