---
title: Windows Communication Foundation での Internet Information Services 7.0 の構成
ms.date: 03/30/2017
ms.assetid: 1050d395-092e-44d3-b4ba-66be3b039ffb
ms.openlocfilehash: 41eedcf78d8ca6f10fcd0380e43420dcc1b328f1
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964516"
---
# <a name="configuring-internet-information-services-70-for-windows-communication-foundation"></a>Windows Communication Foundation での Internet Information Services 7.0 の構成

Internet Information Services (IIS) 7.0 はモジュール設計になっており、必要なコンポーネントを選択してインストールできます。 この設計は、Windows Vista で導入された新しいマニフェスト駆動型コンポーネント化テクノロジをベースにしています。 IIS 7.0 には40以上のスタンドアロン機能コンポーネントがあり、個別にインストールすることができます。 これにより、IT プロフェッショナルは必要に応じてインストールをカスタマイズできます。 このトピックでは、Windows Communication Foundation (WCF) で使用する IIS 7.0 を構成し、どのコンポーネントが必要かを判断する方法について説明します。

## <a name="minimal-installation-installing-was"></a>最小インストール : WAS のインストール
 IIS 7.0 パッケージ全体の最小インストールでは、Windows プロセスアクティブ化サービス (WAS) をインストールします。 WAS はスタンドアロン機能であり、すべての Windows Vista オペレーティングシステム (Home Basic、Home Premium、Business、Ultimate および Enterprise) で利用できる IIS 7.0 の唯一の機能です。

 コントロールパネルで、**プログラム** をクリックし、**プログラムと機能** の下に表示される **Windows の機能の有効化または無効化** をクリックすると、次の図のように WAS コンポーネントが一覧に表示されます。

 ![[機能の有効化または無効化] ダイアログ](../../../../docs/framework/wcf/feature-details/media/wcfc-turnfeaturesonoroffs.gif "wcfc_TurnFeaturesOnOrOffs")

 この機能には、次のサブコンポーネントがあります。

- .NET 環境

- 構成 API

- プロセス モデル

 WAS のルートノードを選択した場合は、 **[プロセスモデル]** サブノードのみが既定でオンになっています。 このインストールでは Web サーバーをサポートしないため、WAS のみをインストールすることに注意してください。

 WCF または ASP.NET アプリケーションを機能させるには、 **[.Net 環境]** チェックボックスをオンにします。 これは、WCF と ASP.NET を適切に機能させるために、すべての WAS コンポーネントが必要であることを意味します。 これらのコンポーネントのいずれかを一度インストールすると、チェック ボックスは自動的にオンになります。

## <a name="iis-70-default-installation"></a>IIS 7.0 : 既定のインストール
 **インターネットインフォメーションサービス**機能を確認することによって、次の図に示すように、一部のサブノードが自動的にチェックされます。

 ![IIS 7.0 機能の既定の設定](../../../../docs/framework/wcf/feature-details/media/wcfc-turningfeaturesonoroff2.gif "wcfc_TurningFeaturesOnOrOff2")

 IIS 7.0 は既定でインストールされています。 このインストールでは、IIS 7.0 を使用して、静的コンテンツ (HTML ページやその他のコンテンツなど) を処理できます。 ただし、ASP.NET または CGI アプリケーションを実行したり、WCF サービスをホストしたりすることはできません。

## <a name="iis-70-installation-with-aspnet-support"></a>IIS 7.0 : ASP.NET サポートを行うインストール
 ASP.NET を IIS 7.0 で動作させるには、ASP.NET をインストールする必要があります。 **ASP.NET**を確認すると、画面は次の図のようになります。

 ![Asp.NET の必須設定](../../../../docs/framework/wcf/feature-details/media/wcfc-trunfeaturesonoroff3s.gif "wcfc_TrunFeaturesOnOrOFf3s")

 これは、WCF と ASP.NET の両方のアプリケーションが IIS 7.0 で動作するための最小限の環境です。

## <a name="iis-70-installation-with-iis-60-compatibility-components"></a>IIS 7.0 : IIS 6.0 互換コンポーネントを備えたインストール
 Visual Studio 2005 を使用しているシステム、または IIS 6.0 メタベース API を使用する仮想アプリケーションを構成するその他の自動化スクリプトまたはツール (Adsutil.vbs など) に IIS 7.0 をインストールする場合は、必ず IIS の 6.0**スクリプトツール**を確認してください。 これにより、IIS 6.0**管理互換性**の他のサブノードが自動的にチェックされます。 次の図は、この処理が完了した後の画面を示しています。

 ![IIS 6.0 の管理互換設定](../../../../docs/framework/wcf/feature-details/media/scfc-turnfeaturesonoroff5s.gif "scfc_TurnFeaturesOnOrOff5s")

 このインストールでは、IIS 7.0、ASP.NET、および WCF の機能と Web で使用可能なサンプルを使用するために必要なすべてのものが揃っています。

## <a name="request-limits"></a>要求制限
 Windows Vista と IIS 7 では、`maxUri` と `maxQueryStringSize` の設定の既定値が変更されています。 既定では、IIS 7.0 における要求のフィルター処理で使用できる文字数は、URL が 4096 文字、クエリ文字列が 2048 文字です。 これらの既定値を変更するには、App.config ファイルに次の XML を追加します。

```xml
 <system.webServer>
    <security>
        <requestFiltering>
            <requestLimits maxUrl="8192" maxQueryString="8192" />
        </requestFiltering>
    </security>
 </system.webServer>
 ```

## <a name="see-also"></a>関連項目

- [WAS アクティベーション アーキテクチャ](../../../../docs/framework/wcf/feature-details/was-activation-architecture.md)
- [WCF で使用するための WAS を設定する](../../../../docs/framework/wcf/feature-details/configuring-the-wpa--service-for-use-with-wcf.md)
- [方法 : WCF アクティブ化コンポーネントをインストールして設定する](../../../../docs/framework/wcf/feature-details/how-to-install-and-configure-wcf-activation-components.md)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
