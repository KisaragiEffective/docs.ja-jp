---
title: <c> - C# プログラミング ガイド
ms.date: 07/20/2015
f1_keywords:
- c
- <c>
helpviewer_keywords:
- text, marking as code [C#]
- code, marking text as [C#]
- c C# XML tag
- <c> C# XML tag
ms.assetid: aad5b16e-a29e-445e-bd0d-eea0b138d7b2
ms.openlocfilehash: d5b28ee6db52d191f8454592d792ac0a1e1dc73b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76793451"
---
# <a name="c-c-programming-guide"></a>\<c> (C# プログラミング ガイド)

## <a name="syntax"></a>構文

```xml
<c>text</c>
```

## <a name="parameters"></a>パラメーター

- `text`

  コードとして指定するテキストです。

## <a name="remarks"></a>解説

\<c> タグを使用すると、説明内のテキストをコードとして指定できます。 複数行をコードとして指定する場合は、[\<code>](./code.md) タグを使用します。

コンパイル時に [-doc](../../language-reference/compiler-options/doc-compiler-option.md) を指定して、ドキュメント コメントをファイルに出力します。

## <a name="example"></a>例

[!code-csharp[csProgGuideDocComments#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#2)]
  
## <a name="see-also"></a>参照

- [C# プログラミング ガイド](../index.md)
- [ドキュメント コメント用の推奨タグ](./recommended-tags-for-documentation-comments.md)
