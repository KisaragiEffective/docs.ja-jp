---
title: ASP.NET Web フォームから Blazor への移行
description: 既存の ASP.NET Web フォームアプリを Blazor に移行する方法について説明します。
author: twsouthwick
ms.author: tasou
ms.date: 09/19/2019
ms.openlocfilehash: 52f463c66c2980d59a93f3210b3cfd825bec33da
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75337448"
---
# <a name="migrate-from-aspnet-web-forms-to-blazor"></a>ASP.NET Web フォームから Blazor への移行

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

ASP.NET Web Forms から Blazor へのコードベースの移行は、計画を必要とする時間のかかる作業です。 この章では、プロセスの概要を示します。 移行を容易にするには、アプリが*N 層*アーキテクチャに準拠していることを確認し、アプリモデル (この場合は Web フォーム) がビジネスロジックとは別のものになるようにします。 レイヤーを論理的に分離することにより、.NET Core と Blazor に移行する必要があることが明確になります。

この例では、 [GitHub](https://github.com/dotnet-architecture/eShopOnBlazor)で入手できる eShop アプリが使用されています。 eShop は、フォームの入力と検証によって CRUD 機能を提供するカタログサービスです。

作業中のアプリを Blazor に移行する理由 何度も必要ありません。 ASP.NET Web フォームは、長年にわたって引き続きサポートされます。 ただし、Blazor が提供する機能の多くは、移行されたアプリでのみサポートされています。 次のような機能があります。

- `Span<T>` などのフレームワークにおけるパフォーマンスの向上
- Webasとして実行する機能
- Linux と macOS のクロスプラットフォームサポート
- アプリローカル配置または共有フレームワークの配置 (他のアプリに影響を与えることはありません)

これらの新機能が十分に説得力を持っている場合は、アプリの移行に価値があるかもしれません。 移行にはさまざまな形があります。アプリ全体、または変更を必要とする特定のエンドポイントのみを指定できます。 移行の決定は、最終的には、開発者によって解決されるビジネス上の問題に基づいています。

## <a name="server-side-versus-client-side-hosting"></a>サーバー側とクライアント側のホスト

[ホスティングモデル](hosting-models.md)の章で説明されているように、Blazor アプリは、サーバー側とクライアント側の2つの異なる方法でホストできます。 サーバー側モデルでは ASP.NET Core SignalR 接続を使用して、サーバー上の実際のコードを実行しながら、DOM の更新を管理します。 クライアント側モデルはブラウザー内で実行されるため、サーバー接続は必要ありません。 特定のアプリに最適な違いがいくつかあります。

- WebAssembly 実行はまだ開発中であり、現在の時点では、すべての機能 (スレッド処理など) をサポートしていない場合があります。
- クライアントとサーバー間の通信を高いと、サーバー側モードで待機時間の問題が発生する可能性があります。
- データベースおよび内部または保護されたサービスへのアクセスには、クライアント側のホストとは別のサービスが必要です。

このドキュメントの執筆時点では、サーバー側モデルは Web フォームによく似ています。 この章では、実稼働の準備として、サーバー側のホスティングモデルに焦点を当てています。

## <a name="create-a-new-project"></a>新しいプロジェクトの作成

この最初の移行手順では、新しいプロジェクトを作成します。 このプロジェクトの種類は、.NET Core の SDK スタイルプロジェクトに基づいており、以前のプロジェクト形式で使用されていた定型句の多くが単純化されています。 詳細については、「プロジェクトの[構造](project-structure.md)」の章を参照してください。

プロジェクトが作成されたら、前のプロジェクトで使用したライブラリをインストールします。 以前の Web フォームプロジェクトでは、 *app.config*ファイルを使用して必要な NuGet パッケージを一覧表示している可能性があります。 新しい SDK スタイルのプロジェクトでは、 *app.config*がプロジェクトファイル内の `<PackageReference>` 要素に置き換えられました。 この方法の利点は、すべての依存関係が推移的にインストールされることです。 必要な最上位レベルの依存関係のみが表示されます。

使用している依存関係の多くは、Entity Framework 6 や log4net など、.NET Core で使用できます。 使用可能な .NET Core または .NET Standard バージョンがない場合は、.NET Framework バージョンを使用することがよくあります。 実際のメリットはケースによって異なります。 .NET Core で使用できない API を使用すると、ランタイムエラーが発生します。 Visual Studio は、このようなパッケージを通知します。 **ソリューションエクスプローラー**のプロジェクトの **[参照]** ノードに黄色いアイコンが表示されます。

Blazor ベースの eShop プロジェクトでは、インストールされているパッケージを確認できます。 以前は、*パッケージ .config*ファイルには、プロジェクトで使用されているすべてのパッケージが一覧表示されており、約50行のファイルが生成されていました。 *App.config*のスニペットは次のとおりです。

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  ...
  <package id="Microsoft.ApplicationInsights.Agent.Intercept" version="2.4.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.DependencyCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.PerfCounterCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.Web" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls.Core" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.MSAjax" version="5.0.0" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.WebForms" version="5.0.0" targetFramework="net472" />
  ...
  <package id="System.Memory" version="4.5.1" targetFramework="net472" />
  <package id="System.Numerics.Vectors" version="4.4.0" targetFramework="net472" />
  <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Channels" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Tasks.Extensions" version="4.5.1" targetFramework="net472" />
  <package id="WebGrease" version="1.6.0" targetFramework="net472" />
</packages>
```

`<packages>` 要素には、すべての必要な依存関係が含まれます。 これらのパッケージが必要であるため、どのパッケージが含まれているかを特定するのは困難です。 いくつかの `<package>` 要素は、必要な依存関係のニーズを満たすためだけに一覧表示されます。

Blazor プロジェクトには、プロジェクトファイル内の `<ItemGroup>` 要素内で必要な依存関係が一覧表示されます。

```xml
<ItemGroup>
    <PackageReference Include="Autofac" Version="4.9.3" />
    <PackageReference Include="EntityFramework" Version="6.3.0-preview9-19423-04" />
    <PackageReference Include="log4net" Version="2.0.8" />
</ItemGroup>
```

Web フォーム開発者の有効期間を簡略化する NuGet パッケージの1つは、 [Windows 互換機能パック](../../core/porting/windows-compat-pack.md)です。 .NET Core はクロスプラットフォームですが、一部の機能は Windows でのみ使用できます。 Windows 固有の機能は、互換機能パックをインストールすることによって提供されます。 このような機能の例としては、レジストリ、WMI、ディレクトリサービスなどがあります。 パッケージによって2万の Api が追加され、既に使い慣れている多くのサービスがアクティブ化されます。 EShop プロジェクトは、互換性パックを必要としません。ただし、プロジェクトで Windows 固有の機能が使用されている場合は、パッケージによって移行作業が容易になります。

## <a name="enable-startup-process"></a>スタートアッププロセスを有効にする

Blazor のスタートアッププロセスは Web フォームから変更され、他の ASP.NET Core サービスと同様のセットアップに従います。 ホストされるサーバー側では、Blazor コンポーネントは通常の ASP.NET Core アプリの一部として実行されます。 WebAssembly ブラウザーでホストされている場合、Blazor コンポーネントは同様のホスティングモデルを使用します。 違いは、コンポーネントが任意のバックエンドプロセスから独立したサービスとして実行されることです。 どちらの方法でも、スタートアップは似ています。

*Global.asax.cs*ファイルは、Web フォームプロジェクトの既定のスタートアップページです。 EShop プロジェクトでは、このファイルによってコントロールの反転 (IoC) コンテナーが構成され、アプリまたは要求のさまざまなライフサイクルイベントが処理されます。 これらのイベントの一部は、ミドルウェア (`Application_BeginRequest`など) で処理されます。 他のイベントでは、依存関係の挿入 (DI) によって特定のサービスをオーバーライドする必要があります。

たとえば、eShop の*Global.asax.cs*ファイルには、次のコードが含まれています。

```csharp
public class Global : HttpApplication, IContainerProviderAccessor
{
    private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

    static IContainerProvider _containerProvider;
    IContainer container;

    public IContainerProvider ContainerProvider
    {
        get { return _containerProvider; }
    }

    protected void Application_Start(object sender, EventArgs e)
    {
        // Code that runs on app startup
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);
        ConfigureContainer();
        ConfigDataBase();
    }

    /// <summary>
    /// Track the machine name and the start time for the session inside the current session
    /// </summary>
    protected void Session_Start(Object sender, EventArgs e)
    {
        HttpContext.Current.Session["MachineName"] = Environment.MachineName;
        HttpContext.Current.Session["SessionStartTime"] = DateTime.Now;
    }

    /// <summary>
    /// http://docs.autofac.org/en/latest/integration/webforms.html
    /// </summary>
    private void ConfigureContainer()
    {
        var builder = new ContainerBuilder();
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);
        builder.RegisterModule(new ApplicationModule(mockData));
        container = builder.Build();
        _containerProvider = new ContainerProvider(container);
    }

    private void ConfigDataBase()
    {
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);

        if (!mockData)
        {
            Database.SetInitializer<CatalogDBContext>(container.Resolve<CatalogDBInitializer>());
        }
    }

    protected void Application_BeginRequest(object sender, EventArgs e)
    {
        //set the property to our new object
        LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper();

        LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo();

        _log.Debug("Application_BeginRequest");
    }
}
```

上記のファイルは、サーバー側 Blazor の `Startup` クラスになります。

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    public IConfiguration Configuration { get; }

    public IWebHostEnvironment Env { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages();
        services.AddServerSideBlazor();

        if (Configuration.GetValue<bool>("UseMockData"))
        {
            services.AddSingleton<ICatalogService, CatalogServiceMock>();
        }
        else
        {
            services.AddScoped<ICatalogService, CatalogService>();
            services.AddScoped<IDatabaseInitializer<CatalogDBContext>, CatalogDBInitializer>();
            services.AddSingleton<CatalogItemHiLoGenerator>();
            services.AddScoped(_ => new CatalogDBContext(Configuration.GetConnectionString("CatalogDBContext")));
        }
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddLog4Net("log4Net.xml");

        if (Env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/Home/Error");
        }

        // Middleware for Application_BeginRequest
        app.Use((ctx, next) =>
        {
            LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper(ctx);
            LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo(ctx);
            return next();
        });

        app.UseStaticFiles();

        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapBlazorHub();
            endpoints.MapFallbackToPage("/_Host");
        });

        ConfigDataBase(app);
    }

    private void ConfigDataBase(IApplicationBuilder app)
    {
        using (var scope = app.ApplicationServices.CreateScope())
        {
            var initializer = scope.ServiceProvider.GetService<IDatabaseInitializer<CatalogDBContext>>();

            if (initializer != null)
            {
                Database.SetInitializer(initializer);
            }
        }
    }
}
```

Web フォームからの重要な変更点の1つに、DI のこうし突出があります。 DI は、ASP.NET Core 設計の基本原則でした。 ASP.NET Core framework のほぼすべての側面のカスタマイズをサポートしています。 多くのシナリオで使用できる組み込みのサービスプロバイダーもあります。 より多くのカスタマイズが必要な場合は、多くのコミュニティプロジェクトでサポートできます。 たとえば、サードパーティの DI ライブラリの投資を進めることができます。

元の eShop アプリには、セッション管理の構成がいくつかあります。 サーバー側の Blazor は通信に ASP.NET Core SignalR を使用するため、接続が HTTP コンテキストとは関係なく発生する可能性があるため、セッション状態はサポートされません。 セッション状態を使用するアプリでは、Blazor アプリとして実行する前に、再設計が必要です。

アプリの起動の詳細については、「[アプリのスタートアップ](app-startup.md)」を参照してください。

## <a name="migrate-http-modules-and-handlers-to-middleware"></a>HTTP モジュールとハンドラーのミドルウェアへの移行

Http モジュールとハンドラーは、HTTP 要求パイプラインを制御するための Web フォームの一般的なパターンです。 `IHttpModule` または `IHttpHandler` を実装するクラスは、登録して、受信要求を処理することができます。 Web フォームは、 *web.config ファイルで*モジュールとハンドラーを構成します。 Web フォームは、アプリライフサイクルイベントの処理にも大きく基づいています。 代わりに、ミドルウェアを使用 ASP.NET Core ます。 ミドルウェアは、`Startup` クラスの `Configure` メソッドに登録されます。 ミドルウェアの実行順序は、登録順序によって決まります。

[[スタートアッププロセスを有効](#enable-startup-process)にする] セクションでは、`Application_BeginRequest` メソッドとして Web フォームによってライフサイクルイベントが発生しました。 このイベントは ASP.NET Core では使用できません。 この動作を実現する1つの方法は、 *Startup.cs*ファイルの例に示すようにミドルウェアを実装することです。 このミドルウェアは同じロジックを行い、ミドルウェアパイプライン内の次のハンドラーに制御を転送します。

モジュールとハンドラーの移行の詳細については、「 [ASP.NET Core ミドルウェアへの HTTP ハンドラーとモジュールの移行](/aspnet/core/migration/http-modules)」を参照してください。

## <a name="migrate-static-files"></a>静的ファイルを移行する

静的ファイル (HTML、CSS、画像、JavaScript など) を提供するには、ミドルウェアによってファイルが公開されている必要があります。 `UseStaticFiles` メソッドを呼び出すと、web ルートパスからの静的ファイルを提供できるようになります。 既定の web ルートディレクトリは*wwwroot*ですが、カスタマイズすることができます。 EShop の `Startup` クラスの `Configure` メソッドに含まれるように、次のようになります。

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    ...
}
```

EShop プロジェクトは、基本的な静的ファイルアクセスを可能にします。 静的ファイルアクセスには多くのカスタマイズが用意されています。 既定のファイルまたはファイルブラウザーを有効にする方法については、「 [ASP.NET Core の静的ファイル](/aspnet/core/fundamentals/static-files)」を参照してください。

## <a name="migrate-runtime-bundling-and-minification-setup"></a>ランタイムのバンドルと縮小のセットアップを移行する

バンドルと縮小は、特定のファイルの種類を取得するためのサーバー要求の数とサイズを減らすためのパフォーマンスの最適化手法です。 多くの場合、JavaScript と CSS では、クライアントに送信される前に何らかの形式のバンドルまたは縮小が行われます。 ASP.NET Web フォームでは、これらの最適化は実行時に処理されます。 最適化規則は、 *App_Start/bundleconfig.cs*ファイルに定義されています。 ASP.NET Core では、より宣言的な方法が採用されています。 ファイルには、特定の縮小設定と共に、縮小するファイルが一覧表示されます。

バンドルと縮小の詳細については、「 [ASP.NET Core での静的資産のバンドルと](/aspnet/core/client-side/bundling-and-minification)縮小」を参照してください。

## <a name="migrate-aspx-pages"></a>ASPX ページの移行

Web フォームアプリのページは、 *.aspx*拡張子を持つファイルです。 Web フォームページは、多くの場合、Blazor 内のコンポーネントにマップできます。 Blazor コンポーネントは、拡張子が*razor*のファイルで作成されます。 EShop プロジェクトの場合、5つのページが Razor ページに変換されます。

たとえば、詳細ビューは、Web フォームプロジェクトでは、 *details*、 *Details.aspx.cs*、および*Details.aspx.designer.cs*の3つのファイルで構成されます。 Blazor に変換する場合、分離コードとマークアップは、*詳細な razor*に結合されます。 Razor コンパイル ( *designer.cs*ファイルの内容と同じ) は、 *obj*ディレクトリに格納されていますが、既定では**ソリューションエクスプローラー**で表示できません。 Web フォームページは、次のマークアップで構成されています。

```aspx-csharp
<%@ Page Title="Details" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Details.aspx.cs" Inherits="eShopLegacyWebForms.Catalog.Details" %>

<asp:Content ID="Details" ContentPlaceHolderID="MainContent" runat="server">
    <h2 class="esh-body-title">Details</h2>

    <div class="container">
        <div class="row">
            <asp:Image runat="server" CssClass="col-md-6 esh-picture" ImageUrl='<%#"/Pics/" + product.PictureFileName%>' />
            <dl class="col-md-6 dl-horizontal">
                <dt>Name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Name%>' />
                </dd>

                <dt>Description
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Description%>' />
                </dd>

                <dt>Brand
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogBrand.Brand%>' />
                </dd>

                <dt>Type
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogType.Type%>' />
                </dd>
                <dt>Price
                </dt>

                <dd>
                    <asp:Label CssClass="esh-price" runat="server" Text='<%#product.Price%>' />
                </dd>

                <dt>Picture name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.PictureFileName%>' />
                </dd>

                <dt>Stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.AvailableStock%>' />
                </dd>

                <dt>Restock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.RestockThreshold%>' />
                </dd>

                <dt>Max stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.MaxStockThreshold%>' />
                </dd>

            </dl>
        </div>

        <div class="form-actions no-color esh-link-list">
            <a runat="server" href='<%# GetRouteUrl("EditProductRoute", new {id =product.Id}) %>' class="esh-link-item">Edit
            </a>
            |
            <a runat="server" href="~" class="esh-link-item">Back to list
            </a>
        </div>

    </div>
</asp:Content>
```

前のマークアップの分離コードには、次のコードが含まれています。

```csharp
using eShopLegacyWebForms.Models;
using eShopLegacyWebForms.Services;
using log4net;
using System;
using System.Web.UI;

namespace eShopLegacyWebForms.Catalog
{
    public partial class Details : System.Web.UI.Page
    {
        private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

        protected CatalogItem product;

        public ICatalogService CatalogService { get; set; }

        protected void Page_Load(object sender, EventArgs e)
        {
            var productId = Convert.ToInt32(Page.RouteData.Values["id"]);
            _log.Info($"Now loading... /Catalog/Details.aspx?id={productId}");
            product = CatalogService.FindCatalogItem(productId);

            this.DataBind();
        }
    }
}
```

Blazor に変換されると、Web フォームページは次のコードに変換されます。

```razor
@page "/Catalog/Details/{id:int}"
@inject ICatalogService CatalogService
@inject ILogger<Details> Logger

<h2 class="esh-body-title">Details</h2>

<div class="container">
    <div class="row">
        <img class="col-md-6 esh-picture" src="@($"/Pics/{_item.PictureFileName}")">

        <dl class="col-md-6 dl-horizontal">
            <dt>
                Name
            </dt>

            <dd>
                @_item.Name
            </dd>

            <dt>
                Description
            </dt>

            <dd>
                @_item.Description
            </dd>

            <dt>
                Brand
            </dt>

            <dd>
                @_item.CatalogBrand.Brand
            </dd>

            <dt>
                Type
            </dt>

            <dd>
                @_item.CatalogType.Type
            </dd>
            <dt>
                Price
            </dt>

            <dd>
                @_item.Price
            </dd>

            <dt>
                Picture name
            </dt>

            <dd>
                @_item.PictureFileName
            </dd>

            <dt>
                Stock
            </dt>

            <dd>
                @_item.AvailableStock
            </dd>

            <dt>
                Restock
            </dt>

            <dd>
                @_item.RestockThreshold
            </dd>

            <dt>
                Max stock
            </dt>

            <dd>
                @_item.MaxStockThreshold
            </dd>

        </dl>
    </div>

    <div class="form-actions no-color esh-link-list">
        <a href="@($"/Catalog/Edit/{_item.Id}")" class="esh-link-item">
            Edit
        </a>
        |
        <a href="/" class="esh-link-item">
            Back to list
        </a>
    </div>

</div>

@code {
    private CatalogItem _item;

    [Parameter]
    public int Id { get; set; }

    protected override void OnInitialized()
    {
        Logger.LogInformation("Now loading... /Catalog/Details/{Id}", Id);

        _item = CatalogService.FindCatalogItem(Id);
    }
}
```

コードとマークアップが同じファイル内にあることに注意してください。 必要なサービスは、`@inject` 属性を使用してアクセスできるようになります。 `@page` ディレクティブごとに、このページには `Catalog/Details/{id}` ルートでアクセスできます。 ルートの `{id}` プレースホルダーの値が、整数に制限されています。 「[ルーティング](pages-routing-layouts.md)」セクションで説明したように、Web フォームとは異なり、Razor コンポーネントはそのルートと、含まれるすべてのパラメーターを明示的に指定します。 多くの Web フォームコントロールには、Blazor の対応するものが含まれていない場合があります。 多くの場合、同じ目的で使用される同等の HTML スニペットがあります。 たとえば、`<asp:Label />` コントロールは HTML `<label>` 要素に置き換えることができます。

### <a name="model-validation-in-blazor"></a>Blazor でのモデルの検証

Web フォームのコードに検証が含まれている場合は、ほとんど変更せずに、必要なものの多くを転送できます。 Blazor で実行する利点は、カスタム JavaScript がなくても同じ検証ロジックを実行できることです。 データ注釈を使用すると、モデルの検証が容易になります。

たとえば、 *Create .aspx*ページには、検証を含むデータ入力フォームがあります。 スニペットの例は次のようになります。

```aspx
<div class="form-group">
    <label class="control-label col-md-2">Name</label>
    <div class="col-md-3">
        <asp:TextBox ID="Name" runat="server" CssClass="form-control"></asp:TextBox>
        <asp:RequiredFieldValidator runat="server" ControlToValidate="Name" Display="Dynamic"
            CssClass="field-validation-valid text-danger" ErrorMessage="The Name field is required." />
    </div>
</div>
```

Blazor では、同じマークアップが、*作成した razor*ファイルで提供されます。

```razor
<EditForm Model="_item" OnValidSubmit="@...">
    <DataAnnotationsValidator />

    <div class="form-group">
        <label class="control-label col-md-2">Name</label>
        <div class="col-md-3">
            <InputText class="form-control" @bind-Value="_item.Name" />
            <ValidationMessage For="(() => _item.Name)" />
        </div>
    </div>

    ...
</EditForm>
```

`EditForm` コンテキストには検証のサポートが含まれており、入力をラップすることができます。 データ注釈は、検証を追加する一般的な方法です。 このような検証のサポートは、`DataAnnotationsValidator` コンポーネントを通じて追加できます。 このメカニズムの詳細については、「 [ASP.NET Core Blazor フォームと検証](/aspnet/core/blazor/forms-validation)」を参照してください。

## <a name="migrate-built-in-web-forms-controls"></a>組み込みの Web フォームコントロールを移行する

*このコンテンツは近日公開予定です。*

## <a name="migrate-configuration"></a>構成の移行

Web フォームプロジェクトでは、通常、構成データは*web.config ファイルに*格納されます。 構成データには `ConfigurationManager`を使用してアクセスします。 多くの場合、オブジェクトの解析にはサービスが必要でした。 .NET Framework 4.7.2 では、`ConfigurationBuilders`を使用して構成に省かが追加されました。 これらのビルダーを使用すると、開発者は、必要な値を取得するために実行時に構成された構成のさまざまなソースを追加できます。

ASP.NET Core には、アプリとデプロイで使用される構成ソースを定義できる柔軟な構成システムが導入されました。 Web フォームアプリで使用している `ConfigurationBuilder` インフラストラクチャは、ASP.NET Core 構成システムで使用されている概念に基づいてモデル化されています。

次のスニペットは、web フォームの eShop プロジェクトが web.config を使用して構成値を格納する方法を示して*い*ます。

```xml
<configuration>
  <configSections>
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
  </configSections>
  <connectionStrings>
    <add name="CatalogDBContext" connectionString="Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
  </connectionStrings>
  <appSettings>
    <add key="UseMockData" value="true" />
    <add key="UseCustomizationData" value="false" />
  </appSettings>
```

データベース接続文字列などの機密情報は、web.config 内に格納されるのが一般的*です。* シークレットは、ソース管理などの安全でない場所に必然的に保持されます。 ASP.NET Core 上の Blazor では、上記の XML ベースの構成が次の JSON に置き換えられます。

```json
{
  "ConnectionStrings": {
    "CatalogDBContext": "Data Source=(localdb)\\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;"
  },
  "UseMockData": true,
  "UseCustomizationData": false
}
```

JSON は、既定の構成形式です。ただし、ASP.NET Core は XML などの他の多くの形式をサポートしています。 コミュニティでサポートされている形式もいくつかあります。

Blazor プロジェクトの `Startup` クラスのコンストラクターは、コンストラクターの挿入と呼ばれる DI の手法を使用して `IConfiguration` インスタンスを受け入れます。

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    ...
}
```

既定では、環境変数、JSON ファイル (*appsettings*および*appsettings. {Environment}. json*)、およびコマンドラインオプションは、構成オブジェクトの有効な構成ソースとして登録されます。 構成ソースには `Configuration[key]`経由でアクセスできます。 より高度な手法は、オプションパターンを使用して、構成データをオブジェクトにバインドすることです。 構成とオプションのパターンの詳細については、それぞれ ASP.NET Core の「ASP.NET Core と[オプションのパターン](/aspnet/core/fundamentals/configuration/options)の[構成](/aspnet/core/fundamentals/configuration/)」を参照してください。

## <a name="migrate-data-access"></a>データアクセスの移行

データアクセスは、あらゆるアプリの重要な側面です。 EShop プロジェクトは、カタログ情報をデータベースに格納し、Entity Framework (EF) 6 でデータを取得します。 EF 6 は .NET Core 3.0 でサポートされているため、プロジェクトで引き続き使用できます。

EShop には、次の EF 関連の変更が必要でした。

- .NET Framework では、`DbContext` オブジェクトは*name = ConnectionString*という形式の文字列を受け取り、`ConfigurationManager.AppSettings[ConnectionString]` の接続文字列を使用して接続します。 .NET Core では、これはサポートされていません。 接続文字列を指定する必要があります。
- データベースは同期的にアクセスされました。 これは機能しますが、スケーラビリティが低下する可能性があります。 このロジックは、非同期パターンに移行する必要があります。

データセットバインドに対するネイティブサポートは同じではありませんが、Blazor でC#は、Razor ページでのサポートの柔軟性と性能が提供されます。 たとえば、計算を実行して結果を表示できます。 Blazor のデータパターンの詳細については、「[データアクセス](data.md)」の章を参照してください。

## <a name="architectural-changes"></a>アーキテクチャの変更

最後に、Blazor に移行するときに考慮すべき重要なアーキテクチャの違いがいくつかあります。 これらの変更の多くは、.NET Core または ASP.NET Core に基づくすべてのものに適用されます。

Blazor は .NET Core 上に構築されているため、.NET Core でのサポートを保証する際に考慮すべき点があります。 主な変更点には、次の機能の削除が含まれます。

- 複数の Appdomain
- リモート処理
- コード アクセス セキュリティ (CAS)
- セキュリティ透過性

.NET Core での実行をサポートするために必要な変更を識別する方法の詳細については、「 [.NET Framework から .Net core へのコードの移植](/dotnet/core/porting)」を参照してください。

ASP.NET Core は ASP.NET の再想像されたバージョンであり、初期に明らかでないと思われる変更がいくつかあります。 主な変更点は次のとおりです。

- 同期コンテキストがありません。つまり、`HttpContext.Current`、`Thread.CurrentPrincipal`、またはその他の静的アクセサーは存在しません。
- シャドウコピーなし
- 要求キューがありません

ASP.NET Core の多くの操作は非同期であるため、i/o バインドタスクを簡単にオフロードできます。 `Task.Wait()` または `Task.GetResult()`を使用してブロックしないことが重要です。これにより、スレッドプールのリソースがすぐに枯渇する可能性があります。

## <a name="migration-conclusion"></a>移行の結論

この時点で、Web フォームプロジェクトを Blazor に移動するために必要ないくつかの例を見てきました。 完全な例については、 [eShopOnBlazor](https://github.com/dotnet-architecture/eShopOnBlazor)プロジェクトを参照してください。

>[!div class="step-by-step"]
>[前へ](security-authentication-authorization.md)
