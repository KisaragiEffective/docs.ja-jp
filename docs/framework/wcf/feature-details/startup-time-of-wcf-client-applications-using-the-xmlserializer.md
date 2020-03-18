---
title: '方法 : XmlSerializer を使用する WCF クライアント アプリケーションの起動時間を短縮する'
ms.date: 03/30/2017
ms.assetid: 21093451-0bc3-4b1a-9a9d-05f7f71fa7d0
ms.openlocfilehash: ca15d710a30586135f0d030e155b09b63a22ee45
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976065"
---
# <a name="how-to-improve-the-startup-time-of-wcf-client-applications-using-the-xmlserializer"></a>方法 : XmlSerializer を使用する WCF クライアント アプリケーションの起動時間を短縮する
<xref:System.Xml.Serialization.XmlSerializer> を使用してシリアル化できるデータ型を使用するサービスおよびクライアント アプリケーションは、実行時にこのようなデータ型のシリアル化コードを生成およびコンパイルします。このため、起動時のパフォーマンスが低下することがあります。  
  
> [!NOTE]
> 事前生成済みシリアル化コードはクライアント アプリケーションでのみ使用できます。サービスでは使用できません。  
  
 [ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)を使用すると、アプリケーションのコンパイル済みアセンブリから必要なシリアル化コードを生成することで、これらのアプリケーションの起動時のパフォーマンスを向上させることができます。 Svcutil.exe は、<xref:System.Xml.Serialization.XmlSerializer> を使用してシリアル化できるコンパイル済みアプリケーション アセンブリに格納されたサービス コントラクトで使用されるすべてのデータ型に対して、シリアル化コードを生成します。 <xref:System.Xml.Serialization.XmlSerializer> を使用するサービスおよび操作コントラクトは、<xref:System.ServiceModel.XmlSerializerFormatAttribute> でマークされます。  
  
### <a name="to-generate-xmlserializer-serialization-code"></a>XmlSerializer シリアル化コードを生成するには  
  
1. サービスまたはクライアント コードを 1 つ以上のアセンブリにコンパイルします。  
  
2. SDK コマンド プロンプトを開きます。  
  
3. コマンド プロンプトで、次の形式を使用して Svcutil.exe ツールを起動します。  
  
    ```console  
    svcutil.exe /t:xmlSerializer  <assemblyPath>*  
    ```  
  
     `assemblyPath` 引数には、サービス コントラクト型を格納するアセンブリへのパスを指定します。 Svcutil.exe は、<xref:System.Xml.Serialization.XmlSerializer> を使用してシリアル化できるコンパイル済みアプリケーション アセンブリに格納されたサービス コントラクトで使用されるすべてのデータ型に対して、シリアル化コードを生成します。  
  
     Svcutil.exe は、C# のシリアル化コードのみを生成できます。 入力アセンブリごとに、1 つのソース コード ファイルが生成されます。 **/言語**スイッチを使用して、生成されたコードの言語を変更することはできません。  
  
     依存アセンブリへのパスを指定するには、 **/reference**オプションを使用します。  
  
4. 次のオプションのいずれかを使用して、生成したシリアル化コードをアプリケーションから利用できるようにします。  
  
    1. 生成されたシリアル化コードを、[*元のアセンブリ*] という名前の別のアセンブリにコンパイルします。XmlSerializers .dll (例、MyApp. XmlSerializers .dll)。 アプリケーションがアセンブリを読み込むことができ、アセンブリが元のアセンブリと同じキーで署名されている必要があります。 元のアセンブリを再コンパイルする場合は、シリアル化アセンブリも再生成する必要があります。  
  
    2. 生成されたシリアル化コードを別個のアセンブリにコンパイルし、<xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute> を使用するサービス コントラクトで <xref:System.ServiceModel.XmlSerializerFormatAttribute> を使用します。 <xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute.AssemblyName%2A> プロパティまたは <xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute.CodeBase%2A> プロパティが、コンパイル済みのシリアル化アセンブリを指すように設定します。  
  
    3. 生成されたシリアル化コードをアプリケーション アセンブリにコンパイルし、<xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute> を使用するサービス コントラクトに <xref:System.ServiceModel.XmlSerializerFormatAttribute> を追加します。 <xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute.AssemblyName%2A> プロパティおよび <xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute.CodeBase%2A> プロパティは設定しないでください。 既定のシリアル化アセンブリが現在のアセンブリであると見なされます。  
  
### <a name="to-generate-xmlserializer-serialization-code-in-visual-studio"></a>Visual Studio で XmlSerializer シリアル化コードを生成するには  
  
1. Visual Studio で WCF サービスとクライアントプロジェクトを作成します。 次に、クライアントプロジェクトにサービス参照を追加します。  
  
2. クライアントアプリプロジェクトの*reference.cs*ファイルにあるサービスコントラクトに <xref:System.ServiceModel.XmlSerializerFormatAttribute> を追加します。 **serviceReference**には、の下の -> を**参照し**てください。 これらのファイルを表示するには、**ソリューションエクスプローラー**内のすべてのファイルを表示する必要があることに注意してください。  
  
3. クライアントアプリをビルドします。  
  
4. [ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して、次のコマンドを使用して、事前に生成さ*れ*たシリアライザーファイルを作成します。  
  
    ```console  
    svcutil.exe /t:xmlSerializer  <assemblyPath>*  
    ```  
  
     AssemblyPath 引数は、WCF クライアントアセンブリへのパスを指定します。  
  
     次に例を示します。  
  
    ```console  
    svcutil.exe /t:xmlSerializer wcfclient.exe  
    ```  
  
     *WCFClient.XmlSerializers.dll.cs*ファイルが生成されます。  
  
5. 事前に生成されたシリアル化アセンブリをコンパイルします。  
  
     前の手順の例に基づいて、compile コマンドは次のようになります。  
  
    ```console  
    csc /r:wcfclient.exe /out:WCFClient.XmlSerializers.dll /t:library WCFClient.XmlSerializers.dll.cs  
    ```  
  
     生成された*WCFClient*がクライアントアプリと同じディレクトリ (この場合は*WCFClient* ) にあることを確認します。  
  
6. 通常どおりにクライアントアプリを実行します。 事前に生成されたシリアル化アセンブリが使用されます。  
  
## <a name="example"></a>例  
 次のコマンドにより、アセンブリに含まれているサービス コントラクトが使用する `XmlSerializer` 型用のシリアル化型を生成します。  
  
```console  
svcutil /t:xmlserializer myContractLibrary.exe  
```  
  
## <a name="see-also"></a>関連項目

- [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)
