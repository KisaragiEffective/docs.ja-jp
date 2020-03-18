---
title: エラー処理およびレポートに対する制御の拡張
ms.date: 03/30/2017
ms.assetid: 45f996a7-fa00-45cb-9d6f-b368f5778aaa
ms.openlocfilehash: b7a3e0fa9b0799d98ea3df8df760e26851febf90
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716411"
---
# <a name="extending-control-over-error-handling-and-reporting"></a>エラー処理およびレポートに対する制御の拡張
このサンプルでは、<xref:System.ServiceModel.Dispatcher.IErrorHandler> インターフェイスを使用して、Windows Communication Foundation (WCF) サービスのエラー処理およびエラー報告に対する制御を拡張する方法を示します。 このサンプルは、エラーを処理するためにサービスに追加のコードが追加された[はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)に基づいています。 クライアントは、強制的にエラーが発生する状態にされます。 サービスはエラーを途中受信して、ファイルに記録します。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 サービスは <xref:System.ServiceModel.Dispatcher.IErrorHandler> インターフェイスを使用して、エラーを途中受信して処理を実行し、エラーを報告する方法を制御できます。 このインターフェイスには、<xref:System.ServiceModel.Dispatcher.IErrorHandler.ProvideFault%28System.Exception%2CSystem.ServiceModel.Channels.MessageVersion%2CSystem.ServiceModel.Channels.Message%40%29> と <xref:System.ServiceModel.Dispatcher.IErrorHandler.HandleError%2A> の 2 つのメソッドを実装できます。 <xref:System.ServiceModel.Dispatcher.IErrorHandler.ProvideFault%28System.Exception%2CSystem.ServiceModel.Channels.MessageVersion%2CSystem.ServiceModel.Channels.Message%40%29> メソッドを使用すると、例外に対して生成されるエラー メッセージを追加または変更したり、非表示にすることができます。 <xref:System.ServiceModel.Dispatcher.IErrorHandler.HandleError%2A> メソッドを使用すると、エラーの発生時にエラー処理を実行することができます。またこのメソッドは追加のエラー処理を実行するかどうかを制御します。  
  
 このサンプルでは、`CalculatorErrorHandler` 型は <xref:System.ServiceModel.Dispatcher.IErrorHandler> インターフェイスを実装します。 「  
  
 <xref:System.ServiceModel.Dispatcher.IErrorHandler.HandleError%2A> メソッドでは、`CalculatorErrorHandler` がエラーのログを c:\logs の Error.txt テキスト ファイルに書き込みます。 このサンプルではエラーを記録して表示します。このエラーの報告は、クライアントに返されます。  
  
```csharp
public class CalculatorErrorHandler : IErrorHandler
{
    // Provide a fault. The Message fault parameter can be replaced, or set to
    // null to suppress reporting a fault.

    public void ProvideFault(Exception error, MessageVersion version, ref Message fault)
    {
    }

    // HandleError. Log an error, then allow the error to be handled as usual.
    // Return true if the error is considered as already handled

    public bool HandleError(Exception error)
    {
        using (TextWriter tw = File.AppendText(@"c:\logs\error.txt"))
        {
            if (error != null)
            {
                tw.WriteLine("Exception: " + error.GetType().Name + " - " + error.Message);
            }
            tw.Close();
        }
        return true;
    }
}  
```  
  
 `ErrorBehaviorAttribute` は、エラー ハンドラをサービスに登録する機構として存在します。 この属性は、単一の型のパラメータを受け取ります。 この型は <xref:System.ServiceModel.Dispatcher.IErrorHandler> インターフェイスを実装し、空のパブリック コンストラクタを含む必要があります。 この属性は、そのエラー ハンドラの型のインスタンスをインスタンス化し、サービスにインストールします。 これを行うには <xref:System.ServiceModel.Description.IServiceBehavior> インターフェイスを実装し、次に <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A> メソッドを使用してエラー ハンドラのインスタンスをサービスに追加します。  
  
```csharp
// This attribute can be used to install a custom error handler for a service.  
public class ErrorBehaviorAttribute : Attribute, IServiceBehavior  
{  
    Type errorHandlerType;  
  
    public ErrorBehaviorAttribute(Type errorHandlerType)  
    {  
        this.errorHandlerType = errorHandlerType;  
    }  
  
    void IServiceBehavior.Validate(ServiceDescription description, ServiceHostBase serviceHostBase)  
    {  
    }  
  
    void IServiceBehavior.AddBindingParameters(ServiceDescription description, ServiceHostBase serviceHostBase, System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints, BindingParameterCollection parameters)  
    {  
    }  
  
    void IServiceBehavior.ApplyDispatchBehavior(ServiceDescription description, ServiceHostBase serviceHostBase)  
    {  
        IErrorHandler errorHandler;  
  
        try  
        {  
            errorHandler = (IErrorHandler)Activator.CreateInstance(errorHandlerType);  
        }  
        catch (MissingMethodException e)  
        {  
            throw new ArgumentException("The errorHandlerType specified in the ErrorBehaviorAttribute constructor must have a public empty constructor.", e);  
        }  
        catch (InvalidCastException e)  
        {  
            throw new ArgumentException("The errorHandlerType specified in the ErrorBehaviorAttribute constructor must implement System.ServiceModel.Dispatcher.IErrorHandler.", e);  
        }  
  
        foreach (ChannelDispatcherBase channelDispatcherBase in serviceHostBase.ChannelDispatchers)  
        {  
            ChannelDispatcher channelDispatcher = channelDispatcherBase as ChannelDispatcher;  
            channelDispatcher.ErrorHandlers.Add(errorHandler);  
        }                                                  
    }  
}  
```  
  
 このサンプルは、電卓サービスを実装しています。 クライアントは、パラメータに無効な値を渡すことによって、サービスで意図的に 2 つのエラーを発生させます。 `CalculatorErrorHandler` は <xref:System.ServiceModel.Dispatcher.IErrorHandler> インターフェイスを使用して、エラーをローカル ファイルに記録します。エラーの記録はその後、クライアントに報告されます。 クライアントは強制的に 0 による除算を行い、引数を範囲外の状態にします。  
  
```csharp
try
{  
    Console.WriteLine("Forcing an error in Divide");  
    // Call the Divide service operation - trigger a divide by 0 error.  
    value1 = 22;  
    value2 = 0;  
    result = proxy.Divide(value1, value2);  
    Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
}  
catch (FaultException e)  
{  
    Console.WriteLine("FaultException: " + e.GetType().Name + " - " + e.Message);  
}  
catch (Exception e)  
{  
    Console.WriteLine("Exception: " + e.GetType().Name + " - " + e.Message);  
}  
```  
  
 このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 0 による除算が行われたことと引数が範囲外の状態であることが、エラーとして報告されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。  
  
```console  
Add(15,3) = 18  
Subtract(145,76) = 69  
Multiply(9,81) = 729  
Forcing an error in Divide  
FaultException: FaultException - Invalid Argument: The second argument must not be zero.  
Forcing an error in Factorial  
FaultException: FaultException - Invalid Argument: The argument must be greater than zero.  
  
Press <ENTER> to terminate client.  
```  
  
 ファイル c:\logs\errors.txt には、サービスによって記録されたエラーに関する情報が格納されます。 サービスがディレクトリに書き込むには、サービスを実行しているプロセス (通常は ASP.NET または Network Service) にディレクトリへの書き込みアクセス許可があることを確認する必要があります。  
  
```txt
Fault: Reason = Invalid Argument: The second argument must not be zero.  
Fault: Reason = Invalid Argument: The argument must be greater than zero.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. error.txt ファイル用に c:\logs ディレクトリを作成したことを確認します。 または、`CalculatorErrorHandler.HandleError` で使用されるファイル名を変更します。  
  
4. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\ErrorHandling`  
