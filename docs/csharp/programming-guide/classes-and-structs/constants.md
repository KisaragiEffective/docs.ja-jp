---
title: 定数 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, constants
- constants [C#]
ms.assetid: 1fb39621-1738-49b1-a1b3-8587f109123f
ms.openlocfilehash: 85f6684617b893bdd85eb5b530aa2481941fbc5d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77093553"
---
# <a name="constants-c-programming-guide"></a>定数 (C# プログラミング ガイド)
定数とは、コンパイル時に既知であり、プログラムの実行期間を通じて変更されない値です。 定数を宣言するには、[const](../../language-reference/keywords/const.md) 修飾子を使用します。 [ として宣言できるのは、C# ](../../language-reference/builtin-types/built-in-types.md)組み込み型<xref:System.Object?displayProperty=nameWithType> (`const` を除く) のみです。 クラス、構造体、配列などのユーザー定義型を `const` にすることはできません。 実行時に (コンストラクターなどで) 一度だけ初期化され、その後は変更できないクラス、構造体、または配列を作成するには、[readonly](../../language-reference/keywords/readonly.md) 修飾子を使用します。  
  
 C# では、`const` のメソッド、プロパティ、またはイベントはサポートされません。  
  
 enum 型を使用すると、組み込みの整数型 (`int`、`uint`、`long` など) の名前付き定数を定義できます。 詳細については、「[enum](../../language-reference/builtin-types/enum.md)」を参照してください。  
  
 定数は、宣言するときに初期化する必要があります。 次に例を示します。  
  
 [!code-csharp[csProgGuideObjects#64](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#64)]  
  
 この例では、定数 `months` は常に 12 になり、クラス自体によっても変更できません。 実際、コンパイラが C# ソース コードで定数の識別子 (この場合は `months`) を検出すると、コンパイラが生成する中間言語 (IL) コードには、識別子の代わりにリテラル値が直接出力されます。 実行時に定数に関連付けられる変数アドレスが存在しないため、`const` フィールドは、参照渡しすることも、式の左辺値として指定することもできません。  
  
> [!NOTE]
> DLL などの他のコードに定義されている定数値を参照するときは、注意が必要です。 新しいバージョンの DLL で定数の新しい値を定義しても、その新バージョンを対象にプログラムを再コンパイルするまで、プログラムには古いリテラル値が保持されたままになります。  
  
 同じ型の複数の定数を、次のように同時に宣言できます。  
  
 [!code-csharp[csProgGuideObjects#65](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#65)]  
  
 定数の初期化に使用する式は、循環参照を形成しない限り別の定数を参照できます。 次に例を示します。  
  
 [!code-csharp[csProgGuideObjects#66](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#66)]  
  
 定数は、[public](../../language-reference/keywords/public.md)、[private](../../language-reference/keywords/private.md)、[protected](../../language-reference/keywords/protected.md)、[internal](../../language-reference/keywords/internal.md)、[protected internal](../../language-reference/keywords/protected-internal.md) または [private protected](../../language-reference/keywords/private-protected.md) としてマークできます。 これらのアクセス修飾子により、クラスのユーザーが定数にアクセスする方法が定義されます。 詳細については、「[アクセス修飾子](./access-modifiers.md)」を参照してください。  
  
 定数の値は型のすべてのインスタンスで同じであるため、定数は[静的](../../language-reference/keywords/static.md)フィールドのようにアクセスされます。 定数の宣言に `static` キーワードは使用しません。 定数を定義しているクラスに含まれていない式で定数を使用する場合は、クラス名、ピリオド、定数の名前を使用する必要があります。 次に例を示します。  
  
 [!code-csharp[csProgGuideObjects#67](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#67)]  
  
## <a name="c-language-specification"></a>C# 言語仕様  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [クラスと構造体](./index.md)
- [Properties](./properties.md)
- [型](../types/index.md)
- [readonly](../../language-reference/keywords/readonly.md)
- [C# の不変性パート 1: 不変性の種類](https://docs.microsoft.com/archive/blogs/ericlippert/immutability-in-c-part-one-kinds-of-immutability)
