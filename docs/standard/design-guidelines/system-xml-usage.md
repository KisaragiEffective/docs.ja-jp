---
title: System.Xml の使用法
ms.date: 10/22/2008
ms.technology: dotnet-standard
ms.assetid: 82302f0d-a621-4c6f-b57d-999bd61f21a6
ms.openlocfilehash: 2ecb709684834a8280c841eb8eef4f024481f7a4
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743589"
---
# <a name="systemxml-usage"></a>System.Xml の使用法
このセクションでは、XML データを表すために使用できる <xref:System.Xml?displayProperty=nameWithType> 名前空間に存在するいくつかの型の使用方法について説明します。

 ❌ <xref:System.Xml.XmlNode> または <xref:System.Xml.XmlDocument> を使用して XML データを表すことはできません。 代わりに、<xref:System.Xml.Linq.XNode> の <xref:System.Xml.XPath.IXPathNavigable>、<xref:System.Xml.XmlReader>、<xref:System.Xml.XmlWriter>、またはサブタイプのインスタンスの使用を優先します。 `XmlNode` と `XmlDocument` は、パブリック Api で公開するように設計されていません。

 ✔️は、XML を受け入れるか返すメンバーの入力または出力として、`XNode` の `XmlReader`、`IXPathNavigable`、またはサブタイプを使用します。

 `XmlDocument`、`XmlNode`、または <xref:System.Xml.XPath.XPathDocument>ではなく、これらの抽象化を使用します。これにより、メモリ内の XML ドキュメントの特定の実装からメソッドが分離され、`XNode`、`XmlReader`、または <xref:System.Xml.XPath.XPathNavigator>を公開する仮想 XML データソースとの連携が可能になります。

 基になるオブジェクトモデルまたはデータソースの XML ビューを表す型を作成する場合は ❌ `XmlDocument` をサブクラス化しません。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [使用方法のガイドライン](../../../docs/standard/design-guidelines/usage-guidelines.md)
