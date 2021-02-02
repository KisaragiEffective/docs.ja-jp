---
title: その他のツール
description: .NET の機能をサポートおよび拡張する、インストール可能な追加ツールについての概要。
author: mlacouture
ms.date: 02/13/2020
ms.custom: mvc
ms.openlocfilehash: 243f2da1c6f7809ac710c9700ea4cbde6f289295
ms.sourcegitcommit: 68c9d9d9a97aab3b59d388914004b5474cf1dbd7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2021
ms.locfileid: "99216396"
---
# <a name="net-additional-tools-overview"></a>.NET の追加ツールの概要

このセクションでは、.NET CLI に加えて、.NET の機能をサポートおよび拡張するツールの一覧をまとめて説明します。

## <a name="net-uninstall-tool"></a>.NET アンインストール ツール

[.NET アンインストール ツール](https://github.com/dotnet/cli-lab/releases) (`dotnet-core-uninstall`) を使用すると、システム上の .NET SDK とランタイムをクリーンアップし、指定したバージョンのみを残すことができます。 一連のオプションを使用して、アンインストールするバージョンを指定できます。

## <a name="net-diagnostic-tools"></a>.NET 診断ツール

[dotnet-カウンター](../diagnostics/dotnet-counters.md)は、第 1 レベルの正常性監視とパフォーマンス調査のためのパフォーマンス監視ツールです。

[dotnet-dump](../diagnostics/dotnet-dump.md) は、ネイティブ デバッガーを使用せずに Windows および Linux のコア ダンプを収集して分析する方法です。

[dotnet-gcdump](../diagnostics/dotnet-gcdump.md) は、ライブ .NET プロセスの GC (ガベージコレクター) ダンプを収集する手段を提供します。

[dotnet-trace](../diagnostics/dotnet-trace.md) では、自分のアプリからプロファイル データを収集できます。これは、アプリの実行速度が低下する原因を特定する必要があるシナリオで役立ちます。

## <a name="net-install-tool-for-extension-authors"></a>拡張機能作成者用の .NET インストール ツール

[拡張機能作成者用の .NET インストール ツール](https://github.com/dotnet/vscode-dotnet-runtime)は、VS Code 拡張機能作成者専用の .NET ランタイムを入手できる、Visual Studio Code の拡張機能です。 このツールは、.NET で記述された拡張機能での利用を目的としており、拡張機能の一部 (言語サーバーなど) を起動するために .NET を必要とします。 この拡張機能は、開発用の .NET をインストールするためにユーザーが直接使用することは意図されていません。

## <a name="wcf-web-service-reference-tool"></a>WCF Web Service Reference ツール

WCF (Windows Communication Foundation) [Web Service Reference ツール](wcf-web-service-reference-guide.md)は、Visual Studio に接続されているサービス プロバイダーです。[Visual Studio 2017 バージョン 15.5](/visualstudio/releasenotes/vs2017-relnotes-v15.5#WCFTools) から導入されました。 このツールを使うと、現在のソリューションの Web サービス、ネットワーク上の場所、または WSDL ファイルから、メタデータを取得できます。 それにより、.NET と互換性のあるソース ファイルが生成され、Web サービス操作へのアクセスに使用できるメソッドを含む WCF プロキシ クラスが定義されます。

## <a name="wcf-dotnet-svcutil-tool"></a>WCF dotnet-svcutil ツール

WCF の [dotnet-svcutil ツール](dotnet-svcutil-guide.md)は、ネットワークの場所にある Web サービスまたは WSDL ファイルからメタデータを取得する .NET ツールです。 それにより、.NET と互換性のあるソース ファイルが生成され、Web サービス操作へのアクセスに使用できるメソッドを含む WCF プロキシ クラスが定義されます。

**dotnet-svcutil** ツールは、Visual Studio 2017 バージョン 15.5 で最初に用意された Visual Studio 接続済みサービス プロバイダーである、[**WCF Web Service Reference**](wcf-web-service-reference-guide.md) に代わる手段です。 **dotnet-svcutil** ツールは、.NET ツールとして、Linux、macOS、Windows 上で利用できます。

## <a name="wcf-dotnet-svcutilxmlserializer-tool"></a>WCF dotnet-svcutil.xmlserializer ツール

.NET Framework 上で、svcutil ツールを使って事前にシリアル化アセンブリを生成することができます。 WCF [dotnet-svcutil.xmlserializer ツール](dotnet-svcutil.xmlserializer-guide.md)には、.NET 5 (および .NET Core) 以降のバージョンに対して同様の機能が用意されています。 これは、クライアント アプリケーション内の型で、WCF サービス コントラクトによって使われ <xref:System.Xml.Serialization.XmlSerializer> によってシリアル化できるものに対して、C# のシリアル化コードを事前に生成します。 これにより、それらの型のオブジェクトをシリアル化または逆シリアル化するときに XML シリアル化の起動時のパフォーマンスが向上します。

## <a name="xml-serializer-generator"></a>XML シリアライザー ジェネレーター

.NET Framework の [Xml シリアライザー ジェネレーター (sgen.exe)](../../standard/serialization/xml-serializer-generator-tool-sgen-exe.md) と同様に、[Microsoft.XmlSerializer.Generator NuGet パッケージ](https://www.nuget.org/packages/Microsoft.XmlSerializer.Generator)は .NET 5 (および .NET Core) 以降のバージョンを対象とするライブラリ用のソリューションです。 アセンブリに含まれる型の XML シリアル化アセンブリを作成することで、<xref:System.Xml.Serialization.XmlSerializer> を使用してその型のオブジェクトをシリアル化または逆シリアル化するときの XML シリアル化の起動パフォーマンスを改善します。

## <a name="generating-self-signed-certificates"></a>自己署名証明書の生成

[dotnet dev-cert](self-signed-certificates-guide.md) を使用して、開発およびテストのシナリオ用の自己署名証明書を作成することができます。
