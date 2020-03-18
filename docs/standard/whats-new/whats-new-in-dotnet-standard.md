---
title: .NET Standard の新機能
description: この記事では、.NET Standard の新しいバージョンごとに、組み込まれた新機能と機能強化をまとめます。
ms.custom: updateeachrelease
ms.date: 04/12/2018
ms.technology: dotnet-standard
ms.openlocfilehash: a90df0360211c3b02f4f2d8697890180099c5807
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76921057"
---
# <a name="whats-new-in-the-net-standard"></a>.NET Standard の新機能

.NET Standard は、バージョン管理されており、各バージョンの標準に準拠した .NET 実装で利用できる必要がある一連の API が定義された正式な仕様です。 .NET Standard はライブラリ開発者を対象としています。 あるバージョンの .NET Standard をターゲットとするライブラリは、そのバージョンの標準をサポートする .NET Framework、.NET Core、または Xamarin 実装で使用できます。

.NET Standard の最新バージョンは 2.0 です。 .NET Core 2.0 SDK に加えて、.NET Core ワークロードがインストールされた Visual Studio 2017 Version 15.3 にも含まれています。

## <a name="supported-net-implementations"></a>サポートされている .NET 実装

.NET Standard 2.0 は、次の .NET 実装でサポートされています。

- .NET Core 2.0 以降
- .NET Framework 4.6.1 以降
- Mono 5.4 以降
- Xamarin.iOS 10.14 以降
- Xamarin.Mac 3.8 以降
- Xamarin.Android 8.0 以降
- ユニバーサル Windows プラットフォーム 10.0.16299 以降

## <a name="whats-new-in-the-net-standard-20"></a>.NET Standard 2.0 の新機能

.NET Standard 2.0 には、次の新機能が含まれています。

### <a name="a-vastly-expanded-set-of-apis"></a>大幅に拡張された一連の API

.NET Standard バージョン 1.6 には、比較的少数の API が含まれました。 除外された API の中には、.NET Framework または Xamarin で一般的に使用されていた多くの API がありました。 その結果、複数の .NET 実装をターゲットとするアプリケーションやライブラリを開発する場合に、使い慣れた API に適した代替のものを開発者が見つけなければならないので、開発は複雑になります。 .NET Standard 2.0 では、以前のバージョンの標準である .NET Standard 1.6 で使用できる API に 20,000 個を超える API を追加して、この制限に対処しています。 .NET Standard 2.0 に追加された API の一覧については、「[.NET Standard 2.0 vs 1.6](https://raw.githubusercontent.com/dotnet/standard/master/docs/versions/netstandard2.0_diff.md)」(.NET Standard 2.0 と 1.6) を参照してください。

.NET Standard 2.0 の <xref:System> 名前空間には、以下のような追加がありました。

- <xref:System.AppDomain> クラスのサポート。
- <xref:System.Array> クラスに追加されたメンバーの配列操作のサポートを改善しました。
- <xref:System.Attribute> クラスに追加されたメンバーの属性操作のサポートを改善しました。
- カレンダーのサポートを改善し、<xref:System.DateTime> 値の書式設定オプションを追加しました。
- <xref:System.Decimal> の丸め処理機能を追加しました。
- <xref:System.Environment> クラスの機能を追加しました。
- <xref:System.GC> クラスによるガベージ コレクターの制御を強化しました。
- <xref:System.String> クラスの文字列の比較、列挙、正規化のサポートを強化しました。
- <xref:System.TimeZoneInfo.AdjustmentRule> クラスと <xref:System.TimeZoneInfo.TransitionTime> クラスでの夏時間の調整および移行時間のサポート。
- <xref:System.Type> クラスの機能が大幅に強化されました。
- <xref:System.Runtime.Serialization.SerializationInfo> および <xref:System.Runtime.Serialization.StreamingContext> パラメーターを使用する例外コンストラクターを追加することで、例外オブジェクトの逆シリアル化のサポートを改善しました。

### <a name="support-for-net-framework-libraries"></a>.NET Framework ライブラリのサポート

大部分のライブラリは、.NET Standard ではなく .NET Framework をターゲットとしています。 ただし、このようなライブラリの呼び出しのほとんどは、.NET Standard 2.0 に含まれている API に対する呼び出しです。 .NET Standard 2.0 以降、[互換性 shim](https://github.com/dotnet/standard/blob/master/docs/planning/netstandard-2.0/README.md#assembly-unification) を使用して .NET Standard ライブラリから .NET Framework ライブラリにアクセスできるようになりました。 この互換レイヤーは開発者に対して透過的です。 .NET Framework ライブラリを利用するために必要なことはありません。

唯一の要件は、.NET Framework クラス ライブラリから呼び出される API が .NET Standard 2.0 に含まれている必要があることです。

### <a name="support-for-visual-basic"></a>Visual Basic のサポート

Visual Basic で .NET Standard ライブラリを開発できるようになりました。 .NET Core ワークロードがインストールされた Visual Studio 2017 Version 15.3 以降を使用している Visual Basic 開発者向けに、Visual Studio には .NET Standard Class Library テンプレートが含まれるようになりました。 他の開発ツールと環境を使用している Visual Basic 開発者の場合、[dotnet new](../../core/tools/dotnet-new.md) コマンドを使用して .NET Standard ライブラリ プロジェクトを作成できます。 詳細については、「[.NET Standard ライブラリのツールのサポート](#tooling-support-for-net-standard-libraries)」を参照してください。

### <a name="tooling-support-for-net-standard-libraries"></a>.NET Standard ライブラリのツールのサポート

.NET Core 2.0 と .NET Standard 2.0 がリリースされ、Visual Studio 2017 と [.NET Core CLI](../../core/tools/index.md) の両方に .NET Standard ライブラリの作成をサポートするツールが追加されました。

**.NET Core クロスプラットフォーム開発**ワークロードを使用して Visual Studio をインストールする場合は、次の図に示すように、プロジェクト テンプレートを使用して .NET Standard 2.0 ライブラリ プロジェクトを作成できます。

<!-- markdownlint-disable MD025 -->

# <a name="c"></a>[C#](#tab/csharp)

![新しい .NET Standard ライブラリ プロジェクトを追加する](./media/std-project-cs.png)

.NET Core CLI を使用している場合、次の [dotnet new](../../core/tools/dotnet-new.md) コマンドを実行すると、.NET Standard 2.0 をターゲットとするクラス ライブラリ プロジェクトが作成されます。

```dotnetcli
dotnet new classlib
```

# <a name="visual-basic"></a>[Visual Basic](#tab/vb)

![新しい .NET Standard ライブラリ プロジェクトを追加する](./media/std-project-vb.png)

.NET Core CLI を使用している場合、次の [dotnet new](../../core/tools/dotnet-new.md) コマンドを実行すると、.NET Standard 2.0 をターゲットとするクラス ライブラリ プロジェクトが作成されます。

```dotnetcli
dotnet new classlib -lang vb
```

---

## <a name="see-also"></a>参照

- [.NET Standard](../net-standard.md)
- [.NET Standard の概要](https://devblogs.microsoft.com/dotnet/introducing-net-standard/)
