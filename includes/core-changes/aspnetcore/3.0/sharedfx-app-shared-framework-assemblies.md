---
ms.openlocfilehash: a8146db1fb54d63d4716b879ce793f7d817cef59
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937294"
---
### <a name="shared-framework-assemblies-removed-from-microsoftaspnetcoreapp"></a>共有フレームワーク: Microsoft.AspNetCore.App から削除されたアセンブリ

ASP.NET Core 3.0 以降、ASP.NET Core 共有フレームワーク (`Microsoft.AspNetCore.App`) には、Microsoft が完全に開発、サポートし、処理可能なファースト パーティのアセンブリだけが含まれています。

#### <a name="change-description"></a>変更の説明

この変更は、ASP.NET Core "プラットフォーム" の境界の再定義と考えてください。 共有フレームワークは、[GitHub を介して誰でもソース ビルドが可能](https://github.com/dotnet/source-build)になり、.NET Core 共有フレームワークの既存の利点をアプリに引き続き提供します。 利点として、デプロイのサイズの縮小、修正プログラムの適用の一元化、起動時間の短縮などがあります。

変更の一貫として、いくつかの注目すべき破壊的変更が `Microsoft.AspNetCore.App` に導入されました。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

プロジェクトでは、プロジェクト ファイル内の `<PackageReference>` 要素を介して `Microsoft.AspNetCore.App` を参照していました。

さらに、`Microsoft.AspNetCore.App` に次のサブコンポーネントが含まれていました。

- Json.NET (`Newtonsoft.Json`)
- Entity Framework Core (`Microsoft.EntityFrameworkCore.` がプレフィックスとして付加されたアセンブリ)
- Roslyn (`Microsoft.CodeAnalysis`)

#### <a name="new-behavior"></a>新しい動作

`Microsoft.AspNetCore.App` への参照に、プロジェクト ファイル内の `<PackageReference>` 要素は不要になりました。 .NET Core SDK では、`<FrameworkReference>` と呼ばれる新しい要素がサポートされています。これは、`<PackageReference>` に代わって使用されます。

詳細については、[dotnet/aspnetcore#3612](https://github.com/dotnet/aspnetcore/issues/3612) を参照してください。

Entity Framework Core は NuGet パッケージとして配布されます。 この変更により、.NET 上の他のすべてのデータ アクセス ライブラリに合わせて配布モデルが調整されます。 さまざまな .NET プラットフォームをサポートしながらイノベーションを継続するための最も簡単なパスが Entity Framework Core に提供されます。 共有フレームワークからの Entity Framework Core の移動は、Microsoft が開発、サポートし、処理可能なライブラリとしての状態には影響しません。 [.NET Core のサポート ポリシー](https://www.microsoft.com/net/platform/support-policy)は、引き続きこれに対応しています。

Json.NET と Entity Framework Core は、ASP.NET Core と引き続き連携します。 ただし、これらは共有フレームワークには含まれません。

詳細については、「[The future of JSON in .NET Core 3.0 (.NET Core 3.0 での JSON の未来)](https://github.com/dotnet/announcements/issues/90)」をご覧ください。 また、共有フレームワークから削除された[バイナリの一覧](https://github.com/dotnet/aspnetcore/issues/3755)もご覧ください。

#### <a name="reason-for-change"></a>変更理由

この変更により、`Microsoft.AspNetCore.App` の使用が簡素化され、NuGet パッケージと共有フレームワーク間の重複が削減されます。

この変更の目的の詳細については、[こちらのブログ記事](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/)をご覧ください。

#### <a name="recommended-action"></a>推奨アクション

プロジェクトでは、`Microsoft.AspNetCore.App` のアセンブリを NuGet パッケージとして使用する必要はありません。 ASP.NET Core 共有フレームワークのターゲット設定と使用を簡素化するために、ASP.NET Core 1.0 以降に配布された多くの NuGet パッケージが生成されなくなりました。 これらのパッケージで提供される API は、`Microsoft.AspNetCore.App` への `<FrameworkReference>` を使用することで引き続きアプリで使用できます。 一般的な API の例として、Kestrel、MVC、Razor などがあります。

この変更は、ASP.NET Core 2.x の `Microsoft.AspNetCore.App` を介して参照されるすべてのバイナリに適用されるわけではありません。 重要な例外は次のとおりです。

- .NET Standard を引き続きターゲットとする `Microsoft.Extensions` ライブラリは、NuGet パッケージとして利用できます (https://github.com/dotnet/extensions) を参照してください)。
- ASP.NET Core チームによって生成され、`Microsoft.AspNetCore.App` に含まれていない API。 たとえば、次のコンポーネントは NuGet パッケージとして利用できます。
  - Entity Framework Core
  - サード パーティの統合を提供するAPI
  - 試験的な機能
  - [共有フレームワークに含めるための要件](https://github.com/dotnet/aspnetcore/blob/4e44e5bcbedd961cc0d4f6b846699c7c494f5597/docs/SharedFramework.md)を満たすことができなかった依存関係を持つ API
- Json.NET のサポートを維持する MVC の拡張機能。 API は、Json.NET と MVC の使用をサポートする NuGet パッケージとして提供されます。
- SignalR .NET クライアントは、.NET Standard を引き続きサポートし、NuGet パッケージとして配布されます。 これは、Xamarin や UWP など、多くの .NET ランタイムでの使用を目的としています。

詳細については、「[Stop producing packages for shared framework assemblies in 3.0 (3.0 における共有フレームワークのアセンブリのパッケージ生成の停止)](https://github.com/dotnet/aspnetcore/issues/3756)」をご覧ください。 ディスカッションについては、[dotnet/aspnetcore#3757](https://github.com/dotnet/aspnetcore/issues/3757) を参照してください。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

- <xref:Microsoft.CodeAnalysis?displayProperty=nameWithType>
- <xref:Microsoft.EntityFrameworkCore?displayProperty=nameWithType>

<!--

#### Affected APIs

- `N:Microsoft.CodeAnalysis`
- `N:Microsoft.EntityFrameworkCore`

-->
