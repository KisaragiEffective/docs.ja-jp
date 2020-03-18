---
title: '方法 : 省略可能なパラメーターを受け取るプロシージャをオーバーロードする'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], parameters
- procedure overloading [Visual Basic], optional parameters
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
ms.assetid: 825f9d56-4cde-43fd-993a-b9171717e2eb
ms.openlocfilehash: 4d31980e4b968cff274091ba4f307dffddab1100
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350864"
---
# <a name="how-to-overload-a-procedure-that-takes-optional-parameters-visual-basic"></a>方法: 省略可能なパラメーターを受け取るプロシージャをオーバーロードする (Visual Basic)
プロシージャに [Optional](../../../../visual-basic/language-reference/modifiers/optional.md) パラメーターが 1 つ以上ある場合、その暗黙的なオーバーロードのいずれかに一致するオーバーロードされたバージョンを定義することはできません。 詳細については、「[プロシージャのオーバーロードに関する考慮事項](./considerations-in-overloading-procedures.md)」の「省略可能なパラメーターの暗黙のオーバーロード」を参照してください。  
  
## <a name="one-optional-parameter"></a>1つの省略可能なパラメーター  
  
#### <a name="to-overload-a-procedure-that-takes-one-optional-parameter"></a>省略可能なパラメーターを1つ受け取るプロシージャをオーバーロードするには  
  
1. パラメーターリストに省略可能なパラメーターを含む `Sub` または `Function` 宣言ステートメントを記述します。 このオーバーロードされたバージョンでは、`Optional` キーワードを使用しないでください。  
  
2. `Sub` または `Function` キーワードの前に[Overloads](../../../../visual-basic/language-reference/modifiers/overloads.md)キーワードを付けます。  
  
3. 呼び出し元のコードがオプションの引数を提供するときに実行する必要があるプロシージャコードを記述します。  
  
4. 必要に応じて、`End Sub` または `End Function` ステートメントを使用してプロシージャを終了します。  
  
5. パラメーターリストに省略可能なパラメーターが含まれない点を除いて、最初の宣言と同じである2番目の宣言ステートメントを記述します。  
  
6. 呼び出し元のコードで省略可能な引数が指定されていない場合に実行するプロシージャコードを記述します。 必要に応じて、`End Sub` または `End Function` ステートメントを使用してプロシージャを終了します。  
  
     次の例では、省略可能なパラメーターを使用して定義されたプロシージャ、2つのオーバーロードされたプロシージャのセット、および無効なオーバーロードバージョンと有効なオーバーロードされたバージョンの両方の最後の例を示します。  
  
     [!code-vb[VbVbcnProcedures#59](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#59)]  
  
     [!code-vb[VbVbcnProcedures#60](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#60)]  
  
     [!code-vb[VbVbcnProcedures#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#61)]  
  
## <a name="multiple-optional-parameters"></a>複数の省略可能なパラメーター  
 省略可能なパラメーターを複数持つプロシージャの場合は、通常、3つ以上のオーバーロードされたバージョンが必要です。 たとえば、2つの省略可能なパラメーターがあり、呼び出し元のコードが互いに独立してそれぞれを指定または省略できる場合は、指定された引数の組み合わせごとに1つずつ、4つのオーバーロードされたバージョンが必要です。  
  
 省略可能なパラメーターの数が増えるにつれて、オーバーロードの複雑さが増します。 指定された引数の組み合わせが受け入れられない場合は、N 個の省略可能なパラメーターについては、2 ^ N のオーバーロードされたバージョンが必要です。 プロシージャの性質によっては、オーバーロードされたすべてのバージョンを定義する余分な作業をロジックが明確にすることがわかります。  
  
#### <a name="to-overload-a-procedure-that-takes-more-than-one-optional-parameter"></a>複数の省略可能なパラメーターを受け取るプロシージャをオーバーロードするには  
  
1. 指定された省略可能な引数の組み合わせを、プロシージャのロジックに対して使用できるかどうかを判断します。 1つの省略可能なパラメーターが別のパラメーターに依存している場合、許容されない組み合わせが発生する可能性が たとえば、1つのパラメーターが人名を受け取り、別のパラメーターがその年齢を受け入れる場合、age を指定し、名前を省略する引数の組み合わせは使用できません。  
  
2. 指定された省略可能な引数の組み合わせを使用できる場合は、対応するパラメーターリストを定義する `Sub` または `Function` 宣言ステートメントを記述します。 `Optional` キーワードは使用しないでください。  
  
3. 各宣言で、`Sub` または `Function` キーワードの前に[Overloads](../../../../visual-basic/language-reference/modifiers/overloads.md)キーワードを使用します。  
  
4. 各宣言の後に、呼び出し元のコードがその宣言のパラメーターリストに対応する引数リストを提供するときに実行する必要があるプロシージャコードを記述します。  
  
5. 必要に応じて、`End Sub` または `End Function` ステートメントを使用して各プロシージャを終了します。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [省略可能なパラメーター](./optional-parameters.md)
- [パラメーター配列](./parameter-arrays.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [プロシージャのトラブルシューティング](./troubleshooting-procedures.md)
- [方法 : プロシージャの複数のバージョンを定義する](./how-to-define-multiple-versions-of-a-procedure.md)
- [方法 : オーバーロードされたプロシージャを呼び出す](./how-to-call-an-overloaded-procedure.md)
- [方法 : 不特定数のパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [オーバーロードの解決](./overload-resolution.md)
