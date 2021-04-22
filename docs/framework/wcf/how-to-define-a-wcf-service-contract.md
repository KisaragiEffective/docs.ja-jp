---
title: 'チュートリアル: Windows Communication Foundation サービス コントラクトを定義する'
description: WCF アプリケーションの作成を始めるときに役立つ一連の記事の一部として、サービス コントラクトを定義する方法について説明します。
ms.date: 03/19/2019
helpviewer_keywords:
- service contracts [WCF], defining
dev_langs:
- CSharp
- VB
ms.assetid: 67bf05b7-1d08-4911-83b7-a45d0b036fc3
ms.openlocfilehash: 5cb371da8c7180b8c4cbf5ac11468fbb8e0e13cc
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246312"
---
# <a name="tutorial-define-a-windows-communication-foundation-service-contract"></a>チュートリアル: Windows Communication Foundation サービス コントラクトを定義する

このチュートリアルでは、基本的な Windows Communication Foundation (WCF) アプリケーションの作成に必要な 5 つのタスクのうち 1 番目のタスクについて説明します。 このチュートリアルの概要については、「[チュートリアル: Windows Communication Foundation アプリケーション入門](getting-started-tutorial.md)」を参照してください。

WCF サービスを作成する場合、最初のタスクとして、サービス コントラクトを定義します。 サービス コントラクトでは、サービスがサポートする操作を指定します。 操作は Web サービス メソッドと見なすことができます。 サービス コントラクトを作成するには、C# または Visual Basic でインターフェイスを定義します。 インターフェイスには、次のような性質があります。

- インターフェイスの各メソッドは、特定のサービス操作に対応しています。
- インターフェイスごとに、<xref:System.ServiceModel.ServiceContractAttribute> 属性を適用する必要があります。
- 操作つまりメソッドごとに、<xref:System.ServiceModel.OperationContractAttribute> 属性を適用する必要があります。

このチュートリアルでは、以下の内容を学習します。
> [!div class="checklist"]
>
> - **WCF サービス ライブラリ** プロジェクトを作成します。
> - サービス コントラクト インターフェイスを定義します。

## <a name="create-a-wcf-service-library-project-and-define-a-service-contract-interface"></a>WCF サービス ライブラリ プロジェクトを作成し、サービス コントラクト インターフェイスを定義する

1. Visual Studio を管理者として開きます。 そのためには、 **[スタート]** メニューの Visual Studio プログラムを選択してから、ショートカット メニューの **[その他]**  >  **[管理者として実行]** を選択します。

2. **WCF サービス ライブラリ** プロジェクトを作成します。

   1. **[ファイル]** メニューで **[新規作成]**  >  **[プロジェクト]** の順に選択します。

   2. **[新しいプロジェクト]** ダイアログの左側にある **[Visual C#]** または **[Visual Basic]** を展開し、 **[WCF]** カテゴリを選択します。 Visual Studio のウィンドウの中央のセクションに、プロジェクト テンプレートの一覧が表示されます。 **[WCF サービス ライブラリ]** を選択します。

      > [!NOTE]
      > **[WCF]** プロジェクト テンプレート カテゴリが表示されない場合は、Visual Studio の **Windows Communication Foundation** コンポーネントをインストールすることが必要な場合があります。 **[新しいプロジェクト]** ダイアログ ボックスで、左側の **[Visual Studio インストーラーを開く]** リンクを選択します。 **[個別のコンポーネント]** タブを選択し、 **[開発アクティビティ]** カテゴリで **[Windows Communication Foundation]** を見つけて選択します。 **[変更]** を選択して、コンポーネントのインストールを開始します。

   3. ウィンドウの下部で、 **[名前]** に「*GettingStartedLib*」と入力し、 **[ソリューション名]** に「*GettingStarted*」と入力します。

   4. **[OK]** を選択します。

      Visual Studio によって、プロジェクトが作成されます。それには、次の 3 つのファイルが含まれます: *IService1.cs* (または、Visual Basic プロジェクトの場合は *IService1.vb*)、*Service1.cs* (または、Visual Basic プロジェクトの場合は *Service1.vb*)、*App.config*。これらのファイルは、Visual Studio によって次のように定義されます。
      - *IService1* ファイルには、サービス コントラクトの既定の定義が含まれます。
      - *Service1* ファイルには、サービス コントラクトの既定の実装が含まれます。
      - *App.config* ファイルには、Visual Studio WCF サービス ホスト ツールに既定のサービスを読み込むために必要な構成情報が含まれます。 WCF サービス ホスト ツールの詳細については、「[WCF サービス ホスト (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)」を参照してください。

      > [!NOTE]
      > Visual Basic 開発者向け環境設定を使用して Visual Studio をインストールした場合は、ソリューションが表示されないことがあります。 この場合は、 **[ツール]** メニューの **[オプション]** を選択し、 **[オプション]** ウィンドウで **[プロジェクトとソリューション]**  >  **[全般]** を選択します。 **[常にソリューションを表示]** を選択します。 また、 **[作成時に新しいプロジェクトを保存]** がオンになっていることを確認します。

3. **ソリューション エクスプローラー** から、**IService1.cs** または **IService1.vb** ファイルを開き、そのコードを次のコードに置き換えます。

    ```csharp
    using System;
    using System.ServiceModel;

    namespace GettingStartedLib
    {
            [ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]
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
    }
    ```

    ```vb
    Imports System.ServiceModel

    Namespace GettingStartedLib

        <ServiceContract(Namespace:="http://Microsoft.ServiceModel.Samples")> _
        Public Interface ICalculator

            <OperationContract()> _
            Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
            <OperationContract()> _
            Function Subtract(ByVal n1 As Double, ByVal n2 As Double) As Double
            <OperationContract()> _
            Function Multiply(ByVal n1 As Double, ByVal n2 As Double) As Double
            <OperationContract()> _
            Function Divide(ByVal n1 As Double, ByVal n2 As Double) As Double
        End Interface
    End Namespace
    ```

     このコントラクトは、オンライン電卓を定義します。 `ICalculator` インターフェイスが <xref:System.ServiceModel.ServiceContractAttribute> 属性 (`ServiceContract` と簡略化されています) でマークされていることに注意してください。 この属性により、コントラクト名を区別するための名前空間が定義されます。 コードにより、各電卓操作が <xref:System.ServiceModel.OperationContractAttribute> 属性 (`OperationContract` と簡略化されています) でマークされています。

## <a name="next-steps"></a>次のステップ

このチュートリアルでは、以下の内容を学習しました。
> [!div class="checklist"]
>
> - WCF サービス ライブラリ プロジェクトを作成します。
> - サービス コントラクト インターフェイスを定義します。

次のチュートリアルに進み、WCF サービス コントラクトを実装する方法を学習してください。

> [!div class="nextstepaction"]
> [チュートリアル: WCF サービス コントラクトを実装する](how-to-implement-a-wcf-contract.md)
