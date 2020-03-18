---
title: .NET ライブラリのクロス プラットフォーム ターゲット
description: クロス プラットフォームの .NET ライブラリを作成する際のベスト プラクティスの推奨事項。
ms.date: 08/12/2019
ms.openlocfilehash: 61adff3759984554bb83531b4f9d8a49e29c929c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76731450"
---
# <a name="cross-platform-targeting"></a>クロス プラットフォーム ターゲット

最新の .NET では、複数のオペレーティング システムとデバイスがサポートされます。 .NET のオープンソース ライブラリでは、開発者が Azure でホストされる ASP.NET Web サイトを構築しているか、Unity で .NET ゲームを構築しているかに関わらず、できるだけ多くの開発者をサポートすることが重要です。

## <a name="net-standard"></a>.NET Standard

.NET Standard は、.NET ライブラリにクロスプラットフォーム サポートを追加する最善の方法です。 [.NET Standard](../net-standard.md) は、すべての .NET 実装で使用できる .NET API の仕様です。 .NET Standard をターゲットにすることで、特定のバージョンの .NET Standard 内にある API を使用するように制約されるライブラリを作成できます。これは、そのバージョンの .NET Standard を実装するすべてのプラットフォームで使用できることを意味します。

![.NET Standard](./media/cross-platform-targeting/platforms-netstandard.png ".NET Standard")

.NET Standard をターゲットとし、プロジェクトを正常にコンパイルした場合、すべてのプラットフォームでライブラリが正常に実行されることは保証されません。

1. プラットフォーム固有の API は、他のプラットフォームでは失敗します。 たとえば、<xref:Microsoft.Win32.Registry?displayProperty=nameWithType> は Windows では成功し、他の OS で使用する場合、<xref:System.PlatformNotSupportedException> がスローされます。
2. API の動作はそれぞれ異なる場合があります。 たとえば、アプリケーションによって iOS や UWP で Ahead Of Time コンパイルが使用される場合、リフレクション API のパフォーマンス特性は異なります。

> [!TIP]
> .NET チームによって [Roslyn アナライザー](../analyzers/api-analyzer.md)が提供されます。これは、考えられる問題を検出するのに役立ちます。

✔️ まず、`netstandard2.0` ターゲットを含めることから始めてください。

> ほとんどの汎用ライブラリでは、.NET Standard 2.0 外の API は必要ありません。 .NET Standard 2.0 はすべての最新のプラットフォームでサポートされており、1 つのターゲットで複数のプラットフォームをサポートする場合に推奨される方法です。

❌ `netstandard1.x` ターゲットを含めることは避けてください。

> .NET Standard 1.x は、NuGet の細かいパッケージ セットとして配布されます。大きなパッケージの依存関係グラフが作成されるため、開発者は構築時に多くのパッケージをダウンロードすることになります。 .NET Framework 4.6.1、UWP および Xamarin など、最新の .NET プラットフォームではすべて、.NET Standard 2.0 がサポートされます。 特に古いプラットフォームをターゲットにする必要がある場合は、.NET Standard 1.x のみをターゲットにしてください。

✔️ `netstandard2.0` ターゲットを必要とする場合は、`netstandard1.x` ターゲットを含めてください。

> .NET Standard 2.0 をサポートするすべてのプラットフォームでは `netstandard2.0` ターゲットを使用し、より小さなパッケージ グラフを作成することで利点が得られますが、古いプラットフォームは引き続き動作し、`netstandard1.x` ターゲットを使用するようにフォールバックします。

❌ ライブラリがプラットフォーム固有のアプリ モデルに依存する場合は、.NET Standard ターゲットを含めないでください。

> たとえば、UWP コントロール ツールキット ライブラリは、UWP でのみ使用できるアプリ モデルによって異なります。 アプリ モデル固有の API を .NET Standard で使用することはできません。

## <a name="multi-targeting"></a>マルチターゲット

場合によっては、ご利用のライブラリからフレームワーク固有の API にアクセスする必要があります。 フレームワーク固有の API を呼び出す最善の方法は、1 つだけでなく、多くの [.NET ターゲット フレームワーク](../frameworks.md)に対するプロジェクトを構築する、マルチ ターゲットを使用することです。

コンシューマーが個々のフレームワークに対して構築しなくても済むように、.NET Standard 出力に加え、1 つ以上のフレームワーク固有の出力が得られるようにします。 マルチ ターゲットを使用すると、単一の NuGet アセンブリ内にすべてのアセンブリがパッケージ化されます。 その後、コンシューマーは同じパッケージを参照でき、NuGet では適切な実装が選択されます。 .NET Standard ライブラリはフォールバック ライブラリとして機能します。このライブラリは、NuGet パッケージでフレームワーク固有の実装が提供される場合を除き、あらゆる場所で使用されます。 マルチ ターゲットでは、コードで条件付きコンパイルを使用して、フレームワーク固有の API を呼び出すことができます。

![複数のアセンブリを含む NuGet パッケージ](./media/cross-platform-targeting/nuget-package-multiple-assemblies.png "複数のアセンブリを含む NuGet パッケージ")

✔️ .NET Standard に加え、.NET 実装をターゲットにすることを検討してください。

> .NET 実装をターゲットにすることで、.NET Standard 外にあるプラットフォーム固有の API を呼び出すことができます。
>
> これを行うときに、.NET Standard のサポートを放棄しないでください。 代わりに、実装から分離させて、機能 API を提供します。 これにより、ライブラリを任意の場所で使用でき、ランタイムの機能を際立たせることができます。

```csharp
public static class GpsLocation
{
    // This project uses multi-targeting to expose device-specific APIs to .NET Standard.
    public static async Task<(double latitude, double longitude)> GetCoordinatesAsync()
    {
#if NET461
        return CallDotNetFramworkApi();
#elif WINDOWS_UWP
        return CallUwpApi();
#else
        throw new PlatformNotSupportedException();
#endif
    }

    // Allows callers to check without having to catch PlatformNotSupportedException
    // or replicating the OS check.
    public static bool IsSupported
    {
        get
        {
#if NET461 || WINDOWS_UWP
            return true;
#else
            return false;
#endif
        }
    }
}
```

❌ すべてのターゲットでソース コードが同じである場合は、マルチターゲットにすること、または .NET Standard をターゲットにすることは避けてください。

> .NET Standard アセンブリは、NuGet によって自動的に使用されます。 個々の .NET 実装をターゲットにすると、`*.nupkg` サイズが増えるだけで、利点はありません。

✔️ `net461` ターゲットを提供する場合、`netstandard2.0` のターゲットを追加することを検討してください。

> .NET Framework から .NET Standard 2.0 を使用する場合、いくつかの問題がありますが、.NET Framework 4.7.2 で対処されました。 .NET Framework 4.6.1 から 4.7.1 を引き続き使用している開発者に、.NET Framework 4.6.1 用にビルドされているバイナリを提供することで、その開発者のエクスペリエンスを向上させることができます。

NuGet パッケージを使用してご利用のライブラリの配布を✔️ 実施してください。

> NuGet では開発者にとって最適なターゲットが選択され、開発者が適切な実装を選ぶ必要はなくなります。

✔️ マルチターゲットにする場合は、プロジェクト ファイルの `TargetFrameworks` プロパティを使用してください。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <!-- This project will output netstandard2.0 and net461 assemblies -->
    <TargetFrameworks>netstandard2.0;net461</TargetFrameworks>
  </PropertyGroup>
</Project>
```

✔️ UWP と Xamarin をマルチターゲットにする場合は、[MSBuild.Sdk.Extras](https://github.com/onovotny/MSBuildSdkExtras) を使用することを検討してください (プロジェクト ファイルが大幅に簡素化されるため)。

## <a name="older-targets"></a>古いターゲット

.NET では、長い間サポート対象外になっている NET Framework のバージョンと、一般的には使用されなくなったプラットフォームをターゲットにすることができます。 できるだけ多くのターゲットでライブラリを動作させる価値はありますが、API の欠落を回避する必要がある場合、オーバーヘッドが大幅に増える可能性があります。 その範囲と制限事項を考えれば、特定のフレームワークをターゲットとする価値はなくなったと思われます。

❌ ポータブル クラス ライブラリ (PCL) ターゲットを含めないでください。 たとえば、「 `portable-net45+win8+wpa81+wp8` 」のように入力します。

> .NET Standard は、クロスプラットフォームの .NET ライブラリをサポートする最新の方法であり、PCL に代わるものです。

❌ サポートされなくなった .NET プラットフォームのターゲットを含めないでください。 たとえば、`SL4`、`WP` のようになります。

>[!div class="step-by-step"]
>[前へ](get-started.md)
>[次へ](strong-naming.md)
