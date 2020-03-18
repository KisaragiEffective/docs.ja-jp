---
title: 'チュートリアル: Windows Communication Foundation クライアントを使用する'
ms.date: 03/19/2019
helpviewer_keywords:
- WCF clients [WCF], using
dev_langs:
- CSharp
- VB
ms.assetid: 190349fc-0573-49c7-bb85-8e316df7f31f
ms.openlocfilehash: d0ef525db16b2b2cedeea5fa03376fb4f3489a4a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346770"
---
# <a name="tutorial-use-a-windows-communication-foundation-client"></a>チュートリアル: Windows Communication Foundation クライアントを使用する

このチュートリアルでは、基本的な Windows Communication Foundation (WCF) アプリケーションを作成するために必要な5つのタスクについて説明します。 チュートリアルの概要については、「[チュートリアル: Windows Communication Foundation アプリケーションの](getting-started-tutorial.md)概要」を参照してください。

Windows Communication Foundation (WCF) プロキシを作成して構成したら、クライアントインスタンスを作成し、クライアントアプリケーションをコンパイルします。 次に、WCF サービスとの通信に使用します。 

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> - WCF クライアントを使用するコードを追加します。
> - WCF クライアントをテストします。

## <a name="add-code-to-use-the-wcf-client"></a>WCF クライアントを使用するコードを追加する

クライアントコードでは、次の手順を実行します。

- WCF クライアントをインスタンス化します。
- 生成されたプロキシからサービス操作を呼び出します。
- 操作の呼び出しが完了した後にクライアントを閉じます。

**GettingProgram.cs クライアント**プロジェクトから、次のコードに置き換えて、**そのファイルの**コードを開きます。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using GettingStartedClient.ServiceReference1;

namespace GettingStartedClient
{
    class Program
    {
        static void Main(string[] args)
        {
            //Step 1: Create an instance of the WCF proxy.
            CalculatorClient client = new CalculatorClient();

            // Step 2: Call the service operations.
            // Call the Add service operation.
            double value1 = 100.00D;
            double value2 = 15.99D;
            double result = client.Add(value1, value2);
            Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

            // Call the Subtract service operation.
            value1 = 145.00D;
            value2 = 76.54D;
            result = client.Subtract(value1, value2);
            Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);

            // Call the Multiply service operation.
            value1 = 9.00D;
            value2 = 81.25D;
            result = client.Multiply(value1, value2);
            Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);

            // Call the Divide service operation.
            value1 = 22.00D;
            value2 = 7.00D;
            result = client.Divide(value1, value2);
            Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);

            // Step 3: Close the client to gracefully close the connection and clean up resources.
            Console.WriteLine("\nPress <Enter> to terminate the client.");
            Console.ReadLine();
            client.Close();
        }
    }
}
```

```vb
Imports System.Collections.Generic
Imports System.Text
Imports System.ServiceModel
Imports GettingStartedClient.ServiceReference1

Module Module1

    Sub Main()
        ' Step 1: Create an instance of the WCF proxy.
        Dim Client As New CalculatorClient()

        ' Step 2: Call the service operations.
        ' Call the Add service operation.
        Dim value1 As Double = 100D
        Dim value2 As Double = 15.99D
        Dim result As Double = Client.Add(value1, value2)
        Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result)

        ' Call the Subtract service operation.
        value1 = 145D
        value2 = 76.54D
        result = Client.Subtract(value1, value2)
        Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result)

        ' Call the Multiply service operation.
        value1 = 9D
        value2 = 81.25D
        result = Client.Multiply(value1, value2)
        Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result)

        ' Call the Divide service operation.
        value1 = 22D
        value2 = 7D
        result = Client.Divide(value1, value2)
        Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result)

        ' Step 3: Close the client to gracefully close the connection and clean up resources.
        Console.WriteLine()
        Console.WriteLine("Press <Enter> to terminate the client.")
        Console.ReadLine()
        Client.Close()

    End Sub

End Module
```

`GettingStartedClient.ServiceReference1`をインポートする `using` ( C#Visual の場合) または `Imports` (Visual Basic) ステートメントに注目してください。 このステートメントは、Visual Studio によって生成されたコードを**サービス参照の追加**関数でインポートします。 このコードは、WCF プロキシをインスタンス化し、電卓サービスが公開する各サービス操作を呼び出します。 次に、プロキシを閉じてプログラムを終了します。

## <a name="test-the-wcf-client"></a>WCF クライアントをテストする

### <a name="test-the-application-from-visual-studio"></a>Visual Studio からのアプリケーションのテスト

1. ソリューションを保存してビルドします。

2. **[Gettingstartup lib]** フォルダーを選択し、ショートカットメニューの **[スタートアッププロジェクトに設定]** を選択します。

3. **スタートアッププロジェクト**から、ドロップダウンリストから **[Gettingstartup lib]** を選択し、 **[実行]** を選択するか、 **F5**キーを押します。

### <a name="test-the-application-from-a-command-prompt"></a>コマンドプロンプトからのアプリケーションのテスト

1. 管理者としてコマンドプロンプトを開き、Visual Studio ソリューションのディレクトリに移動します。 

2. サービスを開始するには、「 *GettingStartedHost\bin\Debug\GettingStartedHost.exe*」と入力します。

3. クライアントを起動するには: 別のコマンドプロンプトを開き、Visual Studio ソリューションのディレクトリに移動して、「 *GettingStartedClient\bin\Debug\GettingStartedClient.exe*」と入力します。

   この*ホスト*は、次の出力を生成します。

   ```text
   The service is ready.
   Press <Enter> to terminate the service.

   Received Add(100,15.99)
   Return: 115.99
   Received Subtract(145,76.54)
   Return: 68.46
   Received Multiply(9,81.25)
   Return: 731.25
   Received Divide(22,7)
   Return: 3.14285714285714
   ```

   *クライアント .exe*は、次の出力を生成します。

   ```text
   Add(100,15.99) = 115.99
   Subtract(145,76.54) = 68.46
   Multiply(9,81.25) = 731.25
   Divide(22,7) = 3.14285714285714

   Press <Enter> to terminate the client.
   ```

## <a name="next-steps"></a>次のステップ:

これで、WCF 入門チュートリアルのすべてのタスクが完了しました。 このチュートリアルでは、次の作業を行う方法を学びました。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> - WCF クライアントを使用するコードを追加します。
> - WCF クライアントをテストします。

いずれかの手順で問題やエラーが発生した場合は、トラブルシューティングの記事に記載されている手順に従って修正してください。

> [!div class="nextstepaction"]
> [WCF の概要チュートリアルのトラブルシューティング](troubleshooting-the-getting-started-tutorial.md)
