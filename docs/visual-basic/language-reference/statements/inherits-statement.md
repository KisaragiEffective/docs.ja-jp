---
title: Inherits ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Inherits
- Inherits
helpviewer_keywords:
- Inherits statement [Visual Basic]
- Inherits statement [Visual Basic], syntax
ms.assetid: 9e6fe042-9af3-4341-8093-fc3537770cf2
ms.openlocfilehash: 6e6e9cc9210232059210862f2bda691c57b372d6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353226"
---
# <a name="inherits-statement"></a>Inherits ステートメント
現在のクラスまたはインターフェイスが、別のクラスまたはインターフェイスのセットから属性、変数、プロパティ、プロシージャ、およびイベントを継承するようにします。  
  
## <a name="syntax"></a>構文  
  
```vb  
Inherits basetypenames  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`basetypenames`|必須。 このクラスの派生元であるクラスの名前。<br /><br /> または<br /><br /> このインターフェイスの派生元のインターフェイスの名前。 複数の名前を区切るには、コンマを使用します。|  
  
## <a name="remarks"></a>コメント  
 使用する場合、`Inherits` ステートメントは、クラスまたはインターフェイス定義の最初の空白でない、コメント以外の行である必要があります。 `Class` または `Interface` ステートメントの直後に記述する必要があります。  
  
 `Inherits` は、クラスまたはインターフェイスでのみ使用できます。 つまり、継承の宣言コンテキストをソースファイル、名前空間、構造体、モジュール、プロシージャ、またはブロックにすることはできません。  
  
## <a name="rules"></a>ルール  
  
- **クラスの継承。** クラスで `Inherits` ステートメントを使用する場合、指定できる基底クラスは1つだけです。  
  
     クラスは、入れ子にされたクラスから継承することはできません。  
  
- **インターフェイスの継承。** インターフェイスで `Inherits` ステートメントが使用されている場合は、1つまたは複数の基本インターフェイスを指定できます。 それぞれが同じ名前のメンバーを定義している場合でも、2つのインターフェイスから継承できます。 その場合、実装するコードでは、実装するメンバーを指定するために名前の修飾を使用する必要があります。  
  
     インターフェイスは、より制限の厳しいアクセスレベルを持つ別のインターフェイスから継承することはできません。 たとえば、`Public` インターフェイスは、`Friend` インターフェイスから継承することはできません。  
  
     インターフェイスは、その中に入れ子にされたインターフェイスから継承することはできません。  
  
 .NET Framework でのクラス継承の例として、<xref:System.SystemException> クラスを継承する <xref:System.ArgumentException> クラスがあります。 これにより、<xref:System.Exception.Message%2A> プロパティや <xref:System.Exception.ToString%2A> メソッドなど、システム例外に必要なすべての定義済みプロパティとプロシージャを <xref:System.ArgumentException> できます。  
  
 .NET Framework でのインターフェイスの継承の例として、<xref:System.Collections.IEnumerable> インターフェイスから継承する <xref:System.Collections.ICollection> インターフェイスがあります。 これにより、<xref:System.Collections.ICollection> は、コレクションを走査するために必要な列挙子の定義を継承します。  
  
## <a name="example"></a>例  
 次の例では、`Inherits` ステートメントを使用して、`thisClass` という名前のクラスが `anotherClass`という名前の基本クラスのすべてのメンバーを継承する方法を示します。  
  
 [!code-vb[VbVbalrStatements#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#37)]  
  
## <a name="example"></a>例  
 次の例は、複数のインターフェイスの継承を示しています。  
  
 [!code-vb[VbVbalrStatements#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#38)]  
  
 `thisInterface` という名前のインターフェイスには、<xref:System.IComparable>、<xref:System.IDisposable>、および <xref:System.IFormattable> インターフェイス内のすべての定義が含まれるようになりました。継承されたメンバーは、2つのオブジェクトの型固有の比較、割り当てられたリソースの解放、およびオブジェクトの値の `String`としての表現をそれぞれ提供します。 `thisInterface` を実装するクラスは、すべての基本インターフェイスのすべてのメンバーを実装する必要があります。  
  
## <a name="see-also"></a>参照

- [MustInherit](../../../visual-basic/language-reference/modifiers/mustinherit.md)
- [NotInheritable](../../../visual-basic/language-reference/modifiers/notinheritable.md)
- [クラスとオブジェクト](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
- [継承の基本](../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)
- [インターフェイス](../../../visual-basic/programming-guide/language-features/interfaces/index.md)
