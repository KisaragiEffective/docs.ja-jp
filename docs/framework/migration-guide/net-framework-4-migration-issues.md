---
title: .NET Framework 4 への移行に関する問題
ms.date: 05/02/2017
helpviewer_keywords:
- .NET Framework 4, migration
- application compatibility
ms.assetid: df478548-8c05-4de2-8ba7-adcdbe1c2a60
ms.openlocfilehash: c1c3298d87ad0f481fa30182e40cd5edcd535d6a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73975617"
---
# <a name="net-framework-4-migration-issues"></a>.NET Framework 4 への移行に関する問題

このトピックでは、.NET Framework バージョン 3.5 Service Pack 1 から .NET Framework バージョン 4 への移行に関する問題、修正、標準への準拠やセキュリティに関する変更、お客様のフィードバックに基づいた変更などについて説明します。 これらの変更点のほとんどは、アプリケーションのプログラミング変更を必要としません。 変更が必要になる可能性のある変更点については、表の「推奨される変更」列をご覧ください。 目立った変更点は、ASP.NET や Windows Presentation Foundation (WPF) などの領域別に分類されています。

このトピックで取り上げる問題の大まかな概要については、「[.NET Framework 4 移行ガイド](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ff657133%28v=vs.100%29)」をご覧ください。

新しい機能については、「[.NET Framework 4 の新機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms171868%28v=vs.100%29)」をご覧ください。

## <a name="aspnet-and-web"></a>ASP.NET と Web

名前空間: <xref:System.Web>、<xref:System.Web.Mobile>、<xref:System.Web.Security>、<xref:System.Web.UI.WebControls>。アセンブリ: System.Web (System.Web.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **ブラウザー定義ファイル** | ブラウザー定義ファイルは、新しいブラウザーとデバイスや、更新されたブラウザーとデバイスに関する情報を含むように、更新されました。 Netscape Navigator などの古いブラウザーとデバイスは削除され、Google Chrome や Apple iPhone などの新しいブラウザーとデバイスが追加されました。<br><br>削除されたブラウザー定義の 1 つを継承するカスタム ブラウザー定義がご利用のアプリケーションに含まれている場合は、エラーが表示されます。<br><br><xref:System.Web.HttpBrowserCapabilities> オブジェクト (ページの `Request.Browse` プロパティで公開されます) は、ブラウザー定義ファイルによって実行されます。 そのため、ASP.NET 4 でこのオブジェクトのプロパティにアクセスすることで返される情報は、旧バージョンの ASP.NET で返された情報とは異なる場合があります。 | ご利用のアプリケーションが古いブラウザー定義ファイルに依存している場合は、次のフォルダーからそれらのファイルをコピーできます。<br><br>*Windows\\Microsoft.NET\\Framework\\v2.0.50727\\CONFIG\\Browsers*<br><br>ASP.NET 4 の対応する *\\CONFIG\\Browsers* フォルダーにファイルをコピーします。 ファイルをコピーしたら、[Aspnet_regbrowsers.exe](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms229858(v=vs.90)) コマンドライン ツールを実行します。 詳細については、Web サイト [https://www.asp.net/mobile](/aspnet/mobile/overview) を参照してください。 |
| **ASP.NET の複数のバージョンが混在する環境で実行される子アプリケーション** | 旧バージョンの ASP.NET を実行する子アプリケーションとして構成されている ASP.NET 4 アプリケーションは、構成エラーまたはコンパイル エラーにより起動しない場合があります。 実際に発生するエラーは、アプリケーションが実行される IIS のバージョン (6.0、7、または 7.5) によって異なります。 | 影響を受けるアプリケーションの構成ファイルを変更することで、構成システムが ASP.NET 4 アプリケーションを適切に認識するように設定できます。 実行する必要がある変更については、ASP.NET Web サイトの「[ASP.NET 4 Breaking Changes (ASP.NET 4 の互換性に影響する変更点)](/aspnet/whitepapers/aspnet4/breaking-changes)」の「ASP.NET 4 Child Applications Fail to Start When Under ASP.NET 2.0 or ASP.NET 3.5 Applications (ASP.NET 2.0 または ASP.NET 3.5 アプリケーションで ASP.NET 4 の子アプリケーションが起動しない)」をご覧ください。 |
| **ClientID の変更** | ASP.NET 4 の新しい `clientIDMode` 設定では、ASP.NET が HTML 要素の `id` 属性を生成する方法を指定できます。 以前のバージョンの ASP.NET では、既定の動作は `clientIDMode` の `AutoID` 設定と同じでした。 現在の既定の設定は `Predictable` です。 詳細については、[ASP.NET Web サーバー コントロールの識別](https://docs.microsoft.com/previous-versions/aspnet/1d04y8ss%28v=vs.100%29)に関する記事をご覧ください。 | Visual Studio を使用して ASP.NET 2.0 または ASP.NET 3.5 からアプリケーションをアップグレードする場合は、旧バージョンの .NET Framework の動作を維持する設定が、ツールによって Web.config ファイルに自動的に追加されます。 ただし、IIS のアプリケーション プールを変更して .NET Framework 4 をターゲットにするようにアプリケーションをアップグレードする場合、ASP.NET では新しいモードが既定で使用されます。 新しいクライアント ID モードを無効にするには、Web.config ファイルに次の設定を追加します。<br><br>`<pages clientIDMode="AutoID" />` |
| **コード アクセス セキュリティ (CAS)** | ASP.NET 3.5 で追加された ASP.NET 2.0 NET 機能は、.NET Framework 1.1 と .NET Framework 2.0 のコード アクセス セキュリティ (CAS) モデルを使用します。 ただし、ASP.NET 4 での CAS の実装は大幅に見直されました。 その結果、グローバル アセンブリ キャッシュで実行されている信頼されるコードに依存する、部分的に信頼された ASP.NET アプリケーションの実行は、さまざまなセキュリティ例外で失敗することがあります。 マシンの CAS ポリシーに対する広範な変更に依存する、部分的に信頼されたアプリケーションも、失敗してセキュリティ例外をスローすることがあります。 | 次の例に示すように、`trust` 構成要素の新しい `legacyCasModel` 属性を使用して、部分的に信頼された ASP.NET 4 アプリケーションを ASP.NET 1.1 および 2.0 の動作に戻すことができます。<br><br>`<trust level= "Medium" legacyCasModel="true" />`<br><br>重要 : より古い CAS モデルに戻すと、セキュリティが低下する可能性があります。<br><br>新しい ASP.NET 4 コード アクセス セキュリティ モデルの詳細については、「[ASP.NET 4 アプリケーションのコード アクセス セキュリティ](https://docs.microsoft.com/previous-versions/dd984947(v=vs.100))」をご覧ください。 |
| **構成ファイル** | .NET Framework と ASP.NET 4 のルート構成ファイル (machine.config ファイルとルート Web.config ファイル) は、ASP.NET 3.5 のアプリケーション Web.config ファイルに記述されていた定型構成情報の大部分を含むように更新されました。 マネージド IIS 7 および IIS 7.5 構成システムは複雑であるため、ASP.NET 4 の下で、および IIS 7 と IIS 7.5 の下で ASP.NET 3.5 アプリケーションを実行すると、ASP.NET エラーまたは IIS エラーが発生することがあります。 | Visual Studio のプロジェクト アップグレード ツールを使用して、ASP.NET 3.5 アプリケーションを ASP.NET 4 にアップグレードします。 Visual Studio 2010 は、ASP.NET 4 の適切な設定を含むように ASP.NET 3.5 アプリケーションの Web.config ファイルを自動的に変更します。<br><br>ただし、ASP.NET 3.5 アプリケーションは、再コンパイルしないで .NET Framework 4 を使用して実行できます。 その場合は、アプリケーションの Web.config ファイルを手動で変更してから、.NET Framework 4 の下で、および IIS 7 または IIS 7.5 の下でアプリケーションの実行が必要になる場合があります。 具体的にどのような変更を行う必要があるかについては、Service Pack (SP) リリースなど、使用しているソフトウェアの組み合わせによって異なります。 この変更点の影響を受ける可能性のあるソフトウェアの組み合わせと、特定の組み合わせでの問題を解決する方法については、ASP.NET Web サイトの「[ASP.NET 4 Breaking Changes (ASP.NET 4 の互換性に影響する変更点)](/aspnet/whitepapers/aspnet4/breaking-changes)」の「Configuration Errors Related to New ASP.NET 4 Root Configuration (新しい ASP.NET 4 ルート構成に関連する構成エラー)」をご覧ください。 |
| **コントロールの表示** | 以前のバージョンの ASP.NET では、一部のコントロールで、無効にできないマークアップが生成されていました。 ASP.NET 4 では、この種類のマークアップが既定で生成されなくなりました。 表示に関する変更点は、次のコントロールに影響します。<br><br>* `Image` コントロールと `ImageButton` コントロールでは、`border="0"` 属性は表示されなくなりました。<br>* `BaseValidator` クラスとそこから派生する検証コントロールでは、既定で赤いテキストが表示されなくなりました。<br>* `HtmlForm` コントロールでは、`name` 属性は表示されません。<br>* `Table` コントロールでは、`border="0"` 属性は表示されなくなりました。<br><br>ユーザー入力用にデザインされていないコントロール (たとえば、`Label` コントロール) では、`Enabled` プロパティが `false` に設定されている場合 (または、コンテナー コントロールからこの設定を継承する場合) に `disabled="disabled"` 属性が表示されなくなりました。 | Visual Studio を使用して ASP.NET 2.0 または ASP.NET 3.5 からアプリケーションをアップグレードする場合は、古い表示を維持する設定が、ツールによって Web.config ファイルに自動的に追加されます。 ただし、IIS のアプリケーション プールを変更して .NET Framework 4 をターゲットにするようにアプリケーションをアップグレードする場合、ASP.NET では新しい表示モードが既定で使用されます。 新しい表示モードを無効にするには、Web.config ファイルに次の設定を追加します。<br><br>`<pages controlRenderingCompatibilityVersion="3.5" />` |
| **既定のドキュメントのイベント ハンドラー** | ASP.NET 4 では、既定のドキュメントがマップされている拡張子なしの URL に対して要求が行われた場合に、HTML `form` 要素の `action` 属性の値を空の文字列として表示します。 旧リリースの ASP.NET では、`http://contoso.com` に対して要求すると Default.aspx に対する要求が生成されました。 そのドキュメントでは、`form` 開始タグが次の例のように表示されました。<br><br>`<form action="Default.aspx" />`<br><br>ASP.NET 4 では、`http://contoso.com` に対する要求によって同じく Default.aspx に対する要求が生成されますが、ASP.NET は現在 HTML の `form` 開始タグを次の例のように表示します。<br><br>`<form action="" />`<br><br>`action` 属性が空の文字列の場合、IIS `DefaultDocumentModule` オブジェクトは Default.aspx への子要求を作成します。 ほとんどの状況では、この子要求はアプリケーション コードに対して透過的であり、Default.aspx ページは正常に実行されます。 ただし、マネージド コードと IIS 7 または IIS 7.5 統合モードとの間のやり取りによって、マネージド .aspx ページが子要求中に正常に動作を停止することがあります。 次の状況が発生した場合、既定の .aspx ドキュメントへの子要求は、エラーになるか予期しない動作をします。<br><br>* `form` 要素の `action` 属性が "" に設定されて .aspx ページがブラウザーに送信された場合。<br>* フォームが ASP.NET にポストバックされた場合。<br>* マネージド HTTP モジュールが、`Request.Form` や `Request.Params` など、エンティティ本体の一部を読み取った場合。 これにより、POST 要求のエンティティ本体がマネージド メモリに読み込まれます。 その結果、エンティティ本体は、IIS 7 または IIS 7.5 統合モードで実行されているネイティブ コード モジュールから使用できなくなります。<br>* IIS `DefaultDocumentModule` オブジェクトが最終的に実行され、Default.aspx ドキュメントへの子要求を作成する場合。 ただし、エンティティ本体はマネージド コードの一部によって既に読み取られているため、子要求に対する送信に使用できるエンティティ本体はありません。<br>* HTTP パイプラインが子要求に対して実行されたときに、.aspx ファイルのハンドラーがハンドラー実行フェーズ中に実行された場合。<br><br>エンティティ本体はないため、フォーム変数とビュー ステートはありません。 したがって、.aspx ページ ハンドラーが (イベントが存在する場合) どのイベントを発生させる必要があるかを判断するために使用できる情報がありません。 その結果、影響を受ける .aspx ページのポストバック イベント ハンドラーは実行されません。 | この変更により発生する可能性のある問題の対処方法については、ASP.NET Web サイトの「[ASP.NET 4 Breaking Changes (ASP.NET 4 の互換性に影響する変更点)](/aspnet/whitepapers/aspnet4/breaking-changes)」の「Event Handlers Might Not Be Not Raised in a Default Document in IIS 7 or IIS 7.5 Integrated Mode (IIS 7 または IIS 7.5 統合モードの既定のドキュメントで起動しない可能性のあるイベント ハンドラー)」をご覧ください。 |
| **ハッシュ アルゴリズム** | ASP.NET では、暗号化とハッシュ アルゴリズムの両方を使用して、フォーム認証 Cookie やビュー ステートなどのデータをセキュリティで保護します。 既定では、ASP.NET 4 は Cookie とビュー ステートのハッシュ操作に <xref:System.Security.Cryptography.HMACSHA256> アルゴリズムを使用します。 旧バージョンの ASP.NET は、古い <xref:System.Security.Cryptography.HMACSHA1> アルゴリズムを使用していました。 | ASP.NET 2.0 と ASP.NET 4 が混在し、フォーム認証 Cookie などのデータが複数の .NET Framework バージョンで動作する必要のあるアプリケーションを実行する場合は、Web.config ファイルに次の設定を追加して ASP.NET 4 Web アプリケーションを構成し、古い <xref:System.Security.Cryptography.HMACSHA1> アルゴリズムを使用するようにします。<br><br>`<machineKey validation="SHA1" />` |
| **Internet Explorer でのコントロールのホスティング** | Web でコントロールをホストするための優れたソリューションが存在するため、Windows フォーム コントロールを Internet Explorer でホストすることができなくなりました。 このため、IEHost.dll と IEExec.exe アセンブリは .NET Framework から削除されました。 | Web アプリケーションのカスタム コントロールの開発には、次のテクノロジが使用できます。<br><br>* Silverlight アプリケーションを作成し、ブラウザー外で実行されるように構成できます。 詳細については、「[ブラウザー外実行のサポート](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/dd550721%28v=vs.95%29)」をご覧ください。<br>* XAML ブラウザー アプリケーション (XBAP) を構築し、WPF 機能を活用することができます (クライアント マシン上に .NET Framework が必要です)。 詳細については、「[WPF XAML ブラウザー アプリケーションの概要](../wpf/app-development/wpf-xaml-browser-applications-overview.md)」をご覧ください。 |
| **HtmlEncode メソッドと UrlEncode メソッド** | <xref:System.Web.HttpUtility> クラスと <xref:System.Web.HttpServerUtility> クラスの `HtmlEncode` メソッドと `UrlEncode` メソッドが更新され、単一引用符文字 (') が次のようにエンコードされるようになりました。<br><br>* `HtmlEncode` メソッドは、単一引用符のインスタンスを `&#39;` としてエンコードします。<br>* `UrlEncode` メソッドは、単一引用符のインスタンスを `%27` としてエンコードします。 | `HtmlEncode` メソッドと `UrlEncode` メソッドが使用されているコード内の箇所を調べ、エンコードの変更によってご使用のアプリケーションが影響を受けてないことを確認してください。 |
| **ASP.NET 2.0 アプリケーションでの HttpException エラー** | ASP.NET 4 を IIS 6 で有効にした後、(Windows Server 2003 または Windows Server 2003 R2 の) IIS 6 で実行された ASP.NET 2.0 アプリケーションで次のようなエラーが発生する可能性があります。`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.` | * Web サイトの実行に ASP.NET 4 が必要ない場合は、サイトをリマップして ASP.NET 2.0 が代わりに使用されるようにします。<br><br>\- または -<br><br>* Web サイトの実行に ASP.NET 4 が必要な場合は、ASP.NET 2.0 の子仮想ディレクトリをすべて、ASP.NET 2.0 にマップされた別の Web サイトに移動させます。<br><br>\- または -<br><br>* 拡張子なしの URL を無効にします。 詳細については、ASP.NET Web サイトの「[ASP.NET 4 Breaking Changes (ASP.NET 4 の互換性に影響する変更点)](/aspnet/whitepapers/aspnet4/breaking-changes)」の「ASP.NET 2.0 Applications Might Generate HttpException Errors That Reference eurl.axd (ASP.NET 2.0 アプリケーションで eurl.axd を参照する HttpException エラーが発生することがある)」をご覧ください。 |
| **メンバーシップ タイプ** | ASP.NET メンバーシップで使用される一部のタイプ (例: <xref:System.Web.Security.MembershipProvider>) は、System.Web.dll から System.Web.ApplicationServices.dll アセンブリに移動しました。 これらのタイプは、クライアントのタイプと拡張 .NET Framework SKU のタイプとの間のアーキテクチャ層の依存関係を解決するために移動しました。 | 旧バージョンの ASP.NET からアップグレードされた、移動したメンバーシップ タイプを使用するクラス ライブラリを ASP.NET 4 プロジェクトで使用すると、コンパイルに失敗することがあります。 この場合は、System.Web.ApplicationServices.dll への参照をクラス ライブラリ プロジェクトに追加します。 |
| **メニュー コントロールの変更** | <xref:System.Web.UI.WebControls.Menu> コントロールの変更によって、以下の動作が発生します。<br><br>* <xref:System.Web.UI.WebControls.MenuRenderingMode> が `List` に設定されている場合、または <xref:System.Web.UI.WebControls.MenuRenderingMode> が `Default` に設定され、<xref:System.Web.Configuration.PagesSection.ControlRenderingCompatibilityVersion> が `4.0` 以降に設定されている場合、<xref:System.Web.UI.WebControls.MenuItem.PopOutImageUrl> プロパティは無効です。<br>* <xref:System.Web.UI.WebControls.Menu.StaticPopOutImageUrl%2A> プロパティと <xref:System.Web.UI.WebControls.Menu.DynamicPopOutImageUrl> プロパティに設定されたパスに円記号 (\\) が含まれている場合、イメージはレンダリングされません (旧バージョンの ASP.NET では、パスに円記号を含めることができました)。 | * 個々のメニュー項目に対して <xref:System.Web.UI.WebControls.MenuItem.PopOutImageUrl> プロパティを設定する代わりに、親の <xref:System.Web.UI.WebControls.Menu> コントロールの <xref:System.Web.UI.WebControls.Menu.StaticPopOutImageUrl%2A> または <xref:System.Web.UI.WebControls.Menu.DynamicPopOutImageUrl> を設定します。<br><br>\- または -<br><br><xref:System.Web.UI.WebControls.MenuRenderingMode> を `Table` に設定します。または、<xref:System.Web.UI.WebControls.MenuRenderingMode> を `Default` に設定し、<xref:System.Web.Configuration.PagesSection.ControlRenderingCompatibilityVersion> を `3.5` に設定します。 これらの設定により、<xref:System.Web.UI.WebControls.Menu> コントロールで、旧バージョンの ASP.NET で使用された HTML テーブル ベースのレイアウトが使用されます。<br>* <xref:System.Web.UI.WebControls.Menu.StaticPopOutImageUrl%2A> プロパティまたは <xref:System.Web.UI.WebControls.Menu.DynamicPopOutImageUrl> プロパティのパスに円記号 (\\) が含まれている場合は、スラッシュ文字 (/) に置き換えます。 |
| **Web.config ファイル内の Mobile アセンブリ** | 以前のバージョンの ASP.NET では、System.Web.Mobile.dll アセンブリへの参照は、`system.web`/`compilation` の `assemblies` セクションにある、ルートの Web.config ファイルに含まれていました。 パフォーマンスを向上させるために、このアセンブリへの参照は削除されました。<br><br>メモ:System.Web.Mobile.dll アセンブリと ASP.NET モバイル コントロールは ASP.NET 4 に含まれていますが、それらの使用は非推奨とされました。 | このアセンブリに含まれる型を使用する場合は、ルートの Web.config ファイルまたはアプリケーションの Web.config ファイルにアセンブリへの参照を追加します。 |
| **出力キャッシュ** | ASP.NET 1.0 のバグが原因で、出力キャッシュの設定として `Location="ServerAndClient"` が指定されたキャッシュ ページでは、応答に `Vary:*` HTTP ヘッダーが生成されていました。 その結果、クライアント ブラウザーに対して、ローカルにページをキャッシュしないように指示がなされていました。 ASP.NET 1.1 では、<xref:System.Web.HttpCachePolicy.SetOmitVaryStar%2A> メソッドが追加され、`Vary:*` ヘッダーを抑制するためにこのメソッドを呼び出すことができました。 ただし、バグ報告では、開発者が既存の `SetOmitVaryStar` の動作を認識していないことが示唆されています。<br><br>ASP.NET 4 では、次のディレクティブを指定する応答から `Vary:*` HTTP ヘッダーは生成されなくなりました。<br><br>`<%@ OutputCache Location="ServerAndClient" %>`<br><br>そのため、<xref:System.Web.HttpCachePolicy.SetOmitVaryStar%2A> ヘッダーを無効にするための `Vary:*` メソッドは不要になりました。 `Location` 属性に "ServerAndClient" を指定するアプリケーションでは、<xref:System.Web.HttpCachePolicy.SetOmitVaryStar%2A> を呼び出す必要なくブラウザーにページをキャッシュできます。 | アプリケーション内のページで `Vary:*` を生成する必要がある場合は、次の例に示すように <xref:System.Web.HttpResponse.AppendHeader%2A> メソッドを呼び出します。<br><br>`System.Web.HttpResponse.AppendHeader("Vary","*");`<br><br>または、出力キャッシュ `Location` 属性の値を "Server" に変更できます。 |
| **ページの解析** | ASP.NET Web ページ (.aspx ファイル) とユーザー コントロール (.ascx ファイル) のページ パーサーは、旧バージョンの ASP.NET よりも ASP.NET 4 の方が厳密であり、無効とみなされて警告が出力されるマークアップの数が、旧バージョンに比べて多くなります。 | ページの実行時に出力されたエラー メッセージを調べて、無効なマークアップが原因で発生したエラーを修正します。 |
| **Passport 型** | Passport (現在の Live ID SDK) の変更により、ASP.NET 2.0 に組み込まれた Passport のサポートは廃止され、サポートされなくなりました。 その結果、<xref:System.Web.Security> の Passport に関連する型は、`ObsoleteAttribute` 属性としてマークされるようになりました。 | <xref:System.Web.Security> 名前空間の Passport 型 (たとえば、<xref:System.Web.Security.PassportIdentity>) を使用するコードは、Windows Live ID SDK を使用するように変更してください。 |
| **FilePath プロパティの PathInfo 情報** | ASP.NET 4 では、<xref:System.Web.HttpRequest.FilePath>、<xref:System.Web.HttpRequest.AppRelativeCurrentExecutionFilePath>、<xref:System.Web.HttpRequest.CurrentExecutionFilePath> などのプロパティからの戻り値に `PathInfo` 値が含まれなくなりました。 代わりに、<xref:System.Web.HttpRequest.PathInfo> 内の `PathInfo` 情報が使用できます。 たとえば、次のような URL フラグメントがあるとします。<br><br>`/testapp/Action.mvc/SomeAction`<br><br>旧バージョンの ASP.NET では、<xref:System.Web.HttpRequest> プロパティは次の値を持ちます。<br><br>* <xref:System.Web.HttpRequest.FilePath>: `/testapp/Action.mvc/SomeAction`<br>* <xref:System.Web.HttpRequest.PathInfo>: (空)<br><br>ASP.NET 4 では、<xref:System.Web.HttpRequest> プロパティは代わりに次の値を持ちます。<br><br>* <xref:System.Web.HttpRequest.FilePath>: `/testapp/Action.mvc`<br>* <xref:System.Web.HttpRequest.PathInfo>: `SomeAction` | <xref:System.Web.HttpRequest> クラスのプロパティに依存してパス情報を返しているコード内の箇所を調べます。コードを変更して、パス情報の返し方に関する変更を反映させてください。 |
| **要求の検証** | 要求の検証を改善するために、ASP.NET 要求の検証は、要求ライフ サイクルの初期に呼び出されます。 その結果、要求の検証は、Web サービスの呼び出しやカスタム ハンドラーに対する要求など、.aspx ファイルが対象ではない要求に対して実行されます。 要求の検証は、カスタム HTTP モジュールが要求処理パイプラインで実行されている場合にもアクティブになります。<br><br>この変更の結果、.aspx ファイル以外のリソースに対する要求では、要求検証エラーがスローされることがあります。 要求パイプラインで実行されるカスタム コード (たとえば、カスタム HTTP モジュール) も、要求検証エラーをスローすることがあります。 | 必要に応じて、Web 構成ファイルで次の設定を使用することにより、.aspx ページでのみ要求検証を起動する古い動作に戻すことができます。<br><br>`<httpRuntime requestValidationMode="2.0" />`<br><br>警告 :古い動作に戻す場合は、既存のハンドラー、モジュール、その他のカスタム コード内のすべてのコードで、XSS 攻撃ベクトルである可能性がある潜在的に安全でない HTTP 入力のチェックが実行されることを確認してください。 |
| **ルーティング** | Visual Studio 2010 でファイル システム Web サイトを作成するときに、その Web サイトがフォルダー名にドット (.) を含むフォルダー内にある場合、URL ルーティングは正常に動作しません。 一部の仮想パスから、HTTP 404 エラーが返されます。 これが発生するのは、Visual Studio 2010 がルート仮想ディレクトリに正しくないパスを使用して Visual Studio 開発サーバーを起動するためです。 | * ファイル ベースの Web サイトの **[プロパティ]** ページで、**仮想パス**属性を "/" に変更します。<br><br>\- または -<br><br>* Web サイト プロジェクトの代わりに、Web アプリケーション プロジェクトを作成します。 Web アプリケーション プロジェクトではこの問題は発生しません。プロジェクト フォルダーの名前にドットが含まれている場合でも、URL ルーティングは動作します。<br><br>\- または -<br><br>* IIS でホストされる HTTP ベースの Web サイトを作成します。 IIS でホストされる Web サイトでは、プロジェクト ファイル フォルダーと同様に、仮想パスでもドットを使用できます。 |
| **SharePoint サイト** | `WSS_Minimal` という名前のカスタムの部分信頼レベルを含む SharePoint Web サイトの子として配置された ASP.NET 4 Web サイトを実行しようとすると、次のエラーが表示されます。<br><br>`Could not find permission set named 'ASP.Net'.` | 現時点では、ASP.NET と互換性がある SharePoint のバージョンはありません。 このため、ASP.NET 4 Web サイトを、SharePoint Web サイトの子として実行しないでください。 |
| **XHTML 1.1 標準** | 新しい Web サイトの XHTML 1.1 への準拠を有効にするために、.NET Framework 4 の ASP.NET コントロールでは、XHTML 1.1 準拠の HTML が生成されます。 この表示は、Web.config ファイルの `<system.Web>` 要素内で次のオプションを使用することで有効にできます。<br><br>`<pages controlRenderingCompatibilityVersion="4.0"/>`<br><br>このオプションは、既定で 4.0 に設定されます。 Visual Studio 2008 からアップグレードされた Web プロジェクトでは、互換性のために 3.5 の設定が有効になります。 | なし。 |

## <a name="core"></a>コア

### <a name="general-features"></a>一般機能

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **CardSpace** | Windows CardSpace は、.NET Framework には含まれる形ではなく、単独で提供されるようになりました。 | [Microsoft ダウンロード センター](https://go.microsoft.com/fwlink/?LinkId=199868)から Windows CardSpace をダウンロードします。 |
| **構成ファイル** | .NET Framework によるアプリケーション構成ファイルへのアクセス方法が修正されました。 | アプリケーション構成ファイルの名前が *アプリケーション名.config* の場合は、*アプリケーション名.exe.config* に変更します。たとえば、*MyApp.config* を *MyApp.exe.config* に変更します。 |
| **C# コード コンパイラ** | <xref:Microsoft.CSharp> 名前空間にあった `Compiler` クラス、`CompilerError` クラス、`ErrorLevel` クラスは使用できなくなったため、これらのアセンブリ (cscompmgd.dll) は .NET Framework に含まれていません。 | <xref:System.CodeDom.Compiler.CodeDomProvider>クラスや <xref:System.CodeDom.Compiler> 名前空間の他のクラスを使用します。 詳細については、「[CodeDOM の使用](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/y2k85ax6%28v=vs.100%29)」をご覧ください。 |
| **ホスティング** (アンマネージ API) | ホスティング機能を改善するために、ホスティング アクティブ化 API の一部が非推奨となくなりました。 インプロセス side-by-side 実行機能を使用すると、アプリケーションは複数のバージョンの .NET Framework を同じプロセスで読み込んで起動することができます。 たとえば、.NET Framework 2.0 SP1 ベースのアドイン (またはコンポーネント) と .NET Framework 4 ベースのアドインを同じプロセスで読み込むアプリケーションを実行できます。 古いコンポーネントは古いバージョンの .NET Framework を引き続き使用し、新しいコンポーネントは新しいバージョンの .NET Framework を使用します。 | 「[インプロセスの side-by-side 実行](../deployment/in-process-side-by-side-execution.md)」で説明されている構成を使用します。 |
| **新しいセキュリティ モデル** | 「[.NET Framework 4 におけるセキュリティの変更点](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd233103%28v=vs.100%29)」で説明しているように、コード アクセス セキュリティ (CAS) ポリシーはオフになり、簡略化されたモデルで置換されています。 | アプリケーションで CAS に依存している場合は、変更が必要になることがあります。 詳細については、「[コード アクセス セキュリティ ポリシーの互換性と移行](../misc/code-access-security-policy-compatibility-and-migration.md)」をご覧ください。 |

### <a name="date-and-time"></a>日付と時刻

名前空間: <xref:System>。アセンブリ: mscorlib (mscorlib.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **夏時間** | システム クロックと一貫させるために、時間プロパティ (<xref:System.TimeZoneInfo.Local> や <xref:System.DateTime.Now> など) では、夏時間操作に他の .NET Framework データではなくオペレーティング システム規則を使用するようになりました。 | なし。 |
| **書式指定文字列** | カルチャで区別される書式設定をサポートするために、新しい `ParseExact` メソッドと `TryParseExact` メソッドに加えて、`ToString`、`Parse`、`TryParse` などのメソッドの新しいオーバーロードが <xref:System.TimeSpan> 構造体に含まれています。 | なし。 |

### <a name="globalization"></a>グローバリゼーション

新しいニュートラル カルチャと特定のカルチャの一覧については、「[グローバリゼーションとローカリゼーションの新機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd997383%28v=vs.100%29)」をご覧ください。

名前空間: <xref:System.Globalization>。アセンブリ: mscorlib (mscorlib.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **カルチャ名** | 次の名前変更は、ドイツ語、ディベヒ語、アフリカーンス語のカルチャに影響します。<br><br>* <xref:System.Globalization.CultureAndRegionInfoBuilder.CurrencyEnglishName>: ドイツ語 (スイス) (de-CH) カルチャの通貨名は "sFr." から "Fr." に変更されました。<br>* <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern>: ディベヒ語 (モルディブ) (dv-MV) カルチャの長い日付パターンは、"dd/MMMM/yyyy" から "dd/MM/yyyy" に変更されました。<br>* <xref:System.Globalization.DateTimeFormatInfo.PMDesignator>:午後: アフリカーンス語 (南アフリカ) (af-ZA) カルチャの午後指定子は、"nm" から "PM" に変更されました。 | カルチャ名の変更に注意してください。 |
| **LCID パラメーター** | オートメーション サーバー設定で予期される動作と一貫させるために、CLR はアンマネージ COM ベースのアプリケーションに `LCID` パラメーターの現在のカルチャを渡さなくなりました。 代わりに、カルチャとして 1033 (en-us) が渡されます。 | 指定したカルチャを必要とするネイティブ アプリケーションを除き、変更は不要です。 |
| **互換性のために残されているカルチャ タイプ** | <xref:System.Globalization.CultureTypes> と <xref:System.Globalization.CultureTypes> のカルチャ タイプは互換性のために残されています。<br><br>下位互換性を維持するために、<xref:System.Globalization.CultureTypes> は以前の .NET Framework に含まれていたニュートラル カルチャと特定のカルチャを返し、<xref:System.Globalization.CultureTypes> は空のリストを返すようになりました。 | <xref:System.Globalization.CultureTypes> 列挙の他の値を使用します。 |
| **カルチャの取得** | Windows 7 以降、.NET Framework 4 ではデータ自体を格納する代わりにオペレーティング システムからカルチャ情報を取得します。 また、.NET Framework では、データの並べ替えおよび大文字と小文字の指定を行うために Windows と同期します。 | なし。 |
| **Unicode 5.1 規格** | .NET Framework では、すべての Unicode 5.1 文字がサポートされるようになり、約 1400 文字が追加されました。 追加された文字には、新しい記号、矢印、分音記号、句読点、算術記号、CJK ストローク、漢字、追加のマラヤラム文字、テルグ語の数字、ミャンマー語、ラテン語、アラビア語、ギリシャ語、モンゴル語、キリル語の文字などがあります。 Unicode 5.1 では、次のスクリプトが新たにサポートされています: スンダ文字、レプチャ文字、オル チキ文字、ヴァイ文字、サウラーシュトラー文字、カヤーリー文字、ルジャン文字、グルムキー文字、オリヤー文字、タミール文字、テルグ文字、マラヤラム文字、チャム文字。 | なし。 |

### <a name="exceptions"></a>例外

名前空間: <xref:System>、<xref:System.Runtime.ExceptionServices>。アセンブリ: mscorlib (mscorlib.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **破損したプロセスの状態に対する例外** | CLR は、破損したプロセス状態に対する例外をマネージド コード内の例外ハンドラーに伝達しなくなりました。 | これらの例外は、プロセスの状態が破損していることを示します。 この状態にあるアプリケーションの実行はお勧めしません。<br><br>詳細については、「<xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute>」と、MSDN マガジンの「[破損状態例外を処理する](https://docs.microsoft.com/archive/msdn-magazine/2009/february/clr-inside-out-handling-corrupted-state-exceptions)」エントリをご覧ください。 |
| **実行エンジンの例外** | キャッチ可能な例外によって不安定なプロセスが実行を継続できるため、<xref:System.ExecutionEngineException> は廃止されました。 この変更により、実行時の予測可能性と信頼性が向上しました。 | 状況を通知するには <xref:System.InvalidOperationException> を使用します。 |

### <a name="reflection"></a>リフレクション

名前空間: <xref:System.Reflection>。アセンブリ: mscorlib (mscorlib.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **アセンブリ ハッシュ アルゴリズム** | ランタイムはアセンブリが読み込まれていない場合に参照アセンブリのハッシュ アルゴリズムがわからないため、<xref:System.Reflection.AssemblyName.HashAlgorithm> プロパティは <xref:System.Configuration.Assemblies.AssemblyHashAlgorithm> を返すようになりました (これは、<xref:System.Reflection.Assembly.GetReferencedAssemblies%2A> メソッドによって返されるような参照アセンブリのプロパティの使用を意味します)。 | なし。 |
| **アセンブリの読み込み** | アセンブリの冗長な読み込みを防ぎ、仮想アドレス空間を節約するために、CLR は Win32 `MapViewOfFile` 関数のみを使用してアセンブリを読み込むようになりました。 また、`LoadLibrary` 関数も呼び出さなくなりました。<br><br>この変更によって、診断アプリケーションは次のような影響を受けます。<br><br>* <xref:System.Diagnostics.ProcessModuleCollection> は、クラス ライブラリ (.dll ファイル) のモジュールを含まなくなりました。これらのモジュールは `Process.GetCurrentProcess().Modules` の呼び出しから取得されるためです。<br>* `EnumProcessModules` 関数を使用する Win32 アプリケーションは、一覧にあるすべてのマネージド モジュールを参照することはなくなりました。 | なし。 |
| **宣言する型** | <xref:System.Type.DeclaringType> プロパティは、宣言する型を持たない場合に null を正しく返すようになりました。 | なし。 |
| **デリゲート** | デリゲートは、デリゲートのコンストラクターに null 値が渡された場合に、<xref:System.NullReferenceException> ではなく <xref:System.ArgumentNullException> をスローするようになりました。 | 例外処理が <xref:System.ArgumentNullException> をキャッチすることを確認します。 |
| **グローバル アセンブリ キャッシュの場所の変更** | .NET Framework 4 アセンブリでは、グローバル アセンブリ キャッシュは Windows ディレクトリ (%WINDIR%) から Microsoft .NET サブディレクトリ ( *%WINDIR%\\Microsoft.Net*) に移動しました。 旧バージョンのアセンブリは以前のディレクトリに残っています。<br><br>アンマネージ [ASM_CACHE_FLAGS](../unmanaged-api/fusion/asm-cache-flags-enumeration.md) 列挙には、新しい `ASM_CACHE_ROOT_EX` フラグが含まれています。 このフラグによって、[GetCachePath](../unmanaged-api/fusion/getcachepath-function.md) 関数で取得できる .NET Framework 4 アセンブリのキャッシュ場所が取得されます。 | なし。アプリケーションがアセンブリへの明示的なパスを使用しないことを想定しており、パスの使用は推奨されていません。 |
| **グローバル アセンブリ キャッシュ ツール** | [Gacutil.exe (グローバル アセンブリ キャッシュ ツール)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ex0ss12c%28v=vs.100%29) は、シェル プラグイン ビューアーをサポートしなくなりました。 | なし。 |

### <a name="interoperability"></a>相互運用性

名前空間: <xref:System.Runtime.InteropServices>。アセンブリ: mscorlib (mscorlib.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **バッファーの長さ** (アンマネージ API) | メモリを節約するために、[ICorProfilerInfo2::GetStringLayout](../unmanaged-api/profiling/icorprofilerinfo2-getstringlayout-method.md) メソッドの `pBufferLengthOffset` パラメーターの機能は、`pStringLengthOffset` パラメーターと一致するように変更されました。 両方のパラメーターが文字列の長さのオフセット位置を指すようになりました。 バッファーの長さは、文字列クラスの表現から削除されました。 | バッファーの長さへの依存を除去します。 |
| **JIT デバッグ** | Just-In-Time (JIT) デバッグの登録を簡略化するために、.NET Framework デバッガーは、ネイティブ コードの JIT デバッグの動作を制御する AeDebug レジストリ キーのみを使用するようになりました。 この変更の結果は次のとおりです。<br><br>* マネージド コードとネイティブ コードに 2 つの異なるデバッガーを登録することはできなくなりました。<br>* 非対話形式のプロセスに対してデバッガーを自動的に開始することはできなくなりましたが、対話形式のプロセスについてはユーザーにプロンプトを表示できます。<br>* デバッガーの起動に失敗した場合、または起動する必要のある登録済みデバッガーがない場合は、通知が行われなくなりました。<br>* アプリケーションの対話機能に依存する自動起動ポリシーは、サポートされなくなりました。 | 必要に応じてデバッグ操作を調整します。 |
| **プラットフォーム呼び出し** | アンマネージ コードとの相互運用性のパフォーマンスを向上させるために、プラットフォーム呼び出しに不適切な呼び出し規約があると、アプリケーションが失敗するようになりました。 以前のバージョンでは、マーシャリング レイヤーがスタック上でこれらのエラーを解決しました。 | Microsoft Visual Studio でアプリケーションをデバッグすると、これらのエラーが警告されるため、エラーを修正できます。<br><br>更新できないバイナリがある場合は、アプリケーションの構成ファイルに [\<NetFx40_PInvokeStackResilience](../configure-apps/file-schema/runtime/netfx40-pinvokestackresilience-element.md) 要素を含めて、以前のバージョンのように呼び出しエラーを 1 つずつ解決できるようにすることができます。 ただし、この処理はアプリケーションのパフォーマンスに影響することがあります。 |
| **削除されたインターフェイス** (アンマネージ API) | 開発者の混乱を避けるために、次のインターフェイスが削除されました。これらのインターフェイスで有用な実行時シナリオが提供されることはなく、CLR でも実装の提供または受け入れを行っていなかったためです。<br><br>* **INativeImageINativeImageDependency**<br>* **INativeImageInstallInfo**<br>* **INativeImageEvaluate**<br>* **INativeImageConverter**<br>* **ICorModule**<br>* **IMetaDataConverter** | なし。 |

## <a name="data"></a>データ

このセクションでは、データ セットと SQL クライアント、Entity Framework、LINQ to SQL、WCF Data Services (以前の ADO.NET Data Services) を使用するための移行に関する問題について説明します。

### <a name="dataset-and-sql-client"></a>データセットと SQL クライアント

次の表に、以前は制限やその他の問題があった機能に対する改善点を示します。

名前空間: <xref:System.Data>、<xref:System.Data.Objects.DataClasses>、<xref:System.Data.SqlClient>。アセンブリ: System.Data (System.Data.dll 内)、System.Data.Entity (System.Data.Entity.dll 内)

| 機能 | 3\.5 SP1 との相違 |
| ------- | ------------------------ |
| **POCO のシナリオ** | <xref:System.Data.Objects.DataClasses.IRelatedEnd> インターフェイスには、単純な従来の CLR オブジェクト (POCO) シナリオでのその使用性を向上させる新しいメソッドがあります。 これらの新しいメソッドは、<xref:System.Data.Objects.DataClasses.IEntityWithRelationships> エンティティではなく <xref:System.Object> をパラメーターとして受け取ります。 |
| **行の編集** | <xref:System.Data.DataView> クラスで実装されているような <xref:System.Collections.IList.IndexOf%2A> メソッドは、-1 を返す代わりに、編集されている行の値を正しく返すようになりました。 |
| **イベント** | <xref:System.Data.DataRowView.PropertyChanged> イベントは、行が変更された状態になり <xref:System.Data.DataRow.RejectChanges%2A> メソッドが呼び出されたときに、発生するようになりました。 この変更により、<xref:System.Data.DataSet> オブジェクトの内容を公開する UI コントロールの作成が簡単になります。 |
| **例外** | <xref:System.Data.SqlClient.SqlCommand.Prepare%2A> メソッドは、接続が設定されていない、または開いていないときに、<xref:System.NullReferenceException> ではなく <xref:System.InvalidOperationException> をスローするようになりました。 |
| **ビューのマッピング** | 実行時に <xref:System.NullReferenceException> をスローする代わりに、デザイン時にクエリ ビュー マッピング エラーがキャッチされるようになりました。<br><br>マッピングの検証は、概念スキーマ (CSDL) 内の 2 つの関連付けセットが同じ列にマッピングされているエラーをキャッチするようになりました。 |
| **トランザクション** | トランザクションの完了後 (中止またはロールバックを含む)、アプリケーションが接続に対してステートメントを実行しようとした場合に、<xref:System.InvalidOperationException> がスローされるようになりました。 以前のバージョンでは、例外がスローされず、トランザクションが中止された場合でも追加のコマンドを実行できました。 |

### <a name="entity-framework"></a>Entity Framework

次の表に、以前は制限やその他の問題があった機能に対する改善点を示します。

名前空間: <xref:System.Data>、<xref:System.Data.Objects>、<xref:System.Data.Objects.DataClasses>。アセンブリ: System.Data.Entity (System.Data.Entity.dll 内)

| 機能 | 3\.5 SP1 との相違 |
| ------- | ------------------------ |
| **エンティティ オブジェクト** | <xref:System.Data.Objects.ObjectContext.SaveChanges%2A> メソッドが呼び出された場合、現在は <xref:System.Data.Objects.ObjectContext.Detach%2A> メソッドとエンティティ オブジェクトの状態の間にパリティがあります。 この一貫性の向上により、予期しない例外がスローされなくなります。 |
| **Entity SQL** | Entity SQL の識別子解決の規則が改善されました。<br><br>Entity SQL パーサーで、マルチパート識別子の解決ロジックが改善されました。 |
| **構造的な注釈** | Entity Framework は、構造的な注釈を認識するようになりました。 |
| **クエリ** | クエリでは次の改良が行われました。<br><br>* 空のコレクションに対する null キーを使用した `GroupBy` クエリは、クエリに追加の選択があるかどうかにかかわらず、行を返しません。<br>* LINQ および Entity-SQL クエリで生成された SQL は、文字列パラメーターを既定で非 Unicode 値として扱うようになりました。 |

### <a name="linq-to-sql"></a>LINQ to SQL

次の表に、以前は制限やその他の問題があった機能に対する改善点を示します。

名前空間: <xref:System.Data.Linq>。アセンブリ: System.Data.Linq (System.Data.Linq.dll 内)

| 機能 | 3\.5 SP1 との相違 |
| ------- | ------------------------ |
| **イベント** | <xref:System.Data.Linq.EntitySet%601> コレクションは、コレクションが読み込まれたときのイベントの生成に加えて、<xref:System.Data.Linq.EntitySet%601> がアンロードされている場合に追加操作と削除操作に対して <xref:System.Data.Linq.EntitySet%601.ListChanged> イベントを生成するようになりました。 |
| **クエリ** | `Skip(0)` は、LINQ to SQL クエリでは無視されるようになりました。 その結果、このメソッドを含むクエリの動作が変わる可能性があります。 たとえば、場合によっては、`OrderBy` 句が `Skip(0)` で必要になり、`OrderBy` 句が含まれていないと <xref:System.NotSupportedException> 例外がクエリからスローされるようになりました。 |

### <a name="wcf-data-services"></a>WCF Data Services

次の表に、以前は制限やその他の問題があった機能に対する改善点を示します。

名前空間: <xref:System.Data.Services>、<xref:System.Data.Services.Client>、<xref:System.Data.Services.Common>、<xref:System.Data.Services.Providers>。アセンブリ: System.Data.Services (System.Data.Services.dll 内)、System.Data.Services.Client (System.Data.Services.Client.dll 内)

| 機能 | 3\.5 SP1 との相違 |
| ------- | ------------------------ |
| **バッチ バイナリ コンテンツ** | WCF Data Services では、要求と応答でバッチ バイナリ コンテンツをサポートするようになりました。 |
| **変更インターセプター** | 変更インターセプターが、削除要求に対して実行されるようになりました。<br><br>変更インターセプターは、エンティティ セット内のエンティティを変更する要求がサーバーによって受信されるたびに実行されるメソッドです。 実行されると、受信した要求が実行されます。 変更インターセプターは、変更されるエンティティとそのエンティティに対して実行される操作へのアクセスを可能にします。 |
| **例外** | 次の条件では、<xref:System.NullReferenceException> ではなく、より役に立つ例外がスローされるようになりました。<br><br>* データ サービスの呼び出しがタイムアウトになった場合の <xref:System.ServiceProcess.TimeoutException>。<br>* データ サービスに対して不適切な要求が行われた場合の <xref:System.Data.Services.Client.DataServiceRequestException>。<br><br>アプリケーションでは、新しい例外をキャッチするように例外処理を変更する必要があります。 |
| **ヘッダー** | ヘッダーに対して次の改良が行われました。<br><br>* WCF Data Services は、指定されていない値を持つ `eTag` ヘッダーを正しく拒否するようになりました。<br>* WCF Data Services は、要求に`if-*` ヘッダーがある場合にエラーを返すようになり、リンクへの削除要求に対する要求を実行しなくなりました。<br>* WCF Data Services は、クライアントが Accept ヘッダーに指定したフォーマット (Atom、JSON) でエラーをクライアントに返すようになりました。 |
| **JSON リーダー** | JavaScript Object Notation (JSON) リーダーは、WCF Data Service に送信された JSON ペイロードを処理する場合に、単一の円記号 ("\\") エスケープ文字を読み取ったときにエラーを正しく返すようになりました。 |
| **マージ** | <xref:System.Data.Services.Client.MergeOption> 列挙に対して次の改良が行われました。<br><br>* <xref:System.Data.Services.Client.MergeOption> マージ オプションは、データ サービスからの後続の応答の結果としてクライアントのエンティティを変更しなくなりました。<br>* <xref:System.Data.Services.Client.MergeOption> オプションは、動的 SQL とストアド プロシージャ ベースの更新との間で一貫性を持つようになりました。 |
| **要求** | データ サービスへの要求が処理される前に、<xref:System.Data.Services.DataService%601.OnStartProcessingRequest%2A> メソッドが呼び出されるようになりました。 これにより、<xref:System.Data.Services.Providers.ServiceOperation> サービスに対する要求が正しく動作するようになります。 |
| **ストリーム** | WCF Data Services は、読み取り操作と書き込み操作の基になるストリームを閉じなくなりました。 |
| **URI** | WCF Data Services クライアントによる URI のエスケープが修正されました。 |

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

次の表に、以前は制限やその他の問題があった機能に対する改善点を示します。

| 機能 | 3\.5 SP1 との相違 |
| ------- | ------------------------ |
| **構成ファイル** | 構成ファイル階層を通じた動作の継承を有効にするために、WCF は構成ファイル間でのマージをサポートするようになりました。<br><br>構成継承モデルは、コンピューターで実行されているすべてのサービスに適用される動作をユーザーが定義できるように拡張されました。<br><br>異なる階層レベルに同じ名前の動作がある場合は、動作の変更が検出されることがあります。 |
| **サービス ホスティング** | `allowDefinition="MachineToApplication"` 属性を要素定義に追加することにより、サービス レベルで `<serviceHostingEnvironment>` 構成要素を指定できなくなりました。<br><br>サービス レベルで `<serviceHostingEnvironment>` 要素を指定することは技術的に不適切であり、一貫性のない動作の原因になります。 |

## <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

### <a name="applications"></a>アプリケーション

名前空間: <xref:System.Windows>、<xref:System.Windows.Controls>。アセンブリ: PresentationFramework (PresentationFramework.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **例外処理** | エラーを早期に検出できるように、WPF では、元の例外をキャッチする代わりに、<xref:System.Reflection.TargetInvocationException> をスローし、<xref:System.Exception.InnerException> プロパティを重大な例外 (<xref:System.NullReferenceException>、<xref:System.OutOfMemoryException>、<xref:System.StackOverflowException>、<xref:System.Security.SecurityException> など) に設定します。 | なし。 |
| **リンク リソース** | リンクを簡単にするために、プロジェクト フォルダー構造以外の場所にあるリソース ファイル (イメージなど) では、アプリケーションがビルドされるときに、ファイル名だけでなくリソース ファイルの完全パスをリソース ID として使用します。 これにより、アプリケーションが実行時にファイルを見つけられるようになります。 | なし。 |
| **部分信頼アプリケーション** | セキュリティを考慮して、部分信頼で実行される Windows ベース アプリケーション、および <xref:System.Windows.Controls.WebBrowser> コントロールまたは HTML を格納する <xref:System.Windows.Controls.Frame> コントロールを含む Windows ベース アプリケーションは、コントロールが作成されるときに <xref:System.Security.SecurityException> をスローします。<br><br>次のすべての条件が満たされると、ブラウザー アプリケーションによって例外がスローされ、メッセージが表示されます。<br><br>* アプリケーションを Firefox で実行している。<br>* アプリケーションを、信頼されていないサイトのインターネット ゾーンで部分信頼によって実行している。<br>* アプリケーションに <xref:System.Windows.Controls.WebBrowser> コントロール、または HTML を格納する <xref:System.Windows.Controls.Frame> コントロールが含まれている。<br><br>信頼されたサイトまたはイントラネット ゾーンから実行するアプリケーションは影響を受けません。 | ブラウザー アプリケーションで、次のいずれかを実行することでこの変更の影響を軽減できます。<br><br>* ブラウザー アプリケーションを完全信頼で実行します。<br>* 顧客に依頼して、アプリケーションのサイトを信頼されたサイト ゾーンに追加してもらいます。<br>* 顧客に依頼して、Internet Explorer を使用してアプリケーションを実行してもらいます。 |
| **リソース ディクショナリ** | テーマ レベルのリソース ディクショナリを改善し、その変更を防ぐために、リソース ディクショナリで定義された固定可能リソース、およびテーマ レベル ディクショナリにマージされた固定可能リソースは、常に固定され、変更できなくなりました。 これは、固定可能リソースで想定される動作です。 | アプリケーションで、テーマ レベルでマージされたディクショナリで定義されたリソースを変更する場合は、リソースを複製して、複製した方のコピーを変更する必要があります。 または、リソースを `x:Shared="false"` とマークして、リソースにクエリが実行されるたびに <xref:System.Windows.ResourceDictionary> が新しいコピーを作成できるようにします。 |
| **Windows 7** | WPF アプリケーションを Windows 7 でより適切に動作させるために、ウィンドウの動作を修正する次の改良が行われました。<br><br>* ドッキングとジェスチャの状態は、ユーザーとのやり取りに基づいて予想どおりに動作するようになりました。<br>* タスク バー コマンドの **[重ねて表示]、[ウィンドウを上下に並べて表示]** 、 **[ウィンドウを左右に並べて表示]** は正しく動作し、適切なプロパティを更新するようになりました。<br>* 最大化または最小化されたウィンドウの `Top`、`Left`、`Width`、`Height` の各プロパティには、モニターに応じて、他の値ではなくウィンドウの適切な復元場所が含まれるようになりました。 | なし。 |
| **ウィンドウのスタイルと透過性** | <xref:System.Windows.Window.AllowsTransparency> が `true` で、<xref:System.Windows.WindowState> が <xref:System.Windows.WindowState> のときに、<xref:System.Windows.Window.WindowStyle> を <xref:System.Windows.WindowStyle> 以外の値に設定しようとすると、<xref:System.InvalidOperationException> がスローされます。 | <xref:System.Windows.Window.AllowsTransparency> が `true` であるときに、<xref:System.Windows.Window.WindowStyle> を変更する必要がある場合は、Win32 の `SetWindowLongPtr` 関数を呼び出すことができます。 |
| **XPS ビューアー** | WPF には Microsoft XML Paper Specification Essentials Pack (XPSEP) が含まれていません。 XPSEP は Windows 7 と Windows Vista に付属しています。<br><br>.NET Framework 3.5 SP1 がインストールされていない Windows XP を実行中のコンピューターでは、<xref:System.Windows.Controls.PrintDialog> 以外の WPF API を使用した印刷は WINSPOOL に依存します。 その場合、印刷の実行中に一部のプリンター機能が表示されないことや、プリンター設定が適用されないことが考えられます。 | 必要に応じて [Microsoft XML Paper Specification Essentials Pack](https://go.microsoft.com/fwlink/?LinkId=178895) をインストールしてください。 |

### <a name="controls"></a>コントロール

名前空間: <xref:System.Windows>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>。アセンブリ: PresentationFramework (PresentationFramework.dll 内)、PresentationCore (PresentationCore.dll 内)、WindowsBase (WindowsBase.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **ダイアログ ボックス** | 信頼性を高めるために、<xref:Microsoft.Win32.CommonDialog.ShowDialog%2A> メソッドは、<xref:Microsoft.Win32.FileDialog> コントロールを作成したのと同じスレッド上で呼び出されます。 | 必ず <xref:Microsoft.Win32.FileDialog> コントロールを作成して、同じスレッド上で <xref:Microsoft.Win32.CommonDialog.ShowDialog%2A> メソッドを呼び出すようにしてください。 |
| **フローティング ウィンドウ** | フローティング ウィンドウの再アクティブ化が間違って保持される (モーダル ダイアログ ボックスのように表示される) フォーカス復元ロジックを修正するために、候補がウィンドウの子でない場合にはフォーカスが復元されないようになりました。 | なし。 |
| **コレクションの項目** | 基になるコレクションに項目が移動または追加されたときは、<xref:System.Windows.Data.CollectionView> を並べ替えない限り、その項目は <xref:System.Windows.Data.CollectionView> でも同じ場所に表示されます。 この処理により、コレクション内の項目の場所と、関連付けられた <xref:System.Windows.Data.CollectionView> 内の項目の場所との間で一貫性が保たれます。 | 固定された場所にある項目に依存する代わりに、<xref:System.Windows.Controls.ItemContainerGenerator.ContainerFromItem%2A> メソッドまたは <xref:System.Windows.Data.CollectionView.IndexOf%2A> メソッドを使用して、<xref:System.Windows.Data.CollectionView> 内で項目の場所を検索します。 |
| **レイアウト** | 不要な再レイアウトを排除するために、<xref:System.Windows.Controls.Page.ShowsNavigationUI> を変更しても、レイアウトが無効にされたり、別のレイアウト パスが実行されたりすることがなくなりました。 | <xref:System.Windows.Controls.Page.ShowsNavigationUI> を変更して別のレイアウト パスを実行する場合は、プロパティの設定後に <xref:System.Windows.UIElement.InvalidateVisual%2A> を呼び出します。 |
| **メニュー** | メニュー ポップアップで ClearType テキストを有効にするために、<xref:System.Windows.Controls.ControlTemplate> クラスと、<xref:System.Windows.Controls.MenuItem> などのコントロールが変更されました。 | アプリケーションを、コントロール テンプレートの視覚的な構造に依存させないでください。 パブリック コントラクトの一部であるのは <xref:System.Windows.Controls.ControlTemplate> の名前付きパーツのみです。 アプリケーションで <xref:System.Windows.Controls.ControlTemplate> 内の任意のオブジェクトを検索する必要がある場合は、ツリー内で固定されたオブジェクトの場所に依存するのではなく、ビジュアル ツリーで特定の型を検索するようにします。 |
| **ナビゲーション** | <xref:System.Windows.Controls.Frame> で、ある場所に直接移動する場合、<xref:System.Windows.Navigation.NavigatingCancelEventArgs.IsNavigationInitiator> プロパティは最初の移動後に `true` になります。 この変更により、起動シナリオ中に他のイベントが発生しなくなります。 | なし。 |
| **ポップアップ** | <xref:System.Windows.Controls.Primitives.CustomPopupPlacementCallback> デリゲートは、レイアウト パスの実行中に複数回 (従来は 1 回のみ) 呼び出すことができるようになりました。 | <xref:System.Windows.Controls.Primitives.CustomPopupPlacementCallback> デリゲートで <xref:System.Windows.Controls.Primitives.Popup> の場所を以前の場所に基づいて計算する場合は、`popupSize`、`targetSize`、`offset` のいずれかのパラメーターの値が変更された場合にのみ場所を再計算します。 |
| **プロパティの値** | <xref:System.Windows.DependencyObject.SetCurrentValue%2A> メソッドでは、バインディング、スタイル、またはトリガーによるプロパティへの影響を維持したまま、プロパティに有効な値を設定できるようになりました。 | コントロールの作成で、ユーザー操作などの他のアクションに影響を与えるプロパティ値の変更が伴う場合は、常に <xref:System.Windows.DependencyObject.SetCurrentValue%2A> を使用する必要があります。 |
| **テキスト ボックス** | セキュリティを考慮して、<xref:System.Windows.Forms.TextBoxBase.Copy%2A> メソッドと <xref:System.Windows.Forms.TextBoxBase.Cut%2A> メソッドは、部分信頼で呼び出されるとエラーなしで失敗します。<br><br>また、<xref:System.Windows.Controls.Primitives.TextBoxBase> から継承するコントロールで、<xref:System.Windows.Input.ApplicationCommands.Copy> プロパティまたは <xref:System.Windows.Input.ApplicationCommands.Cut> プロパティをプログラムによって実行すると、部分信頼が原因でブロックされます。 ただし、ユーザーが起動したコピーおよび切り取りのコマンド (<xref:System.Windows.Controls.Primitives.ButtonBase.Command> プロパティがこれらのコマンドの 1 つにバインドされているボタンをクリックする場合など) は動作します。 キーボード ショートカットおよびコンテキスト メニューを使用した標準的なコピーと切り取りも、以前と同様に部分信頼で機能します。 | <xref:System.Windows.Input.ApplicationCommands.Copy> コマンドまたは <xref:System.Windows.Input.ApplicationCommands.Cut> コマンドを、ボタンのクリックのようなユーザーが開始するアクションにバインドします。 |

### <a name="graphics"></a>グラフィックス

名前空間: <xref:System.Windows>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>、<xref:System.Windows.Media.Effects>。アセンブリ: PresentationFramework (PresentationFramework.dll 内)、PresentationCore (PresentationCore.dll 内)、WindowsBase (WindowsBase.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **ビットマップ効果** | パフォーマンスを向上させるために、<xref:System.Windows.Media.Effects.BitmapEffect> クラスと、<xref:System.Windows.Media.Effects.BitmapEffect> クラスから継承するクラスは、引き続き存在していますが、無効になっています。 効果は、次の条件を満たす場合に、ハードウェア アクセラレータによるレンダリング パイプラインを使用して表示されます。<br><br>* アプリケーションで、100 DIU 未満の半径のプロパティ セットを持つ <xref:System.Windows.Media.Effects.DropShadowBitmapEffect> または <xref:System.Windows.Media.Effects.BlurBitmapEffect> を使用する。<br>* アプリケーションを実行するコンピューター上のビデオ カードがピクセル シェーダー 2.0 をサポートしている。<br><br>これらの条件が満たされない場合、<xref:System.Windows.Media.Effects.BitmapEffect> オブジェクトは無効になります。<br><br>また、Visual Studio では、<xref:System.Windows.Media.Effects.BitmapEffect> オブジェクトまたはサブクラスが見つかった場合にコンパイラの警告が生成されます。<br><br><xref:System.Windows.Media.DrawingContext.PushEffect%2A> メソッドは現在は使用されていません。 | 従来の <xref:System.Windows.Media.Effects.BitmapEffect> と派生クラスの使用を中止し、<xref:System.Windows.Media.Effects.Effect>、<xref:System.Windows.Media.Effects.BlurEffect>、<xref:System.Windows.Media.Effects.DropShadowEffect><xref:System.Windows.Media.Effects.ShaderEffect> から派生した新しいクラスを使用します。<br><br><xref:System.Windows.Media.Effects.ShaderEffect> クラスから継承することで、独自の効果を作成することもできます。 |
| **ビットマップ フレーム** | 複製された <xref:System.Windows.Media.Imaging.BitmapFrame> オブジェクトは <xref:System.Windows.Media.Imaging.BitmapSource.DownloadProgress> イベント、<xref:System.Windows.Media.Imaging.BitmapSource.DownloadCompleted> イベント、<xref:System.Windows.Media.Imaging.BitmapSource.DownloadFailed> イベントを受け取るようになりました。 この処理により、Web からダウンロードされ、<xref:System.Windows.Controls.Image> コントロールに適用されたイメージが <xref:System.Windows.Style> で適切に動作するようになります。<br><br>次の状況のすべてに当てはまる場合にのみ、動作が変化します。<br><br>* <xref:System.Windows.Media.Imaging.BitmapSource.DownloadProgress> イベント、<xref:System.Windows.Media.Imaging.BitmapSource.DownloadCompleted> イベント、または <xref:System.Windows.Media.Imaging.BitmapSource.DownloadFailed> イベントをサブスクライブします。<br>* <xref:System.Windows.Media.Imaging.BitmapFrame> のソースを Web から取得します。<br>* ダウンロードの実行中に、<xref:System.Windows.Media.Imaging.BitmapFrame> が複製されます。 | イベント ハンドラーで送信元をチェックして、送信元が元の <xref:System.Windows.Media.Imaging.BitmapFrame> である場合にのみ対応策を取ります。 |
| **イメージのデコード** | イメージをデコードできない可能性がある場合に <xref:System.IO.IOException> が処理されるのを防ぐため、<xref:System.Windows.Media.Imaging.BitmapSource> クラスは、イメージをデコードできない場合に <xref:System.Windows.Media.Imaging.BitmapSource.DecodeFailed> イベントを発生させます。 | <xref:System.IO.IOException> の例外処理を削除し、<xref:System.Windows.Media.Imaging.BitmapSource.DecodeFailed> イベントを使用してデコードの失敗を確認します。 |

### <a name="input"></a>入力

名前空間: <xref:System.Windows>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>。アセンブリ: PresentationFramework (PresentationFramework.dll 内)、PresentationCore (PresentationCore.dll 内)、WindowsBase (WindowsBase.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **コマンド インスタンスのバインディング** | ビュー モデル ベースのコマンド インスタンスをビュー ベースの入力ジェスチャにバインドするメカニズムを提供するために、<xref:System.Windows.Input.InputBinding> クラスは、<xref:System.Windows.DependencyObject> ではなく <xref:System.Windows.Freezable> から継承するようになりました。 次のプロパティは依存関係プロパティになりました。<br><br>* <xref:System.Windows.Input.InputBinding.Command><br>* <xref:System.Windows.Input.InputBinding.CommandParameter><br>* <xref:System.Windows.Input.InputBinding.CommandTarget><br><br>この変更の結果は次のとおりです。<br><br>* <xref:System.Windows.Input.InputBinding> オブジェクトは、登録時に固定され、変更できない状態になります。<br>* <xref:System.Windows.DependencyObject> クラスの制限により、複数のスレッドからインスタンス レベルの <xref:System.Windows.Input.InputBinding> オブジェクトにアクセスすることはできません。<br>* <xref:System.Windows.Freezable> クラスの制限により、登録後にクラス レベルの入力バインディングを変更することはできません。<br>* ビュー モデルで作成されたコマンド インスタンスに入力バインディングを指定することはできません。 | バインディングを変更可能にする場合は、スレッドごとに <xref:System.Windows.Input.InputBinding> クラスのインスタンスを個別に作成します。それ以外の場合は、バインディングを固定します。 クラス レベルの静的 <xref:System.Windows.Input.InputBinding> は、登録後に変更しないでください。 |
| **ブラウザー アプリケーション** | WPF ブラウザー アプリケーション (.XBAP) では、スタンドアロン WPF アプリケーションと同じようにキー イベントを処理することで、オブジェクトが正しい順序でルーティング キー イベントを受け取れるようになりました。 | なし。 |
| **デッド キーの組み合わせ** | WPF ではデッド キーが難読化されています。デッド キーとは、そのキーを押しても文字は表示されないものの、続けて文字キーを押してその文字と組み合わせることで、1 つの文字が生成されるキーのことです。 <xref:System.Windows.Input.Keyboard.KeyDownEvent> イベントなどのキー入力イベントでは、<xref:System.Windows.Input.KeyEventArgs.Key> プロパティを <xref:System.Windows.Input.Key> 値に設定することで、入力されたキーがデッド キーであることを通知します。 これは通常予期される動作です。アプリケーションは一般に、1 つの文字を組み合わせて作成する場合のキーボード入力に応答しないためです。 | 組み合わせ文字の一部になり得るキーの読み取りが予測されるアプリケーションでは、<xref:System.Windows.Input.KeyEventArgs.DeadCharProcessedKey> プロパティを使用して、難読化されたキーを取得できるようになりました。 |
| **フォーカス マネージャー** | [IsFocusScope](xref:System.Windows.Input.FocusManager.IsFocusScope%2A) 添付プロパティが `true` に設定されている要素が <xref:System.Windows.Input.FocusManager.GetFocusedElement(System.Windows.DependencyObject)?displayProperty=nameWithType> メソッドに渡されると、このメソッドは、そのフォーカス スコープ内で最後にキーボード フォーカスされた要素を返します。ただしこれは、返される要素が、メソッドに渡された要素と同じ <xref:System.Windows.PresentationSource> オブジェクトに属する場合に限られます。 | なし。 |

### <a name="ui-automation"></a>UI オートメーション

Namespace: <xref:System.Windows>、<xref:System.Windows.Automation.Peers>、<xref:System.Windows.Automation.Provider>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>。アセンブリ: PresentationFramework (PresentationFramework.dll 内)、PresentationCore (PresentationCore.dll 内)、UIAutomationProvider (UIAutomationProvider.dll 内)、WindowsBase (WindowsBase.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **ビューのクラス階層** | <xref:System.Windows.Automation.Peers.TreeViewAutomationPeer> クラスと <xref:System.Windows.Automation.Peers.TreeViewItemAutomationPeer> クラスは、<xref:System.Windows.Automation.Peers.FrameworkElementAutomationPeer> ではなく <xref:System.Windows.Automation.Peers.ItemsControlAutomationPeer> から継承されます。 | <xref:System.Windows.Automation.Peers.TreeViewItemAutomationPeer> クラスから継承し、<xref:System.Windows.Automation.Peers.TreeViewItemAutomationPeer.GetChildrenCore%2A> メソッドをオーバーライドする場合は、新しい <xref:System.Windows.Automation.Peers.TreeViewDataItemAutomationPeer> クラスから継承するオブジェクトを返すことを検討します。 |
| **コンテナー オフ スクリーン** | 正しくない戻り値を修正するために、<xref:System.Windows.Automation.Peers.UIElementAutomationPeer.IsOffscreenCore%2A> メソッドは、ビューのスクロールの範囲外にある項目コンテナーについて、`false` を正しく返すようになりました。 このメソッドの値は、他のウィンドウを閉じても変化しません。また、要素が特定のモニターに表示されるかどうかに影響されません。 | なし。 |
| **メニューと子オブジェクト** | <xref:System.Windows.Controls.MenuItem> オブジェクト以外の子を含むメニューの UI オートメーションを有効にするために、<xref:System.Windows.Automation.Peers.MenuItemAutomationPeer.GetChildrenCore%2A> メソッドは、<xref:System.Windows.Automation.Peers.MenuItemAutomationPeer> オブジェクトではなく、子 <xref:System.Windows.UIElement> オブジェクトの <xref:System.Windows.Automation.Peers.AutomationPeer> オブジェクトを返すようになりました。 | なし。 |
| **新しいインターフェイスとアセンブリ** | UI オートメーションの新機能を有効にするために、次のインターフェイスが追加されました。<br><br>* <xref:System.Windows.Automation.Provider.IItemContainerProvider><br>* <xref:System.Windows.Automation.Provider.ISynchronizedInputProvider><br>* <xref:System.Windows.Automation.Provider.IVirtualizedItemProvider> | WPF オートメーション ピアを構築するプロジェクトでは、UIAutomationProvider.dll への明示的な参照を追加する必要があります。 |
| **Thumb** | <xref:System.Windows.Automation.Peers.ThumbAutomationPeer.GetClassNameCore%2A> メソッドは、null の代わりに値を返します。 このため、<xref:System.Windows.Controls.GridSplitter> などの、<xref:System.Windows.Controls.Primitives.Thumb> クラスから継承されたコントロールでは、UI オートメーションに名前を報告します。 | なし。 |
| **仮想化された要素** | パフォーマンスを向上させるために、<xref:System.Windows.Automation.Peers.UIElementAutomationPeer.GetChildrenCore%2A> メソッドは、すべての子オブジェクトではなく、ビジュアル ツリーに存在する子オブジェクトのみを返します (オブジェクトが仮想化されているかどうかは関係ありません)。 | <xref:System.Windows.Automation.ItemContainerPattern> を使用して <xref:System.Windows.Automation.Peers.ItemsControlAutomationPeer> のすべての項目を反復処理します。 |

### <a name="xaml"></a>XAML

名前空間: <xref:System.Windows>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>、<xref:System.Windows.Markup>。アセンブリ: PresentationFramework (PresentationFramework.dll 内)、PresentationCore (PresentationCore.dll 内)、WindowsBase (WindowsBase.dll 内)

| 機能 | 3\.5 SP1 との相違 | 推奨される変更 |
| ------- | ------------------------ | ------------------- |
| **マークアップ拡張機能** | WPF でマークアップ拡張機能を使用するときは、状況に応じて <xref:System.Windows.Markup.MarkupExtension> オブジェクトを返す代わりに、常に <xref:System.Windows.Markup.MarkupExtension.ProvideValue%2A> メソッドの値を正確に使用して、プロパティの設定またはコレクションの項目作成を行うようになりました。 マークアップ拡張機能は、状況によっては自身の値を返すことがあります。 | 旧バージョンの <xref:System.Windows.Markup.MarkupExtension> オブジェクトを返すリソースにアクセスするアプリケーションの場合は、<xref:System.Windows.Markup.MarkupExtension> オブジェクトではなく、<xref:System.Windows.Markup.MarkupExtension.ProvideValue%2A> から返されるオブジェクトを参照します。 |
| **属性の解析** | XAML の属性は 1 つのピリオドのみを持つことができるようになりました。 たとえば、次の例は有効です。<br><br>`<Button Background="Red"/>` (ピリオドなし)<br><br>`<Button Button.Background = "Red"/>` (1 つのピリオド)<br><br>次の例は無効です。<br><br>`<Button Control.Button.Background = "Red"/>` (複数のピリオド) | 複数のピリオドを持つ XAML 属性を修正します。 |

## <a name="xml"></a>XML

次の表に、以前は制限やその他の問題があった機能に対する改善点を示します。

### <a name="schema-and-transforms"></a>スキーマと変換

名前空間: <xref:System.Xml.Linq>、<xref:System.Xml.Schema>、<xref:System.Xml.XPath>。アセンブリ: System.Xml (System.Xml.dll 内)、System.Xml.Linq (System.Xml.Linq.dll 内)

| 機能 | 3\.5 SP1 との相違 |
| ------- | ------------------------ |
| **カメレオン スキーマ** | データの破損を防ぐために、カメレオン スキーマは、複数のスキーマにインクルードされている場合に、正しく複製されるようになりました。<br><br>カメレオン スキーマは、ターゲット名前空間を持たないスキーマであり、別の XSD にインクルードされている場合は、インポート スキーマのターゲット名前空間を引き継ぎます。 これらは、スキーマに共通の型をインクルードするためによく使用されます。 |
| **ID 関数** | XSLT [id 関数](/sql/xquery/functions-on-sequences-id)は、<xref:System.Xml.XmlReader> オブジェクトが XLST に渡される場合に、null ではなく適切な値を返すようになりました。<br><br>ユーザーが <xref:System.Xml.Linq.XNode.CreateReader%2A> メソッドを使用して LINQ to XML クラスから <xref:System.Xml.XmlReader> オブジェクトを作成し、この <xref:System.Xml.XmlReader> オブジェクトが XSLT に渡された場合、XSLT の `id` 関数のインスタンスは以前は null を返しました。 これは、`id` 関数に対して許容される戻り値ではありません。 |
| **Namespace 属性** | データの破損を防ぐために、<xref:System.Xml.XPath.XPathNavigator> オブジェクトは `x:xmlns` 属性のローカル名を正しく返すようになりました。 |
| **名前空間の宣言** | サブツリーの <xref:System.Xml.XmlReader> オブジェクトは、1 つの XML 要素内に重複する名前空間宣言を作成しなくなりました。 |
| **スキーマの検証** | 誤ったスキーマ検証を防ぐために、<xref:System.Xml.Schema.XmlSchemaSet> クラスは、XSD スキーマが正しく一貫してコンパイルされるようにします。 これらのスキーマには、他のスキーマを含めることができます。たとえば、`A.xsd` は `B.xsd` を含むことができ、さらにその中には `C.xsd` を含むことができます。 これらのいずれかをコンパイルすると、この依存関係のグラフがスキャンされます。 |
| **スクリプト関数** | [function-available 関数](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256124(v=vs.100))は、関数が実際に使用可能な場合に `false` を間違って返さなくなりました。 |
| **URI** | <xref:System.Xml.Linq.XElement.Load%2A> メソッドは、LINQ クエリで正しい BaseURI を返すようになりました。 |

### <a name="validation"></a>検証

名前空間: <xref:System.Xml.Linq>、<xref:System.Xml.Schema>、<xref:System.Xml.XPath>。アセンブリ: System.Xml (System.Xml.dll 内)、System.Xml.Linq (System.Xml.Linq.dll 内)

| 機能 | 3\.5 SP1 との相違 |
| ------- | ------------------------ |
| **名前空間リゾルバー** | <xref:System.Xml.XmlReader.ReadContentAs%2A> メソッドは、渡された <xref:System.Xml.IXmlNamespaceResolver> リゾルバーを無視しなくなりました。<br><br>以前のバージョンでは、指定した名前空間リゾルバーが無視され、<xref:System.Xml.XmlReader> が代わりに使用されました。 |
| **空白** | リーダーを作成するときのデータの消失を防ぐために、<xref:System.Xml.XmlReader.Create%2A> メソッドは有意の空白を破棄しなくなりました。<br><br>XML 検証では、テキストを XML マークアップと混合できる混合コンテンツ モードが認識されます。 混合モードでは、すべての空白が有意であり、報告される必要があります。 |

### <a name="writing"></a>書き込み

名前空間: <xref:System.Xml.Linq>、<xref:System.Xml.Schema>、<xref:System.Xml.XPath>。アセンブリ: System.Xml (System.Xml.dll 内)、System.Xml.Linq (System.Xml.Linq.dll 内)

| 機能 | 3\.5 SP1 との相違 |
| ------- | ------------------------ |
| **エンティティ参照** | データの破損を防ぐために、エンティティ参照は XML 属性を 2 回エンティティ化しなくなりました。<br><br>ユーザーがエンティティを `xmlns` 属性に書き込もうとするか、または <xref:System.Xml.XmlWriter.WriteEntityRef%2A> メソッドを使用して `xml:lang` 属性または `xml:space` 属性に書き込もうとした場合、エンティティは出力に 2 回エンティティ化されるため、データが破損します。 |
| **改行処理** | データの破損を防ぐために、<xref:System.Xml.XmlWriter> オブジェクトは <xref:System.Xml.NewLineHandling> オプションを考慮します。 |

## <a name="see-also"></a>関連項目

- [.NET Framework 4 における新しい型とメンバー](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ff641764%28v=vs.100%29)
- [.NET Framework 4 移行ガイド](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ff657133%28v=vs.100%29)
- [.NET Framework 4 の新機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms171868%28v=vs.100%29)
- [.NET Framework のバージョンの互換性](version-compatibility.md)
- [.NET Framework 4 への Office ソリューションの移行](/visualstudio/vsto/migrating-office-solutions-to-the-dotnet-framework-4-or-later)
- [.NET Framework クラス ライブラリの互換性のために残されている機能](../whats-new/whats-obsolete.md)
