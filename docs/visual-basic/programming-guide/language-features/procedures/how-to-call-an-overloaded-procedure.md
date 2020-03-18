---
title: '方法 : オーバーロードされたプロシージャを呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], overloading
- procedures [Visual Basic], calling
- procedures [Visual Basic], multiple versions
- procedure calls [Visual Basic], overloaded
ms.assetid: 3bb331fb-f6bc-406f-9ca0-9609b497014c
ms.openlocfilehash: d983f5f6183c33141079ed35171f7a73f254450f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74340208"
---
# <a name="how-to-call-an-overloaded-procedure-visual-basic"></a>方法: オーバーロードされたプロシージャを呼び出す (Visual Basic)
プロシージャをオーバーロードする利点は、呼び出しの柔軟性です。 呼び出し元のコードは、プロシージャに渡す必要がある情報を取得し、渡す引数に関係なく、1つのプロシージャ名を呼び出すことができます。  
  
### <a name="to-call-a-procedure-that-has-more-than-one-version-defined"></a>複数のバージョンが定義されているプロシージャを呼び出すには  
  
1. 呼び出し元のコードで、プロシージャに渡すデータを決定します。  
  
2. 通常の方法でプロシージャ呼び出しを記述し、引数リストにデータを提示します。 引数が、プロシージャに対して定義されているいずれかのバージョンのパラメーターリストと一致していることを確認してください。  
  
3. 呼び出すプロシージャのバージョンを判断する必要はありません。 Visual Basic は、引数リストに一致するバージョンに制御を渡します。  
  
     次の例では、 [「方法: 複数のバージョンのプロシージャを定義する](./how-to-define-multiple-versions-of-a-procedure.md)」で宣言した `post` プロシージャを呼び出します。 顧客 id を取得し、それが `String` か `Integer`かを判断して、どちらの場合も同じプロシージャを呼び出します。  
  
     [!code-vb[VbVbcnProcedures#56](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#56)]  
  
     [!code-vb[VbVbcnProcedures#57](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#57)]  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [プロシージャのトラブルシューティング](./troubleshooting-procedures.md)
- [方法 : プロシージャの複数のバージョンを定義する](./how-to-define-multiple-versions-of-a-procedure.md)
- [方法 : 省略可能なパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [方法 : 不特定数のパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [プロシージャのオーバーロードに関する注意事項](./considerations-in-overloading-procedures.md)
- [オーバーロードの解決](./overload-resolution.md)
- [Overloads](../../../../visual-basic/language-reference/modifiers/overloads.md)
