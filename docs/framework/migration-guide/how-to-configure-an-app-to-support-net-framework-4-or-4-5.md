---
title: '方法: .NET Framework 4 以降のバージョンをサポートするアプリを構成する'
ms.date: 03/30/2017
helpviewer_keywords:
- configuring apps to support .NET Framework
- .NET Framework, configuring apps
ms.assetid: 63c6b9a8-0088-4077-9aa3-521ab7290f79
ms.openlocfilehash: 586f39fc9b50dcd45bb959ebd0063e3c38d9c3ed
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75716247"
---
# <a name="how-to-configure-an-app-to-support-net-framework-4-or-later-versions"></a>方法: .NET Framework 4 以降のバージョンをサポートするアプリを構成する

共通言語ランタイム (CLR) をホストするすべてのアプリでは、マネージド コードを実行するために CLR を開始または*アクティブ化*する必要があります。 通常、.NET Framework アプリはビルドされた CLR のバージョンで実行されますが、アプリケーション構成ファイル (app.config ファイルと呼ばれることもあります) を使用して、デスクトップ アプリのこの動作を変更できます。 ただし、アプリケーション構成ファイルを使用して Windows ストア アプリまたは Windows Phone アプリの既定のアクティベーション動作は変更できません。 この記事では、デスクトップ アプリを .NET Framework の別のバージョンで実行できるようにする方法を説明し、バージョン 4 以降を対象とする方法の例を示します。

 アプリが実行される .NET Framework のバージョンは、次の順序で決まります。

- 構成ファイル。

     アプリケーション構成ファイルに 1 つ以上の .NET Framework のバージョンを指定する [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) エントリがあり、これらのバージョンの 1 つがユーザーのコンピューターにある場合、アプリはそのバージョンで実行されます。 構成ファイルは、一覧表示された順序で [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) エントリを読み込み、ユーザーのコンピューターにある、最初に表示される .NET Framework バージョンを使用します (Version 1.0 では [\<requiredRuntime>](../configure-apps/file-schema/startup/requiredruntime-element.md) 要素を使用します)。

- コンパイル後のバージョン。

     構成ファイルがなく、アプリがビルドされた .NET Framework のバージョンがユーザーのコンピューターに存在する場合、アプリはそのバージョンで実行されます。

- インストールされた最新バージョン。

     アプリがビルドされた .NET Framework のバージョンがなく、構成ファイルの [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 要素でバージョンが指定されていない場合、アプリはユーザーのコンピューターにある .NET Framework の最新バージョンで実行が試みられます。

     ただし、.NET Framework 1.0、1.1、2.0、3.0、および 3.5 のアプリは、.NET Framework 4 以降では自動的に実行されず、場合によっては、エラーが発生し、.NET Framework 3.5 をインストールするように求めるメッセージが表示されることがあります。 また、Windows システムの異なるバージョンには .NET Framework の異なるバージョンが含まれるので、アクティベーション動作はユーザーのオペレーティング システムに応じて異なる場合があります。 アプリが .NET Framework 3.5 と 4、またはそれ以降をサポートしている場合は、構成ファイルの複数のエントリでこれを示して .NET Framework 初期化エラーを回避することをお勧めします。 詳細については、「[.NET Framework のバージョンおよび依存関係](versions-and-dependencies.md)」を参照してください。

 また、.NET Framework 3.5 がコンピューターにインストールされている場合でも、.NET Framework 4 以降のバージョンで実行できるように .NET Framework 3.5 アプリを構成して、Version 4 以降のバージョンのパフォーマンス向上を活用することもできます。

> [!IMPORTANT]
> サポートするすべての .NET Framework のバージョンで必ずアプリをテストすることをお勧めします。 以降の .NET Framework のバージョンをサポートするためのアプリケーションのアップグレードについての詳細は、「[.NET Framework のバージョンの互換性](version-compatibility.md)」を参照してください。

 .NET Framework 1.0 および 1.1 アプリを変更して Windows 7 と Windows 8 をサポートする方法の詳細については、「[.NET Framework 1.1 からの移行](migrating-from-the-net-framework-1-1.md)」を参照してください。

## <a name="to-configure-your-app-to-run-on-the-net-framework-4-or-later-versions"></a>.NET Framework 4 以降のバージョンで実行できるようにアプリを構成するには

1. .NET Framework プロジェクトの構成ファイルを追加または検索します。 アプリの構成ファイルは、同じディレクトリにあり、アプリと同じ名前ですが、.config 拡張子があります。 たとえば、アプリが MyExecutable.exe という名前であれば、アプリケーション構成ファイルの名前は MyExecutable.exe.config です。

     構成ファイルを追加するには、Visual Studio のメニュー バーで **[プロジェクト]** 、 **[新しい項目の追加]** の順にクリックします。 左ペインで **[全般]** をクリックし、 **[構成ファイル]** をクリックします。 構成ファイルに *appName*.exe.config という名前を付けます。これらのメニューの選択は、Windows ストア アプリ プロジェクトまたは Windows phone アプリ プロジェクトでは使用できません。これらのプラットフォームのアクティベーション ポリシーを変更できないためです。

2. 次のように、アプリケーション構成ファイルに [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 要素を追加します。

    ```xml
    <configuration>
      <startup>
        <supportedRuntime version="<version>"/>
      </startup>
    </configuration>
    ```

     *\<version>* が、アプリがサポートする .NET Framework のバージョンに合わせて CLR バージョンを指定する場合。 次の文字列を使用します。

    - .NET Framework 1.0: 「v1.0.3705」

    - .NET Framework 1.1: 「v1.1.4322」

    - .NET Framework 2.0、3.0、および 3.5: 「v2.0.50727」

    - .NET Framework 4 以降のバージョン: "v4.0"

     複数の [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 要素を優先順に追加して、複数バージョンの .NET Framework のサポートを指定できます。

 アプリケーション構成ファイルの設定およびコンピューターにインストールされている .NET Framework のバージョンによって、.NET Framework 3.5 アプリが実行されるバージョンがどのように決定されるかを次の表に示します。 この例は、.NET Framework 3.5 のアプリケーションに固有ですが、以前の .NET Framework バージョンで作成されたアプリケーションを対象にする場合も同様のロジックを使用できます。 アプリケーション構成ファイルで .NET Framework 3.5 を指定するために、.NET Framework 2.0 のバージョン番号 (v2.0.50727) が使用されることに注意してください。

|App.config ファイルの設定|バージョン 3.5 がインストールされているコンピューター|3\.5、4、それ以降のバージョンがインストールされているコンピューター|4 以降のバージョンがインストールされているコンピューター|
|-|-|-|-|
|None|3\.5 で動作|3\.5 で動作|ユーザーに正しいバージョン * をインストールするように求めるエラー メッセージが表示されます。|
|`<supportedRuntime version="v2.0.50727"/>`|3\.5 で動作|3\.5 で動作|ユーザーに正しいバージョン * をインストールするように求めるエラー メッセージが表示されます。|
|`<supportedRuntime version="v2.0.50727"/>` <br /> `<supportedRuntime version="v4.0"/>`|3\.5 で動作|3\.5 で動作|4 以降のバージョンで動作|
|`<supportedRuntime version="v4.0"/>` <br /> `<supportedRuntime version="v2.0.50727"/>`|3\.5 で動作|4 以降のバージョンで動作|4 以降のバージョンで動作|
|`<supportedRuntime version="v4.0"/>`|ユーザーに正しいバージョン * をインストールするように求めるエラー メッセージが表示されます。|4 以降のバージョンで動作|4 以降のバージョンで動作|

 \* このエラー メッセージの詳細と回避方法については、「[.NET Framework 初期化エラー: ユーザー エクスペリエンスの管理](../deployment/initialization-errors-managing-the-user-experience.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [.NET Framework 1.1 からの移行](migrating-from-the-net-framework-1-1.md)
- [移行ガイド](index.md)
