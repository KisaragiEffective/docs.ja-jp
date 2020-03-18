---
title: その他のツール
description: .NET Core 機能をサポートおよび拡張する、インストール可能な追加ツールについての概要。
author: mlacouture
ms.date: 02/13/2020
ms.custom: mvc
ms.openlocfilehash: c0224a1cc6cbb9ae6fa88e5f869c47a1e84289e0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77451526"
---
# <a name="net-core-additional-tools-overview"></a>.NET Core の追加ツールの概要

このセクションでは、.NET Core CLI に加えて、.NET Core 機能をサポートおよび拡張するツールの一覧をまとめて説明します。

## <a name="net-core-uninstall-tool"></a>.NET Core アンインストール ツール

[.NET Core アンインストール ツール](https://github.com/dotnet/cli-lab/releases) (`dotnet-core-uninstall`) を使うと、システム上の .NET Core SDK とランタイムをクリーンアップし、指定したバージョンのみを残すことができます。 一連のオプションを使用して、アンインストールするバージョンを指定できます。

## <a name="net-core-diagnostic-tools"></a>.NET Core 診断ツール

[dotnet-カウンター](../diagnostics/dotnet-counters.md)は、第 1 レベルの正常性監視とパフォーマンス調査のためのパフォーマンス監視ツールです。

[dotnet-dump](../diagnostics/dotnet-dump.md) は、ネイティブ デバッガーを使用せずに Windows および Linux のコア ダンプを収集して分析する方法です。

[dotnet-trace](../diagnostics/dotnet-trace.md) では、自分のアプリからプロファイル データを収集できます。これは、アプリの実行速度が低下する原因を特定する必要があるシナリオで役立ちます。

## <a name="wcf-web-service-reference-tool"></a>WCF Web Service Reference ツール

WCF (Windows Communication Foundation) [Web Service Reference ツール](wcf-web-service-reference-guide.md)は、Visual Studio に接続されているサービス プロバイダーです。[Visual Studio 2017 バージョン 15.5](/visualstudio/releasenotes/vs2017-relnotes-v15.5#WCFTools) から導入されました。 このツールを使うと、現在のソリューションの Web サービス、ネットワーク上の場所、または WSDL ファイルから、メタデータを取得できます。 それにより、.NET Core と互換性のあるソース ファイルが生成され、Web サービス操作へのアクセスに使用できるメソッドを含む WCF プロキシ クラスが定義されます。

## <a name="wcf-dotnet-svcutil-tool"></a>WCF dotnet-svcutil ツール

WCF の [dotnet-svcutil ツール](dotnet-svcutil-guide.md)は、ネットワークの場所にある Web サービスまたは WSDL ファイルからメタデータを取得する .NET ツールです。 それにより、.NET Core と互換性のあるソース ファイルが生成され、Web サービス操作へのアクセスに使用できるメソッドを含む WCF プロキシ クラスが定義されます。

**dotnet-svcutil** ツールは、Visual Studio 2017 バージョン 15.5 で最初に用意された Visual Studio 接続済みサービス プロバイダーである、[**WCF Web Service Reference**](wcf-web-service-reference-guide.md) に代わる手段です。 **dotnet-svcutil** ツールは、.NET ツールとして、Linux、macOS、Windows 上で利用できます。

## <a name="wcf-dotnet-svcutilxmlserializer-tool"></a>WCF dotnet-svcutil.xmlserializer ツール

.NET Framework 上で、svcutil ツールを使って事前にシリアル化アセンブリを生成することができます。 WCF [dotnet-svcutil.xmlserializer ツール](dotnet-svcutil.xmlserializer-guide.md)には、.NET Core に対して同様の機能が用意されています。 これは、クライアント アプリケーション内の型で、WCF サービス コントラクトによって使われ <xref:System.Xml.Serialization.XmlSerializer> によってシリアル化できるものに対して、C# のシリアル化コードを事前に生成します。 これにより、それらの型のオブジェクトをシリアル化または逆シリアル化するときに XML シリアル化の起動時のパフォーマンスが向上します。

## <a name="xml-serializer-generator"></a>XML シリアライザー ジェネレーター

.NET Framework の [Xml シリアライザー ジェネレーター (sgen.exe)](../../standard/serialization/xml-serializer-generator-tool-sgen-exe.md) と同様に、[Microsoft.XmlSerializer.Generator NuGet パッケージ](https://www.nuget.org/packages/Microsoft.XmlSerializer.Generator)は .NET Core および .NET Standard ライブラリのソリューションです。 アセンブリに含まれる型の XML シリアル化アセンブリを作成することで、<xref:System.Xml.Serialization.XmlSerializer> を使用してその型のオブジェクトをシリアル化または逆シリアル化するときの XML シリアル化の起動パフォーマンスを改善します。
