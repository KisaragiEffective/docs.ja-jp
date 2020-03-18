---
title: <include> - C# プログラミング ガイド
ms.date: 07/20/2015
f1_keywords:
- include
- <include>
helpviewer_keywords:
- <include> C# XML tag
- include C# XML tag
ms.assetid: a8a70302-6196-4643-bd09-ef33f411f18f
ms.openlocfilehash: 22d87559766c04e53141e843ee8768c8aab89a85
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156975"
---
# <a name="include-c-programming-guide"></a>\<include> (C# プログラミング ガイド)

## <a name="syntax"></a>構文

```xml
<include file='filename' path='tagpath[@name="id"]' />
```

## <a name="parameters"></a>パラメーター

- `filename`

  文書を含む XML ファイルの名前。 ファイル名は、ソース コード ファイルの相対パスを使用して修飾することができます。 `filename` を単一引用符 (' ') で囲みます。

- `tagpath`

  タグ `filename` につながる `name` 内のタグのパス。 パスを単一引用符 (' ') で囲みます。

- `name`

  コメントの前に配置するタグの名前指定子。`name` には `id` が指定されます。

- `id`

コメントの前に配置するタグの ID。 ID は二重引用符 (" ") で囲みます。

## <a name="remarks"></a>解説

\<include> タグを使用して、ソース コード内の型とメンバーを記述する別のファイル内のコメントを参照することができます。 これは文書化のコメントをソース コード ファイル内に直接配置する方法の代替です。 別のファイルにドキュメントを配置することで、ソース コードから分離して、ソースの制御をドキュメントに適用できます。 1 人のユーザーがソース コード ファイルをチェックアウトし、他のユーザーがドキュメント ファイルをチェックアウトすることができます。

\<include> タグは XML XPath 構文を使用します。 \<include> の使用をカスタマイズする方法については、XPath に関するドキュメントを参照してください。

## <a name="example"></a>例

これは、複数ファイルの例です。 \<include> を使用する最初のファイルを、次に示します。

[!code-csharp[csProgGuideDocComments#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#5)]

2 番目のファイル *xml_include_tag.doc* には、次のドキュメント コメントが含まれています。

```xml
<MyDocs>

<MyMembers name="test">
<summary>
The summary for this type.
</summary>
</MyMembers>

<MyMembers name="test2">
<summary>
The summary for this other type.
</summary>
</MyMembers>

</MyDocs>
```

## <a name="program-output"></a>プログラムの出力

コマンド ライン `-doc:DocFileName.xml.` で Test および Test2 クラスをコンパイルするときに、次の出力が生成されます。Visual Studio では、プロジェクト デザイナーの [ビルド] ウィンドウで、XML ドキュメント コメントのオプションを指定します。 C# コンパイラが \<include> タグを認識すると、現在のソース ファイルではなく xml_include_tag.doc でドキュメントのコメントを検索します。 コンパイラは次に、DocFileName.xml を生成します。これは、最終的なドキュメントを生成するために、[DocFX](https://dotnet.github.io/docfx/) や [Sandcastle](https://github.com/EWSoftware/SHFB) などのドキュメント ツールによって利用されます。  
  
```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>xml_include_tag</name>
    </assembly>
    <members>
        <member name="T:Test">
            <summary>
The summary for this type.
</summary>
        </member>
        <member name="T:Test2">
            <summary>
The summary for this other type.
</summary>
        </member>
    </members>
</doc>
```  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [ドキュメント コメントとして推奨されるタグ](./recommended-tags-for-documentation-comments.md)
