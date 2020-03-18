---
title: サービス メタデータからの WCF クライアントの生成
ms.date: 03/30/2017
ms.assetid: 27f8f545-cc44-412a-b104-617e0781b803
ms.openlocfilehash: e40a908894ca5acac33401ff20a9110d617547ec
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75963902"
---
# <a name="generating-a-wcf-client-from-service-metadata"></a>サービス メタデータからの WCF クライアントの生成
ここでは、Svcutil.exe の各種のスイッチを使用して、メタデータ ドキュメントからクライアントを生成する方法を説明します。  
  
 メタデータ ドキュメントは、永続ストレージに保存したり、オンラインで取得したりできます。 オンライン取得では、WS-MetadataExchange プロトコルまたは DISCO (Microsoft Discovery) プロトコルに従います。 Svcutil.exe は、メタデータを取得するために次のメタデータ要求を同時に発行します。  
  
- 指定されたアドレスへの WS-MetadataExchange (MEX) 要求  
  
- 指定された `/mex` 付きアドレスへの MEX 要求  
  
- ASP.NET Web サービスからの <xref:System.Web.Services.Discovery.DiscoveryClientProtocol> を使用して、指定されたアドレスに DISCO 要求を行います。  
  
 Svcutil.exe は、Web サービス記述言語 (WSDL: Web Services Description Language) ファイル、またはサービスから受け取ったポリシー ファイルに基づいてクライアントを生成します。 ユーザープリンシパル名 (UPN) は、ユーザー名と "\@" を連結し、完全修飾ドメイン名 (FQDN) を追加することによって生成されます。 ただし、Active Directory に登録したユーザーについては、この形式は無効であり、ツールが生成する UPN によって Kerberos 認証でエラーが発生し、次のエラーメッセージが表示されます。**ログオン試行が失敗しました。** この問題を解決するには、このツールが生成するクライアント ファイルを手動で修正する必要があります。  
  
```console
svcutil.exe [/t:code]  <metadataDocumentPath>* | <url>* | <epr>  
```  
  
## <a name="referencing-and-sharing-types"></a>型の参照と共有  
  
|オプション|説明|  
|------------|-----------------|  
|**/reference:\<ファイルパス >**|指定されたアセンブリの型を参照します。 クライアントの生成時に、このオプションを使用して、インポートするメタデータを表す型を含むアセンブリを指定します。<br /><br /> 短縮形 : `/r`|  
|**/excludeType:\<type>**|参照されるコントラクト型から除外する完全修飾またはアセンブリ修飾の型名を指定します。<br /><br /> 短縮形 : `/et`|  
  
## <a name="choosing-a-serializer"></a>シリアライザーの選択  
  
|オプション|説明|  
|------------|-----------------|  
|**/serializer:Auto**|シリアライザーを自動的に選択します。 これは、`DataContract` シリアライザーを使用します。 この処理が失敗すると、`XmlSerializer` が使用されます。<br /><br /> 短縮形: `/ser:Auto`|  
|**/serializer:DataContractSerializer**|シリアル化と逆シリアル化に `DataContract` シリアライザーを使用するデータ型を生成します。<br /><br /> 短縮形 : `/ser:DataContractSerializer`|  
|**/serializer:XmlSerializer**|シリアル化と逆シリアル化に `XmlSerializer` を使用するデータ型を生成します。<br /><br /> 短縮形 : `/ser:XmlSerializer`|  
|**/importXmlTypes**|`DataContract` 型として非 `DataContract` 型をインポートする `IXmlSerializable` シリアライザーを構成します。<br /><br /> 短縮形 : `/ixt`|  
|**/dataContractOnly**|`DataContract` 型に対してのみコードを生成します。 `ServiceContract` 型が生成されます。<br /><br /> このオプションにはローカル メタデータ ファイルだけを指定する必要があります。<br /><br /> 短縮形 : `/dconly`|  
  
## <a name="choosing-a-language-for-the-client"></a>クライアントの言語の選択  
  
|オプション|説明|  
|------------|-----------------|  
|**/言語:\<言語 >**|コード生成に使用するプログラミング言語を指定します。 Machine.config ファイルに登録された言語名か、<xref:System.CodeDom.Compiler.CodeDomProvider> から継承するクラスの完全修飾名のいずれかを指定します。<br /><br /> 値は、c#、cs、csharp、vb、vbs、visualbasic、vbscript、javascript、c++、mc、cpp になります。<br /><br /> 既定値: csharp<br /><br /> 短縮形 : `/l`<br /><br /> 詳細については、<xref:System.CodeDom.Compiler.CodeDomProvider> クラスを参照してください。|  
  
## <a name="choosing-a-namespace-for-the-client"></a>クライアントの名前空間の選択  
  
|オプション|説明|  
|------------|-----------------|  
|**/namespace:\<string,string>**|WSDL または XML スキーマの `targetNamespace` から共通言語ランタイム (CLR: Common Language Runtime) 名前空間へのマッピングを指定します。 `targetNamespace` にワイルドカード (*) を使用すると、マッピングを明示的に指定せずにすべての `targetNamespaces` がその CLR 名前空間にマップされます。<br /><br /> メッセージ コントラクト名が操作名と競合しないようにするには、型参照を 2 つのコロン `::` で修飾するか、名前を一意にします。<br /><br /> 既定 : `DataContracts` のスキーマ ドキュメントのターゲット名前空間から派生します。 既定の名前空間は、生成される他のすべての型に使用されます。<br /><br /> 短縮形 : `/n`|  
  
## <a name="choosing-a-data-binding"></a>データ バインディングの選択  
  
|オプション|説明|  
|------------|-----------------|  
|**/enableDataBinding**|データ バインディングを有効にするために、すべての <xref:System.ComponentModel.INotifyPropertyChanged> 型に `DataContract` インターフェイスを実装します。<br /><br /> 短縮形 : `/edb`|  
  
## <a name="generating-configuration"></a>構成ファイルの生成  
  
|オプション|説明|  
|------------|-----------------|  
|**/config:\<configFile>**|生成される構成ファイルの名前を指定します。<br /><br /> 既定値: output.config|  
|**/mergeConfig**|既存のファイルを上書きする代わりに、生成される構成ファイルを既存のファイルにマージします。|  
|**/noConfig**|構成ファイルを生成しません。|  
  
## <a name="see-also"></a>関連項目

- [メタデータを使用する](../../../../docs/framework/wcf/feature-details/using-metadata.md)
- [メタデータ アーキテクチャの概要](../../../../docs/framework/wcf/feature-details/metadata-architecture-overview.md)
