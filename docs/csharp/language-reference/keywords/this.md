---
title: this キーワード - C# リファレンス
description: this キーワード (C# リファレンス)
ms.date: 07/20/2015
f1_keywords:
- this
- this_CSharpKeyword
helpviewer_keywords:
- this keyword [C#]
ms.assetid: d4f827fe-4710-410b-89b8-867dad44b8a3
ms.openlocfilehash: 2a2c487ad93e6fc75ecf95c541e859b8b60bb5b5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75715101"
---
# <a name="this-c-reference"></a>this (C# リファレンス)

`this` キーワードはクラスの現在のインスタンスを参照します。拡張メソッドの最初のパラメーターの修飾子としても使用されます。

> [!NOTE]
> ここでは、クラス インスタンスでの `this` の使用について説明します。 拡張メソッドでの使用の詳細については、「[拡張メソッド](../../programming-guide/classes-and-structs/extension-methods.md)」を参照してください。

`this` の一般的な使い方を次に示します。

- 似た名前によって非表示にされるメンバーを修飾する場合は次のようになります。

  [!code-csharp[csrefKeywordsAccess#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsAccess/CS/csrefKeywordsAccess.cs#4)]  

- オブジェクトを他のメソッドにパラメーターとして渡す場合は次のようになります。

  ```csharp
  CalcTax(this);
  ```

- インデクサーを宣言する場合は、次のようになります。

  [!code-csharp[csrefKeywordsAccess#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsAccess/CS/csrefKeywordsAccess.cs#5)]

静的メンバー関数は、クラス レベルで存在し、オブジェクトの一部ではないため、`this` ポインターがありません。 静的メソッドで `this` を参照するとエラーになります。

## <a name="example"></a>例

この例では、似た名前によって非表示にされている `this` クラスのメンバー `Employee` と `name` を修飾するために `alias` が使用されています。 また、別のクラスに属するメソッド `CalcTax` にオブジェクトを渡すためにも使用されています。

[!code-csharp[csrefKeywordsAccess#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsAccess/CS/csrefKeywordsAccess.cs#3)]

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [base](base.md)
- [メソッド](../../programming-guide/classes-and-structs/methods.md)
