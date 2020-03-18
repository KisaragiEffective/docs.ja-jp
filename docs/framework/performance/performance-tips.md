---
title: .NET のパフォーマンスに関するヒント
ms.date: 03/30/2017
helpviewer_keywords:
- C# language, performance
- performance [C#]
- Visual Basic, performance
- performance [Visual Basic]
ms.assetid: ae275793-857d-4102-9095-b4c2a02d57f4
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: 12e8d9398a1cf76267f4e8441845007da17949cd
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937897"
---
# <a name="net-performance-tips"></a>.NET のパフォーマンスに関するヒント
*パフォーマンス*という用語は、プログラムの実行速度を表す一般的な用語です。 ソース コード内で特定の基本規則に従うことにより、実行速度を上げることができることもあります。 プログラムによっては、コードを綿密に調べることが重要で、プロファイラーを使用して、可能な限り速く実行しているかどうかを確認することが必要な場合もあります。 一方、記述どおりに許容可能な速度でコードが実行されているため、このような最適化が必要ないプログラムもあります。 ここでは、パフォーマンスの低下が発生する一般的な状況と、パフォーマンスを向上させるためのヒント、およびパフォーマンスに関する追加のトピックについて説明します。 パフォーマンスの計画と計測の詳細については、「[Performance](index.md)」(パフォーマンス) を参照してください。  
  
## <a name="boxing-and-unboxing"></a>ボックス化とボックス化解除  
 たとえば、<xref:System.Collections.ArrayList?displayProperty=nameWithType> などの非ジェネリック コレクション クラスで、頻繁に値型をボックス化する必要がある状況では、値型の使用を回避するのが最も適しています。 <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> などのジェネリック コレクションを使用することで、値型のボックス化を回避できます。 ボックス化とボックス化解除は、負荷の大きい処理です。 値型をボックス化するときは、完全に新しいオブジェクトを作成する必要があります。 これには、単純な参照割り当てと比較して最大 20 倍もの時間がかかることがあります。 また、ボックス化を解除するときは、キャストのプロセスで代入の 4 倍の時間がかかることがあります。 詳しくは、「[ボックス化とボックス化解除](../../csharp/programming-guide/types/boxing-and-unboxing.md)」をご覧ください。  
  
## <a name="strings"></a>文字列  
 たとえば、短いループ内で多くの文字列変数を連結する場合は、C# の [+ 演算子](../../csharp/language-reference/operators/addition-operator.md)または Visual Basic の[連結演算子](../../visual-basic/language-reference/operators/concatenation-operators.md)ではなく、<xref:System.Text.StringBuilder?displayProperty=nameWithType> を使用します。 詳細については、「 [Visual Basic で](../../visual-basic/programming-guide/language-features/operators-and-expressions/concatenation-operators.md)複数の文字列と連結演算子[を連結する方法](../../csharp/how-to/concatenate-multiple-strings.md)」を参照してください。  
  
## <a name="destructors"></a>デストラクター  
 空のデストラクターは使用しないでください。 デストラクターがクラスに存在するときは、エントリが終了キューで作成されます。 デストラクターを呼び出すと、ガベージ コレクターが呼び出され、このキューを処理します。 デストラクターが空の場合は、これによってパフォーマンスが低下します。 詳細については、「[デストラクタ](../../csharp/programming-guide/classes-and-structs/destructors.md)」と「[Object Lifetime: How Objects Are Created and Destroyed](../../visual-basic/programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)」(オブジェクトの有効期間 : オブジェクトの作成と破棄) を参照してください。  
  
## <a name="other-resources"></a>その他の参照情報  
  
- [高速なマネージド コードを書く: 何にコストがかかるのかを知る](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973852(v=msdn.10))  
  
- [高パフォーマンスのマネージド アプリケーションを書く: 入門編](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973858(v=msdn.10))  
  
- [ガベージ コレクターの基本とパフォーマンスのヒント](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973837(v=msdn.10))  
  
- [.NET アプリケーションのパフォーマンス関連のヒントとトリック](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973839(v=msdn.10))  

- [Rico Mariani が紹介するパフォーマンスに関するニュース](https://docs.microsoft.com/archive/blogs/ricom/)  

- [Vance Morrison 氏のブログ](https://docs.microsoft.com/archive/blogs/vancem/)
  
## <a name="see-also"></a>関連項目

- [パフォーマンス](index.md)
- [Visual Basic のプログラミング ガイド](../../visual-basic/programming-guide/index.md)
- [C# プログラミング ガイド](../../csharp/programming-guide/index.md)
