---
description: '詳細については、「方法: サービスエンドポイントからメタデータをエクスポートする」を参照してください。'
title: '方法: メタデータをサービス エンドポイントからエクスポートする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b6c4dfd0-f270-43ec-961a-e16eb6af2f2c
ms.openlocfilehash: f690d2dc0bd3ac0f89e527412a63ce0967daa8f6
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99802880"
---
# <a name="how-to-export-metadata-from-service-endpoints"></a>方法: メタデータをサービス エンドポイントからエクスポートする

このトピックでは、メタデータをサービス エンドポイントからエクスポートする方法について説明します。  
  
### <a name="to-export-metadata-from-service-endpoints"></a>メタデータをサービス エンドポイントからエクスポートするには  
  
1. 新しい Visual Studio コンソール アプリケーション プロジェクトを作成します。 以下の手順で示されているコードを、生成された Program.cs ファイルの main() メソッド内に追加します。  
  
2. <xref:System.ServiceModel.Description.WsdlExporter> を作成します。  
  
     [!code-csharp[S_UEWsdlExporter#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#1)]
     [!code-vb[S_UEWsdlExporter#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#1)]  
  
3. <xref:System.ServiceModel.Description.MetadataExporter.PolicyVersion%2A> プロパティを <xref:System.ServiceModel.Description.PolicyVersion> 列挙体のいずれかの値に設定します。 この例では、値を、WS-Policy 1.5 に対応する <xref:System.ServiceModel.Description.PolicyVersion.Policy15%2A> に設定します。  
  
     [!code-csharp[S_UEWsdlExporter#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#2)]
     [!code-vb[S_UEWsdlExporter#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#2)]  
  
4. <xref:System.ServiceModel.Description.ServiceEndpoint> オブジェクトの配列を作成します。  
  
     [!code-csharp[S_UEWsdlExporter#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#3)]
     [!code-vb[S_UEWsdlExporter#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#3)]  
  
5. サービス エンドポイントのメタデータをエクスポートします。  
  
     [!code-csharp[S_UEWsdlExporter#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#4)]
     [!code-vb[S_UEWsdlExporter#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#4)]  
  
6. エクスポート プロセス中にエラーが発生していないことを確認し、メタデータを取得します。  
  
     [!code-csharp[S_UEWsdlExporter#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#5)]
     [!code-vb[S_UEWsdlExporter#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#5)]  
  
7. これで、メタデータを使用できます。たとえば、<xref:System.ServiceModel.Description.MetadataSet.WriteTo%28System.Xml.XmlWriter%29> メソッドを呼び出してメタデータをファイルに書き込むことができます。  
  
## <a name="example"></a>例  

 この例の完全なコードの一覧を以下に示します。  
  
 [!code-csharp[S_UEWsdlExporter#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#0)]
 [!code-vb[S_UEWsdlExporter#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#0)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  

 Program.cs をコンパイルするときは、System.ServiceModel.dll への参照を追加してください。  
  
## <a name="see-also"></a>関連項目

- [メタデータ アーキテクチャの概要](metadata-architecture-overview.md)
- [メタデータを使用する](using-metadata.md)
- [エンドポイント:アドレス、バインディング、およびコントラクト](endpoints-addresses-bindings-and-contracts.md)
