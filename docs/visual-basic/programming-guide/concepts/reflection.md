---
title: リフレクション
ms.date: 07/20/2015
ms.assetid: d991bc0f-d16a-4ac5-9351-70e5c5b9891b
ms.openlocfilehash: 28f33c88f7aaaf51938a7d27fd2218a97b628acd
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349275"
---
# <a name="reflection-visual-basic"></a>リフレクション (Visual Basic)
リフレクションは、アセンブリ、モジュール、および型を記述する (<xref:System.Type> 型の) オブジェクトを提供します。 リフレクションを使用すると、動的に型のインスタンスを作成したり、作成したインスタンスを既存のオブジェクトにバインドしたり、さらに既存のオブジェクトから型を取得してそのオブジェクトのメソッドを呼び出したり、フィールドやプロパティにアクセスしたりできます。 コードで属性を使用している場合は、リフレクションを使用してそれらにアクセスできます。 詳細については、「[属性](../../../standard/attributes/index.md)」を参照してください。  
  
 次の例は、`GetType` メソッドを使用して変数の型を取得する簡単なリフレクションを示しています。このメソッドは、`Object` 基底クラスからすべての型に継承される静的メソッドです。  
  
```vb  
' Using GetType to obtain type information:  
Dim i As Integer = 42  
Dim type As System.Type = i.GetType()  
System.Console.WriteLine(type)  
```  
  
 出力は次のようになります。  
  
 `System.Int32`  
  
 次の例では、リフレクションを使用して、読み込まれたアセンブリの完全名を取得します。  
  
```vb  
' Using Reflection to get information from an Assembly:  
Dim info As System.Reflection.Assembly = GetType(System.Int32).Assembly  
System.Console.WriteLine(info)  
```  
  
 出力は次のようになります。  
  
 `mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089`  
  
## <a name="reflection-overview"></a>リフレクションの概要  
 リフレクションは、次の場合に役立ちます。  
  
- プログラムのメタデータ内の属性にアクセスする必要がある。 詳細については、「[属性に格納されている情報の取得](../../../standard/attributes/retrieving-information-stored-in-attributes.md)」を参照してください。  
  
- アセンブリの型をチェックし、インスタンス化する。  
  
- 実行時に新しい型を作成する。 <xref:System.Reflection.Emit> でクラスを使います。  
  
- 遅延バインディングを実行するために、実行時に作成された型でメソッドにアクセスする。 「[型の動的な読み込みおよび使用](../../../framework/reflection-and-codedom/dynamically-loading-and-using-types.md)」を参照してください。  
  
## <a name="related-sections"></a>関連セクション  
 詳細:  
  
- [リフレクション](../../../framework/reflection-and-codedom/reflection.md)  
  
- [型情報の表示](../../../framework/reflection-and-codedom/viewing-type-information.md)  
  
- [リフレクションとジェネリック型](../../../framework/reflection-and-codedom/reflection-and-generic-types.md)  
  
- <xref:System.Reflection.Emit>  
  
- [属性に格納されている情報の取得](../../../standard/attributes/retrieving-information-stored-in-attributes.md)  
  
## <a name="see-also"></a>参照

- [Visual Basic のプログラミング ガイド](../../../visual-basic/programming-guide/index.md)
- [.NET のアセンブリ](../../../standard/assembly/index.md)
