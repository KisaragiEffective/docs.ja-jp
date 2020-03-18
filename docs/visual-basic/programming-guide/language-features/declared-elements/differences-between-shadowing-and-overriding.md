---
title: シャドウとオーバーライドの違い
ms.date: 07/20/2015
helpviewer_keywords:
- shadowing, vs. overriding
- overriding, vs. shadowing
ms.assetid: 2d014a0b-7630-407d-8f4e-24bd87987923
ms.openlocfilehash: 8d1ebdcd0a23dff69a7acca22268c03e30ec06d9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345416"
---
# <a name="differences-between-shadowing-and-overriding-visual-basic"></a>シャドウとオーバーライドの違い (Visual Basic)
基底クラスを継承するクラスを定義する場合、派生クラスの1つ以上の基底クラス要素を再定義することが必要になることがあります。 この目的では、シャドウとオーバーライドの両方を使用できます。  
  
## <a name="comparison"></a>条件式  
 シャドウとオーバーライドは、派生クラスが基底クラスから継承し、宣言された要素を別の要素で再定義する場合に使用されます。 ただし、2つの間には大きな違いがあります。  
  
 次の表は、シャドウとオーバーライドの比較を比較したものです。  
  
||||  
|---|---|---|  
|比較のポイント|シャドウ|オーバーライド|  
|用途|派生クラスで既に定義されているメンバーを導入する、後続の基本クラスの変更から保護します。|同じ呼び出しシーケンスを使用して、プロシージャまたはプロパティの異なる実装を定義することでポリモーフィズムを実現します<sup>1</sup>|  
|再定義要素|宣言された要素型|プロシージャ (`Function`、`Sub`、または `Operator`) またはプロパティのみ|  
|再定義 (要素を)|宣言された要素型|同じ呼び出しシーケンス<sup>1</sup>を持つプロシージャまたはプロパティのみ|  
|再定義要素のアクセスレベル|任意のアクセスレベル|オーバーライドされた要素のアクセスレベルを変更できません|  
|再定義要素の読みやすさと書き込み機能|任意の組み合わせ|オーバーライドされたプロパティの読みやすさや書き込み機能を変更することはできません|  
|再定義の制御|基底クラスの要素はシャドウを強制または禁止できません|基本クラス要素には、`MustOverride`、`NotOverridable`、またはを指定でき `Overridable`|  
|キーワードの使用法|`Shadows` 派生クラスで推奨される方法です。`Shadows` も `Overrides`<sup>も指定さ</sup>れていない場合に `Shadows`|基本クラスで `Overridable` または `MustOverride` が必要です。派生クラスで `Overrides` が必要です。|  
|派生クラスから派生するクラスによる再定義要素の継承|さらに派生したクラスによって継承された要素のシャドウシャドウされる要素の非表示<sup>3</sup>|オーバーライド (他の派生クラスによって継承された要素を)オーバーライドされた要素はオーバーライドされます|  
  
 <sup>1</sup> *呼び出しシーケンス*は、要素の型 (`Function`、`Sub`、`Operator`、または `Property`)、名前、パラメーターリスト、および戻り値の型で構成されます。 プロパティを使用してプロシージャをオーバーライドすることはできません。また、他の方法を使用することもできません。 1つの種類のプロシージャ (`Function`、`Sub`、または `Operator`) を別の種類でオーバーライドすることはできません。  
  
 <sup>2</sup> `Shadows` または `Overrides`を指定しない場合、コンパイラは、使用する再定義の種類を確認できるように、警告メッセージを発行します。 警告を無視すると、シャドウメカニズムが使用されます。  
  
 <sup>3</sup>追加の派生クラスでシャドウ要素にアクセスできない場合、シャドウは継承されません。 たとえば、シャドウ要素を `Private`として宣言した場合、派生クラスから派生したクラスは、シャドウ要素ではなく元の要素を継承します。  
  
## <a name="guidelines"></a>ガイドライン  
 通常は、次の場合にオーバーライドを使用します。  
  
- ポリモーフィックな派生クラスを定義しています。  
  
- 同じ要素の型と呼び出し元のシーケンスをコンパイラが強制的に適用することの安全性が必要です。  
  
 通常は、次の場合にシャドウを使用します。  
  
- 基本クラスが変更されている可能性があることを予測し、自分と同じ名前を使用して要素を定義します。  
  
- 要素の型を自由に変更したり、シーケンスを呼び出したりできます。  
  
## <a name="see-also"></a>参照

- [宣言された要素の参照](../../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [Visual Basic でのシャドウ処理](../../../../visual-basic/programming-guide/language-features/declared-elements/shadowing.md)
- [方法: 自分で宣言した変数と同じ名前の変数を隠す](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-hide-a-variable-with-the-same-name-as-your-variable.md)
- [方法: 継承された変数を隠す](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-hide-an-inherited-variable.md)
- [方法: 派生クラスによって非表示になっている変数にアクセスする](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-access-a-variable-hidden-by-a-derived-class.md)
- [Shadows](../../../../visual-basic/language-reference/modifiers/shadows.md)
- [Overrides](../../../../visual-basic/language-reference/modifiers/overrides.md)
