---
description: '詳細情報: アプリケーションの構成'
title: アプリケーションの構成
ms.date: 03/30/2017
ms.assetid: a2f995b0-669d-4721-b00f-4561ec7eb6a4
ms.openlocfilehash: d28876068f23a67ea1ff005d7d217e09b2854783
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99646063"
---
# <a name="configuring-your-application"></a>アプリケーションの構成

Windows Communication Foundation (WCF) では、.NET 構成システムを使用して、コンピューターとアプリケーションのスコープでサービスを構成できるようにします。  
  
 WCF によって定義された構成設定は、`<system.serviceModel>` セクション グループにあります。 WCF サービスの構成方法の詳細については、次のトピックを参照してください。  
  
- [サービスの構成](../configuring-services.md)  
  
- [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md)  
  
 アプリケーション定義の構成設定は、`<appSettings>` セクション グループで定義されています。 .NET 構成ファイルでのアプリケーション設定について詳しくは、「[\<appSettings>](/previous-versions/dotnet/netframework-4.0/ms228154(v=vs.100))」を参照してください。  
  
## <a name="using-the-configuration-editor"></a>構成エディターの使用  

 管理者と開発者は、WCF [構成エディター ツール (SvcConfigEditor.exe)](../configuration-editor-tool-svcconfigeditor-exe.md) のグラフィカル ユーザー インターフェイスを使用して、WCF サービスの構成設定を作成および変更できます。 このツールを使用すると、XML 構成ファイルを直接編集せずに、WCF のバインディング、動作、サービス、および診断の設定を管理できます。  
  
## <a name="editing-configuration-files-in-visual-studio"></a>Visual Studio の構成ファイルの編集  

 Visual Studio で WCF サービス プロジェクトの構成ファイルを編集するには、**ソリューション エクスプローラー** で右クリックし、 **[Edit WCF Config]\(WCF 構成の編集\)** コンテキスト メニュー項目を選択します。 これにより、[構成エディター ツール (SvcConfigEditor.exe)](../configuration-editor-tool-svcconfigeditor-exe.md) が起動します。  
  
> [!NOTE]
> Visual Studio の WCF Web サービス プロジェクトの構成ファイルを **ソリューション エクスプローラー** で右クリックして編集する場合は、 **[Edit WCF Config]\(WCF 構成の編集\)** コンテキスト メニュー項目が表示されません。 この問題を回避するには、 **[ツール]** メニューをクリックし、 **[WCF Service Config Editor]\(WCF サービス構成エディター\)** を選択します。 この操作を行った後は、構成ファイルを右クリックして **[Edit WCF Config]\(WCF 構成の編集\)** コンテキスト メニュー項目を表示できます。  
  
## <a name="see-also"></a>関連項目

- [構成エディター ツール (SvcConfigEditor.exe)](../configuration-editor-tool-svcconfigeditor-exe.md)
- [サービスの構成](../configuring-services.md)
- [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md)
