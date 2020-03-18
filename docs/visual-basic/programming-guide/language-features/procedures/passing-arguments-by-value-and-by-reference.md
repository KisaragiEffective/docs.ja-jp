---
title: 引数の値渡しと参照渡し
ms.date: 07/20/2015
helpviewer_keywords:
- ByRef keyword [Visual Basic], passing arguments by reference
- Visual Basic code, procedures
- passing arguments [Visual Basic], by value or by reference
- ByVal keyword [Visual Basic], passing arguments by value
- arguments [Visual Basic], passing by value or by reference
- argument passing [Visual Basic], by value or by reference
ms.assetid: fd8a9de6-7178-44d5-a9bf-458d4ad907c2
ms.openlocfilehash: 28e59753a35ab67b15fbc549df5bb8a3489aba5a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352606"
---
# <a name="passing-arguments-by-value-and-by-reference-visual-basic"></a>引数の値渡しと参照渡し (Visual Basic)
Visual Basic では、値または*参照に* *よって*プロシージャに引数を渡すことができます。 これは、*引き渡し機構*と呼ばれ、プロシージャが呼び出し元のコードの引数の基になるプログラミング要素を変更できるかどうかを決定します。 プロシージャ宣言は、 [ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)または[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)キーワードを指定することによって、各パラメーターの引き渡し機構を決定します。  
  
## <a name="distinctions"></a>区分  
 プロシージャに引数を渡すときは、相互作用するいくつかの異なる違いに注意してください。  
  
- 基になるプログラミング要素が変更可能か変更不可能か  
  
- 引数自体が変更可能か変更不可能か  
  
- 引数が値渡しか参照渡しか  
  
- 引数のデータ型が値型であるか、参照型であるか  
  
 詳細については、「[変更できる引数と変更できない引数の違い](./differences-between-modifiable-and-nonmodifiable-arguments.md)」と「[引数の値渡しと参照渡しの違い](./differences-between-passing-an-argument-by-value-and-by-reference.md)」を参照してください。  
  
## <a name="choice-of-passing-mechanism"></a>渡すメカニズムの選択  
 引数ごとに、渡すメカニズムを慎重に選択する必要があります。  
  
- **保護**。 2つの渡すメカニズムの中で最も重要なのは、変更する変数を呼び出すことによる影響です。 引数 `ByRef` を渡す利点は、プロシージャが引数を使用して呼び出し元のコードに値を返すことができることです。 引数 `ByVal` を渡す利点は、プロシージャによって変数が変更されるのを防ぐことです。  
  
- **パフォーマンス**。 渡されるメカニズムは、コードのパフォーマンスに影響を与える可能性がありますが、違いは通常は意味がありません。 この例外の1つとして、`ByVal`渡される値の型があります。 この場合、Visual Basic は、引数のデータコンテンツ全体をコピーします。 そのため、構造体などの大きな値の型の場合は、`ByRef`渡す方が効率的です。  
  
     参照型の場合は、データへのポインターのみがコピーされます (32 ビットプラットフォームでは4バイト、64ビットプラットフォームでは8バイト)。 そのため、型 `String` の引数または `Object` の値によって、パフォーマンスを損なうことなく渡すことができます。  
  
## <a name="determination-of-the-passing-mechanism"></a>渡すメカニズムの決定  
 プロシージャの宣言では、各パラメーターに渡すメカニズムを指定します。 呼び出し元のコードでは、`ByVal` メカニズムをオーバーライドすることはできません。  
  
 パラメーターが `ByRef`で宣言されている場合、呼び出し元のコードでは、呼び出しで引数名をかっこで囲むことによって、機構を強制的に `ByVal` することができます。 詳細については、「[方法: 引数を強制的に値で渡す](./how-to-force-an-argument-to-be-passed-by-value.md)」を参照してください。  
  
 Visual Basic の既定では、値渡しで引数を渡します。  
  
## <a name="when-to-pass-an-argument-by-value"></a>値渡しで引数を渡す場合  
  
- 引数の基になる呼び出し元のコード要素が、不変の要素である場合は、対応するパラメーター [ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)を宣言します。 変更できない要素の値を変更できるコードはありません。  
  
- 基になる要素が変更可能であるが、プロシージャがその値を変更できないようにするには、パラメーター `ByVal`を宣言します。 値渡しで渡される変更可能な要素の値を変更できるのは、呼び出し元のコードだけです。  
  
## <a name="when-to-pass-an-argument-by-reference"></a>参照渡しで引数を渡す場合  
  
- プロシージャが、呼び出し元のコード内の基になる要素を変更する必要がある場合は、対応するパラメーター [ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)を宣言します。  
  
- コードの正しい実行が、呼び出し元のコードの基になる要素を変更するプロシージャに依存している場合は、パラメーター `ByRef`を宣言します。 値渡しで渡す場合、または呼び出し元のコードが引数をかっこで囲むことによって `ByRef` 渡す機構をオーバーライドする場合は、プロシージャ呼び出しによって予期しない結果が生じる可能性があります。  
  
## <a name="example"></a>例  
  
### <a name="description"></a>説明  
 次の例は、値渡しで引数を渡す場合と、参照渡しで渡す場合を示しています。 プロシージャ `Calculate` には、`ByVal` と `ByRef` パラメーターの両方があります。 利子率、`rate`、および合計金額 `debt`を指定すると、手順のタスクは、`debt`の元の値に利率を適用した結果として得られる `debt` の新しい値を計算することになります。 `debt` は `ByRef` パラメーターであるため、新しい合計は `debt`に対応する呼び出し元のコードの引数の値に反映されます。 パラメーター `rate` は、`Calculate` で値を変更してはならないため、`ByVal` パラメーターです。  
  
### <a name="code"></a>コード  
 [!code-vb[VbVbcnProcedures#74](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class2.vb#74)]  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [方法: プロシージャ引数の値を変更する](./how-to-change-the-value-of-a-procedure-argument.md)
- [方法: プロシージャ引数の値が変化しないようにする](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [方法: 引数の値渡しを強制する](./how-to-force-an-argument-to-be-passed-by-value.md)
- [位置と名前による引数渡し](./passing-arguments-by-position-and-by-name.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
