---
description: '詳細情報: 方法: カスタム ポリシー アサーションをインポートする'
title: '方法: カスタム ポリシー アサーションをインポートする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1f41d787-accb-4a10-bfc6-a807671d1581
ms.openlocfilehash: e9190a97ed7d2d369b8f9ddc3d7fec71daf1e54f
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99644334"
---
# <a name="how-to-import-custom-policy-assertions"></a>方法: カスタム ポリシー アサーションをインポートする

ポリシー アサーションはサービス エンドポイントの機能と要件を説明します。  クライアント アプリケーションはサービス メタデータにあるポリシー アサーションを使用して、クライアント バインディングを構成したり、サービス エンドポイントのサービス コントラクトをカスタマイズしたりできます。  
  
 カスタム ポリシー アサーションは、<xref:System.ServiceModel.Description.IPolicyImportExtension?displayProperty=nameWithType> インターフェイスを実装して、このオブジェクトをメタデータ システムに渡すか、またはアプリケーション構成ファイルに実装型を登録することによってインポートします。  <xref:System.ServiceModel.Description.IPolicyImportExtension> インターフェイスの実装は、パラメーターなしのコンストラクターを提供する必要があります。  
  
### <a name="to-import-custom-policy-assertions"></a>カスタム ポリシー アサーションをインポートするには  
  
1. クラスに <xref:System.ServiceModel.Description.IPolicyImportExtension?displayProperty=nameWithType> インターフェイスを実装します。 次の手順を参照してください。  
  
2. 次のいずれかの方法でカスタム ポリシー インポーターを挿入します。  
  
3. 構成ファイルを使用します。 次の手順を参照してください。  
  
4. [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) で構成ファイルを使用します。 次の手順を参照してください。  
  
5. プログラムでポリシー インポーターを挿入します。 次の手順を参照してください。  
  
### <a name="to-implement-the-systemservicemodeldescriptionipolicyimportextension-interface-on-any-class"></a>任意のクラスに System.ServiceModel.Description.IPolicyImportExtension インターフェイスを実装するには  
  
1. <xref:System.ServiceModel.Description.IPolicyImportExtension.ImportPolicy%2A?displayProperty=nameWithType> メソッドで、対象となる各ポリシーについて、メソッドに渡された <xref:System.ServiceModel.Description.PolicyConversionContext?displayProperty=nameWithType> オブジェクトで (必要となるアサーションのスコープに応じて) 適切なメソッドを呼び出すことにより、インポートする必要があるポリシー アサーションを見つけます。 次のコード例では、<xref:System.ServiceModel.Description.PolicyAssertionCollection.Remove%2A?displayProperty=nameWithType> メソッドを使用して、一度にカスタム ポリシー アサーションを見つけてコレクションから削除する方法を示します。 remove メソッドを使用してアサーションの検索と削除を行う場合は、手順 4. を実行する必要はありません。  
  
     [!code-csharp[CustomPolicySample#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/custompolicysample/cs/policyimporter.cs#9)]
     [!code-vb[CustomPolicySample#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/custompolicysample/vb/policyimporter.vb#9)]  
  
2. ポリシー アサーションを処理します。 ポリシー システムでは、入れ子になったポリシーと `wsp:optional` を正規化しないので注意してください。 これらの構造体については、ポリシー インポート拡張機能の実装で処理する必要があります。  
  
3. ポリシー アサーションで指定されている機能または要件をサポートするバインディングまたはコントラクトのカスタマイズを実行します。 アサーションでは、通常、バインディングに特定の構成、または特定のバインド要素が必要です。 <xref:System.ServiceModel.Description.PolicyConversionContext.BindingElements%2A?displayProperty=nameWithType> プロパティにアクセスすることで、これらの変更を実行します。 これとは別に、コントラクトの変更が必要なアサーションがあります。  コントラクトへのアクセスと変更には、<xref:System.ServiceModel.Description.PolicyConversionContext.Contract%2A?displayProperty=nameWithType> プロパティを使用します。  ポリシー代替手段のインポートに失敗した場合、バインディングとコントラクトは同じなのに、ポリシー代替手段が異なるために、ポリシー インポーターが複数回呼び出されることがあるので注意してください。 作成するコードでは、この動作に対応する必要があります。  
  
4. アサーション コレクションからカスタム ポリシー アサーションを削除します。 アサーションを削除しない場合、Windows Communication Foundation (WCF) は、ポリシーのインポートが失敗して、関連付けられているバインディングがインポートされていないと見なします。 <xref:System.ServiceModel.Description.PolicyAssertionCollection.Remove%2A?displayProperty=nameWithType> メソッドを使用して、カスタム ポリシー アサーションの検索とコレクションからの削除を一度に行う場合は、この手順を実行する必要はありません。  
  
### <a name="to-insert-the-custom-policy-importer-into-the-metadata-system-using-a-configuration-file"></a>構成ファイルを使用してメタデータ システムにカスタム ポリシー インポーターを挿入するには  
  
1. クライアント構成ファイルの [\<policyImporters>](../../configure-apps/file-schema/wcf/policyimporters.md) 要素の中にある `<extensions>` 要素にインポーター型を追加します。  
  
     [!code-xml[CustomPolicySample#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/custompolicysample/cs/client.exe.config#7)]
  
2. クライアント アプリケーションで、<xref:System.ServiceModel.Description.MetadataResolver?displayProperty=nameWithType> または <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> を使用してメタデータを解決すると、インポーターが自動的に呼び出されます。  
  
     [!code-csharp[CustomPolicySample#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/custompolicysample/cs/client.cs#10)]
     [!code-vb[CustomPolicySample#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/custompolicysample/vb/client.vb#10)]  
  
### <a name="to-insert-the-custom-policy-importer-into-the-metadata-system-using-svcutilexe"></a>Svcutil.exe を使用してメタデータ システムにカスタム ポリシー インポーターを挿入するには  
  
1. Svcutil.exe.config 構成ファイルの [\<policyImporters>](../../configure-apps/file-schema/wcf/policyimporters.md) 要素の中にある `<extensions>` 要素にインポーター型を追加します。 また、`/svcutilConfig` オプションを使用して、異なる構成ファイルに登録されているポリシー インポーター型を読み込むように Svcutil.exe を指定することもできます。  
  
2. [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) を使用してメタデータをインポートすると、インポーターが自動的に呼び出されます。  
  
### <a name="to-insert-the-custom-policy-importer-into-the-metadata-system-programmatically"></a>プログラム使用してメタデータ システムにカスタム ポリシー インポーターを挿入するには  
  
1. メタデータをインポートする前に、インポーターを <xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A?displayProperty=nameWithType> プロパティに追加します (たとえば、<xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> を使用している場合)。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Description.MetadataResolver?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MetadataResolver?displayProperty=nameWithType>
- [メタデータ システムの拡張](extending-the-metadata-system.md)
