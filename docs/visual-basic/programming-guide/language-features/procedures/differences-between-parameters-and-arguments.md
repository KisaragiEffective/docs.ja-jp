---
title: パラメーターと引数の違い
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- parameters [Visual Basic], and arguments
- procedure arguments
- Visual Basic code, procedures
- arguments [Visual Basic], and parameters
- procedure parameters
- parameters [Visual Basic], definition
ms.assetid: c237c056-74f4-4749-9f2c-15864f139a31
ms.openlocfilehash: c4249dbf86bd1bfa7ef08e94059d2880333e9a92
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74341375"
---
# <a name="differences-between-parameters-and-arguments-visual-basic"></a>パラメーターと引数の違い (Visual Basic)
ほとんどの場合、プロシージャには、呼び出された状況に関する情報が含まれている必要があります。 繰り返しまたは共有タスクを実行するプロシージャは、呼び出しごとに異なる情報を使用します。 この情報は、呼び出し時にプロシージャに渡す変数、定数、および式で構成されます。  
  
 プロシージャにこの情報を伝えるために、プロシージャは*パラメーター*を定義し、呼び出し元のコードはそのパラメーターに*引数*を渡します。 パラメーターは、駐車スペースおよび自動車としての引数と考えることができます。 さまざまな自動車が駐車スペースで異なるタイミングでパークできるのと同様に、呼び出し元のコードは、プロシージャを呼び出すたびに、同じパラメーターに別の引数を渡すことができます。  
  
## <a name="parameters"></a>パラメーター  
 *パラメーター*は、プロシージャが呼び出し時に渡すことを予期している値を表します。 プロシージャの宣言では、パラメーターを定義します。  
  
 `Function` または `Sub` プロシージャを定義する場合は、プロシージャ名の直後にかっこで囲まれた*パラメーターリスト*を指定します。 各パラメーターには、名前、データ型、および渡す機構 ([ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)または[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)) を指定します。 また、パラメーターが省略可能であることを示すこともできます。 これは、呼び出し元のコードが値を渡す必要がないことを意味します。  
  
 各パラメーターの名前は、プロシージャの*ローカル変数*として機能します。 パラメーター名は、他の変数を使用する場合と同じ方法で使用します。  
  
## <a name="arguments"></a>引数  
 *引数*は、プロシージャを呼び出すときにプロシージャパラメーターに渡す値を表します。 呼び出し元のコードは、プロシージャを呼び出すときに引数を指定します。  
  
 `Function` または `Sub` プロシージャを呼び出すときには、プロシージャ名の直後に*引数リスト*をかっこで囲んで指定します。 各引数は、リスト内の同じ位置にあるパラメーターに対応します。  
  
 パラメーターの定義とは異なり、引数には名前がありません。 各引数は式であり、0個以上の変数、定数、およびリテラルを含むことができます。 評価された式のデータ型は、通常、対応するパラメーターに対して定義されているデータ型と一致し、任意の場合はパラメーターの型に変換可能である必要があります。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Sub プロシージャ](./sub-procedures.md)
- [Function プロシージャ](./function-procedures.md)
- [Property プロシージャ](./property-procedures.md)
- [演算子プロシージャ](./operator-procedures.md)
- [方法 : プロシージャにパラメーターを定義する](./how-to-define-a-parameter-for-a-procedure.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [再帰プロシージャ](./recursive-procedures.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
