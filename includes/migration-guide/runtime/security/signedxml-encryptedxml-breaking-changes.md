---
ms.openlocfilehash: 68da7216890da1819a994161507355a0b5ea1f9f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67857533"
---
### <a name="signedxml-and-encryptedxml-breaking-changes"></a>SignedXml と EncryptedXml の互換性に影響する変更点

|   |   |
|---|---|
|説明|.NET Framework 4.6.2 では、<xref:System.Security.Cryptography.Xml.SignedXml?displayProperty=name> および <xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=name> のセキュリティ修正プログラムにより、実行時の動作が変わります。 次に例を示します。<ul><li>ドキュメントに <code>id</code> 属性が同じ値の複数の要素があり、署名でそれらの要素の 1 つが署名のルートとして指定されている場合、ドキュメントは無効と見なされるようになります。</li><li>参照で非正規 XPath 変換アルゴリズムを使っているドキュメントは、無効と見なされるようになります。</li><li>参照で非正規 XSLT 変換アルゴリズムを使っているドキュメントは、無効と見なされるようになります。</li><li>外部リソースのデタッチされた署名を利用しているプログラムは、利用できなくなります。</li></ul>|
|提案される解決策|ドキュメントの受信者が処理できない可能性があるため、開発者は <xref:System.Security.Cryptography.Xml.XmlDsigXsltTransform> と <xref:System.Security.Cryptography.Xml.XmlDsigXsltTransform> の使用、および <xref:System.Security.Cryptography.Xml.Transform> の派生型を確認することが必要な場合があります。|
|スコープ|Minor|
|バージョン|4.6.2|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Security.Cryptography.Xml.Transform?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.Xml.XmlDsigXPathTransform?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.Xml.XmlDsigXsltTransform?displayProperty=nameWithType></li></ul>|
