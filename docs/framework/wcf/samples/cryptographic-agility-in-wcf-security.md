---
title: WCF セキュリティにおける暗号化の機敏性
ms.date: 03/30/2017
ms.assetid: c2c549e5-ac19-40c5-b686-8f67f52b6dbf
ms.openlocfilehash: 2dbacd53876ded76ea212dd5656cd2dded4a6e48
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74714922"
---
# <a name="cryptographic-agility-in-wcf-security"></a>WCF セキュリティにおける暗号化の機敏性

このサンプルでは、標準/カスタムアルゴリズムでを指定して、Windows Communication Foundation (WCF) クライアントおよびサービスでの暗号化アジャイル実装を提供する方法を示します。 サンプルは、以下のプロジェクトで構成されます。

**サービス**

これは、`ICalculator` インターフェイスを実装し、セキュリティで保護されたセッションと信頼できるセッションが無効になっている <xref:System.ServiceModel.WSHttpBinding> を使用してエンドポイントをセキュリティで保護する、自己ホスト型 WCF サービスです。 このサービスは、カスタム `SecurityAlgorithmSuite` クラスを定義し、メッセージのセキュリティを確保するために使用される暗号化アルゴリズムを指定します。

**クライアント**

これは、認証が成功した後にサービスにアクセスする WCF クライアントです。 このクライアントは、`ICalculator` インターフェイスによって公開され、サービスによって実装される操作を呼び出します。 このクライアントも、同じカスタム `SecurityAlgorithmSuite` クラスを定義し、メッセージのセキュリティを確保するために使用される暗号化アルゴリズムを指定します。

## <a name="to-use-this-sample"></a>このサンプルを使用するには

1. Visual Studio 2012 で、CryptoAgility 性 .sln ソリューションを開きます。

2. Ctrl キーと Shift キーを押しながら B キーを押して、ソリューションをビルドします。

3. ファイルエクスプローラーを開き、\WCF\Basic\Security\CryptoAgility\Service\bin ディレクトリに移動して、管理者特権で service .exe ファイルを実行します。そのためには、service .exe を右クリックし、 **[管理者として実行]** を選択します。

4. \WCF\Basic\Security\CryptoAgility\Client\bin ディレクトリに移動して、client.exe ファイルを通常の方法で実行します。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Security\CryptoAgility`

## <a name="see-also"></a>参照

- [Windows Communication Foundation のセキュリティ](../feature-details/security.md)
