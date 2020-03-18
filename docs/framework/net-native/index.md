---
title: .NET ネイティブによるアプリのコンパイル
ms.date: 03/30/2017
helpviewer_keywords:
- native compilation
- .NET and native code
- compilation with .NET Native
- .NET Native
- C# and native compilation
ms.assetid: 47cd5648-9469-4b1d-804c-43cc04384045
ms.openlocfilehash: 1f176e81905fe68c6d740a13240fe814659a7a59
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128376"
---
# <a name="compiling-apps-with-net-native"></a>.NET ネイティブによるアプリのコンパイル

.NET ネイティブは、Visual Studio 2015 以降のバージョンに付属している Windows アプリをビルドおよび配置するためのプリコンパイルテクノロジです。 マネージド コード (C# または Visual Basic) で記述された、.NET Framework および Windows 10 を対象とするアプリのリリース バージョンを自動的にネイティブ コードにコンパイルします。

通常、.NET Framework を対象とするアプリは中間言語 (IL) にコンパイルされます。 実行時に、Just-In-Time (JIT) コンパイラによって IL がネイティブ コードに変換されます。 これに対し、.NET ネイティブは、Windows アプリを直接ネイティブコードにコンパイルします。 開発者にとって、これは次のことを意味します。

- アプリは、ネイティブコードのパフォーマンスを特徴としています。 通常、パフォーマンスは、最初に IL にコンパイルされてから JIT コンパイラによってネイティブコードにコンパイルされるコードよりも優れています。

- 引き続き C# または Visual Basic でプログラムを作成できます。

- クラス ライブラリ、自動メモリ管理とガベージ コレクション、例外処理など、.NET Framework で提供されるリソースを引き続き利用できます。

アプリのユーザーのために、.NET ネイティブには次のような利点があります。

- 大部分のアプリとシナリオの実行時間が短縮されます。

- ほとんどのアプリとシナリオで、起動時間が短縮されます。

- デプロイコストと更新コストが低くなります。

- アプリのメモリ使用量が最適化されています。

> [!IMPORTANT]
> アプリとシナリオの大部分では、IL または NGEN イメージにコンパイルされたアプリと比較した場合、起動時間が大幅に短縮され、パフォーマンスが向上します。 .NET ネイティブ。 ただし、結果は異なる場合があります。 .NET ネイティブのパフォーマンスの向上によってアプリのパフォーマンスを向上させるには、アプリの non-.NET ネイティブバージョンとのパフォーマンスを比較する必要があります。 詳細については、「[パフォーマンスセッションの概要](https://docs.microsoft.com/visualstudio/profiling/performance-session-overview)」を参照してください。

ただし .NET ネイティブには、ネイティブコードへのコンパイル以外にも関係があります。 .NET Framework アプリのビルド方法と実行方法が変更されます。 特に次の点に注意してください。

- プリコンパイル時に、.NET Framework の必要な部分がアプリに静的にリンクされます。 これにより、アプリは .NET Framework のアプリ ローカルのライブラリを使用して実行でき、コンパイラはグローバル分析を実行してパフォーマンスを向上させることができます。 その結果、.NET Framework の更新後であっても、アプリは常に高速に起動します。

- .NET ネイティブランタイムは静的プリコンパイル用に最適化されており、ほとんどのケースでは優れたパフォーマンスを提供します。 同時に、開発者に役立つ主要なリフレクション機能もあります。

- .NET ネイティブは、静的プリコンパイルシナリオ用C++に最適化された、コンパイラと同じバックエンドを使用します。

.NET ネイティブは、次の表に示すようC++に、内部のものとC++同じまたは類似のツールを使用するため、のパフォーマンス上の利点をマネージコードの開発者にもたらします。

||.NET Native|C++|
|-|----------------------------------------------------------------|-----------|
|ライブラリ|.NET Framework + Windows ランタイム|Win32 + Windows ランタイム|
|コンパイラ|UTC 最適化コンパイラ|UTC 最適化コンパイラ|
|配置|実行可能バイナリ|実行可能バイナリ (ASM)|
|ランタイム|MRT.dll (最小 CLR ランタイム)|CRT.dll (C ランタイム)|

Windows 10 用の Windows アプリの場合は、.NET ネイティブ コード コンパイル バイナリをアプリ パッケージ (.appx ファイル) に入れて Windows ストアにアップロードします。

## <a name="in-this-section"></a>このセクションの内容

.NET ネイティブ コード コンパイルを使用したアプリ開発の詳細については、次のトピックを参照してください。

- [.NET ネイティブ コード コンパイルの概要: 開発者向けチュートリアル](getting-started-with-net-native.md)

- [.NET ネイティブとコンパイル:](net-native-and-compilation.md) .NET ネイティブがプロジェクトをネイティブ コードにコンパイルする方法を取り上げます。

- [リフレクションおよび .NET ネイティブ](reflection-and-net-native.md)

  - [リフレクションに依存する API](apis-that-rely-on-reflection.md)

  - [リフレクション API リファレンス](net-native-reflection-api-reference.md)

  - [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)

- [シリアル化とメタデータ](serialization-and-metadata.md)

- [Windows ストア アプリの .NET ネイティブへの移行](migrating-your-windows-store-app-to-net-native.md)

- [.NET ネイティブの一般的なトラブルシューティング](net-native-general-troubleshooting.md)
