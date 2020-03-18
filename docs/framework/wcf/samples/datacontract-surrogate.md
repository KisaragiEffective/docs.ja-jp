---
title: DataContract サロゲート
ms.date: 03/30/2017
ms.assetid: b0188f3c-00a9-4cf0-a887-a2284c8fb014
ms.openlocfilehash: 3246466f9268fc920fd58d4f1ba2c06c3627c88e
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715368"
---
# <a name="datacontract-surrogate"></a>DataContract サロゲート
このサンプルでは、シリアル化、逆シリアル化、スキーマのエクスポート、スキーマのインポートなどのプロセスを、データ コントラクト サロゲート クラスを使用してカスタマイズする方法を示します。 このサンプルでは、クライアントとサーバーのシナリオでサロゲートを使用する方法を示します。このシナリオでは、データがシリアル化され、Windows Communication Foundation (WCF) クライアントとサービスの間で転送されます。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 このサンプルでは、次のサービス コントラクトを使用します。  
  
```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
[AllowNonSerializableTypes]  
public interface IPersonnelDataService  
{  
    [OperationContract]  
    void AddEmployee(Employee employee);  
  
    [OperationContract]  
    Employee GetEmployee(string name);  
}  
```  
  
 `AddEmployee` 操作は、ユーザーが新しい従業員に関するデータを追加できるようにし、`GetEmployee` 操作は名前に基づく従業員の検索をサポートします。  
  
 これらの操作では、次のデータ型を使用します。  
  
```csharp
[DataContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
class Employee  
{  
    [DataMember]  
    public DateTime dateHired;  
  
    [DataMember]  
    public Decimal salary;  
  
    [DataMember]  
    public Person person;  
}  
```  
  
 `Employee` 型では、`Person` クラス (次のサンプル コードを参照) は有効なデータ コントラクト クラスではないので、<xref:System.Runtime.Serialization.DataContractSerializer> によってシリアル化できません。  
  
```csharp
public class Person  
{  
    public string firstName;  
  
    public string lastName;  
  
    public int age;  
  
    public Person() { }  
}  
```  
  
 <xref:System.Runtime.Serialization.DataContractAttribute> 属性は `Person` クラスに適用できますが、適用できない場合もあります。 たとえば、`Person` クラスは、ユーザーが制御できない別のアセンブリで定義することができます。  
  
 このような制限がある場合、`Person` クラスをシリアル化するには、このクラスを <xref:System.Runtime.Serialization.DataContractAttribute> でマークされた別のクラスに置き換え、必要なデータをこの新しいクラスにコピーするという方法があります。 この目的は、`Person` クラスを DataContract として <xref:System.Runtime.Serialization.DataContractSerializer> に表示することです。 これは、データ コントラクト クラス以外のクラスをシリアル化する 1 つの方法です。  
  
 このサンプルでは、 `Person` クラスを `PersonSurrogated` という別のクラスに論理的に置き換えます。  
  
```csharp
[DataContract(Name="Person", Namespace = "http://Microsoft.ServiceModel.Samples")]  
public class PersonSurrogated  
{  
    [DataMember]  
    public string FirstName;  
  
    [DataMember]  
    public string LastName;  
  
    [DataMember]  
    public int Age;  
}  
```  
  
 データ コントラクト サロゲートは、この置き換えを実現するために使用されます。 データ コントラクト サロゲートは、<xref:System.Runtime.Serialization.IDataContractSurrogate> を実装するクラスです。 このサンプルでは、`AllowNonSerializableTypesSurrogate` クラスがこのインターフェイスを実装しています。  
  
 このインターフェイスの実装での最初のタスクは、`Person` から `PersonSurrogated` への型のマップを確立することです。 これは、シリアル化の時点およびスキーマをエクスポートする時点の両方で使用されます。 このマッピングは、<xref:System.Runtime.Serialization.IDataContractSurrogate.GetDataContractType%28System.Type%29> メソッドを実装することによって実現されます。  
  
```csharp
public Type GetDataContractType(Type type)  
{  
    if (typeof(Person).IsAssignableFrom(type))  
    {  
        return typeof(PersonSurrogated);  
    }  
    return type;  
}  
```  
  
 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetObjectToSerialize%28System.Object%2CSystem.Type%29> メソッドは、シリアル化中に `Person` インスタンスを `PersonSurrogated` インスタンスにマップします。次のサンプル コードを参照してください。  
  
```csharp
public object GetObjectToSerialize(object obj, Type targetType)  
{  
    if (obj is Person)  
    {  
        Person person = (Person)obj;  
        PersonSurrogated personSurrogated = new PersonSurrogated();  
        personSurrogated.FirstName = person.firstName;  
        personSurrogated.LastName = person.lastName;  
        personSurrogated.Age = person.age;  
        return personSurrogated;  
    }  
    return obj;  
}  
```  
  
 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDeserializedObject%28System.Object%2CSystem.Type%29> メソッドは、逆シリアル化のための逆マップを実現します。次のサンプル コードを参照してください。  
  
```csharp
public object GetDeserializedObject(object obj,   
Type targetType)  
{  
    if (obj is PersonSurrogated)  
    {  
        PersonSurrogated personSurrogated = (PersonSurrogated)obj;  
        Person person = new Person();  
        person.firstName = personSurrogated.FirstName;  
        person.lastName = personSurrogated.LastName;  
        person.age = personSurrogated.Age;  
        return person;  
    }  
    return obj;  
}  
```  
  
 スキーマのインポート中に `PersonSurrogated` データ コントラクトを既存の `Person` クラスにマップするため、このサンプルでは <xref:System.Runtime.Serialization.IDataContractSurrogate.GetReferencedTypeOnImport%28System.String%2CSystem.String%2CSystem.Object%29> メソッドを実装しています。次のサンプル コードを参照してください。  
  
```csharp
public Type GetReferencedTypeOnImport(string typeName,   
               string typeNamespace, object customData)  
{  
if (  
typeNamespace.Equals("http://schemas.datacontract.org/2004/07/DCSurrogateSample")  
)  
    {  
         if (typeName.Equals("PersonSurrogated"))  
        {  
             return typeof(Person);  
        }  
     }  
     return null;  
}  
```  
  
 <xref:System.Runtime.Serialization.IDataContractSurrogate> インターフェイスの実装を完了するサンプル コードを次に示します。  
  
```csharp
public System.CodeDom.CodeTypeDeclaration ProcessImportedType(  
          System.CodeDom.CodeTypeDeclaration typeDeclaration,   
          System.CodeDom.CodeCompileUnit compileUnit)  
{  
    return typeDeclaration;  
}  
public object GetCustomDataToExport(Type clrType,   
                               Type dataContractType)  
{  
    return null;  
}  
  
public object GetCustomDataToExport(  
System.Reflection.MemberInfo memberInfo, Type dataContractType)  
{  
    return null;  
}  
public void GetKnownCustomDataTypes(  
        KnownTypeCollection customDataTypes)  
{  
    // It does not matter what we do here.  
    throw new NotImplementedException();  
}  
```  
  
 このサンプルでは、`AllowNonSerializableTypesAttribute` という属性により、ServiceModel でサロゲートが有効になります。 開発の際には、この属性をサービス コントラクトに適用する必要があります。上の `IPersonnelDataService` サービス コントラクトを参照してください。 この属性は `IContractBehavior` を実装し、`ApplyClientBehavior` メソッドと `ApplyDispatchBehavior` メソッドでの操作にサロゲートを設定します。  
  
 この場合、この属性は必要ありません。このサンプルでのデモンストレーション用にのみ使用されます。 これ以外の方法として、コードまたは構成を使用して同様の `IContractBehavior`、`IEndpointBehavior`、または `IOperationBehavior` を手動で追加することにより、サロゲートを有効にすることもできます。  
  
 `IContractBehavior` の実装は、操作に `DataContractSerializerOperationBehavior` が登録されているかどうかをチェックすることにより、DataContract を使用する操作を検索します。 登録されている場合は、その動作に `DataContractSurrogate` プロパティが設定されます。 この処理を行うサンプル コードを次に示します。 この操作にサロゲートが設定されると、シリアル化および逆シリアル化のために動作でサロゲートが有効化されます。  
  
```csharp
public void ApplyClientBehavior(ContractDescription description, ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.ClientRuntime proxy)  
{  
    foreach (OperationDescription opDesc in description.Operations)  
    {  
        ApplyDataContractSurrogate(opDesc);  
    }  
}  
  
public void ApplyDispatchBehavior(ContractDescription description, ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.DispatchRuntime dispatch)  
{  
    foreach (OperationDescription opDesc in description.Operations)  
    {  
        ApplyDataContractSurrogate(opDesc);  
    }  
}  
  
private static void ApplyDataContractSurrogate(OperationDescription description)  
{  
    DataContractSerializerOperationBehavior dcsOperationBehavior = description.Behaviors.Find<DataContractSerializerOperationBehavior>();  
    if (dcsOperationBehavior != null)  
    {  
        if (dcsOperationBehavior.DataContractSurrogate == null)  
            dcsOperationBehavior.DataContractSurrogate = new AllowNonSerializableTypesSurrogate();  
    }  
}  
```  
  
 メタデータの生成中にサロゲートをプラグインとして使用するには、追加手順が必要です。 これを行うための機構として、このサンプルで示す `IWsdlExportExtension` が用意されています。 さらに、`WsdlExporter` を直接変更するという方法もあります。  
  
 `AllowNonSerializableTypesAttribute` 属性は `IWsdlExportExtension` と `IContractBehavior`を実装します。 この場合、拡張機能には `IContractBehavior` または `IEndpointBehavior` のいずれかを指定できます。 `IWsdlExportExtension.ExportContract` メソッドの実装は、DataContract のスキーマ生成中に使用される`XsdDataContractExporter` にサロゲートを追加することによって、サロゲートを有効にします。 これを行う方法を次のコード スニペットに示します。  
  
```csharp
public void ExportContract(WsdlExporter exporter, WsdlContractConversionContext context)  
{  
    if (exporter == null)  
        throw new ArgumentNullException("exporter");  
  
    object dataContractExporter;  
    XsdDataContractExporter xsdDCExporter;  
    if (!exporter.State.TryGetValue(typeof(XsdDataContractExporter), out dataContractExporter))  
    {  
        xsdDCExporter = new XsdDataContractExporter(exporter.GeneratedXmlSchemas);  
        exporter.State.Add(typeof(XsdDataContractExporter), xsdDCExporter);  
    }  
    else  
    {  
        xsdDCExporter = (XsdDataContractExporter)dataContractExporter;  
    }  
    if (xsdDCExporter.Options == null)  
        xsdDCExporter.Options = new ExportOptions();  
  
    if (xsdDCExporter.Options.DataContractSurrogate == null)  
        xsdDCExporter.Options.DataContractSurrogate = new AllowNonSerializableTypesSurrogate();  
}  
```  
  
 サンプルを実行すると、クライアントは AddEmployee の呼び出しに続いて、最初の呼び出しが正常に行われたかどうかをチェックする GetEmployee を呼び出します。 GetEmployee の操作要求の結果は、クライアント コンソール ウィンドウに表示されます。 GetEmployee 操作では、従業員の検索と "found" の印刷を成功させる必要があります。  
  
> [!NOTE]
> このサンプルでは、シリアル化、逆シリアル化、およびメタデータの生成で、サロゲートをプラグインとして使用する方法を示します。 メタデータからコードを生成するためにサロゲートをプラグインとして使用する方法を示すものではありません。 サロゲートを使用してクライアントコード生成にプラグインする方法のサンプルについては、「[カスタム WSDL 発行](../../../../docs/framework/wcf/samples/custom-wsdl-publication.md)のサンプル」を参照してください。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションのC#エディションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\DataContract`  
