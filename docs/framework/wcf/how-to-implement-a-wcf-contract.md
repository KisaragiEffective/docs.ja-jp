---
title: 'チュートリアル: Windows Communication Foundation のサービス コントラクトを実装する'
description: WCF アプリケーションの作成を始めるときに役立つ一連の記事の一部として、WCF サービス インターフェイスを実装するコードを追加する方法について説明します。
ms.date: 03/19/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF], implementing
ms.assetid: d5ab51ba-61ae-403e-b3c8-e2669e326806
ms.openlocfilehash: 89f97610cccd42c2a5d298baa667327d077fd472
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244648"
---
# <a name="tutorial-implement-a-windows-communication-foundation-service-contract"></a>チュートリアル: Windows Communication Foundation のサービス コントラクトを実装する

このチュートリアルでは、基本的な Windows Communication Foundation (WCF) アプリケーションの作成に必要な 5 つのタスクのうち 2 番目のタスクについて説明します。 このチュートリアルの概要については、「[チュートリアル: Windows Communication Foundation アプリケーション入門](getting-started-tutorial.md)」を参照してください。

WCF アプリケーションを作成するための次のステップは、前のステップで作成した WCF サービス インターフェイスを実装するコードを追加することです。 このステップでは、ユーザー定義の `ICalculator` インターフェイスを実装する `CalculatorService` という名前のクラスを作成します。 次のコードの各メソッドにより、電卓操作が呼び出されて、テストのためのテキストがコンソールに書き込まれます。

このチュートリアルでは、以下の内容を学習します。
> [!div class="checklist"]
>
> - WCF サービス コントラクトを実装するコードを追加します。
> - ソリューションをビルドします。

## <a name="add-code-to-implement-the-wcf-service-contract"></a>WCF サービス コントラクトを実装するコードを追加する

**GettingStartedLib** で、**Service1.cs** または **Service1.vb** ファイルを開き、そのコードを次のコードに置き換えます。

```csharp
using System;
using System.ServiceModel;

namespace GettingStartedLib
{
    public class CalculatorService : ICalculator
    {
        public double Add(double n1, double n2)
        {
            double result = n1 + n2;
            Console.WriteLine("Received Add({0},{1})", n1, n2);
            // Code added to write output to the console window.
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Subtract(double n1, double n2)
        {
            double result = n1 - n2;
            Console.WriteLine("Received Subtract({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Multiply(double n1, double n2)
        {
            double result = n1 * n2;
            Console.WriteLine("Received Multiply({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Divide(double n1, double n2)
        {
            double result = n1 / n2;
            Console.WriteLine("Received Divide({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }
    }
}
```

```vb
Imports System.ServiceModel

Namespace GettingStartedLib

    Public Class CalculatorService
        Implements ICalculator

        Public Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Add
            Dim result As Double = n1 + n2
            ' Code added to write output to the console window.
            Console.WriteLine("Received Add({0},{1})", n1, n2)
            Console.WriteLine("Return: {0}", result)
            Return result
        End Function

        Public Function Subtract(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Subtract
            Dim result As Double = n1 - n2
            Console.WriteLine("Received Subtract({0},{1})", n1, n2)
            Console.WriteLine("Return: {0}", result)
            Return result

        End Function

        Public Function Multiply(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Multiply
            Dim result As Double = n1 * n2
            Console.WriteLine("Received Multiply({0},{1})", n1, n2)
            Console.WriteLine("Return: {0}", result)
            Return result

        End Function

        Public Function Divide(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Divide
            Dim result As Double = n1 / n2
            Console.WriteLine("Received Divide({0},{1})", n1, n2)
            Console.WriteLine("Return: {0}", result)
            Return result

        End Function
    End Class
End Namespace
```

## <a name="edit-appconfig"></a>App.config を編集する

**GettingStartedLib** で **App.config** を編集して、コードに行った変更を反映します。

- Visual C# プロジェクトの場合:
  - 14 行目を `<service name="GettingStartedLib.CalculatorService">` に変更します
  - 17 行目を `<add baseAddress = "http://localhost:8000/GettingStarted/CalculatorService" />` に変更します
  - 22 行目を `<endpoint address="" binding="wsHttpBinding" contract="GettingStartedLib.ICalculator">` に変更します

- Visual Basic プロジェクトの場合は、次の操作を行います。
  - 14 行目を `<service name="GettingStartedLib.GettingStartedLib.CalculatorService">` に変更します
  - 17 行目を `<add baseAddress = "http://localhost:8000/GettingStarted/CalculatorService" />` に変更します
  - 22 行目を `<endpoint address="" binding="wsHttpBinding" contract="GettingStartedLib.GettingStartedLib.ICalculator">` に変更します

## <a name="compile-the-code"></a>コードのコンパイル

ソリューションをビルドして、コンパイル エラーがないことを確認します。 Visual Studio を使用している場合は、 **[ビルド]** メニューの **[ソリューションのビルド]** を選択します (または、**Ctrl** + **Shift** + **B** キーを押します)。

## <a name="next-steps"></a>次のステップ

このチュートリアルでは、以下の内容を学習しました。
> [!div class="checklist"]
>
> - WCF サービス コントラクトを実装するコードを追加します。
> - ソリューションをビルドします。

次のチュートリアルに進み、WCF サービスを実行する方法を学習します。

> [!div class="nextstepaction"]
> [チュートリアル: 基本的な WCF サービスをホストおよび実行する](how-to-host-and-run-a-basic-wcf-service.md)
