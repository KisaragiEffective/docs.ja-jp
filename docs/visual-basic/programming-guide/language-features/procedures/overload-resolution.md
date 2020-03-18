---
title: オーバーロードの解決
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- overload resolution
- procedures [Visual Basic], overloading
- procedures [Visual Basic], calling
- procedure overloading [Visual Basic], overload resolution
- signatures [Visual Basic], procedure
- overloads [Visual Basic], resolution
ms.assetid: 766115d1-4352-45fb-859f-6063e0de0ec0
ms.openlocfilehash: 0e69136b1e3015055cad9852bf04151f57558b88
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352646"
---
# <a name="overload-resolution-visual-basic"></a>オーバーロードの解決法 (Visual Basic)
オーバーロードされたいくつかのバージョンで定義されているプロシージャへの呼び出しが Visual Basic コンパイラによって検出されると、コンパイラは呼び出すオーバーロードを決定する必要があります。 これを行うには、次の手順を実行します。  
  
1. **アクセシビリティ。** これにより、呼び出し元のコードが呼び出しを妨げるアクセスレベルのオーバーロードが排除されます。  
  
2. **パラメーターの数。** これにより、呼び出しで指定された数よりも多くのパラメーターを定義するオーバーロードが不要になります。  
  
3. **パラメーターのデータ型。** コンパイラは、拡張メソッドよりもインスタンスメソッドを優先します。 プロシージャ呼び出しに一致するために拡大変換のみを必要とするインスタンスメソッドが見つかった場合は、すべての拡張メソッドが削除され、コンパイラはインスタンスメソッドの候補だけを使用して続行します。 このようなインスタンスメソッドが見つからない場合は、インスタンスメソッドと拡張メソッドの両方で続行されます。  
  
     このステップでは、呼び出し元の引数のデータ型をオーバーロードで定義されているパラメーター型に変換できないすべてのオーバーロードを排除します。  
  
4. **縮小変換。** これにより、呼び出し元の引数の型から定義済みのパラメーターの型への縮小変換を必要とするすべてのオーバーロードが排除されます。 これは、型チェックスイッチ ([Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)) が `On` か `Off`かにかかわらず、true になります。  
  
5. **最小の拡大。** コンパイラは、残りのオーバーロードをペアで考慮します。 各ペアについて、定義されたパラメーターのデータ型を比較します。 オーバーロードのいずれかの型が、他のオーバーロード内の対応する型に拡大変換されると、コンパイラは後者を除外します。 つまり、拡大率が最も低いオーバーロードを保持します。  
  
6. **1つの候補。** オーバーロードを1つだけ保持し、そのオーバーロードの呼び出しを解決するまで、ペアのオーバーロードを検討し続けます。 コンパイラがオーバーロードを1つの候補に減らすことができない場合は、エラーが生成されます。  
  
 次の図は、一連のオーバーロードされたバージョンを呼び出すプロセスを示しています。  
  
 ![オーバーロードの解決プロセスのフロー図](./media/overload-resolution/determine-overloaded-version.gif "オーバーロードされたバージョン間の解決")    
  
 次の例は、このオーバーロードの解決プロセスを示しています。  
  
 [!code-vb[VbVbcnProcedures#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#62)]  
  
 [!code-vb[VbVbcnProcedures#63](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#63)]  
  
 最初の呼び出しでは、最初の引数の型 (`Short`) が対応するパラメーター (`Byte`) の型に限定されるため、コンパイラは最初のオーバーロードを削除します。 次に、2番目のオーバーロード (`Short` および `Single`) の各引数の型が、3番目のオーバーロード (`Integer` および `Single`) の対応する型に拡大変換されるため、3番目のオーバーロードが削除されます。 2番目のオーバーロードでは、より少ない拡大が必要になるため、コンパイラはこれを呼び出しに使用します。  
  
 2番目の呼び出しでは、コンパイラは、縮小に基づいてオーバーロードのいずれかを削除することはできません。 最初の呼び出しの場合と同じ理由で3番目のオーバーロードを削除します。これは、引数の型をより拡大して、2番目のオーバーロードを呼び出すことができるためです。 ただし、コンパイラは1番目と2番目のオーバーロード間で解決できません。 各には、もう一方の型に拡大変換する定義済みのパラメーター型が1つあります (`Short`に`Byte` ますが、`Double`には `Single` ます)。 そのため、コンパイラはオーバーロードの解決エラーを生成します。  
  
## <a name="overloaded-optional-and-paramarray-arguments"></a>オーバーロードされた省略可能な引数と ParamArray 引数  
 1 つのプロシージャの 2 つのオーバーロードが同じシグネチャを持つ場合、最後のパラメーターは [Optional](../../../../visual-basic/language-reference/modifiers/optional.md)として宣言され、もう一方では [ParamArray](../../../../visual-basic/language-reference/modifiers/paramarray.md)が宣言されますが、コンパイラは、そのプロシージャへの呼び出しを次のように解決します。 
  
|呼び出しで最後の引数がとして渡された場合|コンパイラは、最後の引数をとして宣言しているオーバーロードへの呼び出しを解決します。|  
|---|---|  
|値なし (引数は省略されます)|`Optional`|  
|単一の値|`Optional`|  
|コンマ区切りリスト内の2つ以上の値|`ParamArray`|  
|任意の長さの配列 (空の配列を含む)|`ParamArray`|  
  
## <a name="see-also"></a>関連項目

- [省略可能なパラメーター](./optional-parameters.md)
- [パラメーター配列](./parameter-arrays.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [プロシージャのトラブルシューティング](./troubleshooting-procedures.md)
- [方法 : プロシージャの複数のバージョンを定義する](./how-to-define-multiple-versions-of-a-procedure.md)
- [方法 : オーバーロードされたプロシージャを呼び出す](./how-to-call-an-overloaded-procedure.md)
- [方法 : 省略可能なパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [方法 : 不特定数のパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [プロシージャのオーバーロードに関する注意事項](./considerations-in-overloading-procedures.md)
- [Overloads](../../../../visual-basic/language-reference/modifiers/overloads.md)
- [拡張メソッド](./extension-methods.md)
