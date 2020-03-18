---
title: WCF モニカーの COM クライアントと組み合わせての使用
ms.date: 03/30/2017
ms.assetid: e2799bfe-88bd-49d7-9d6d-ac16a9b16b04
ms.openlocfilehash: 281921bf42910086b874194cb2d8b56fbf71e671
ms.sourcegitcommit: 8c99457955fc31785b36b3330c4ab6ce7984a7ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/29/2019
ms.locfileid: "75544697"
---
# <a name="using-the-wcf-moniker-with-com-clients"></a>WCF モニカーの COM クライアントと組み合わせての使用
このサンプルでは、Windows Communication Foundation (WCF) サービスモニカーを使用して、Microsoft Office Visual Basic for Applications (Office VBA) や Visual Basic 6.0 などの COM ベースの開発環境に Web サービスを統合する方法を示します。 このサンプルは、Windows スクリプト ホストのクライアント (.vbs)、サポート クライアント ライブラリ (.dll)、およびインターネット インフォメーション サービス (IIS) でホストされるサービス ライブラリ (.dll) で構成されています。 このサービスは電卓サービスの 1 つであり、COM クライアントはサービスの算術演算 (Add、Subtract、Multiply、および Divide) を呼び出します。 クライアント アクティビティは、メッセージ ボックス ウィンドウに表示されます。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Interop\COM`  
  
 サービスは、次に示すコード例で定義される `ICalculator` コントラクトを実装します。  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract]  
    double Add(double n1, double n2);  
    [OperationContract]  
    double Subtract(double n1, double n2);  
    [OperationContract]  
    double Multiply(double n1, double n2);  
    [OperationContract]  
    double Divide(double n1, double n2);  
}  
```  
  
 このサンプルでは、モニカーを使用するための次の 3 つの代替方法を示します。  
  
- 型指定のあるコントラクト - クライアント コンピューターで COM から参照できる型として登録されます。  
  
- WSDL コントラクト - WSDL ドキュメントという形で供給されます。  
  
- Metadata Exchange コントラクト – 実行時に Metadata Exchange (MEX) エンドポイントから取得されます。  
  
## <a name="typed-contract"></a>型指定のあるコントラクト  
 型指定のあるコントラクトと共にモニカーを使用するには、属性が適切に設定されているサービス コントラクトの型を COM に登録する必要があります。 まず、 [ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して、クライアントを生成する必要があります。 次のコマンドをクライアント ディレクトリでコマンド プロンプトから実行して、型指定のあるプロキシを生成します。  
  
```console  
svcutil.exe /n:http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples http://localhost/servicemodelsamples/service.svc /out:generatedClient.cs  
```  
  
 このクラスはプロジェクトに含まれている必要があり、そのプロジェクトは、COM から参照できる署名付きのアセンブリをコンパイル時に生成するように構成されている必要があります。 AssemblyInfo.cs ファイルには次の属性を含める必要があります。  
  
```csharp
[assembly: ComVisible(true)]  
```  
  
 プロジェクトをビルドしたら、次の例で示すように `regasm` を使用して、COM から参照できる型を登録します。  
  
```console  
regasm.exe /tlb:CalcProxy.tlb client.dll  
```  
  
 作成されたアセンブリは、グローバル アセンブリ キャッシュに追加する必要があります。 必要性は強くありませんが、これによってアセンブリを検索するランタイムのプロセスが簡略化されます。 グローバル アセンブリ キャッシュにアセンブリを追加するコマンドを次に示します。  
  
```console  
gacutil.exe /i client.dll  
```  
  
> [!NOTE]
> サービス モニカーで必要なのは型の登録だけであり、サービスと通信するためのプロキシは使用されません。  
  
 ComCalcClient.vbs クライアント アプリケーションは、サービス モニカーの構文を使用してサービスのアドレス、バインディング、およびコントラクトを指定し、`GetObject` 関数を使用してサービスのプロキシを構築します。  
  
```vbscript
Set typedServiceMoniker = GetObject(  
"service4:address=http://localhost/ServiceModelSamples/service.svc, binding=wsHttpBinding,   
contractType={9213C6D2-5A6F-3D26-839B-3BA9B82228D3}")  
```  
  
 モニカーが使用するパラメーターでの指定:  
  
- サービス エンドポイントのアドレス。  
  
- エンドポイントとの接続にクライアントが使用する必要のあるバインディング。 クライアントの構成ファイルにはカスタム バインドを定義できますが、この場合はシステム定義の wsHttpBinding を使用します。 Windows スクリプト ホストで使用する場合、カスタム バインディングは、Cscript.exe と同じディレクトリにある Cscript.exe.config で定義されます。  
  
- エンドポイントでサポートされるコントラクトの型。 これは上の手順で生成され、登録された型です。 Visual Basic スクリプトには厳密に型指定された COM 環境がないので、コントラクトの識別子を指定する必要があります。 この GUID は CalcProxy.tlb からの `interfaceID` で、OLE/COM オブジェクト ビューアー (OleView.exe) などの COM ツールを使用して表示できます。 Office VBA や Visual Basic 6.0 などの厳密に型指定された環境では、コントラクト パラメーターを使用する代わりに、タイプ ライブラリへの明示的な参照を追加してプロキシ オブジェクトの型を宣言することができます。 これにより、クライアント アプリケーションの開発中に IntelliSense のサポートも提供されます。  
  
 サービス モニカーを使用してプロキシ インスタンスを構築しておくと、クライアント アプリケーションはプロキシでメソッドを呼び出すことができます。これにより、対応するサービス操作を呼び出すサービス モニカー インフラストラクチャは次のようになります。  
  
```vbscript  
' Call the service operations using the moniker object  
WScript.Echo "Typed service moniker: 100 + 15.99 = " & typedServiceMoniker.Add(100, 15.99)  
```  
  
 サンプルを実行すると、操作の応答が Windows スクリプト ホストのメッセージ ボックス ウィンドウに表示されます。 これは、型指定されたモニカーを使用して COM 呼び出しを行い、WCF サービスと通信する COM クライアントを示しています。 クライアント アプリケーションでは COM が使用されますが、サービスとの通信は Web サービス呼び出しのみで構成されます。  
  
## <a name="wsdl-contract"></a>WSDL コントラクト  
 WSDL コントラクトと共にモニカーを使用するには、クライアント ライブラリを登録する必要はありません。ただし、ブラウサを使用してサービスの WSDL エンドポイントにアクセスするなど、帯域外機構を通じてサービスの WSDL コントラクトを取得する必要があります。 これにより、モニカーは実行時にそのコントラクトにアクセスできるようになります。  
  
 ComCalcClient.vbs クライアント アプリケーションは、次のように `FileSystemObject` を使用してローカルに保存されている WSDL ファイルにアクセスし、その後 `GetObject` 関数を再度使用してサービスのプロキシを構築します。  
  
```vbscript  
' Open the WSDL contract file and read it all into the wsdlContract string  
Const ForReading = 1  
Set objFSO = CreateObject("Scripting.FileSystemObject")  
Set objFile = objFSO.OpenTextFile("serviceWsdl.xml", ForReading)  
wsdlContract = objFile.ReadAll  
objFile.Close  
  
' Create a string for the service moniker including the content of the WSDL contract file  
wsdlMonikerString = "service4:address='http://localhost/ServiceModelSamples/service.svc'"  
wsdlMonikerString = wsdlMonikerString + ", binding=WSHttpBinding_ICalculator, bindingNamespace='http://Microsoft.ServiceModel.Samples'"  
wsdlMonikerString = wsdlMonikerString + ", wsdl='" & wsdlContract & "'"  
wsdlMonikerString = wsdlMonikerString + ", contract=ICalculator, contractNamespace='http://Microsoft.ServiceModel.Samples'"  
  
' Create the service moniker object  
Set wsdlServiceMoniker = GetObject(wsdlMonikerString)  
```  
  
 モニカーが使用するパラメーターでの指定:  
  
- サービス エンドポイントのアドレス。  
  
- そのエンドポイントと接続するためにクライアントが使用する必要があるバインディングと、そのバインディングが定義されている名前空間。 この場合、`wsHttpBinding_ICalculator` が使用されます。  
  
- コントラクトを定義する WSDL。 この場合は、サービスの Wsdl.xml ファイルから読み取られた文字列です。  
  
- コントラクトの名前と名前空間。 WSDL に複数のコントラクトが含まれている場合があるので、識別情報が必要です。  
  
    > [!NOTE]
    > 既定では、WCF サービスは、使用する名前空間ごとに個別の WSDL ファイルを生成します。 これらのファイルは、WSDL インポート コンストラクターを使用してリンクされます。 モニカーでは単一の WSDL 定義が想定されているので、サービスはこのサンプルで示すように単一の名前空間を使用するか、または別のファイルを手動でマージする必要があります。  
  
 サービス モニカーを使用してプロキシ インスタンスを構築しておくと、クライアント アプリケーションはプロキシでメソッドを呼び出すことができます。これにより、対応するサービス操作を呼び出すサービス モニカー インフラストラクチャは次のようになります。  
  
```vbscript  
' Call the service operations using the moniker object  
WScript.Echo "WSDL service moniker: 145 - 76.54 = " & wsdlServiceMoniker.Subtract(145, 76.54)  
```  
  
 サンプルを実行すると、操作の応答が Windows スクリプト ホストのメッセージ ボックス ウィンドウに表示されます。 これは、com クライアントが、モニカーと WSDL コントラクトを使用して WCF サービスと通信する com クライアントを示しています。  
  
## <a name="metadata-exchange-contract"></a>Metadata Exchange コントラクト  
 MEX コントラクトと共にモニカーを使用するには、WSDL コントラクトと同様、クライアント登録は必要ありません。 サービスのコントラクトは、実行時に Metadata Exchange を内部使用して取得されます。  
  
 ComCalcClient.vbs クライアント アプリケーションは次のように `GetObject` 関数を再度使用して、サービスのプロキシを構築します。  
  
```vbscript  
' Create a string for the service moniker specifying the address to retrieve the service metadata from  
mexMonikerString = "service4:mexAddress='http://localhost/servicemodelsamples/service.svc/mex'"  
mexMonikerString = mexMonikerString + ", address='http://localhost/ServiceModelSamples/service.svc'"  
mexMonikerString = mexMonikerString + ", binding=WSHttpBinding_ICalculator, bindingNamespace='http://Microsoft.ServiceModel.Samples'"  
mexMonikerString = mexMonikerString + ", contract=ICalculator, contractNamespace='http://Microsoft.ServiceModel.Samples'"  
  
' Create the service moniker object  
Set mexServiceMoniker = GetObject(mexMonikerString)  
```  
  
 モニカーが使用するパラメーターでの指定:  
  
- サービスの Metadata Exchange エンドポイントのアドレス。  
  
- サービス エンドポイントのアドレス。  
  
- そのエンドポイントと接続するためにクライアントが使用する必要があるバインディングと、そのバインディングが定義されている名前空間。 この場合、`wsHttpBinding_ICalculator` が使用されます。  
  
- コントラクトの名前と名前空間。 WSDL に複数のコントラクトが含まれている場合があるので、識別情報が必要です。  
  
 サービス モニカーを使用してプロキシ インスタンスを構築しておくと、クライアント アプリケーションはプロキシでメソッドを呼び出すことができます。これにより、対応するサービス操作を呼び出すサービス モニカー インフラストラクチャは次のようになります。  
  
```vbscript  
' Call the service operations using the moniker object  
WScript.Echo "MEX service moniker: 9 * 81.25 = " & mexServiceMoniker.Multiply(9, 81.25)  
```  
  
 サンプルを実行すると、操作の応答が Windows スクリプト ホストのメッセージ ボックス ウィンドウに表示されます。 これは、com クライアントが、モニカーを使用して COM 呼び出しを行い、MEX コントラクトを使用して WCF サービスと通信する方法を示しています。  
  
#### <a name="to-set-up-and-build-the-sample"></a>サンプルをセットアップしてビルドするには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. Visual Studio の開発者コマンドプロンプトで、言語固有のフォルダーの下にある \ client\bin フォルダーを開きます。  
  
    > [!NOTE]
    > Windows Vista、Windows Server 2008、Windows 7、または Windows Server 2008 R2 を使用している場合は、管理者特権でコマンドプロンプトを実行してください。  
  
4. 「`tlbexp.exe client.dll /out:CalcProxy.tlb`」と入力して、dll を tlb ファイルにエクスポートします。 "タイプ ライブラリ エクスポーターの警告" が表示されることが予想されますが、ジェネリック型は不要なので問題にはなりません。  
  
5. 型を COM に登録するには、`regasm.exe /tlb:CalcProxy.tlb client.dll` を入力します。 "タイプ ライブラリ エクスポーターの警告" が表示されることが予想されますが、ジェネリック型は不要なので問題にはなりません。  
  
6. アセンブリをグローバルアセンブリキャッシュに追加するには、「`gacutil.exe /i client.dll`」と入力します。  
  
#### <a name="to-run-the-sample-on-the-same-computer"></a>サンプルを同じコンピューターで実行するには  
  
1. 次のアドレスを入力して、ブラウザーを使用してサービスにアクセスできることをテストします: `http://localhost/servicemodelsamples/service.svc`。 これに応答して、確認ページが表示されます。  
  
2. 言語固有のフォルダーの下の \client にある ComCalcClient.vbs を実行します。 クライアント アクティビティがメッセージ ボックス ウィンドウに表示されます。  
  
3. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
#### <a name="to-run-the-sample-across-computers"></a>サンプルを複数のコンピューターで実行するには  
  
1. サービス コンピューターで、ServiceModelSamples という仮想ディレクトリを作成します。 サンプルに含まれている Setupvroot.bat スクリプトを使用して、ディスク ディレクトリと仮想ディレクトリを作成できます。  
  
2. サービス プログラム ファイルを %SystemDrive%\Inetpub\wwwroot\servicemodelsamples からサービス コンピューターの ServiceModelSamples 仮想ディレクトリにコピーします。 \bin ディレクトリのファイルが含まれていることを確認してください。  
  
3. クライアント スクリプト ファイルを、言語固有のフォルダーにある \client フォルダーからクライアント コンピューターにコピーします。  
  
4. このスクリプト ファイルで、エンドポイント定義のアドレス値をサービスの新しいアドレスに変更します。 アドレスの "localhost" への参照をすべて完全修飾ドメイン名に置き換えます。  
  
5. WSDL ファイルをクライアント コンピューターにコピーします。 WSDL ファイルのサービスの Wsdl.xml で、アドレスの "localhost" への参照をすべて完全修飾ドメイン名に置き換えます。  
  
6. Client.dll ライブラリを、言語固有のフォルダーにある \client\bin\ フォルダーからクライアント コンピューターのディレクトリにコピーします。  
  
7. コマンド プロンプトで、クライアント コンピューターのコピー先ディレクトリに移動します。 Windows Vista または Windows Server 2008 を使用している場合は、管理者としてコマンドプロンプトを実行してください。  
  
8. 「`tlbexp.exe client.dll /out:CalcProxy.tlb`」と入力して、dll を tlb ファイルにエクスポートします。 "タイプ ライブラリ エクスポーターの警告" が表示されることが予想されますが、ジェネリック型は不要なので問題にはなりません。  
  
9. 型を COM に登録するには、`regasm.exe /tlb:CalcProxy.tlb client.dll` を入力します。 コマンドを実行する前に、パスが `regasm.exe` を含むフォルダーに設定されていることを確認してください。  
  
10. アセンブリをグローバルアセンブリキャッシュに追加するには、「`gacutil.exe /i client.dll`」と入力します。 コマンドを実行する前に、パスが `gacutil.exe` を含むフォルダーに設定されていることを確認してください。  
  
11. ブラウザーを使用して、サービスにクライアント コンピューターからアクセスできるかどうかをテストします。  
  
12. クライアント コンピューターで ComCalcClient.vbs を起動します。  
  
#### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
- セキュリティの目的で、サンプルの使用が終わったら、このセットアップで付与された仮想ディレクトリの定義とアクセス許可を削除してください。  
