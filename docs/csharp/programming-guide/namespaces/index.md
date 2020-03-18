---
title: 名前空間 - C# プログラミング ガイド
ms.date: 08/21/2018
helpviewer_keywords:
- C# language, namespaces
- namespaces [C#]
ms.assetid: b1c4ab46-3fad-4ffa-9deb-dd50a2d8c65a
ms.openlocfilehash: 21452e259596c9ab10b3d653ec1d8fb90fad131d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75937609"
---
# <a name="namespaces-c-programming-guide"></a>名前空間 (C# プログラミング ガイド)

C# プログラミングでは、名前空間が 2 つの方法でよく使用されます。 最初の方法では、次のように .NET で名前空間を使用して、その多くのクラスを整理します。  

[!code-csharp[csProgGuide#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#22)]

<xref:System> は名前空間で、<xref:System.Console> はその名前空間内のクラスです。 以下の例のように、`using` キーワードを使用できるため、完全な名前は必要ありません。

[!code-csharp[csProgGuide#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/using.cs#1)]

[!code-csharp[csProgGuide#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#25)]

詳細については、「[using ディレクティブ](../../language-reference/keywords/using-directive.md)」をご覧ください。

2 つ目の方法では、独自の名前空間を宣言します。これは、より大きなプログラミング プロジェクトでクラス名とメソッド名のスコープを制御するのに役立ちます。 名前空間を宣言するには、以下の例のように、[namespace](../../language-reference/keywords/namespace.md) キーワードを使用します。

[!code-csharp[csProgGuideNamespaces#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#6)]

名前空間の名前を、有効な C# の[識別子名](../inside-a-program/identifier-names.md)にする必要があります。

## <a name="namespaces-overview"></a>名前空間の概要

名前空間には次の特徴があります。

- 大きなコード プロジェクトを整理します。
- `.` 演算子を使用して、区切られます。
- `using` ディレクティブを使用すると、クラスごとに名前空間の名前を指定する必要がなくなります。
- `global` 名前空間は "ルート" 名前空間です。`global::System` は常に .NET 名前空間の <xref:System> を参照します。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/namespaces.md)に関する記事の「[名前空間](~/_csharplang/spec/introduction.md)」に関するセクションを参照してください。

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [名前空間の使用](using-namespaces.md)
- [My 名前空間を使用する方法](how-to-use-the-my-namespace.md)
- [識別子名](../inside-a-program/identifier-names.md)
- [using ディレクティブ](../../language-reference/keywords/using-directive.md)
- [:: 演算子](../../language-reference/operators/namespace-alias-qualifier.md)
