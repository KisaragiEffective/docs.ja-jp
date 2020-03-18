---
title: 引数の値渡しと参照渡しの違い
ms.date: 07/20/2015
helpviewer_keywords:
- ByRef keyword [Visual Basic], passing arguments by reference
- Visual Basic code, procedures
- procedures [Visual Basic], passing arguments
- ByVal keyword [Visual Basic], passing arguments by value
- arguments [Visual Basic], passing by value or by reference
ms.assetid: 5f5c38fe-3e2d-494c-8fff-f4025b55ec93
ms.openlocfilehash: 84ec3bac2532b2cef72ddda347251bc987801c3b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74341230"
---
# <a name="differences-between-passing-an-argument-by-value-and-by-reference-visual-basic"></a>引数の値渡しと参照渡しの違い (Visual Basic)
プロシージャに1つ以上の引数を渡すと、各引数は、呼び出し元のコード内の基になるプログラミング要素に対応します。 この基になる要素の値、またはこの要素への参照を渡すことができます。 これは、*引き渡しメカニズム*と呼ばれます。  
  
## <a name="passing-by-value"></a>値渡し  
 引数を*値で*渡すには、プロシージャ定義の対応するパラメーターに[ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)キーワードを指定します。 この引き渡し機構を使用すると、Visual Basic は、基になるプログラミング要素の値をプロシージャのローカル変数にコピーします。 プロシージャコードには、呼び出し元のコード内の基になる要素へのアクセス権がありません。  
  
## <a name="passing-by-reference"></a>渡す (参照渡しで)  
 *参照で*引数を渡すには、プロシージャ定義の対応するパラメーターに[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)キーワードを指定します。 この引き渡し機構を使用すると、Visual Basic は、呼び出し元のコード内の基になるプログラミング要素への直接参照をプロシージャに付与します。  
  
## <a name="passing-mechanism-and-element-type"></a>渡す (機構と要素の型を)  
 渡す機構の選択は、基になる要素型の分類と同じではありません。 値渡しまたは参照渡しでは、プロシージャコードに提供される Visual Basic が参照されます。 値型または参照型は、プログラミング要素がどのようにメモリに格納されるかを示します。  
  
 ただし、渡すメカニズムと要素の型は相互に関連しています。 参照型の値は、メモリ内の別の場所にあるデータへのポインターです。 これは、参照型を値で渡す場合、基になる要素自体にアクセスできない場合でも、プロシージャコードには基になる要素のデータへのポインターがあることを意味します。 たとえば、要素が配列変数の場合、プロシージャコードは変数自体にアクセスすることはできませんが、配列のメンバーにアクセスすることはできます。  
  
## <a name="ability-to-modify"></a>変更する機能  
 変更できない要素を引数として渡すと、その要素が `ByVal` または `ByRef`に渡されたかどうかにかかわらず、呼び出し元のコードでプロシージャを変更することはできません。  
  
 変更可能な要素の場合、次の表は、要素の型と渡す機構の相互作用をまとめたものです。  
  
|要素型|渡された `ByVal`|渡された `ByRef`|  
|------------------|--------------------|--------------------|  
|値の型 (値のみを含む)|プロシージャでは、変数またはそのメンバーを変更することはできません。|プロシージャは、変数とそのメンバーを変更できます。|  
|参照型 (クラスまたは構造体のインスタンスへのポインターを含む)|プロシージャは変数を変更できませんが、参照先のインスタンスのメンバーを変更することはできます。|プロシージャは、参照先のインスタンスの変数とメンバーを変更できます。|  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [変更できる引数と変更できない引数の違い](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [方法: プロシージャ引数の値を変更する](./how-to-change-the-value-of-a-procedure-argument.md)
- [方法: プロシージャ引数の値が変化しないようにする](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [方法: 引数の値渡しを強制する](./how-to-force-an-argument-to-be-passed-by-value.md)
- [位置と名前による引数渡し](./passing-arguments-by-position-and-by-name.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
