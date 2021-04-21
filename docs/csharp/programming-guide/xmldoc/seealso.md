---
title: <seealso> - C# プログラミング ガイド
description: XML タグを使用する方法について説明 <seealso> します。 このタグを使用して、「関連項目」セクションに表示するテキストを指定することができます。
ms.date: 07/20/2015
f1_keywords:
- cref
- <seealso>
- seealso
helpviewer_keywords:
- cref [C#], see also
- seealso C# XML tag
- cref [C#]
- cross-references [C#], tags
- <seealso> C# XML tag
ms.assetid: 8e157f3f-f220-4fcf-9010-88905b080b18
ms.openlocfilehash: c5baefbfca3a94095a90eb43c2bf13eb924c8cdc
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477757"
---
# <a name="seealso-c-programming-guide"></a>\<seealso> (C# プログラミング ガイド)

## <a name="syntax"></a>構文

```xml
<seealso cref="member"/>
```

## <a name="parameters"></a>パラメーター

- cref = "`member`"

  現在のコンパイル環境からの呼び出しに利用できる、メンバーまたはフィールドへの参照。 コンパイラは、指定されたコード要素が存在するかどうかを確認し、`member` を出力 XML 内の要素名に渡します。`member` は二重引用符 (" ") で囲む必要があります。

  ジェネリック型への cref 参照を作成する方法については、[cref 属性](./cref-attribute.md)に関するページを参照してください。

## <a name="remarks"></a>Remarks

`<seealso>` タグを使用して、「関連項目」セクションに表示するテキストを指定することができます。 [\<see>](./see.md) を使用すると、テキスト内からリンクを指定できます。

コンパイル時に [**DocumentationFile**](../../language-reference/compiler-options/output.md#documentationfile) を指定して、ドキュメント コメントをファイルに出力します。

## <a name="example"></a>例

\<seealso> の使用例については、[\<summary>](./summary.md) を参照してください。

## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [ドキュメント コメント用の推奨タグ](./recommended-tags-for-documentation-comments.md)
