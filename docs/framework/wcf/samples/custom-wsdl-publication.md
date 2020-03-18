---
title: カスタム WSDL パブリケーション
ms.date: 03/30/2017
ms.assetid: 3b3e8103-2c95-4db3-a05b-46aa8e9d4d29
ms.openlocfilehash: 173deaf280c052b76e6937b2cec44ebdeafc57f9
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74714913"
---
# <a name="custom-wsdl-publication"></a>カスタム WSDL パブリケーション
このサンプルでは、次の方法を示します。  
  
- カスタム <xref:System.ServiceModel.Description.IWsdlExportExtension?displayProperty=nameWithType> 属性に <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType> を実装し、属性プロパティを WSDL 注釈としてエクスポートします。  
  
- <xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> を実装し、カスタム WSDL 注釈をインポートします。  
  
- カスタム コントラクトの動作とカスタム操作の動作に、それぞれ <xref:System.ServiceModel.Description.IServiceContractGenerationExtension?displayProperty=nameWithType> と <xref:System.ServiceModel.Description.IOperationContractGenerationExtension?displayProperty=nameWithType> を実装し、インポートされたコントラクトと操作の CodeDOM に、インポートされた注釈をコメントとして書き込みます。  
  
- <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> を使用して、wsdl をダウンロードし、カスタム WSDL インポーターを使用して WSDL をインポートするための <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> を使用します。また、<xref:System.ServiceModel.Description.ServiceContractGenerator?displayProperty=nameWithType> を使用して Windows Communication Foundation (WCF) クライアントコードを生成C#します。これは、および Visual Basic のコメント///および ' ' ' です。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
## <a name="service"></a>サービス  
 このサンプルのサービスは、2 つのカスタム属性でマークされています。 1 つ目の属性は `WsdlDocumentationAttribute` で、この属性を使用するとコントラクタ内で文字列を使用でき、用途を説明する文字列をコントラクト インターフェイスまたは操作に提供するために適用できます。 2 つ目は `WsdlParamOrReturnDocumentationAttribute` で、この属性は、操作内で値またはその値を表すパラメータを返すために適用できます。 これらの属性を使用して表されたサービス コントラクト `ICalculator` を、次の例に示します。  
  
```csharp  
// Define a service contract.      
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
// Document it.  
[WsdlDocumentation("The ICalculator contract performs basic calculation services.")]  
public interface ICalculator  
{  
    [OperationContract]  
    [WsdlDocumentation("The Add operation adds two numbers and returns the result.")]  
    [return:WsdlParamOrReturnDocumentation("The result of adding the two arguments together.")]  
    double Add(  
      [WsdlParamOrReturnDocumentation("The first value to add.")]double n1,   
      [WsdlParamOrReturnDocumentation("The second value to add.")]double n2  
    );  
  
    [OperationContract]  
    [WsdlDocumentation("The Subtract operation subtracts the second argument from the first.")]  
    [return:WsdlParamOrReturnDocumentation("The result of the second argument subtracted from the first.")]  
    double Subtract(  
      [WsdlParamOrReturnDocumentation("The value from which the second is subtracted.")]double n1,   
      [WsdlParamOrReturnDocumentation("The value that is subtracted from the first.")]double n2  
    );  
  
    [OperationContract]  
    [WsdlDocumentation("The Multiply operation multiplies two values.")]  
    [return:WsdlParamOrReturnDocumentation("The result of multiplying the first and second arguments.")]  
    double Multiply(  
      [WsdlParamOrReturnDocumentation("The first value to multiply.")]double n1,   
      [WsdlParamOrReturnDocumentation("The second value to multiply.")]double n2  
    );  
  
    [OperationContract]  
    [WsdlDocumentation("The Divide operation returns the value of the first argument divided by the second argument.")]  
    [return:WsdlParamOrReturnDocumentation("The result of dividing the first argument by the second.")]  
    double Divide(  
      [WsdlParamOrReturnDocumentation("The numerator.")]double n1,   
      [WsdlParamOrReturnDocumentation("The denominator.")]double n2  
    );  
}  
```  
  
 `WsdlDocumentationAttribute` は <xref:System.ServiceModel.Description.IContractBehavior> と <xref:System.ServiceModel.Description.IOperationBehavior> を実装しています。したがってサービスが開かれている場合は、この属性のインスタンスを、対応する <xref:System.ServiceModel.Description.ContractDescription> または <xref:System.ServiceModel.Description.OperationDescription> に追加できます。 この属性は、さらに <xref:System.ServiceModel.Description.IWsdlExportExtension> を実装しています。 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportContract%28System.ServiceModel.Description.WsdlExporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> が呼び出されると、メタデータのエクスポートに使用される <xref:System.ServiceModel.Description.WsdlExporter> と、サービス説明オブジェクトを含む <xref:System.ServiceModel.Description.WsdlContractConversionContext> が、エクスポートされたメタデータの変更を有効にするパラメータとして渡されます。  
  
 このサンプルでは、次のコードに示すように、エクスポート コンテキスト オブジェクトに <xref:System.ServiceModel.Description.ContractDescription> または <xref:System.ServiceModel.Description.OperationDescription> のどちらが含まれているかに応じて、そのテキスト プロパティを使用して属性からコメントが抽出され、WSDL 注釈の要素に追加されます。  
  
```csharp
public void ExportContract(WsdlExporter exporter, WsdlContractConversionContext context)
{
    if (contractDescription != null)
    {
        // Inside this block it is the contract-level comment attribute.
        // This.Text returns the string for the contract attribute.
        // Set the doc element; if this isn't done first, there is no XmlElement in the
        // DocumentElement property.
        context.WsdlPortType.Documentation = string.Empty;
        // Contract comments.
        XmlDocument owner = context.WsdlPortType.DocumentationElement.OwnerDocument;
        XmlElement summaryElement = owner.CreateElement("summary");
        summaryElement.InnerText = this.Text;
        context.WsdlPortType.DocumentationElement.AppendChild(summaryElement);
    }
    else
    {
        Operation operation = context.GetOperation(operationDescription);
        if (operation != null)
        {
            // We are dealing strictly with the operation here.
            // This.Text returns the string for the operation-level attributes.
            // Set the doc element; if this isn't done first, there is no XmlElement in the
            // DocumentElement property.
            operation.Documentation = String.Empty;

            // Operation C# triple comments.
            XmlDocument owner = operation.DocumentationElement.OwnerDocument;
            XmlElement newSummaryElement = owner.CreateElement("summary");
            newSummaryElement.InnerText = this.Text;
            operation.DocumentationElement.AppendChild(newSummaryElement);
        }
    }
}
```  
  
 操作がエクスポートされる場合、このサンプルでは次に示すように、リフレクションを使用してパラメータの `WsdlParamOrReturnDocumentationAttribute` 値を取得して返し、次にそれらの値を操作の WSDL 注釈の要素に追加します。  
  
```csharp
// Get returns information  
ParameterInfo returnValue = operationDescription.SyncMethod.ReturnParameter;  
object[] returnAttrs = returnValue.GetCustomAttributes(typeof(WsdlParamOrReturnDocumentationAttribute), false);  
if (returnAttrs.Length != 0)  
{  
    // <returns>text.</returns>  
    XmlElement returnsElement = owner.CreateElement("returns");  
    returnsElement.InnerText = ((WsdlParamOrReturnDocumentationAttribute)returnAttrs[0]).ParamComment;  
    operation.DocumentationElement.AppendChild(returnsElement);  
}  
  
// Get parameter information.  
ParameterInfo[] args = operationDescription.SyncMethod.GetParameters();  
for (int i = 0; i < args.Length; i++)  
{  
    object[] docAttrs = args[i].GetCustomAttributes(typeof(WsdlParamOrReturnDocumentationAttribute), false);  
    if (docAttrs.Length == 1)  
    {  
        // <param name="Int1">Text.</param>  
        XmlElement newParamElement = owner.CreateElement("param");  
        XmlAttribute paramName = owner.CreateAttribute("name");  
        paramName.Value = args[i].Name;  
        newParamElement.InnerText = ((WsdlParamOrReturnDocumentationAttribute)docAttrs[0]).ParamComment;  
        newParamElement.Attributes.Append(paramName);  
        operation.DocumentationElement.AppendChild(newParamElement);  
    }  
}  
```  
  
 その後、サンプルでは次の構成ファイルを使用して、メタデータを標準的な方法で公開します。  
  
```xml  
<services>  
  <service   
      name="Microsoft.ServiceModel.Samples.CalculatorService"  
      behaviorConfiguration="CalculatorServiceBehavior">  
    <!-- ICalculator is exposed at the base address provided by host: http://localhost/servicemodelsamples/service.svc  -->  
    <endpoint address=""  
              binding="wsHttpBinding"  
              contract="Microsoft.ServiceModel.Samples.ICalculator" />  
    <!-- the mex endpoint is exposed at http://localhost/servicemodelsamples/service.svc/mex -->  
    <endpoint address="mex"  
              binding="mexHttpBinding"  
              contract="IMetadataExchange" />  
  </service>  
</services>  
  
<!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="CalculatorServiceBehavior">  
      <serviceMetadata httpGetEnabled="True"/>  
      <serviceDebug includeExceptionDetailInFaults="False" />  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
## <a name="svcutil-client"></a>Svcutil クライアント  
 このサンプルでは、Svcutil.exe を使用しません。 コントラクトは generatedClient.cs ファイルで提供されます。したがって、サンプルでカスタム WSDL のインポートとコードの生成を確認した後で、サービスを呼び出すことができます。 次のカスタム WSDL インポータをこのサンプルで使用するには、Svcutil.exe を実行して `/svcutilConfig` オプションを指定します。このオプションは、このサンプルで使用されているクライアント構成ファイルへのパスを示します。また、Svcutil.exe は `WsdlDocumentation.dll` ライブラリを参照します。 ただし、`WsdlDocumentationImporter` を読み込むには、Svuctil.exe が `WsdlDocumentation.dll` ライブラリの場所を特定し、これを読み込むことができる必要があります。つまり、このライブラリがグローバル アセンブリ キャッシュに登録されているか、パスに含まれているか、または Svcutil.exe と同じディレクトリにあることが必要です。 このサンプルのような基本的なサンプルにおいてこれを行う最も簡単な方法は、Svcutil.exe とクライアント構成ファイルを `WsdlDocumentation.dll` と同じディレクトリにコピーして、そのディレクトリで実行することです。  
  
## <a name="the-custom-wsdl-importer"></a>カスタム WSDL インポータ  
 カスタム <xref:System.ServiceModel.Description.IWsdlImportExtension> オブジェクトの `WsdlDocumentationImporter` は、<xref:System.ServiceModel.Description.IContractBehavior> と <xref:System.ServiceModel.Description.IOperationBehavior> も実装しています。これらはインポートされた ServiceEndpoints、<xref:System.ServiceModel.Description.IServiceContractGenerationExtension>、および <xref:System.ServiceModel.Description.IOperationContractGenerationExtension> に追加され、コントラクトまたは操作コードが作成されるときにコード生成を変更するために呼び出されます。  
  
 サンプルでは、最初に <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportContract%28System.ServiceModel.Description.WsdlImporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> メソッドで、WSDL 注釈がコントラクト レベルまたは操作レベルのどちらにあるかを判断し、適切なスコープで WDSL 注釈自体を動作として追加します。その際、インポートされた注釈テキストをコンストラクタに渡します。  
  
```csharp
public void ImportContract(WsdlImporter importer, WsdlContractConversionContext context)  
{  
    // Contract Documentation  
    if (context.WsdlPortType.Documentation != null)  
    {  
        // System examines the contract behaviors to see whether any implement IWsdlImportExtension.  
        context.Contract.Behaviors.Add(new WsdlDocumentationImporter(context.WsdlPortType.Documentation));  
    }  
    // Operation Documentation  
    foreach (Operation operation in context.WsdlPortType.Operations)  
    {  
        if (operation.Documentation != null)  
        {  
            OperationDescription operationDescription = context.Contract.Operations.Find(operation.Name);  
            if (operationDescription != null)  
            {  
                // System examines the operation behaviors to see whether any implement IWsdlImportExtension.  
                operationDescription.Behaviors.Add(new WsdlDocumentationImporter(operation.Documentation));  
            }  
        }  
    }  
}  
```  
  
 その後、コードが生成される際に、システムは <xref:System.ServiceModel.Description.IServiceContractGenerationExtension.GenerateContract%28System.ServiceModel.Description.ServiceContractGenerationContext%29> メソッドと <xref:System.ServiceModel.Description.IOperationContractGenerationExtension.GenerateOperation%28System.ServiceModel.Description.OperationContractGenerationContext%29> メソッドを呼び出し、適切なコンテキスト情報を渡します。 サンプルでは、カスタム WSDL 注釈の書式を設定し、コメントとして CodeDom に挿入します。  
  
```csharp
public void GenerateContract(ServiceContractGenerationContext context)  
{  
    Debug.WriteLine("In generate contract.");  
    context.ContractType.Comments.AddRange(FormatComments(text));  
}  
  
public void GenerateOperation(OperationContractGenerationContext context)  
{  
    context.SyncMethod.Comments.AddRange(FormatComments(text));  
    Debug.WriteLine("In generate operation.");  
}  
```  
  
## <a name="the-client-application"></a>クライアント アプリケーション  
 クライアント アプリケーションは、アプリケーション構成ファイルにカスタム WSDL インポータを指定することにより、このインポータを読み込みます。  
  
```xml  
<client>  
  <endpoint address="http://localhost/servicemodelsamples/service.svc"   
  binding="wsHttpBinding"   
  contract="ICalculator" />  
  <metadata>  
    <wsdlImporters>  
      <extension type="Microsoft.ServiceModel.Samples.WsdlDocumentationImporter, WsdlDocumentation"/>  
    </wsdlImporters>  
  </metadata>  
</client>  
```  
  
 カスタムインポーターを指定すると、WCF メタデータシステムによって、その目的のために作成された任意の <xref:System.ServiceModel.Description.WsdlImporter> にカスタムインポーターが読み込まれます。 このサンプルでは、<xref:System.ServiceModel.Description.MetadataExchangeClient> を使用してメタデータをダウンロードし、適切に構成された <xref:System.ServiceModel.Description.WsdlImporter> を使用してサンプルで作成されたカスタム インポータによってメタデータをインポートします。さらに、<xref:System.ServiceModel.Description.ServiceContractGenerator> を使用して、変更されたコントラクト情報を Visual Basic および C# のクライアント コードにコンパイルします。このコードは Intellisense をサポートする Visual Studio で使用するか、または XML ドキュメントにコンパイルできます。  
  
```csharp
/// From WSDL Documentation:  
///   
/// <summary>The ICalculator contract performs basic calculation   
/// services.</summary>   
///   
[System.CodeDom.Compiler.GeneratedCodeAttribute("System.ServiceModel", "3.0.0.0")]  
[System.ServiceModel.ServiceContractAttribute(Namespace="http://Microsoft.ServiceModel.Samples", ConfigurationName="ICalculator")]  
public interface ICalculator  
{  
  
    /// From WSDL Documentation:  
    ///   
    /// <summary>The Add operation adds two numbers and returns the   
    /// result.</summary><returns>The result of adding the two arguments   
    /// together.</returns><param name="n1">The first value to add.</param><param   
    /// name="n2">The second value to add.</param>   
    ///   
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Add", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/AddResponse")]  
    double Add(double n1, double n2);  
  
    /// From WSDL Documentation:  
    ///   
    /// <summary>The Subtract operation subtracts the second argument from the   
    /// first.</summary><returns>The result of the second argument subtracted from the   
    /// first.</returns><param name="n1">The value from which the second is   
    /// subtracted.</param><param name="n2">The value that is subtracted from the   
    /// first.</param>   
    ///   
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Subtract", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/SubtractResponse")]  
    double Subtract(double n1, double n2);  
  
    /// From WSDL Documentation:  
    ///   
    /// <summary>The Multiply operation multiplies two values.</summary><returns>The   
    /// result of multiplying the first and second arguments.</returns><param   
    /// name="n1">The first value to multiply.</param><param name="n2">The second value   
    /// to multiply.</param>   
    ///   
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Multiply", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/MultiplyResponse")]  
    double Multiply(double n1, double n2);  
  
    /// From WSDL Documentation:  
    ///   
    /// <summary>The Divide operation returns the value of the first argument divided   
    /// by the second argument.</summary><returns>The result of dividing the first   
    /// argument by the second.</returns><param name="n1">The numerator.</param><param   
    /// name="n2">The denominator.</param>   
    ///   
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Divide", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/DivideResponse")]  
    double Divide(double n1, double n2);  
}  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Metadata\WsdlDocumentation`  
