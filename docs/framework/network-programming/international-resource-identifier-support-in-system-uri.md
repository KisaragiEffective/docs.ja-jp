---
title: System.Uri での International Resource Identifier のサポート
ms.date: 03/30/2017
ms.assetid: b5e994c3-3535-4aff-8e1b-b69be22e9a22
ms.openlocfilehash: f78fff250aae177b5f0360e77a1c41a2f2bb0527
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "64647337"
---
# <a name="international-resource-identifier-support-in-systemuri"></a>System.Uri での International Resource Identifier のサポート
<xref:System.Uri?displayProperty=nameWithType> クラスは、International Resource Identifier (IRI) および国際化ドメイン名 (IDN) のサポートにより拡張されました。 これらの機能拡張は、.NET Framework 3.5、3.0 SP1、および 2.0 SP1 で使用できます。  
  
## <a name="iri-and-idn-support"></a>IRI と IDN のサポート  
 Web アドレスは通常、次の非常に限られた文字のセットで構成される Uniform Resource Identifier (URI) を使用して表されます。  
  
- 英文字の大文字と小文字の ASCII 文字。  
  
- 0 から 9 の数字。  
  
- その他の少数の ASCII シンボル。  
  
 URI の仕様は、インターネット技術標準化委員会 (IETF) が公開している RFC 2396 および RFC 3986 で規定されています。  
  
 インターネットの成長により、英語以外の言語を使用するリソースを識別する必要性が高まっています。 このニーズに対応し、非 ASCII 文字 (Unicode/ISO 10646 の文字セット内の文字) を許可する識別子が、International Resource Identifier (IRI) として知られています。 IRI の仕様は、IETF によって発行された RFC 3987 で規定されています。 IRI を使用すると、URL に Unicode 文字を含めることができます。  
  
 既存の <xref:System.Uri?displayProperty=nameWithType> クラスは、RFC 3987 に基づいて IRI サポートを提供するために拡張されています。 現在のユーザーは、自分で明確に IRI を有効にしない限り、.NET Framework 2.0 の動作からは変更に気付きません。 これにより、.NET Framework の以前のバージョンとのアプリケーションの互換性を保証します。  
  
 アプリケーションで、ドメイン名に適用する国際ドメイン名 (IDN) 解析を使用するかどうか、および IRI 解析規則を適用すべきかどうかを指定できます。 これは、machine.config ファイルまたは app.config ファイルで指定できます。  
  
 IDN を有効にすると、ドメイン名に含まれるすべての Unicode のラベルが Punycode のラベルに変換されます。 Punycode 名には ASCII 文字のみが含まれ、常に xn-- プレフィックスで始まります。 この理由は、ほとんどの DNS サーバーは ASCII 文字しかサポートしていないため、インターネットで既存の DNS サーバーをサポートするためです (RFC 3940 を参照)。  
  
 IRI と IDN を有効にすると、<xref:System.Uri.DnsSafeHost%2A?displayProperty=nameWithType> プロパティの値に影響します。 IRI と IDN を有効にすると、<xref:System.Uri.Equals%2A?displayProperty=nameWithType>、<xref:System.Uri.OriginalString%2A?displayProperty=nameWithType>、<xref:System.Uri.GetComponents%2A?displayProperty=nameWithType>、および <xref:System.Uri.IsWellFormedOriginalString%2A> メソッドの動作を変更することもできます。  
  
 IRI と IDN をサポートするカスタマイズ可能なパーサーを作成できるように、<xref:System.GenericUriParser?displayProperty=nameWithType> クラスも拡張されています。 <xref:System.GenericUriParser?displayProperty=nameWithType> オブジェクトの動作は、<xref:System.GenericUriParserOptions?displayProperty=nameWithType> 列挙型で使用可能な値のビットごとの組み合わせを <xref:System.GenericUriParser?displayProperty=nameWithType> コンストラクターに渡すことによって指定されます。 <xref:System.GenericUriParserOptions.IriParsing?displayProperty=nameWithType> 型は、パーサーが International Resource Identifiers (IRI) の RFC 3987 で規定された解析規則をサポートしていることを示します。 IRI が実際に使用されるかどうかは、IRI が有効になっているかどうかに依存します。  
  
 <xref:System.GenericUriParserOptions.Idn?displayProperty=nameWithType> 型は、パーサーがホスト名の国際化ドメイン名 (IDN) の解析をサポートしていることを示します。 IDN が実際に使用されるかどうかは、IDN が有効になっているかどうかに依存します。  
  
 IRI 解析を有効にすると、RFC 3987 の最新の IRI 規則に従って、正規化と文字チェックが実行されます。 RFC 2396 および RFC 3986 に従って正規化と文字チェックが実行されるように、既定値では IRI 解析は無効になっています。  
  
 <xref:System.Uri?displayProperty=nameWithType> クラスでの IRI と IDN の処理は、<xref:System.Configuration.IriParsingElement?displayProperty=nameWithType> および <xref:System.Configuration.IdnElement?displayProperty=nameWithType> の構成設定クラスを使用しても制御することができます。 <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType> 設定は、<xref:System.Uri?displayProperty=nameWithType> クラスでの IRI 処理を有効または無効にします。 <xref:System.Configuration.IdnElement?displayProperty=nameWithType> 設定は、<xref:System.Uri> クラスでの IDN 処理を有効または無効にします。 <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType> 設定も間接的に IDN を制御します。 IDN 処理を可能にするためには、IRI 処理を有効にする必要があります。 IRI 処理が無効になっている場合、IDN 処理は既定の設定に設定されます。既定の設定では、互換性のために .NET Framework 2.0 の動作が使用され、IDN 名は使用されません。  
  
 <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType> 構成クラスと <xref:System.Configuration.IdnElement?displayProperty=nameWithType> 構成クラスの構成設定は、最初の <xref:System.Uri?displayProperty=nameWithType> クラスが構築されるときに 1 回、読み取られます。 それ以降の構成設定の変更は無視されます。  
  
## <a name="see-also"></a>参照

- <xref:System.Configuration.IdnElement?displayProperty=nameWithType>
- <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType>
- <xref:System.Uri?displayProperty=nameWithType>
- <xref:System.Uri.DnsSafeHost%2A?displayProperty=nameWithType>
