---
title: 型の上位変換
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic], scope
- visibility [Visual Basic], declared elements
- Partial keyword [Visual Basic], unexpected results with type promotion
- scope [Visual Basic], declared elements
- scope [Visual Basic], Visual Basic
- type promotion
- declared elements [Visual Basic], visibility
ms.assetid: 035eeb15-e4c5-4288-ab3c-6bd5d22f7051
ms.openlocfilehash: aa05bd7dc87510aedb0facadf4b7590c8ec57d1f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345271"
---
# <a name="type-promotion-visual-basic"></a>型の上位変換 (Visual Basic)
モジュールでプログラミング要素を宣言すると、Visual Basic はそのスコープをモジュールを含む名前空間に昇格させます。 これは、*型の上位変換*と呼ばれます。  
  
 次の例は、モジュールのスケルトン定義とそのモジュールの2つのメンバーを示しています。  
  
 [!code-vb[VbVbalrDeclaredElements#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDeclaredElements/VB/Class1.vb#1)]  
  
 `projModule`内では、モジュールレベルで宣言されたプログラミング要素が `projNamespace`に昇格されます。 前の例では、`basicEnum` と `innerClass` は昇格されますが、`numberSub` はありません。これは、モジュールレベルで宣言されていないためです。  
  
## <a name="effect-of-type-promotion"></a>型の上位変換の効果  
 型の上位変換の効果は、修飾文字列にモジュール名を含める必要がないことです。 次の例では、前の例のプロシージャを2回呼び出します。  
  
 [!code-vb[VbVbalrDeclaredElements#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDeclaredElements/VB/Class1.vb#2)]  
  
 前の例では、最初の呼び出しで完全修飾文字列を使用しています。 ただし、型の上位変換のためには必要ありません。 2番目の呼び出しは、修飾文字列に `projModule` を含めずに、モジュールのメンバーにもアクセスします。  
  
## <a name="defeat-of-type-promotion"></a>型の上位変換の無効化  
 名前空間にモジュールメンバーと同じ名前のメンバーが既に存在する場合、そのモジュールメンバーに対して型の上位変換は使用されません。 次の例は、同じ名前空間内の列挙型とモジュールのスケルトン定義を示しています。  
  
 [!code-vb[VbVbalrDeclaredElements#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDeclaredElements/VB/Class1.vb#3)]  
  
 前の例では、名前空間レベルで同じ名前の列挙が既に存在するため、Visual Basic はクラス `abc` を `thisNameSpace` に昇格できません。 `abcSub`にアクセスするには、完全修飾文字列 `thisNamespace.thisModule.abc.abcSub`を使用する必要があります。 ただし、クラス `xyz` は引き続き昇格され、`xyzSub` には、短い修飾文字列 `thisNamespace.xyz.xyzSub`を使用してアクセスできます。  
  
### <a name="defeat-of-type-promotion-for-partial-types"></a>部分型の型の上位変換の無効化  
 モジュール内のクラスまたは構造体が[Partial](../../../../visual-basic/language-reference/modifiers/partial.md)キーワードを使用している場合は、そのクラスまたは構造体に対して型の昇格が自動的に無効になります。名前空間に同じ名前のメンバーが含まれているかどうかは異なります。 モジュール内の他の要素は、引き続き型の上位変換の対象になります。  
  
 **措置.** 部分定義の型の上位変換を無効にすると、予期しない結果が発生し、コンパイラエラーが発生する可能性があります。 次の例は、クラスのスケルトン部分定義を示しています。そのうちの1つがモジュール内にあります。  
  
 [!code-vb[VbVbalrDeclaredElements#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDeclaredElements/VB/Class1.vb#4)]  
  
 前の例では、開発者は、`sampleClass`の2つの部分定義をマージすることをコンパイラに要求する場合があります。 ただし、コンパイラは `sampleModule`内の部分定義の上位変換を考慮しません。 結果として、2つの個別のクラスと別個のクラスをコンパイルしようとします。このクラスには `sampleClass` 名前が付けられますが、修飾パスは異なります。  
  
 コンパイラは、完全修飾されたパスがまったく同じ場合にのみ、部分定義をマージします。  
  
## <a name="recommendations"></a>推奨事項  
 次の推奨事項は、適切なプログラミング手法を示しています。  
  
- **一意の名前。** プログラミング要素の名前付けを完全に制御できる場合は、常に一意の名前を使用することをお勧めします。 同一の名前には追加の修飾が必要であり、コードが読みにくくなる可能性があります。 また、軽度のエラーや予期しない結果につながる可能性もあります。  
  
- **完全修飾。** 同じ名前空間内のモジュールとその他の要素を使用する場合、最も安全な方法は、すべてのプログラミング要素に対して常に完全修飾を使用することです。 モジュールメンバーの型の上位変換が無効になっていて、そのメンバーを完全に修飾していない場合、誤って別のプログラミング要素にアクセスする可能性があります。  
  
## <a name="see-also"></a>参照

- [Module ステートメント](../../../../visual-basic/language-reference/statements/module-statement.md)
- [Namespace ステートメント](../../../../visual-basic/language-reference/statements/namespace-statement.md)
- [Partial](../../../../visual-basic/language-reference/modifiers/partial.md)
- [Visual Basic におけるスコープ](../../../../visual-basic/programming-guide/language-features/declared-elements/scope.md)
- [方法: 変数のスコープを制御する](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-control-the-scope-of-a-variable.md)
- [宣言された要素の参照](../../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)
