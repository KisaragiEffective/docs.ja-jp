---
title: WCF Data Service クライアント ユーティリティ (DataSvcUtil.exe)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, generating client data classes
- WCF Data Services, client library
- WCF Data Services, consuming
ms.assetid: 9d0af606-929b-4c03-b307-3ef5f705afce
ms.openlocfilehash: 1348ba73eb87a140b42e3565b4388a70f1f47ca1
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802007"
---
# <a name="wcf-data-service-client-utility-datasvcutilexe"></a>WCF Data Service クライアント ユーティリティ (DataSvcUtil.exe)

Datasvcutil.exe は、Open Data Protocol (OData) フィードを使用し、.NET Framework クライアントアプリケーションからデータサービスにアクセスするために必要なクライアントデータサービスクラスを生成する WCF Data Services によって提供されるコマンドラインツールです。 このユーティリティでは、以下のメタデータ ソースを使用してデータ クラスを生成できます。

- データ サービスのルート URI。 ユーティリティは、データ サービスによって公開されるデータ モデルが記述されたサービス メタデータ ドキュメントを要求します。 詳細については、「 [AtomPub (RFC5023)](https://tools.ietf.org/html/rfc5023#section-8)」を参照してください。

- [\[MC-csdl\]: 概念スキーマ定義ファイル形式](https://docs.microsoft.com/openspecs/windows_protocols/mc-csdl/c03ad8c3-e8b7-4306-af96-a9e52bb3df12)の仕様で定義されているように、概念スキーマ定義言語 (csdl) を使用して定義されるデータモデルファイル (.csdl)。

- Entity Framework に付属する Entity Data Model ツールを使用して作成された .edmx ファイル。 詳細については、「 [\[MC-EDMX\]: Entity Data Model Data Services パッケージング形式の](https://docs.microsoft.com/openspecs/windows_protocols/mc-edmx/5dff5e25-56a1-408b-9d44-bff6634c7d16)仕様」を参照してください。

詳細については、「[方法: クライアントデータサービスクラスを手動で生成する](how-to-manually-generate-client-data-service-classes-wcf-data-services.md)」を参照してください。

Datasvcutil.exe ツールは、.NET Framework ディレクトリにインストールされます。 多くの場合、これは*c:\windows\ microsoft.net \framework\v4.0*にあります。 64ビットシステムの場合は、 *C:\Windows\Microsoft.NET\Framework64\v4.0*にあります。 開発者コマンドプロンプトの Visual Studio では、Datasvcutil.exe ツールにアクセスすることもできます。

## <a name="syntax"></a>構文

```console
datasvcutil /out:file [/in:file | /uri:serviceuri] [/dataservicecollection] [/language:devlang] [/nologo] [/version:ver] [/help]
```

## <a name="parameters"></a>パラメーター

|オプション|説明|
|------------|-----------------|
|`/dataservicecollection`|オブジェクトをコントロールにバインドするために必要なコードも生成することを指定します。|
|`/help`<br /><br /> -または-<br /><br /> `/?`|このツールのコマンド構文とオプションを表示します。|
|`/in:` *\<ファイル >*|.csdl ファイルまたは .edmx ファイル、またはファイルがあるディレクトリを指定します。|
|`/language:`[VB&#124;CSharp]|生成されるソース コード ファイルの言語を指定します。 既定の言語は C# です。|
|`/nologo`|著作権メッセージが表示されないようにします。|
|`/out:` *\<ファイル >*|生成されたクライアント データ サービス クラスを含むソース コード ファイルの名前を指定します。|
|`/uri:` *\<文字列 >*|OData フィードの URI。|
|`/version:`[1.0&#124;2.0]|OData の許容される最大バージョンを指定します。 バージョンは、返されるデータサービスメタデータ内の DataService 要素の `DataServiceVersion` 属性に基づいて決定されます。 詳細については、「[データサービスのバージョン管理](data-service-versioning-wcf-data-services.md)」を参照してください。 `/dataservicecollection` パラメーターを指定する場合は、データバインディングを有効にする `/version:2.0` も指定する必要があります。|

## <a name="see-also"></a>参照

- [データ サービス クライアント ライブラリの生成](generating-the-data-service-client-library-wcf-data-services.md)
- [方法: データ サービス参照を追加する](how-to-add-a-data-service-reference-wcf-data-services.md)
