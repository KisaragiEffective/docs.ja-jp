---
title: '方法 : 不特定数のパラメーターを受け取るプロシージャをオーバーロードする'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], parameters
- procedure overloading [Visual Basic], indefinite number of parameters
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
ms.assetid: c7042de2-2422-4039-94e8-ac298896af69
ms.openlocfilehash: 94f12b4cc6cb35864fefbb3b5bb1378bec5e974c
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347560"
---
# <a name="how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters-visual-basic"></a>方法: 不特定数のパラメーターを受け取るプロシージャをオーバーロードする (Visual Basic)
プロシージャに[ParamArray](../../../../visual-basic/language-reference/modifiers/paramarray.md)パラメーターがある場合、パラメーター配列に1次元配列を受け取るオーバーロードされたバージョンを定義することはできません。 詳細については、「[プロシージャのオーバーロードに関する考慮事項](./considerations-in-overloading-procedures.md)」の「ParamArray パラメーターの暗黙のオーバーロード」を参照してください。  
  
### <a name="to-overload-a-procedure-that-takes-a-variable-number-of-parameters"></a>可変個のパラメーターを受け取るプロシージャをオーバーロードするには  
  
1. プロシージャと呼び出しコードロジックが、`ParamArray` パラメーターよりも多くのオーバーロードされたバージョンの恩恵を得られることを確認します。 「オーバーロードと ParamArrays」の「[オーバーロードと](./considerations-in-overloading-procedures.md)paramarrays」を参照してください。  
  
2. パラメーターリストの変数部分で、プロシージャが受け入れる指定された値の数を決定します。 これには、値のないケース、1つの1次元配列のケースなどが含まれます。  
  
3. 指定された許容される値の数ごとに、対応するパラメーターリストを定義する `Sub` または `Function` 宣言ステートメントを記述します。 このオーバーロードされたバージョンでは、`Optional` または `ParamArray` キーワードは使用しないでください。  
  
4. 各宣言で、`Sub` または `Function` キーワードの前に[Overloads](../../../../visual-basic/language-reference/modifiers/overloads.md)キーワードを使用します。  
  
5. 各宣言の後に、呼び出し元のコードがその宣言のパラメーターリストに対応する値を指定したときに実行する必要があるプロシージャコードを記述します。  
  
6. 必要に応じて、`End Sub` または `End Function` ステートメントを使用して各プロシージャを終了します。  
  
## <a name="example"></a>使用例  
 次の例では、 [ParamArray](../../../../visual-basic/language-reference/modifiers/paramarray.md)パラメーターを使用して定義されたプロシージャと、それと同等のオーバーロードされたプロシージャのセットを示します。  
  
 [!code-vb[VbVbcnProcedures#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#69)]  
  
 [!code-vb[VbVbcnProcedures#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#70)]  
  
 パラメーター配列の1次元配列を受け取るパラメーターリストを使用して、このようなプロシージャをオーバーロードすることはできません。 ただし、他の暗黙的なオーバーロードのシグネチャを使用することもできます。 次の宣言はこれを示しています。  
  
 [!code-vb[VbVbcnProcedures#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#71)]  
  
 オーバーロードされたバージョンのコードでは、呼び出し元のコードが `ParamArray` パラメーターに対して1つ以上の値を提供したかどうかをテストする必要はありません。 Visual Basic は、呼び出し元の引数リストと一致するバージョンに制御を渡します。  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 `ParamArray` パラメーターを持つプロシージャは、オーバーロードされたバージョンのセットと等価であるため、このようなプロシージャは、これらの暗黙的なオーバーロードのいずれかに対応するパラメーターリストを使用してオーバーロードすることはできません。 詳細については、「[プロシージャのオーバーロードに関する考慮事項](./considerations-in-overloading-procedures.md)」を参照してください。  
  
## <a name="net-framework-security"></a>.NET Framework セキュリティ  
 無限に大きくなる可能性がある配列を処理する場合、アプリケーションの内部容量がオーバーランするリスクがあります。 パラメーター配列を受け入れる場合は、渡された呼び出し元のコードが配列の長さであるかどうかをテストし、アプリケーションに対して大きすぎる場合は適切な手順を実行する必要があります。  
  
## <a name="see-also"></a>関連項目

- [手順](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [省略可能なパラメーター](./optional-parameters.md)
- [パラメーター配列](./parameter-arrays.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [プロシージャのトラブルシューティング](./troubleshooting-procedures.md)
- [方法 : プロシージャの複数のバージョンを定義する](./how-to-define-multiple-versions-of-a-procedure.md)
- [方法 : オーバーロードされたプロシージャを呼び出す](./how-to-call-an-overloaded-procedure.md)
- [方法 : 省略可能なパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [オーバーロードの解決](./overload-resolution.md)
