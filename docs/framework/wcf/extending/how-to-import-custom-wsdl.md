---
title: '方法 : カスタム WSDL をインポートする'
ms.date: 03/30/2017
ms.assetid: ddc3718d-ce60-44f6-92af-a5c67477dd99
ms.openlocfilehash: 10fc3282560d35e61044a367f8172571096d76bd
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975886"
---
# <a name="how-to-import-custom-wsdl"></a>方法 : カスタム WSDL をインポートする
このトピックでは、カスタム WSDL をインポートする方法について説明します。 カスタム WSDL を処理するには、<xref:System.ServiceModel.Description.IWsdlImportExtension> インターフェイスを実装する必要があります。  
  
### <a name="to-import-custom-wsdl"></a>カスタム WSDL をインポートするには  
  
1. <xref:System.ServiceModel.Description.IWsdlImportExtension>を実装します。 <xref:System.ServiceModel.Description.IWsdlImportExtension.BeforeImport%28System.Web.Services.Description.ServiceDescriptionCollection%2CSystem.Xml.Schema.XmlSchemaSet%2CSystem.Collections.Generic.ICollection%7BSystem.Xml.XmlElement%7D%29> メソッドを実装してメタデータをインポートする前に変更します。 <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportEndpoint%28System.ServiceModel.Description.WsdlImporter%2CSystem.ServiceModel.Description.WsdlEndpointConversionContext%29> メソッドと <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportContract%28System.ServiceModel.Description.WsdlImporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> メソッドを実装してメタデータからインポートされたコントラクトとエンドポイントを変更します。 インポートしたコントラクトまたはエンドポイントにアクセスするには、対応するコンテキスト オブジェクト (<xref:System.ServiceModel.Description.WsdlContractConversionContext> または <xref:System.ServiceModel.Description.WsdlEndpointConversionContext>) を使用します。  
  
    ```csharp
    public class WsdlDocumentationImporter : IWsdlImportExtension
    {
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
    }
    ```
  
2. カスタム WSDL インポーターを使用するようクライアント アプリケーションを構成します。 Svcutil.exe を使用する場合は、Svcutil.exe の構成ファイル (Svcutil.exe.config) にこの構成を追加する必要があります。  
  
    ```xml  
    <system.serviceModel>  
          <client>  
            <endpoint   
              address="http://localhost:8000/Fibonacci"   
              binding="wsHttpBinding"  
              contract="IFibonacci"  
            />  
            <metadata>  
              <wsdlImporters>  
                <extension type="Microsoft.WCF.Documentation.WsdlDocumentationImporter, WsdlDocumentation" />  
              </wsdlImporters>  
            </metadata>  
          </client>  
        </system.serviceModel>  
    ```  
  
3. 新しい <xref:System.ServiceModel.Description.WsdlImporter> インスタンス (インポートする WSDL ドキュメントが含まれる <xref:System.ServiceModel.Description.MetadataSet> インスタンスを渡す) を作成し、<xref:System.ServiceModel.Description.WsdlImporter.ImportAllContracts%2A> を呼び出します。  
  
    ```csharp
    WsdlImporter importer = new WsdlImporter(metaDocs);
    System.Collections.ObjectModel.Collection<ContractDescription> contracts = importer.ImportAllContracts();  
    ```  
  
## <a name="see-also"></a>関連項目

- [メタデータ](../feature-details/metadata.md)
- [メタデータのエクスポートとインポート](../feature-details/exporting-and-importing-metadata.md)
- [カスタム WSDL パブリケーション](../samples/custom-wsdl-publication.md)
