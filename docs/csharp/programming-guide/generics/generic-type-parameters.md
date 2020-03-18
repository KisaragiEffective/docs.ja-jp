---
title: ジェネリック型の型パラメーター - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], type parameters
- type parameters [C#]
ms.assetid: a03b0ab2-0606-4b41-b7bf-e64d5bb4d18f
ms.openlocfilehash: 8412980d35989c445d2e0a44c0b9f35e6087bb8d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712183"
---
# <a name="generic-type-parameters-c-programming-guide"></a>ジェネリック型の型パラメーター (C# プログラミング ガイド)

ジェネリック型またはメソッド定義で、型パラメーターは、ジェネリック型のインスタンスを作成するときにクライアントが指定する特定の型のためのプレースホルダーになります。 「`GenericList<T>`ジェネリックの概要[」に記載されている ](./index.md) などのジェネリック クラスは、実際は型でないため、そのままでは使用できません。これは型の設計図のようなものです。 `GenericList<T>` を使用するには、クライアント コードで構築された型を宣言し、インスタンス化する必要があります。山かっこ内に型引数を指定します。 この特定のクラスの型引数は、コンパイラで認識されるあらゆる型にすることができます。 構築された型インスタンスは、次のようにさまざまな型引数を利用し、いくつでも作成できます。  
  
[!code-csharp[csProgGuideGenerics#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#7)]  
  
`GenericList<T>` の各インスタンスでは、クラス内のすべての `T` は実行時に型引数に置換されます。 この置換を使用し、単一クラス定義を使用してタイプ セーフで効率的なオブジェクトを 3 つ作成しました。 この置換が CLR で実行されるしくみについては、「[ランタイムのジェネリック](./generics-in-the-run-time.md)」を参照してください。  
  
## <a name="type-parameter-naming-guidelines"></a>型パラメーターの名前付けガイドライン  
  
- 1 文字の名前でも完全に説明され、説明的な名前を付けることに意味がない場合を除き、ジェネリック型パラメーターには**説明的な名前を付けてください**。  
  
   [!code-csharp[csProgGuideGenerics#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#8)]  
  
- 1 文字の型パラメーターを持つ型の型パラメーター名として T を利用することを**検討してください**。  
  
   [!code-csharp[csProgGuideGenerics#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#9)]  
  
- 型パラメーターの説明的な名前には "T" という**接頭辞を付けてください**。  
  
   [!code-csharp[csProgGuideGenerics#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#10)]  
  
- 型パラメーターに与えられた制約をパラメーターの名前で示唆することを**検討してください**。 たとえば、`ISession` に制約されているパラメーターの名前を `TSession` にします。

コード分析規則 [CA1715](/visualstudio/code-quality/ca1715) を使用して、型パラメーターの名前が適切に付けられていることを確認できます。
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Generic>
- [C# プログラミングガイド](../index.md)
- [ジェネリック](./index.md)
- [C++ テンプレートと C# ジェネリックの違い](./differences-between-cpp-templates-and-csharp-generics.md)
