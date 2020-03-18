---
title: '方法 : 変数の可用性を制御する'
ms.date: 07/20/2015
helpviewer_keywords:
- access levels, declared elements
- Private keyword [Visual Basic], accessing variables
- access levels, variables
- Public keyword [Visual Basic], accessing variables
- Friend keyword [Visual Basic], accessing variables
- variables [Visual Basic], access level
- declared elements [Visual Basic], access level
- Protected keyword [Visual Basic], accessing variables
ms.assetid: eaf4f073-7922-43ce-ae1e-90ff376ae947
ms.openlocfilehash: 886b57909cf6ba25dbaceea5c5f06eb4e3ba6f1f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345390"
---
# <a name="how-to-control-the-availability-of-a-variable-visual-basic"></a>方法: 変数の可用性を制御する (Visual Basic)
変数の可用性を制御するには、*アクセスレベル*を指定します。 アクセスレベルによって、変数に対する読み取りまたは書き込みのアクセス許可を持つコードが決まります。  
  
- *メンバー変数*(モジュールレベルおよびプロシージャ外で定義されます) は、既定でパブリックアクセスになります。これは、参照できるすべてのコードがアクセスできることを意味します。 これは、アクセス修飾子を指定することによって変更できます。  
  
- *ローカル変数*(プロシージャ内で定義) とにはパブリックアクセスがありますが、そのプロシージャ内のコードだけがアクセスできます。 ローカル変数のアクセスレベルを変更することはできませんが、それを含むプロシージャのアクセスレベルを変更することはできます。  
  
 詳細については、「[Visual Basic でのアクセス レベル](../../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。  
  
## <a name="private-and-public-access"></a>プライベートアクセスとパブリックアクセス  
  
#### <a name="to-make-a-variable-accessible-only-from-within-its-module-class-or-structure"></a>モジュール、クラス、または構造体内からのみ変数にアクセスできるようにするには  
  
1. 変数の[Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)をモジュール、クラス、または構造体の内部に配置しますが、プロシージャの外側に配置します。  
  
2. `Dim` ステートメントに[Private](../../../../visual-basic/language-reference/modifiers/private.md)キーワードを含めます。  
  
     モジュール、クラス、または構造体内のどこからでも変数の読み取りまたは書き込みを行うことができますが、外部からは読み取ることはできません。  
  
#### <a name="to-make-a-variable-accessible-from-any-code-that-can-see-it"></a>参照可能なコードから変数にアクセスできるようにするには  
  
1. メンバー変数の場合は、変数の `Dim` ステートメントをモジュール、クラス、または構造体の内部に配置しますが、プロシージャの外側に配置します。  
  
2. `Dim` ステートメントに[Public](../../../../visual-basic/language-reference/modifiers/public.md)キーワードを含めます。  
  
     アセンブリと相互運用できる任意のコードから変数の読み取りまたは書き込みを行うことができます。  
  
 または  
  
1. ローカル変数の場合は、プロシージャ内に変数の `Dim` ステートメントを配置します。  
  
2. `Dim` ステートメントに `Public` キーワードを含めないでください。  
  
     変数の読み取りまたは書き込みは、プロシージャ内の任意の場所から行うことができますが、外部からは実行できません。  
  
## <a name="protected-and-friend-access"></a>保護されたアクセスとフレンドアクセス  
 変数のアクセスレベルは、そのクラス、派生クラス、またはそのアセンブリに限定できます。 また、これらの制限の和集合を指定することもできます。これにより、任意の派生クラスまたは同じアセンブリ内の他の場所のコードからアクセスできるようになります。 この共用体を指定するには、同じ宣言で `Protected` キーワードと `Friend` キーワードを組み合わせます。  
  
#### <a name="to-make-a-variable-accessible-only-from-within-its-class-and-any-derived-classes"></a>クラスと派生クラス内からのみ変数にアクセスできるようにするには  
  
1. 変数の `Dim` ステートメントをクラス内に配置しますが、プロシージャの外側に配置します。  
  
2. `Dim` ステートメントに[Protected](../../../../visual-basic/language-reference/modifiers/protected.md)キーワードを含めます。  
  
     変数の読み取りまたは書き込みは、クラス内の任意の場所から行うことができます。また、派生チェーン内のクラスの外部から派生したクラスから派生することもできます。  
  
#### <a name="to-make-a-variable-accessible-only-from-within-the-same-assembly"></a>同じアセンブリ内からのみ変数にアクセスできるようにするには  
  
1. 変数の `Dim` ステートメントをモジュール、クラス、または構造体の内部に配置しますが、プロシージャの外側に配置します。  
  
2. `Dim` ステートメントに[Friend](../../../../visual-basic/language-reference/modifiers/friend.md)キーワードを含めます。  
  
     モジュール、クラス、または構造体内の任意の場所から、また同じアセンブリ内の任意のコードから、変数に対する読み取りまたは書き込みを行うことができます。ただし、アセンブリの外部からは参照できません。  
  
## <a name="example"></a>例  
 次の例では、`Public`、`Protected`、`Friend`、`Protected Friend`、および `Private` アクセスレベルを持つ変数の宣言を示します。 `Dim` ステートメントでアクセスレベルを指定する場合は、`Dim` キーワードを含める必要はありません。  
  
```vb  
Public Class classForEverybody  
Protected Class classForMyHeirs  
Friend stringForThisProject As String  
Protected Friend stringForProjectAndHeirs As String  
Private numberForMeOnly As Integer  
```  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  
 変数のアクセスレベルが制限されているほど、悪意のあるコードによって不適切に使用される可能性が小さくなります。  
  
## <a name="see-also"></a>参照

- [Visual Basic でのアクセス レベル](../../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
- [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)
- [Public](../../../../visual-basic/language-reference/modifiers/public.md)
- [Protected](../../../../visual-basic/language-reference/modifiers/protected.md)
- [Friend](../../../../visual-basic/language-reference/modifiers/friend.md)
- [Private](../../../../visual-basic/language-reference/modifiers/private.md)
