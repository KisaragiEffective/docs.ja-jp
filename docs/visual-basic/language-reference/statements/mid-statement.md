---
title: Mid ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.MidB
- vb.Mid
helpviewer_keywords:
- substrings [Visual Basic], Mid statement
- strings [Visual Basic], substrings
- Mid statement [Visual Basic]
- strings [Visual Basic], replacing
ms.assetid: 2b82d7a8-9646-4cb0-bec5-80abc98297bf
ms.openlocfilehash: eeef4c13743b75a3d5e61ac46afb94d9ea105b7a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348027"
---
# <a name="mid-statement"></a>Mid ステートメント
`String` 変数内の指定された数の文字を、別の文字列の文字に置き換えます。  
  
## <a name="syntax"></a>構文  
  
```vb  
Mid( _  
   ByRef Target As String, _  
   ByVal Start As Integer, _  
   Optional ByVal Length As Integer _  
) = StringExpression  
```  
  
## <a name="parts"></a>指定項目  
 `Target`  
 必須。 変更する `String` 変数の名前。  
  
 `Start`  
 必須。 `Integer` 式です。 `Target` の文字位置。テキストの置換が開始されます。 `Start` は、1から始まるインデックスを使用します。  
  
 `Length`  
 省略可。 `Integer` 式です。 置換する文字数。 省略した場合、すべての `String` が使用されます。  
  
 `StringExpression`  
 必須。 `Target`の一部を置き換える `String` 式。  
  
## <a name="exceptions"></a>例外  
  
|例外の種類|条件|  
|--------------------|---------------|  
|<xref:System.ArgumentException>|`Start` < = 0 または `Length` < 0 です。|  
  
## <a name="remarks"></a>コメント  
 置換される文字数は、常に `Target`の文字数以下です。  
  
 Visual Basic には <xref:Microsoft.VisualBasic.Strings.Mid%2A> 関数と `Mid` ステートメントがあります。 これらの要素は、どちらも文字列内の指定された文字数に対して動作しますが、`Mid` ステートメントによって文字が置き換えられる間、`Mid` 関数は文字を返します。 詳細については、「 <xref:Microsoft.VisualBasic.Strings.Mid%2A>」を参照してください。  
  
> [!NOTE]
> 以前のバージョンの Visual Basic の `MidB` ステートメントは、文字ではなく、バイト単位の部分文字列を置き換えます。 これは主に、2バイト文字セット (DBCS) アプリケーションで文字列を変換するために使用されます。 すべての Visual Basic 文字列は Unicode 形式であり、`MidB` はサポートされなくなりました。  
  
## <a name="example"></a>例  
 この例では、`Mid` ステートメントを使用して、文字列変数内の指定された数の文字を別の文字列の文字に置き換えます。  
  
 [!code-vb[VbVbalrStrings#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#5)]  
  
## <a name="requirements"></a>要件  
 **名前空間:** [Microsoft. visual basic](../../../visual-basic/language-reference/runtime-library-members.md)  
  
 **モジュール:** `Strings`  
  
 **アセンブリ:** Visual Basic ランタイムライブラリ (Microsoft... .dll)  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Strings.Mid%2A>
- [文字列](../../../visual-basic/programming-guide/language-features/strings/index.md)
- [Visual Basic の文字列の概要](../../../visual-basic/programming-guide/language-features/strings/introduction-to-strings.md)
