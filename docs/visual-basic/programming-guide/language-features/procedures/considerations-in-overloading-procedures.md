---
title: プロシージャのオーバーロードに関する注意事項
ms.date: 07/20/2015
helpviewer_keywords:
- signatures [Visual Basic], ParamArray arguments
- ParamArray keyword [Visual Basic], parameter arrays
- ParamArray keyword [Visual Basic], arguments and signatures
- function overloading [Visual Basic], implicit overloads for ParamArray
- ParamArray keyword [Visual Basic], signatures
- Visual Basic code, procedures
- arguments [Visual Basic], parameter arrays
- procedures [Visual Basic], overloading
- parameters [Visual Basic], lists
- function overloading [Visual Basic], typeless programming
- typeless programming
- function overloading [Visual Basic], restrictions
- arguments [Visual Basic], optional
- optional arguments [Visual Basic], overloading
- signatures [Visual Basic], procedure
- parameter lists [Visual Basic]
- parameter arrays [Visual Basic], overloading arguments
- Visual Basic code, parameter lists
- procedure overloading [Visual Basic], considerations
- Option Explicit statement [Visual Basic]
- restrictions [Visual Basic], overloading procedures
- procedures [Visual Basic], parameter lists
ms.assetid: a2001248-10d0-42c5-b0ce-eeedc987319f
ms.openlocfilehash: 4a0cfe176a59b3f90f5850ae8b4e34784c400c6b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351010"
---
# <a name="considerations-in-overloading-procedures-visual-basic"></a>プロシージャのオーバーロードに関する注意事項 (Visual Basic)
プロシージャをオーバーロードする場合は、オーバーロードされたバージョンごとに異なる*シグネチャ*を使用する必要があります。 これは通常、各バージョンで別のパラメーターリストを指定する必要があることを意味します。 詳細については、「[プロシージャのオーバーロード](./procedure-overloading.md)」の「別の署名」を参照してください。  
  
 `Sub` プロシージャを使用して `Function` プロシージャをオーバーロードすることができます。また、シグネチャが異なる場合は、その逆も可能です。 2つのオーバーロードは、一方が戻り値を持ち、もう一方が戻り値を持たない場合にのみ、異なることはできません。  
  
 同じ制限を使用して、プロシージャをオーバーロードするのと同じ方法でプロパティをオーバーロードすることができます。 ただし、プロパティを使用してプロシージャをオーバーロードすることはできません。また、その逆もできません。  
  
## <a name="alternatives-to-overloaded-versions"></a>オーバーロードされたバージョンの代替手段  
 オーバーロードされたバージョンに代わる選択肢がある場合があります。特に、引数の存在が省略可能な場合や、その数値が可変である場合です。  
  
 省略可能な引数は、必ずしもすべての言語でサポートされているとは限らず、パラメーター配列は Visual Basic に制限されていることに注意してください。 複数の異なる言語で記述されたコードから呼び出される可能性のあるプロシージャを作成する場合は、オーバーロードされたバージョンによって最大限の柔軟性が得られます。  
  
### <a name="overloads-and-optional-arguments"></a>オーバーロードと省略可能な引数  
 呼び出し元のコードが必要に応じて1つ以上の引数を指定または省略できる場合は、オーバーロードされた複数のバージョンを定義したり、省略可能なパラメーターを使用したりできます。  
  
#### <a name="when-to-use-overloaded-versions"></a>オーバーロードされたバージョンを使用する場合  
 次の場合は、一連のオーバーロードされたバージョンを定義することを検討してください。  
  
- プロシージャコードのロジックは、呼び出し元のコードがオプションの引数を提供するかどうかによって大きく異なります。  
  
- プロシージャコードは、呼び出し元のコードが省略可能な引数を指定しているかどうかを確実にテストすることはできません。 これは、たとえば、呼び出し元のコードが提供する必要のない既定値の候補がない場合などです。  
  
#### <a name="when-to-use-optional-parameters"></a>省略可能なパラメーターを使用する場合  
 次の場合には、省略可能なパラメーターを1つ以上使用することができます。  
  
- 呼び出し元のコードがオプションの引数を指定しない場合に必要な操作は、パラメーターを既定値に設定することだけです。 このような場合、1つ以上の `Optional` パラメーターを使用して1つのバージョンを定義すると、プロシージャコードが複雑になる可能性があります。  
  
 詳細については、「[省略可能なパラメーター](./optional-parameters.md)」を参照してください。  
  
### <a name="overloads-and-paramarrays"></a>オーバーロードと ParamArrays  
 呼び出し元のコードが可変個の引数を渡すことができる場合は、オーバーロードされた複数のバージョンを定義することも、パラメーター配列を使用することもできます。  
  
#### <a name="when-to-use-overloaded-versions"></a>オーバーロードされたバージョンを使用する場合  
 次の場合は、一連のオーバーロードされたバージョンを定義することを検討してください。  
  
- 呼び出し元のコードは、少数の値をパラメーター配列に渡すことはないことがわかっています。  
  
- プロシージャコードのロジックは、呼び出し元のコードが渡す値の数によって大きく異なります。  
  
- 呼び出し元のコードは、さまざまなデータ型の値を渡すことができます。  
  
#### <a name="when-to-use-a-parameter-array"></a>パラメーター配列を使用する場合  
 次の場合には、`ParamArray` パラメーターによって適切に処理されます。  
  
- 呼び出し元のコードがパラメーター配列に渡すことができる値の数を予測することはできません。また、数値が大きい可能性もあります。  
  
- プロシージャロジックは、呼び出し元のコードが渡すすべての値を反復処理するために使用され、実質的にすべての値に対して同じ操作を実行します。  
  
 詳細については、「[パラメーター配列](./parameter-arrays.md)」を参照してください。  
  
## <a name="implicit-overloads-for-optional-parameters"></a>省略可能なパラメーターの暗黙のオーバーロード  
 [Optional](../../../../visual-basic/language-reference/modifiers/optional.md)なパラメーターを持つプロシージャは、2 つのオーバーロードされたプロシージャに相当します。1 つは省略可能なパラメーターで、もう 1 つは使用しません。 このようなプロシージャは、これらのいずれかに対応するパラメーターリストを使用してオーバーロードすることはできません。 次の宣言はこれを示しています。  
  
 [!code-vb[VbVbcnProcedures#58](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#58)]  
  
 [!code-vb[VbVbcnProcedures#60](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#60)]  
  
 [!code-vb[VbVbcnProcedures#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#61)]  
  
 省略可能なパラメーターを複数持つプロシージャの場合は、前の例のようにロジックによって受信された一連の暗黙的なオーバーロードがあります。  
  
## <a name="implicit-overloads-for-a-paramarray-parameter"></a>ParamArray パラメーターの暗黙のオーバーロード  
 コンパイラは、 [ParamArray](../../../../visual-basic/language-reference/modifiers/paramarray.md)パラメーターを持つプロシージャが、次のように、呼び出し元のコードがパラメーター配列に渡すものとは異なる無限のオーバーロードを持つと見なします。  
  
- 呼び出し元のコードが `ParamArray` に引数を指定しない場合の1つのオーバーロード  
  
- 呼び出し元のコードが `ParamArray` 要素型の1次元配列を提供する場合の1つのオーバーロード  
  
- すべての正の整数に対して、呼び出し元のコードが引数の数を指定する場合の1つのオーバーロード (`ParamArray` 要素型)  
  
 次の宣言は、これらの暗黙的なオーバーロードを示しています。  
  
 [!code-vb[VbVbcnProcedures#68](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#68)]  
  
 [!code-vb[VbVbcnProcedures#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#70)]  
  
 パラメーター配列の1次元配列を受け取るパラメーターリストを使用して、このようなプロシージャをオーバーロードすることはできません。 ただし、他の暗黙的なオーバーロードのシグネチャを使用することもできます。 次の宣言はこれを示しています。  
  
 [!code-vb[VbVbcnProcedures#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#71)]  
  
## <a name="typeless-programming-as-an-alternative-to-overloading"></a>オーバーロードの代替手段としてのタイプレスプログラミング  
 呼び出し元のコードが異なるデータ型をパラメーターに渡すことを許可する場合は、別の方法として、型指定のないプログラミングがあります。 [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)または[-optionstrict](../../../../visual-basic/reference/command-line-compiler/optionstrict.md)コンパイラオプションのいずれかを使用して、型チェックスイッチを `Off` に設定できます。 その後、パラメーターのデータ型を宣言する必要はありません。 ただし、この方法には、オーバーロードと比較して次の欠点があります。  
  
- タイプレスプログラミングでは、実行コードの効率が低下します。  
  
- このプロシージャでは、渡されると予想されるすべてのデータ型をテストする必要があります。  
  
- 呼び出し元のコードが、プロシージャがサポートしていないデータ型を渡した場合、コンパイラはエラーを通知できません。  
  
## <a name="see-also"></a>関連項目

- [Visual Basic におけるプロシージャ](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [プロシージャのトラブルシューティング](./troubleshooting-procedures.md)
- [方法 : プロシージャの複数のバージョンを定義する](./how-to-define-multiple-versions-of-a-procedure.md)
- [方法 : オーバーロードされたプロシージャを呼び出す](./how-to-call-an-overloaded-procedure.md)
- [方法 : 省略可能なパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [方法 : 不特定数のパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [オーバーロードの解決](./overload-resolution.md)
- [Overloads](../../../../visual-basic/language-reference/modifiers/overloads.md)
