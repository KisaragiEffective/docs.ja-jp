---
title: 識別子名
description: C# プログラミング言語の有効な識別子名に関する規則について説明します。
ms.date: 08/21/2018
ms.openlocfilehash: bef6e2ea285b5391af3350ae42a4105d140c6d1b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77673369"
---
# <a name="identifier-names"></a>識別子名

**識別子**は、型 (クラス、インターフェイス、構造体、デリゲート、列挙型)、メンバー、変数、名前空間に割り当てる名前です。 識別子を有効にするには次の規則に従う必要があります。

- 識別子の名前は文字または `_` で始まらなければなりません。
- 識別子には、Unicode 文字、10 進数文字、Unicode 接続文字、Unicode 結合文字、Unicode 書式設定文字を含めることができます。 Unicode カテゴリの詳細については、[Unicode カテゴリ データベース](https://www.unicode.org/reports/tr44/)に関するページを参照してください。
識別子で `@` プレフィックスを使用することで、C# キーワードに一致する識別子を宣言できます。 `@` は識別子名の一部ではありません。 たとえば、`@if` の場合、`if` という名前の識別子が宣言されます。 このような[逐語的識別子](../../language-reference/tokens/verbatim.md)は主に、他の言語で宣言された識別子との相互運用性のために使用されます。

有効な識別子の完全な定義は、[C# 言語仕様の「Identifiers」 (識別子) トピック](../../../../_csharplang/spec/lexical-structure.md#identifiers)にあります。

## <a name="naming-conventions"></a>名前付け規則

規則に加え、.NET API 全体で使用されるさまざまな識別子[命名規則](../../../standard/design-guidelines/naming-guidelines.md)があります。 慣例により、C# プログラムでは、型名、名前空間、すべてのパブリック メンバーに `PascalCase` が使用されます。 また、次の規則が一般的です。

- インターフェイス名は大文字 `I` で始まります。
- 属性型は `Attribute` という単語で終わります。
- 列挙型では、フラグ以外に単数名詞が使用され、フラグに複数名詞が使用されます。
- 識別子には、連続する 2 つの `_` 文字を含めないでください。 このような文字は、コンパイラで生成される識別子のために予約されています。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [インサイド C# プログラム](./index.md)
- [C# リファレンス](../../language-reference/index.md)
- [クラス](../classes-and-structs/classes.md)
- [構造体型](../../language-reference/builtin-types/struct.md)
- [名前空間](../namespaces/index.md)
- [インターフェイス](../interfaces/index.md)
- [デリゲート](../delegates/index.md)
