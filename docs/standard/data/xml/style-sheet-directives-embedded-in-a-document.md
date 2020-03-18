---
title: ドキュメントに埋め込まれたスタイル シート ディレクティブ
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: d79fb295-ebc7-438d-ba1b-05be7d534834
ms.openlocfilehash: 19e25ab7262bb006144eea71e74bd7454066b3f6
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710155"
---
# <a name="style-sheet-directives-embedded-in-a-document"></a>ドキュメントに埋め込まれたスタイル シート ディレクティブ

既存の XML に `<?xml:stylesheet?>` というスタイル シート ディレクティブが含まれていることがあります。 Microsoft Internet Explorer は、これを `<?xml-stylesheet?>` 構文に代わるものとして受け入れます。 次のように `<?xml:stylesheet?>` ディレクティブが含まれている XML データを XML ドキュメント オブジェクト モデル (DOM) に読み込もうとすると、例外がスローされます。

```xml
<?xml version="1.0" ?>
<?xml:stylesheet type="text/xsl" href="test2.xsl"?>
<root>
    <test>Node 1</test>
    <test>Node 2</test>
</root>
```

例外が発生するのは、`<?xml:stylesheet?>` が DOM に対する無効な **ProcessingInstruction** と見なされるからです。 『Namespaces in XML』仕様によれば、**ProcessingInstruction** は、QNames (修飾名) ではなく、NCNames (コロンが含まれない名前) にする必要があります。

『Namespaces in XML』仕様書のセクション 6 によると、**Load** メソッドと **LoadXml** メソッドをこの仕様に準拠させた場合の影響として、ドキュメントには次の規則が適用されることになります。

- すべての要素型と属性名は、0 個または 1 個のコロンを含みます。

- エンティティ名、処理命令ターゲット、記法名は、いずれもコロンを含みません。

コロンが含まれた `<?xml:stylesheet?>` は、2 番目の規則に違反します。

W3C (World Wide Web Consortium) 勧告『[Associating Style Sheets with XML documents Version 1.0](https://www.w3.org/TR/xml-stylesheet/)』によれば、XSLT スタイル シートを XML ドキュメントに関連付ける処理命令は、コロンをダッシュに置き換えた `<?xml-stylesheet?>` です。

## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](xml-document-object-model-dom.md)
