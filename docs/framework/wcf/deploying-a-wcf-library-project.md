---
description: 詳細については、「WCF ライブラリプロジェクトの配置」を参照してください。
title: WCF ライブラリ プロジェクトの配置
ms.date: 03/30/2017
ms.assetid: 9f9222fe-d358-443c-9a49-12c5498e35e7
ms.openlocfilehash: 1476f564e3e341c77ab9ba4e4281d6f242a735cd
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99646102"
---
# <a name="deploying-a-wcf-library-project"></a>WCF ライブラリ プロジェクトの配置

このトピックでは、Windows Communication Foundation (WCF) サービスライブラリプロジェクトを配置する方法について説明します。  
  
## <a name="deploying-a-wcf-service-library"></a>WCF サービス ライブラリの配置  

 WCF サービスライブラリは、ダイナミックリンクライブラリ (DLL) です。 それ自体を単独で実行することはできません。 ホスティング環境に配置する必要があります。 このプロセスの詳細については、「 [WCF サービスのホストと](/previous-versions/dotnet/articles/bb332338(v=msdn.10))使用」を参照してください。  
  
 WCF サービスライブラリは、他の WCF サービスと同様にデプロイできます。 ただし、.NET Framework は Dll の構成をサポートしていないことに注意してください。 <xref:System.Configuration> では、アプリケーション ドメイン 1 つにつき、1 つの構成ファイルがサポートされています。 WCF サービスライブラリプロジェクトでは、開発時にライブラリの App.config ファイルを提供することによって、この制限を軽減します。 ただし、配置後、この App.config ファイルは認識されません。  
  
 構成コードは、ホスティング環境で認識されている構成ファイルに移動する必要があります。 自己ホストを行うには、App.config ファイルの内容をホスティング実行可能形式の App.config ファイルにコピーしてください。 サービスのホスティングに IIS を使用する場合は、App.config ファイルの内容を仮想ディレクトリの Web.config ファイルにコピーする必要があります。
