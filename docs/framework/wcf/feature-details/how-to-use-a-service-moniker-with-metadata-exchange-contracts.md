---
description: '詳細情報: 方法 : Metadata Exchange コントラクトと共にサービス モニカーを使用する'
title: '方法: Metadata Exchange コントラクトと共にサービス モニカーを使用する'
ms.date: 03/30/2017
ms.assetid: c41a07e5-cb9d-45d6-9ea4-34511e227faf
ms.openlocfilehash: 220132a10cb637be9e3724232d0ddaf80a13551a
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643099"
---
# <a name="how-to-use-a-service-moniker-with-metadata-exchange-contracts"></a>方法: Metadata Exchange コントラクトと共にサービス モニカーを使用する

新しい WCF サービスをいくつか開発した後に、そのサービスをスクリプトまたは Visual Basic 6.0 アプリケーションから呼び出せるようにする必要が生じる場合があります。 1 つの方法としては、WCF クライアント アセンブリを生成し、そのアセンブリを COM に登録し、GAC にインストールした後、Visual Basic コードから COM 型を参照する方法があります。 アプリケーションを配布するときに、WCF クライアント アセンブリも配信する必要があります。 次にユーザーは COM を使用して WCF クライアント アセンブリを登録し、それを GAC に配置する必要があります。 WCF COM Interop でも、WCF クライアント アセンブリに依存しない同じサービス呼び出しを作成できます。 WCF モニカーを使用すれば、サービスに関する型情報を抽出するためにサービス モニカーで使用されるメタデータ交換 (Mex) エンドポイント URI を指定することにより、必要な WCF サービスを任意の COM 互換言語 (Visual Basic、VBScript、Visual Basic for Applications (VBA) など) から呼び出すことができます。 ここでは、Mex エンドポイントを指定する WCF モニカーを使用して、WCF の入門サンプルを呼び出す方法を説明します。  
  
> [!NOTE]
> WCF クライアント アセンブリで定義された型は、実際にインスタンス化されることはありません。 アセンブリはメタデータにのみ使用されます。  
  
### <a name="using-the-service-moniker-with-a-mex-address"></a>Mex アドレスを使うサービス モニカーの使用  
  
1. 入門サンプルを構築し、Internet Explorer を使用してその URL (`http://localhost/ServiceModelSamples/Service.svc`) を参照し、サービスが動作していることを確認します。  
  
2. Visual Basic スクリプトまたは Visual Basic アプリケーションを作成し、次のコードを記述します。  
  
    ```vb
    monString = "service:mexaddress=http://localhost/ServiceModelSamples/Service.svc/MEX"  
    monString = monString + ", address=http://localhost/ServiceModelSamples/Service.svc"  
    monString = monString + ", contract=ICalculator, contractNamespace=http://Microsoft.ServiceModel.Samples"  
    monString = monString + ", binding=WSHttpBinding_ICalculator, bindingNamespace=http://Microsoft.ServiceModel.Samples"  
  
    Set calc = GetObject(monString)  
    MsgBox calc.Add(3, 4)  
    ```  
  
3. 作成した Visual Basic アプリケーションまたはスクリプトを実行します。  
  
    > [!NOTE]
    > モニカーがサービスのメタデータを読み取るには、呼び出すサービスで、Mex エンドポイントが公開されている必要があります。 詳細については、「[方法: 構成ファイルを使用してサービスのメタデータを公開する](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)」を参照してください。  
  
    > [!NOTE]
    > モニカーの形式が正しくないか、`GetObject` を呼び出せない場合は、"構文が無効です" というメッセージが返されます。  このエラーが発生した場合は、使用しているモニカーが正しく、サービスが使用可能であることを確認してください。  
  
## <a name="see-also"></a>関連項目

- [方法: 未登録で Windows Communication Foundation のサービス モニカーを使用する](use-the-wcf-service-moniker-without-registration.md)
- [方法: WSDL コントラクトと共にサービス モニカーを使用する](how-to-use-a-service-moniker-with-wsdl-contracts.md)
