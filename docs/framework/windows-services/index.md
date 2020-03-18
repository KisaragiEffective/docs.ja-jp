---
title: Windows サービス アプリケーションの開発
ms.date: 03/30/2017
helpviewer_keywords:
- ServiceInstaller class, Windows Service applications
- Service class, Windows Service applications
- Windows Service applications
- Windows NT services
- ServiceProcessInstaller class, Windows Service applications
- services
- .NET applications, Windows applications
ms.assetid: ba72d648-9553-4849-b829-069ad5ea014b
author: ghogen
ms.openlocfilehash: 61f969c22ac06bd6ed20ccfa9124db3bb35d0692
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71053542"
---
# <a name="develop-windows-service-apps"></a>Windows サービス アプリを開発する

Visual Studio または .NET Framework SDK を使用すると、サービスとしてインストールするアプリケーションを作成することで簡単に作成できます。 この種類のアプリケーションは、Windows サービスと呼ばれます。 フレームワーク機能を使用することで、サービスを作成、インストール、開始、停止したり、動作を制御したりできます。

> [!NOTE]
> Visual Studio では、必要に応じて既存の C++ コードと相互運用できる Visual C# または Visual Basic のマネージド コードでサービスを作成できます。 また、[ATL プロジェクト ウィザード](/cpp/atl/reference/atl-project-wizard)を使用して、ネイティブ C ++で Windows サービスを作成することもできます。

## <a name="in-this-section"></a>このセクションの内容

[Windows サービス アプリケーションの概要](introduction-to-windows-service-applications.md)

Windows サービス アプリケーションの概要、サービスの有効期間、およびサービスがその他の一般的な種類のプロジェクトどのように異なるかを説明します。

[チュートリアル: コンポーネント デザイナーによる Windows サービス アプリケーションの作成](walkthrough-creating-a-windows-service-application-in-the-component-designer.md)

Visual Basic および Visual C# でサービスを作成する例を提供します。

[サービス アプリケーションのプログラミング アーキテクチャ](service-application-programming-architecture.md)

サービスのプログラミングで使用される言語要素について説明します。

[方法 : Windows サービスを作成する](how-to-create-windows-services.md)

Windows サービス プロジェクトのテンプレートを使用して Windows サービスを作成、構成するプロセスについて説明します。

## <a name="related-sections"></a>関連項目

<xref:System.ServiceProcess.ServiceBase>: サービスの作成に使用される、<xref:System.ServiceProcess.ServiceBase> クラスの主要な機能を説明します。

<xref:System.ServiceProcess.ServiceProcessInstaller>: <xref:System.ServiceProcess.ServiceProcessInstaller> クラスと共に使用して、サービスをインストール/アンインストールする <xref:System.ServiceProcess.ServiceInstaller> クラスの機能について説明します。

<xref:System.ServiceProcess.ServiceInstaller>: <xref:System.ServiceProcess.ServiceInstaller> クラスと共に使用して、サービスをインストール/アンインストールする <xref:System.ServiceProcess.ServiceProcessInstaller> クラスの機能について説明します。

[テンプレートからプロジェクトを作成する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/0fyc0azh(v=vs.120)): この章で使用されるプロジェクトの種類と、それを選択する際に役立つガイドラインを説明します。
