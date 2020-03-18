---
title: 登録を必要としない COM 相互運用機能
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], interop
- COM interop, registration-free COM interop
- registration-free COM interop
- manifests [.NET Framework]
- registration-free activation
- object activation
- registration-free COM interop, about registration-free COM interop
ms.assetid: 90f308b9-82dc-414a-bce1-77e0155e56bd
ms.openlocfilehash: f73bb2ac9e1b942c08f83507589b534d7e67748d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124971"
---
# <a name="registration-free-com-interop"></a>登録を必要としない COM 相互運用機能
登録を必要としない COM 相互運用機能は、アセンブリ情報を格納するために Windows レジストリを使用しないで、コンポーネントをアクティブにします。 展開中にコンピューター上のコンポーネントを登録するのではなく、バインディングとアクティベーションに関する情報を含む Win32 スタイルのマニフェスト ファイルをデザイン時に作成します。 レジストリ キーではなく、これらのマニフェスト ファイルが、オブジェクトのアクティベーションを指示します。  
  
 展開中に登録するのではなく、登録を必要としないアクティベーションをアセンブリに使用することには、次の 2 つの利点があります。  
  
- 複数のバージョンの DLL がコンピューターにインストールされているとき、どのバージョンをアクティブ化するかを制御できます。  
  
- エンドユーザーは XCOPY または FTP を使用して、アプリケーションをコンピューターの適切なディレクトリにコピーできます。 その後、そのディレクトリでアプリケーションを実行できます。  
  
 このセクションでは、登録を必要としない COM 相互運用機能に必要な 2 つの種類のマニフェストについて説明します。それらはアプリケーション マニフェストとコンポーネント マニフェストです。 これらのマニフェストは、XML ファイルです。 アプリケーション マニフェストは、アプリケーション開発者によって作成され、アセンブリやアセンブリの依存関係を記述するメタデータを含んでいます。 コンポーネント マニフェストは、コンポーネント開発者によって作成されるものであり、そこには、本来 Windows レジストリに存在する情報が含まれています。  
  
### <a name="requirements-for-registration-free-com-interop"></a>登録を必要としない COM 相互運用機能の要件  
  
1. 登録を必要としない COM 相互運用機能のサポートは、ライブラリ アセンブリの種類に応じて、具体的にはアセンブリがアンマネージド (COM side-by-side) であるかマネージド (.NET ベース) であるかによって、多少異なります。 次の表は、それぞれのアセンブリの種類について、オペレーティング システムと .NET Framework のバージョン要件を示しています。  
  
    |アセンブリの種類|オペレーティング システム|.NET Framework のバージョン|  
    |-------------------|----------------------|----------------------------|  
    |COM side-by-side|Microsoft Windows XP|不要。|  
    |.NET ベース|Windows XP SP2|.NET Framework バージョン 1.1|  
  
     Windows Server 2003 ファミリも、.NET ベースのアセンブリで、登録を必要としない COM 相互運用機能をサポートしています。  
  
     NET ベースのクラスが COM からの登録を必要としないアクティブ化と互換性を持つためには、そのクラスがパラメーターなしのコンストラクターを持ち、パブリックであることが必要です。  
  
### <a name="configuring-com-components-for-registration-free-activation"></a>登録を必要としないアクティベーション用の COM コンポーネントの構成  
  
1. COM コンポーネントが登録を必要としないアクティベーションに含まれるようにするには、それを side-by-side アセンブリとして展開する必要があります。 Side-by-side アセンブリはアンマネージ アセンブリです。  詳しくは、「[Using Side-by-side Assemblies](/windows/desktop/SbsCs/using-side-by-side-assemblies)」(side-by-side アセンブリの使用) をご覧ください。  
  
     COM の side-by-side アセンブリを使用するには、.NET ベースのアプリケーション開発者が、バインディングとアクティベーションの情報を含むアプリケーション マニフェストを提供する必要があります。 Windows XP オペレーティング システムには、アンマネージの side-by-side アセンブリのサポートが組み込まれています。 オペレーティング システムでサポートされている COM ランタイムは、アクティブ化されるコンポーネントがレジストリに存在しない場合、アプリケーション マニフェストをスキャンしてアクティベーション情報を見つけます。  
  
     登録を必要としないアクティベーションは、Windows XP にインストールされている COM コンポーネントでは省略可能です。 アプリケーションに side-by-side アセンブリを追加する方法の詳細な手順については、「[Using Side-by-side Assemblies](/windows/desktop/SbsCs/using-side-by-side-assemblies)」(side-by-side アセンブリの使用) をご覧ください。  
  
    > [!NOTE]
    > Side-by-side 実行は、ランタイムの複数のバージョンと、特定のバージョンのランタイムを使用するアプリケーションおよびコンポーネントの複数のバージョンを、同一のコンピューター上で同時に実行できるようにするための、.NET Framework の機能です。 Side-by-side 実行と side-by-side アセンブリは、side-by-side 機能を提供するための別個のメカニズムです。  
  
## <a name="see-also"></a>関連項目

- [方法: 登録を必要としないアクティベーション用の .NET Framework ベースの COM コンポーネントを構成する](configure-net-framework-based-com-components-for-reg.md)
