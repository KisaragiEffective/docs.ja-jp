---
title: データ サービス クライアント ライブラリの生成 (WCF Data Services)
ms.date: 03/30/2017
helpviewer_keywords:
- client applications, WCF Data Services
- WCF Data Services, client library
- Add Service Reference dialog box
ms.assetid: 314077c1-ac10-47e1-bed4-940b5462359d
ms.openlocfilehash: b938e419a5a650fe0e24445c44a67aead13349fa
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348105"
---
# <a name="generating-the-data-service-client-library-wcf-data-services"></a>データ サービス クライアント ライブラリの生成 (WCF Data Services)
Open Data Protocol (OData) を実装するデータサービスは、OData フィードによって公開されるデータモデルを記述したサービスメタデータドキュメントを返すことができます。 詳細については、 [OData: 概要](https://www.odata.org/documentation/odata-version-2-0/overview/)に関する記事の「サービスメタデータドキュメント」セクションを参照してください。 Visual Studio の **[サービス参照の追加]** ダイアログボックスを使用して、OData ベースのサービスへの参照を追加できます。 このツールを使用して、クライアントプロジェクトの OData フィードによって返されるメタデータへの参照を追加すると、次のアクションが実行されます。  
  
- データ サービスからのサービス メタデータ ドキュメントが要求され、返されたメタデータが解釈されます。  
  
    > [!NOTE]
    > 返されたメタデータは、クライアント プロジェクトに .edmx ファイルとして保存されます。 .edmx ファイルは、Entity Framework で使用される .edmx ファイルと形式が異なるので、Entity Data Model デザイナーを使用して開くことができません。 このメタデータ ファイルは、XML エディターまたは任意のテキスト エディターを使用して表示できます。 詳細については、「 [\[MC-EDMX\]: Entity Data Model Data Services パッケージング形式](https://docs.microsoft.com/openspecs/windows_protocols/mc-edmx/5dff5e25-56a1-408b-9d44-bff6634c7d16)」を参照してください。
  
- <xref:System.Data.Services.Client.DataServiceContext> から継承するエンティティ コンテナー クラスとしてサービスの表現が生成されます。 この生成されたエンティティ コンテナー クラスは、Entity Data Model ツールが生成するエンティティ コンテナーに似ています。 詳細は、[Object Services の概要 (Entity Framework)](https://docs.microsoft.com/previous-versions/bb386871(v=vs.100)) をご覧ください。  
  
- サービス メタデータ内で見つかったデータ モデル型のデータ クラスが生成されます。  
  
- `System.Data.Services.Client` アセンブリへの参照がプロジェクトに追加されます。  
  
 詳細については、「[方法: データサービス参照を追加する](how-to-add-a-data-service-reference-wcf-data-services.md)」を参照してください。  
  
 クライアントデータサービスクラスは、コマンドプロンプトで[datasvcutil.exe](wcf-data-service-client-utility-datasvcutil-exe.md)ツールを使用して生成することもできます。 詳細については、「[方法: クライアントデータサービスクラスを手動で生成する](how-to-manually-generate-client-data-service-classes-wcf-data-services.md)」を参照してください。  
  
## <a name="client-data-type-mapping"></a>クライアント データ型のマッピング  
 Visual Studio の **[サービス参照の追加]** ダイアログまたは `DataSvcUtil.exe` ツールを使用して、OData フィードに基づくクライアントデータクラスを生成する場合、.NET Framework のデータ型は、次のようにデータモデルのプリミティブ型にマップされます。  
  
|データ モデル型|.NET Framework データ型|  
|---------------------|------------------------------|  
|`Edm.Binary`|<xref:System.Byte> `[]`|  
|`Edm.Boolean`|<xref:System.Boolean>|  
|`Edm.Byte`|<xref:System.Byte>|  
|`Edm.DateTime`|<xref:System.DateTime>|  
|`Edm.Decimal`|<xref:System.Decimal>|  
|`Edm.Double`|<xref:System.Double>|  
|`Edm.Guid`|<xref:System.Guid>|  
|`Edm.Int16`|<xref:System.Int16>|  
|`Edm.Int32`|<xref:System.Int32>|  
|`Edm.Int64`|<xref:System.Int64>|  
|`Edm.SByte`|<xref:System.SByte>|  
|`Edm.Single`|<xref:System.Single>|  
|`Edm.String`|<xref:System.String>|  
  
 詳細については、 [OData: 概要](https://www.odata.org/documentation/odata-version-2-0/overview/)に関する記事のプリミティブデータ型に関するセクションを参照してください。
  
## <a name="see-also"></a>関連項目

- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
- [クイック スタート](quickstart-wcf-data-services.md)
