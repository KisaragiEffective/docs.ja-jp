---
title: Web サービスプロトコルの相互運用性ガイド
ms.date: 03/30/2017
ms.assetid: f2981678-ebdb-433d-899b-467f7df95fb2
ms.openlocfilehash: 1b949880b3ebbaf121b79a958d17cf5708affcf3
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636147"
---
# <a name="web-services-protocols-interoperability-guide"></a>Web サービスプロトコルの相互運用性ガイド

Windows Communication Foundation (WCF) は、多くの Web サービスプロトコルを実装します。 これらのプロトコルの多くには、さまざまなオプションと拡張ポイントが用意されており、それらの実装は実装者の裁量に任されています。 このトピックでは、WCF が実装する Web サービスプロトコルの一覧を示します。 サポートされる各プロトコルの実装の詳細については、このセクションの他のトピックで説明します。

## <a name="web-services-protocols-implemented-by-wcf"></a>WCF によって実装された Web サービスプロトコル

WCF では、コントラクト機能を通じて、チャネルと Web サービスアプリケーションプロトコルを介して Web サービス (WS) インフラストラクチャプロトコルがサポートされます。 アプリケーション プロトコルの相互運用性は、XML スキーマ記述言語 (XSD) 1.0 と Web サービス記述言語 (WSDL) 1.1 を通じて実現されます。

インフラストラクチャ プロトコルの相互運用性は、WS-* 仕様によって提供されます。 WCF チャネルは、さまざまな\* WS-MANAGEMENT プロトコルのサポートを提供します。 WCF チャネルは、バインド要素を使用して構成されます。 次の表には、さまざまな WCF バインド要素によって実装される\* WS-POLICY インフラストラクチャプロトコルの完全な一覧が含まれています。

<xref:System.ServiceModel.Channels.HttpTransportBindingElement> は、次の表の仕様をサポートします。

|仕様/ドキュメント|Link|
|-----------------------------|----------|
|HTTP 1.1|[RFC 2616](https://www.rfc-editor.org/rfc/rfc2616.txt)|
|SOAP 1.1 HTTP バインディング|[Simple Object Access Protocol (SOAP) 1.1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/)、セクション7|
|SOAP 1.2 HTTP バインディング|[SOAP バージョン1.2 パート 2: Adjuncts (Second Edition)](https://www.w3.org/TR/soap12-part2/)、セクション7|

<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> および <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> は、次の表の仕様をサポートします。

|仕様/ドキュメント|Link|
|-----------------------------|----------|
|XML|[拡張マークアップ言語 (XML) 1.0 (第4版)](https://www.w3.org/TR/REC-xml/)|
|SOAP 1.1|[Simple Object Access Protocol (SOAP) 1.1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/)|
|SOAP 1.2 コア|[SOAP Version 1.2 パート 1: Messaging Framework (Second Edition)](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)|
|WS-Addressing 2004/08|[Web サービスのアドレス指定 (WS-ADDRESSING)](https://www.w3.org/Submission/2004/SUBM-ws-addressing-20040810/)|
|W3C Web Services Addressing 1.0 - コア|[Web サービスアドレス指定 1.0-コア](https://www.w3.org/TR/2006/REC-ws-addr-core-20060509/)|
|W3C Web Services Addressing 1.0 - SOAP バインディング|[Web サービスアドレス指定 1.0-SOAP バインド](https://www.w3.org/TR/2006/REC-ws-addr-soap-20060509/)|
|W3C Web Services Addressing 1.0 - WSDL バインディング*|[Web サービスアドレス指定 1.0-WSDL バインド](https://www.w3.org/TR/2006/CR-ws-addr-wsdl-20060529/)|
|W3C Web Services Addressing 1.0 - メタデータ|[Web サービスアドレス指定 1.0-メタデータ](https://www.w3.org/TR/ws-addr-metadata/)|
|WSDL SOAP1.1 バインディング|[Web サービス記述言語 (WSDL) 1.1](https://www.w3.org/TR/wsdl/)|
|WSDL SOAP1.2 バインディング|[SOAP 1.2 の WSDL 1.1 バインド拡張](https://www.w3.org/Submission/wsdl11soap12/)|

<xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> は、次の表の仕様をサポートします。

|仕様/ドキュメント|Link|
|-----------------------------|----------|
|XOP|[XML-バイナリ最適化パッケージ](https://www.w3.org/TR/xop10/)|
|MTOM + SOAP1.2 バインディング|[SOAP メッセージ転送の最適化メカニズム](https://www.w3.org/TR/soap12-mtom/)|
|MTOM SOAP 1.1 バインディング|[MTOM 1.0 の SOAP 1.1 バインド](https://www.w3.org/Submission/soap11mtom10/)|
|MTOM WS-Policy アサーション|[MTOM シリアル化ポリシーアサーション (MTOMPolicy)](https://www.w3.org/Submission/WS-MTOMPolicy/)|

<xref:System.ServiceModel.Channels.SecurityBindingElement> は、次の表の仕様をサポートします。

|仕様/ドキュメント|Link|
|-----------------------------|----------|
|WSS SOAP Message Security 1.0|[Web Services Security: SOAP メッセージセキュリティ1.0](https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf)|
|WSS: Username Token Profile 1.0|[Web Services Security UsernameToken Profile 1.0](https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0.pdf)<br /><br /> 必須 Password/@Type= PasswordText (既定値)|
|WSS: X.509 Token Profile 1.0|[Web Services Security x.509 証明書トークンプロファイル](https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-x509-token-profile-1.0.pdf)|
|WSS: SAML 1.1 Token Profile 1.0|[Web Services Security: SAML トークンプロファイル](https://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.0.pdf)|
|WSS SOAP Message Security 1.1|[Web Services Security: SOAP メッセージセキュリティ1.1](https://www.oasis-open.org/committees/download.php/16790/wss-v1.1-spec-os-SOAPMessageSecurity.pdf)|
|WSS Username Token Profile 1.1|[Web Services Security UsernameToken Profile 1.1](https://www.oasis-open.org/committees/download.php/16782/wss-v1.1-spec-os-UsernameTokenProfile.pdf)<br /><br /> パスワード ベースのキー派生は実装していません。<br /><br /> 必須 Password/@Type= PasswordText (既定値)|
|WSS: X509 Token Profile 1.1|[Web Services Security x.509 証明書トークンプロファイル1.1](https://www.oasis-open.org/committees/download.php/16785/wss-v1.1-spec-os-x509TokenProfile.pdf)|
|WSS: Kerberos Token Profile 1.1|[Web Services Security Kerberos トークンプロファイル1.1](https://www.oasis-open.org/committees/download.php/16788/wss-v1.1-spec-os-KerberosTokenProfile.pdf)|
|WSS: SAML 1.1 Token Profile 1.1|[Web Services Security SAML トークンプロファイル1.1](https://www.oasis-open.org/committees/download.php/16768/wss-v1.1-spec-os-SAMLTokenProfile.pdf)|
|WS-SecureConversation|[Web サービスのセキュリティで保護されたメッセージ交換の言語](https://specs.xmlsoap.org/ws/2005/02/sc/ws-secureconversation.pdf)|
|WS-Trust 1.4|[Web サービスの信頼の言語](https://docs.oasis-open.org/ws-sx/ws-trust/200802)|
|WS-SecurityPolicy 2005/07|[Web サービスのセキュリティで保護されたメッセージ交換の言語](https://specs.xmlsoap.org/ws/2005/02/sc/ws-secureconversation.pdf)<br /><br /> OASIS WS-SX 技術委員会に提出された正誤表で修正されています。<br /><br /> [ws-policy メッセージ](https://lists.oasis-open.org/archives/ws-sx/200512/msg00017.html)|
|WS-ReliableMessaging 1.1|[信頼できるメッセージング プロトコル バージョン 1.1](reliable-messaging-protocol-version-1-1.md)|

<xref:System.ServiceModel.Channels.TransactionFlowBindingElement> は、次の表の仕様をサポートします。

|仕様/ドキュメント|Link|
|-----------------------------|----------|
|WS-Coordination|[Web サービスの調整](https://docs.microsoft.com/previous-versions/ms951231(v=msdn.10))|
|WS-AtomicTransaction|[Web サービスのアトミックトランザクション](https://specs.xmlsoap.org/ws/2004/10/wsat/wsat.pdf)|

<xref:System.ServiceModel.Description.MetadataExporter>、<xref:System.ServiceModel.Description.MetadataImporter>、<xref:System.ServiceModel.Description.WsdlExporter>、<xref:System.ServiceModel.Description.WsdlImporter>、および <xref:System.ServiceModel.Description.MetadataResolver> の各クラスは、次のメタデータ仕様をサポートします。

- [XML スキーマパート 1: 2 番目のエディション](https://www.w3.org/TR/xmlschema-1/)

- [XML スキーマ第2部: データ型2番目のエディション](https://www.w3.org/TR/xmlschema-2/)

- [WSDL 1.1](https://www.w3.org/TR/wsdl/)

- [WS-POLICY 1.2](https://www.w3.org/Submission/2006/SUBM-WS-Policy-20060425/)

- [WS-POLICY 1.5](https://www.w3.org/TR/ws-policy/)

- [WS-POLICY 添付ファイル1.2](https://www.w3.org/Submission/2006/SUBM-WS-PolicyAttachment-20060425/)

- [WS-MetadataExchange 1.1](https://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)

- [メタデータの取得のための WS 転送の取得](https://www.w3.org/Submission/2006/SUBM-WS-Transfer-20060315/)

さらに、次の相互運用性プロファイルが WCF 全体に実装されます。

- [基本プロファイル1.1](http://www.ws-i.org/Profiles/BasicProfile-1.1-2004-08-24.html)

- [単純な SOAP バインディング1.0](http://www.ws-i.org/Profiles/SimpleSoapBindingProfile-1.0-2004-08-24.html)

- [基本セキュリティプロファイル1.0 作業ドラフト](http://www.ws-i.org/Profiles/BasicSecurityProfile-1.0-2006-03-29.html)

## <a name="see-also"></a>関連項目

- [システム標準の相互運用性バインディングがサポートしている Web サービス プロトコル](web-services-protocols-supported-by-system-provided-interoperability-bindings.md)
- [メッセージング プロトコル](messaging-protocols.md)
- [データ コントラクト スキーマの参照](data-contract-schema-reference.md)
- [WSDL とポリシー](wsdl-and-policy.md)
- [セキュリティ プロトコル](security-protocols.md)
- [信頼できるメッセージング プロトコル バージョン 1.0](reliable-messaging-protocol-version-1-0.md)
- [信頼できるメッセージング プロトコル バージョン 1.1](reliable-messaging-protocol-version-1-1.md)
- [トランザクション プロトコル](transaction-protocols.md)
- [コンテキスト交換プロトコル](context-exchange-protocol.md)
