---
title: 参照の等価性 (同値) をテストする方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- object identity [C#]
- reference equality [C#]
ms.assetid: 91307fda-267b-4fd2-a338-2aada39ee791
ms.openlocfilehash: 77ce2ef0ccf47d619134c120101ba2aa04f485e6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75699055"
---
# <a name="how-to-test-for-reference-equality-identity-c-programming-guide"></a>参照の等価性 (同値) をテストする方法 (C# プログラミング ガイド)
独自の型で参照の等価性の比較をサポートするためにカスタム ロジックを実装する必要はありません。 この機能は、すべての型に対して <xref:System.Object.ReferenceEquals%2A?displayProperty=nameWithType> 静的メソッドとして用意されています。  
  
 次の例に、2 つの変数の*参照の等価性*、つまり、それらの変数がメモリ内で同一のオブジェクトを参照しているかどうかを確認する方法を示します。  
  
 また、<xref:System.Object.ReferenceEquals%2A?displayProperty=nameWithType> が値型に対して常に `false` を返す理由と、文字列が等しいかどうかを確認するために <xref:System.Object.ReferenceEquals%2A> を使用しない理由を例に示します。  
  
## <a name="example"></a>例  
 [!code-csharp[csProgGuideObjects#90](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#90)]  
  
 `Equals` 汎用基底クラスの <xref:System.Object?displayProperty=nameWithType> の実装では、参照が等しいかどうかのチェックを実行しますが、このチェックを使用しないようにお勧めします。クラスがメソッドをオーバーライドする場合、予期した結果が得られない可能性があるためです。 `==` 演算子と `!=` 演算子についても同様です。 参照型を操作しようとしている場合、`==` および `!=` の既定の動作では参照が等しいかどうかのチェックを実行します。 ただし、派生クラスは演算子をオーバーロードすることによって、値が等しいかどうかのチェックを実行します。 エラーが発生する可能性を最小限に抑えるために、2 つのオブジェクトの参照が等しいかどうかを確認する必要がある場合は、常に <xref:System.Object.ReferenceEquals%2A> を使用することをお勧めします。  
  
 同じアセンブリ内の定数文字列は、常に、実行時にインターンされます。 つまり、一意のリテラル文字列ごとにそのインスタンスが 1 つだけ保持されます。 ただし、ランタイムは実行時に作成された文字列がインターンされることを保証しません。また、異なるアセンブリの 2 つの等しい定数文字列がインターンされることも保証しません。  
  
## <a name="see-also"></a>参照

- [等価比較](./equality-comparisons.md)
