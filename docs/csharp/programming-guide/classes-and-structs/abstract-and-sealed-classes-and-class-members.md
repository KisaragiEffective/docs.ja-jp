---
title: 抽象クラスとシール クラス、およびクラス メンバー - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- abstract classes [C#]
- sealed classes [C#]
- C# language, abstract classes
- C# language, sealed
ms.assetid: 99aa52f7-b435-43f9-936e-2470af734c4e
ms.openlocfilehash: 07738031f1dec05424f7c3756f49a8f1f9a2c44b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75715009"
---
# <a name="abstract-and-sealed-classes-and-class-members-c-programming-guide"></a>抽象クラスとシール クラス、およびクラス メンバー (C# プログラミング ガイド)
[abstract](../../language-reference/keywords/abstract.md) キーワードを使用すると、派生クラスで実装する必要のある不完全な[クラス](../../language-reference/keywords/class.md) メンバーを作成できます。  
  
 また、[sealed](../../language-reference/keywords/sealed.md) キーワードを使用すると、既に [virtual](../../language-reference/keywords/virtual.md) とマークされているクラスや特定のクラス メンバーを継承しないようにできます。  
  
## <a name="abstract-classes-and-class-members"></a>抽象クラスと抽象クラス メンバー  
 クラス定義の前にキーワード `abstract` を指定することで、クラスを抽象として宣言できます。 次に例を示します。  
  
 [!code-csharp[csProgGuideInheritance#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#13)]  
  
 抽象クラスはインスタンス化できません。 抽象クラスの目的は、複数の派生クラスで共有できる基底クラスの共通の定義を提供することです。 たとえば、クラス ライブラリでは、その多くの関数のパラメーターとして使用される抽象クラスを定義できます。このライブラリを使用する場合は、派生クラスを作成してクラスの独自の実装を提供する必要があります。  
  
 抽象クラスでは、抽象メソッドも定義できます。 抽象メソッドを定義するには、メソッドの戻り値の型の前に `abstract` キーワードを記述します。 次に例を示します。  
  
 [!code-csharp[csProgGuideInheritance#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#14)]  
  
 抽象メソッドには実装がないので、メソッド定義の後に、通常のメソッド ブロックの代わりにセミコロン (;) を配置します。 抽象クラスの派生クラスでは、すべての抽象メソッドを実装する必要があります。 抽象クラスが基底クラスから仮想メソッドを継承した場合は、この抽象クラスでは抽象メソッドで仮想メソッドをオーバーライドできます。 次に例を示します。  
  
 [!code-csharp[csProgGuideInheritance#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#15)]  
  
 `virtual` メソッドが `abstract` として宣言されている場合、そのメソッドは、その抽象クラスを継承するどのクラスでも仮想になります。 抽象メソッドを継承するクラスでは、そのメソッドの元の実装にアクセスできません。上の例では、クラス F の `DoWork` は、クラス D の `DoWork` を呼び出すことができません。このようにして抽象クラスは、派生クラスに対し、仮想メソッドの新しいメソッド実装を強制的に提供させることができます。  
  
## <a name="sealed-classes-and-class-members"></a>シール クラスとシール クラス メンバー  
 クラス定義の前にキーワード [ を指定することで、クラスを ](../../language-reference/keywords/sealed.md)sealed`sealed` として宣言できます。 次に例を示します。  
  
 [!code-csharp[csProgGuideInheritance#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#16)]  
  
 シール クラスは、基底クラスとして使用できません。 このため、シール クラスは抽象クラスになることもできません。 シール クラスにより、派生が防止されます。 シール クラスは基底クラスとして使用できないので、実行時の最適化で、シール クラス メンバーを多少高速に呼び出すことができる場合があります。  
  
 基底クラスの仮想メンバーをオーバーライドしている派生クラスのメソッド、インデクサー、プロパティ、またはイベントでは、そのメンバーをシールとして宣言できます。 これにより、その後の派生クラスでは、メンバーの仮想性が無効になります。 このように宣言するには、クラス メンバー宣言で `sealed`override[ キーワードの前に ](../../language-reference/keywords/override.md) キーワードを配置します。 次に例を示します。  
  
 [!code-csharp[csProgGuideInheritance#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#17)]  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [クラスと構造体](./index.md)
- [継承](./inheritance.md)
- [メソッド](./methods.md)
- [フィールド](./fields.md)
- [抽象プロパティを定義する方法](./how-to-define-abstract-properties.md)
