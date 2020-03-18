---
title: 属性リスト
ms.date: 07/20/2015
helpviewer_keywords:
- attribute list
- attributes [Visual Basic], applying
ms.assetid: 5880073a-68a4-4b6b-8a07-ace32959a4e2
ms.openlocfilehash: f9332f52622551bb6b944242f71bd80f439982e9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354060"
---
# <a name="attribute-list-visual-basic"></a>属性リスト (Visual Basic)
宣言されたプログラミング要素に適用する属性を指定します。 複数の属性を指定するときは、コンマで区切ります。 1つの属性の構文を次に示します。  
  
## <a name="syntax"></a>構文  
  
```vb  
[ attributemodifier ] attributename [ ( attributearguments | attributeinitializer ) ]  
```  
  
## <a name="parts"></a>指定項目  
|||
|---|---|
|`attributemodifier`|ソースファイルの先頭で適用される属性に必要です。 [Assembly](../../../visual-basic/language-reference/modifiers/assembly.md)または[Module](../../../visual-basic/language-reference/modifiers/module-keyword.md)を指定できます。|
|`attributename`| 必須。 属性の名前。|
|`attributearguments`|省略可。 この属性の位置指定引数の一覧。 複数の引数は、コンマで区切ります。|
|`attributeinitializer`|省略可。 この属性の変数またはプロパティ初期化子のリスト。 複数の初期化子は、コンマで区切られます。|
  
## <a name="remarks"></a>コメント  
 1つまたは複数の属性を、ほぼすべてのプログラミング要素 (型、プロシージャ、プロパティなど) に適用できます。 属性は、アセンブリのメタデータに表示され、コードに注釈を付けたり、特定のプログラミング要素の使用方法を指定したりするのに役立ちます。 Visual Basic と .NET Framework で定義された属性を適用し、独自の属性を定義することができます。  

 属性を使用する場合の詳細については、「[属性の概要](../../../visual-basic/programming-guide/concepts/attributes/index.md)」を参照してください。 属性名の詳細については、「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。  
  
## <a name="rules"></a>ルール  
  
- **場所.** 最も宣言されたプログラミング要素に属性を適用できます。 1つまたは複数の属性を適用するには、要素宣言の先頭に*属性ブロック*を配置します。 属性リストの各エントリは、適用する属性、および属性のこの呼び出しに使用する修飾子と引数を指定します。  
  
- **山かっこ。** 属性リストを指定する場合は、山かっこ ("`<`" および "`>`") で囲む必要があります。  
  
- **宣言の一部。** 属性は、個別のステートメントではなく、要素宣言の一部である必要があります。 行連結シーケンス ("`_`") を使用して、宣言ステートメントを複数のソースコード行に拡張することができます。  
  
- **ド.** ソースファイルの先頭でプログラミング要素に適用されるすべての属性に対して、属性修飾子 (`Assembly` または `Module`) が必要です。 ソースファイルの先頭にない要素に適用される属性では、属性修飾子は使用できません。  
  
- **数値.** 属性のすべての位置指定引数は、変数またはプロパティ初期化子の前に記述する必要があります。  
  
## <a name="example"></a>例  
 次の例では、`Function` プロシージャのスケルトン定義に <xref:System.Runtime.InteropServices.DllImportAttribute> 属性を適用します。  
  
 [!code-vb[VbVbalrStatements#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#1)]  
  
 <xref:System.Runtime.InteropServices.DllImportAttribute> は、属性付きプロシージャがアンマネージダイナミックリンクライブラリ (DLL) のエントリポイントを表すことを示します。 属性は、DLL 名を位置引数として指定し、その他の情報を変数初期化子として提供します。  
  
## <a name="see-also"></a>参照

- [Assembly](../../../visual-basic/language-reference/modifiers/assembly.md)
- [Module \<キーワード>](../../../visual-basic/language-reference/modifiers/module-keyword.md)
- [属性の概要](../../../visual-basic/programming-guide/concepts/attributes/index.md)
- [方法 : コード内でステートメントを分割および連結する](../../../visual-basic/programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
