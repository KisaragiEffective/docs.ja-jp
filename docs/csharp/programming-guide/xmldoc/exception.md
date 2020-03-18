---
title: <exception> - C# プログラミング ガイド
ms.date: 07/20/2015
f1_keywords:
- exception
- <exception>
helpviewer_keywords:
- <exception> C# XML tag
- exception C# XML tag
ms.assetid: dd73aac5-3c74-4fcf-9498-f11bff3a2f3c
ms.openlocfilehash: 14318ac0b0cdf781d0488eecaf934879017d91f6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76789805"
---
# <a name="exception-c-programming-guide"></a>\<exception> (C# プログラミング ガイド)

## <a name="syntax"></a>構文

```xml
<exception cref="member">description</exception>
```

## <a name="parameters"></a>パラメーター

- cref = "`member`"

  現在のコンパイル環境から使用できる例外の参照。 コンパイラは、指定された例外が存在し、出力の XML で `member` が正規要素名に変換されることを確認します。 `member` は、二重引用符 (" ") で囲む必要があります。

  ジェネリック型を参照するよう `member` を書式設定する方法について詳しくは、[XML ファイルの処理](processing-the-xml-file.md)に関するページをご覧ください。

- `description`

  例外の説明。

## <a name="remarks"></a>解説

\<exception> タグを使用すると、スローできる例外を指定できます。 このタブは、メソッド、プロパティ、イベント、インデクサーの定義に適用できます。

コンパイル時に [-doc](../../language-reference/compiler-options/doc-compiler-option.md) を指定して、ドキュメント コメントをファイルに出力します。

例外処理の詳細については、「[例外と例外処理](../exceptions/index.md)」を参照してください。

## <a name="example"></a>例

[!code-csharp[csProgGuideDocComments#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#4)]

## <a name="see-also"></a>参照

- [C# プログラミング ガイド](../index.md)
- [ドキュメント コメント用の推奨タグ](recommended-tags-for-documentation-comments.md)
