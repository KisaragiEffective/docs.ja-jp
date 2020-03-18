---
ms.openlocfilehash: ac5a3c4f3aefbb59418ad92b2d795f36916f877f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72394113"
---
### <a name="spas-spaservices-and-nodeservices-marked-obsolete"></a>SPA: SpaServices および NodeServices が古いものとしてマークされた

以下の NuGet パッケージの内容は、ASP.NET Core 2.1 以降ではすべて不要になりました。 その結果、以下のパッケージは古いものとしてマークされています。

- [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/)
- [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/)

同じ理由から、以下の npm モジュールは非推奨としてマークされています。

- [aspnet-angular](https://www.npmjs.com/package/aspnet-angular)
- [aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering)
- [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack)
- [aspnet-webpack-react](https://www.npmjs.com/package/aspnet-webpack-react)
- [domain-task](https://www.npmjs.com/package/domain-task)

上記のパッケージと npm モジュールは、今後 .NET 5 で削除される予定です。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

非推奨のパッケージと npm モジュールは、ASP.NET Core をさまざまなシングルページ アプリ (SPA) フレームワークと統合するためのものでした。 そのようなフレームワークとしては、Angular、React、React with Redux などがあります。

#### <a name="new-behavior"></a>新しい動作

[Microsoft.AspNetCore.SpaServices.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions/) NuGet パッケージには新しい統合メカニズムがあります。 そのパッケージは、ASP.NET Core 2.1 以降、Angular および React プロジェクト テンプレートの基礎になっています。

#### <a name="reason-for-change"></a>変更理由

ASP.NET Core では、Angular、React、React with Redux など、さまざまなシングルページ アプリ (SPA) フレームワークとの統合がサポートされています。 当初、これらのフレームワークとの統合は、サーバー側プリレンダリングや Webpack との統合などのシナリオを処理する ASP.NET Core 固有のコンポーネントを使用して行われていました。 時間と共に、業界標準は変化しました。 各 SPA フレームワークにおいて、独自の標準コマンドライン インターフェイスがリリースされました。 たとえば、Angular CLI や create-react-app などです。

2018 年 5 月に ASP.NET Core 2.1 がリリースされたとき、チームは標準の変更に対応しました。 SPA フレームワーク独自のツールチェーンと統合するための、より新しくて簡単な方法が提供されました。 その新しい統合メカニズムはパッケージ `Microsoft.AspNetCore.SpaServices.Extensions` に存在しており、ASP.NET Core 2.1 以降、Angular および React プロジェクト テンプレートの基礎になっています。

古い ASP.NET Core 固有のコンポーネントは不適切であり、推奨されないことを明確にするために、次のようにしました。

- 2\.1 より前の統合メカニズムは、旧式とマークされています。
- npm パッケージのサポートは非推奨とマークされています。

#### <a name="recommended-action"></a>推奨アクション

これらのパッケージを使用している場合は、次の機能を使用するようにアプリを更新してください。

- `Microsoft.AspNetCore.SpaServices.Extensions` パッケージ内のもの。
- 使用している SPA フレームワークによって提供されるもの

サーバー側プリレンダリングやホット モジュール リロードなどの機能を有効にするには、対応する SPA フレームワークのドキュメントを参照してください。 `Microsoft.AspNetCore.SpaServices.Extensions` の機能は古くなって "*おらず*"、引き続きサポートされます。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

- <xref:Microsoft.AspNetCore.Builder.SpaRouteExtensions?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Builder.WebpackDevMiddleware?displayProperty=nameWithType>

- <xref:Microsoft.AspNetCore.NodeServices.EmbeddedResourceReader?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.INodeServices?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.NodeServicesFactory?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.NodeServicesOptions?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.StringAsTempFile?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.INodeInstance?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.NodeInvocationException?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.NodeInvocationInfo?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.NodeServicesOptionsExtensions?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.OutOfProcessNodeInstance?displayProperty=nameWithType>

- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.ISpaPrerenderer?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.ISpaPrerendererBuilder?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.JavaScriptModuleExport?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.Prerenderer?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.PrerenderTagHelper?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.RenderToStringResult?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Webpack.WebpackDevMiddlewareOptions?displayProperty=nameWithType>

- <xref:Microsoft.Extensions.DependencyInjection.NodeServicesServiceCollectionExtensions?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.DependencyInjection.PrerenderingServiceCollectionExtensions?displayProperty=nameWithType>

<!--

#### Affected APIs

- `T:Microsoft.AspNetCore.Builder.SpaRouteExtensions`
- `T:Microsoft.AspNetCore.Builder.WebpackDevMiddleware`

- `T:Microsoft.AspNetCore.NodeServices.EmbeddedResourceReader`
- `T:Microsoft.AspNetCore.NodeServices.INodeServices`
- `T:Microsoft.AspNetCore.NodeServices.NodeServicesFactory`
- `T:Microsoft.AspNetCore.NodeServices.NodeServicesOptions`
- `T:Microsoft.AspNetCore.NodeServices.StringAsTempFile`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.INodeInstance`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.NodeInvocationException`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.NodeInvocationInfo`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.NodeServicesOptionsExtensions`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.OutOfProcessNodeInstance`

- `T:Microsoft.AspNetCore.SpaServices.Prerendering.ISpaPrerenderer`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.ISpaPrerendererBuilder`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.JavaScriptModuleExport`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.Prerenderer`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.PrerenderTagHelper`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.RenderToStringResult`
- `T:Microsoft.AspNetCore.SpaServices.Webpack.WebpackDevMiddlewareOptions`

- `T:Microsoft.Extensions.DependencyInjection.NodeServicesServiceCollectionExtensions`
- `T:Microsoft.Extensions.DependencyInjection.PrerenderingServiceCollectionExtensions`

-->
