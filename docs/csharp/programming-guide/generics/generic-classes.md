---
title: ジェネリック クラス - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, generic classes
- generics [C#], classes
ms.assetid: 27d6f256-cd61-41e3-bc6e-b990a53b0224
ms.openlocfilehash: 1fdfaa833ad32428d341b6f3a61cc7f638036183
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937506"
---
# <a name="generic-classes-c-programming-guide"></a>ジェネリック クラス (C# プログラミング ガイド)
ジェネリック クラスは、特定のデータ型に固有ではない操作をカプセル化します。 ジェネリック クラスは最も一般的に、リンク リスト、ハッシュ テーブル、スタック、キュー、ツリーなどのコレクションと共に使用されます。 コレクションの項目を追加または削除するなどの操作は、保存されているデータの型に関係なく、基本的に同じように実行されます。  
  
 コレクション クラスを必要とするほとんどのシナリオで、.NET クラス ライブラリで提供されているものを使用するという方法が推奨されます。 これらのクラスの使用の詳細については、「[.NET のジェネリック コレクション](../../../standard/generics/collections.md)」を参照してください。  
  
 通常、ジェネリック クラスを作成するには、既存の具象クラスから始め、汎用性と使いやすさの間で最適なバランスが取れるまで、一度に 1 つずつ型を型パラメーターに変更します。 独自のジェネリック クラスを作成するときの重要な考慮事項は次のとおりです。  
  
- 型パラメーターに汎用化する型。  
  
     通例、パラメーター化できる型が多ければ多いほど、コードの柔軟性が上がり、再利用しやすくなります。 ただし、汎用化が多すぎると、他の開発者にとって読みにくい、理解しにくいコードが生成されます。  
  
- 型パラメーターに適用する制約 (制約がある場合) (「[型パラメーターの制約](./constraints-on-type-parameters.md)」を参照)。  
  
     処理しなければならない型を処理できる範囲で最大の制約を適用することが推奨されます。 たとえば、ジェネリック クラスが参照型でのみ使用される場合、クラス制約を適用します。 それにより、意図しない、値型でのクラスの使用が回避され、`as` で `T` 演算子を使用したり、null 値を確認したりできます。  
  
- 基底クラスやサブクラスの要因としてジェネリック動作を考慮するかどうか。  
  
     ジェネリック クラスは基底クラスとして機能できるので、非ジェネリック クラスと同様の設計考慮事項がここで適用されます。 ジェネリック基底クラスからの継承ルールについて、このトピックの後半で確認してください。  
  
- 1 つまたは複数のジェネリック インターフェイスを実装するかどうか。  
  
     たとえば、ジェネリック基盤のコレクションで項目を作成するために使用されるクラスを設計するとき、場合によっては、<xref:System.IComparable%601> のようなインターフェイスを実装する必要があります。ここで `T` はクラスの型です。  
  
 単純なジェネリック クラスの例については、「[ジェネリックの概要](./index.md)」を参照してください。  
  
 型パラメーターや制約のルールは、特に継承とメンバーのアクセシビリティに関して、ジェネリック クラスの動作と密接な関係があります。 続行する前に、いくつかの用語を理解してください。 ジェネリック クラス `Node<T>,` の場合、型引数を指定することで、クライアント コードはクラスを参照し、構築されたクローズ型を作成できます (`Node<int>`)。 あるいは、ジェネリック基底クラスを指定するときなど、型パラメーターを指定せず、構築されたオープン型を作成できます (`Node<T>`)。 ジェネリック クラスは、具象、構築されたクローズ型、または構築されたオープン型の基底クラスから継承できます。  
  
 [!code-csharp[csProgGuideGenerics#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#16)]  
  
 非ジェネリック クラス、言い換えれば、具象クラスは、構築されたクローズ型の基底クラスから継承できますが、構築されたオープン型のクラスや型パラメーターからは継承できません。ランタイム時、基底クラスのインスタンス化に必要な型引数をクライアント コードが提供できないためです。  
  
 [!code-csharp[csProgGuideGenerics#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#17)]  
  
 構築されたオープン型から継承するジェネリック クラスは、継承クラスで共有されない基底クラスの型パラメーターに対して型引数を提供する必要があります。次のコードをご覧ください。  
  
 [!code-csharp[csProgGuideGenerics#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#18)]  
  
 構築されたオープン型から継承するジェネリック クラスは、基底クラスの制約のスーパーセットである (基底クラスの制約を暗黙に定義する) 制約を指定する必要があります。  
  
 [!code-csharp[csProgGuideGenerics#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#19)]  
  
 ジェネリック型では、次のように、複数の型パラメーターと制約を使用できます。  
  
 [!code-csharp[csProgGuideGenerics#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#20)]  
  
 構築されたオープン型と構築されたクローズ型をメソッド パラメーターとして使用できます。  
  
 [!code-csharp[csProgGuideGenerics#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#21)]  
  
 ジェネリック クラスでインターフェイスを実装する場合、そのクラスのすべてのインスタンスをそのインターフェイスにキャストできます。  
  
 ジェネリック クラスは変化しません。 言い換えると、入力パラメーターが `List<BaseClass>` を指定するとき、`List<DerivedClass>` を指定するとコンパイル時エラーが表示されます。  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Generic>
- [C# プログラミングガイド](../index.md)
- [ジェネリック](./index.md)
- [列挙子の状態を保存する](https://docs.microsoft.com/archive/blogs/wesdyer/saving-the-state-of-enumerators)
- [継承パズル、パート 1](https://docs.microsoft.com/archive/blogs/ericlippert/an-inheritance-puzzle-part-one)
