---
description: '詳細情報: 方法: ServiceContractGenerator の拡張を記述する'
title: '方法: ServiceContractGenerator の拡張を記述する'
ms.date: 03/30/2017
ms.assetid: 876ca823-bd16-4bdf-9e0f-02092df90e51
ms.openlocfilehash: 29d6d65cc3f9d29009f72b9a0c81b6cb1f472ac9
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99644204"
---
# <a name="how-to-write-an-extension-for-the-servicecontractgenerator"></a>方法: ServiceContractGenerator の拡張を記述する

このトピックでは、<xref:System.ServiceModel.Description.ServiceContractGenerator> の拡張を記述する方法について説明します。 これは、操作の動作に <xref:System.ServiceModel.Description.IOperationContractGenerationExtension> インターフェイスを実装するか、コントラクトの動作に <xref:System.ServiceModel.Description.IServiceContractGenerationExtension> インターフェイスを実装することで可能になります。 このトピックでは、<xref:System.ServiceModel.Description.IServiceContractGenerationExtension> インターフェイスをコントラクト動作に実装する方法を説明します。  
  
 <xref:System.ServiceModel.Description.ServiceContractGenerator> は、サービス コントラクト、クライアント型、およびクライアント構成を <xref:System.ServiceModel.Description.ServiceEndpoint>、<xref:System.ServiceModel.Description.ContractDescription>、<xref:System.ServiceModel.Channels.Binding> の各インターフェイスから生成します。 通常は、サービス メタデータから <xref:System.ServiceModel.Description.ServiceEndpoint>、<xref:System.ServiceModel.Description.ContractDescription>、および <xref:System.ServiceModel.Channels.Binding> インスタンスをインポートし、これらのインスタンスを使用してサービスを呼び出すコードを生成します。 この例では、<xref:System.ServiceModel.Description.IWsdlImportExtension> の実装を使用して WSDL 注釈を処理し、生成されたコードに関するコメントを生成するために、インポートしたコントラクトにコード生成拡張を追加します。  
  
### <a name="to-write-an-extension-for-the-servicecontractgenerator"></a>ServiceContractGenerator の拡張を記述するには  
  
1. <xref:System.ServiceModel.Description.IServiceContractGenerationExtension>を実装します。 生成されたサービス コントラクトを変更するには、<xref:System.ServiceModel.Description.ServiceContractGenerationContext> メソッドに渡された <xref:System.ServiceModel.Description.IServiceContractGenerationExtension.GenerateContract%28System.ServiceModel.Description.ServiceContractGenerationContext%29> インスタンスを使用します。  
  
    ```csharp
    public void GenerateContract(ServiceContractGenerationContext context)  
    {  
        Console.WriteLine("In generate contract.");  
        context.ContractType.Comments.AddRange(Formatter.FormatComments(commentText));  
    }  
    ```  
  
2. 同じクラスに <xref:System.ServiceModel.Description.IWsdlImportExtension> を実装します。 <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportContract%28System.ServiceModel.Description.WsdlImporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> メソッドは、インポートされた <xref:System.ServiceModel.Description.ContractDescription> インスタンスにコード生成拡張を追加することによって、特定の WSDL 拡張 (この場合は WSDL 注釈) を処理できます。  
  
    ```csharp
    public void ImportContract(WsdlImporter importer, WsdlContractConversionContext context)
    {
        // Contract documentation
        if (context.WsdlPortType.Documentation != null)
        {
            context.Contract.Behaviors.Add(new WsdlDocumentationImporter(context.WsdlPortType.Documentation));
        }
        // Operation documentation
        foreach (Operation operation in context.WsdlPortType.Operations)
        {
            if (operation.Documentation != null)
            {
                OperationDescription operationDescription = context.Contract.Operations.Find(operation.Name);
                if (operationDescription != null)
                {
                    operationDescription.Behaviors.Add(new WsdlDocumentationImporter(operation.Documentation));
                }
            }
        }
    }
    public void BeforeImport(ServiceDescriptionCollection wsdlDocuments, XmlSchemaSet xmlSchemas, ICollection<XmlElement> policy)
    {
        Console.WriteLine("BeforeImport called.");
    }

    public void ImportEndpoint(WsdlImporter importer, WsdlEndpointConversionContext context)
    {
        Console.WriteLine("ImportEndpoint called.");
    }
    ```  
  
3. クライアント構成に WSDL インポーターを追加します。  
  
    ```xml  
    <metadata>  
      <wsdlImporters>  
        <extension type="Microsoft.WCF.Documentation.WsdlDocumentationImporter, WsdlDocumentation" />  
      </wsdlImporters>  
    </metadata>  
    ```  
  
4. クライアント コードで、`MetadataExchangeClient` を作成して `GetMetadata` を呼び出します。  
  
    ```csharp
    var mexClient = new MetadataExchangeClient(metadataAddress);  
    mexClient.ResolveMetadataReferences = true;  
    MetadataSet metaDocs = mexClient.GetMetadata();  
    ```  
  
5. `WsdlImporter` を作成して `ImportAllContracts` を呼び出します。  
  
    ```csharp
    var importer = new WsdlImporter(metaDocs);
    System.Collections.ObjectModel.Collection<ContractDescription> contracts = importer.ImportAllContracts();  
    ```  
  
6. `ServiceContractGenerator` を作成して、コントラクトごとに `GenerateServiceContractType` を呼び出します。  
  
    ```csharp
    var generator = new ServiceContractGenerator();  
    foreach (ContractDescription contract in contracts)  
    {  
       generator.GenerateServiceContractType(contract);  
    }  
    if (generator.Errors.Count != 0)  
       throw new Exception("There were errors during code compilation.");  
    ```  
  
7. <xref:System.ServiceModel.Description.IServiceContractGenerationExtension.GenerateContract%28System.ServiceModel.Description.ServiceContractGenerationContext%29> を実装する特定のコントラクトのコントラクト動作ごとに、<xref:System.ServiceModel.Description.IServiceContractGenerationExtension> が自動的に呼び出されます。 その後、渡された <xref:System.ServiceModel.Description.ServiceContractGenerationContext> をこのメソッドで変更できます。 この例では、コメントが追加されています。  
  
## <a name="see-also"></a>関連項目

- [メタデータ](../feature-details/metadata.md)
- [方法: カスタム WSDL をインポートする](how-to-import-custom-wsdl.md)
