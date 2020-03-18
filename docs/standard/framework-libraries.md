---
title: フレームワーク ライブラリ
description: 多くの一般的な型、アプリ固有の型、アルゴリズム、ユーティリティの機能の実装を提供しているライブラリについて説明します。
author: richlander
ms.date: 06/20/2016
ms.technology: dotnet-standard
ms.assetid: 7b77b6c1-8367-4602-bff3-91e4c05ac643
ms.openlocfilehash: d4444b6d080afa92a4e7fd9f30c5f9358f02f0ef
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159235"
---
# <a name="framework-libraries"></a>フレームワーク ライブラリ

.NET には、基底クラス ライブラリ (コア セット) またはフレームワーク クラス ライブラリ (完全なセット) と呼ばれるクラス ライブラリの包括的な標準セットがあります。 これらのライブラリは、多くの汎用およびアプリ固有の型、アルゴリズムおよびユーティリティの機能の実装を提供します。 商用ライブラリとコミュニティ ライブラリはどちらもフレームワーク クラス ライブラリの上に構築され、コンピューティング タスクの広範なセット用に使いやすい市販ライブラリを提供します。

これらのライブラリのサブセットには、各 .NET 実装が提供されます。 基底クラス ライブラリ (BCL) API には、任意の .NET 実装が想定されています。これは、開発者がそれらを必要としていることと、よく使われるライブラリでそれらを実行する必要があるという 2 つの理由からです。 ASP.NET などの BCL 上のアプリ固有のライブラリは、すべての .NET 実装で使用できません。

## <a name="base-class-libraries"></a>基底クラス ライブラリ

BCL は最も基本的な型およびユーティリティの機能を提供し、他のすべての .NET クラス ライブラリの基本となります。 これらは、任意のワークロードに偏ることなく、非常に汎用的な実装を提供することを目的としています。 アプリは、高スループットよりも低待機時間、低い CPU 使用率よりも低いメモリ使用率のように、特定のポリシーを選ぶ場合があるため、パフォーマンスは常に重要な考慮事項です。 これらのライブラリは、一般に、高パフォーマンスになることを意図しており、これらさまざまなパフォーマンス上の問題に応じて、妥協案を採用します。 ほとんどのアプリでは、この方法は成功しています。

## <a name="primitive-types"></a>プリミティブ型

.NET には、すべてのプログラムで (さまざまな程度で) 使用されるプリミティブ型のセットが含まれています。 これらの型には、数値、文字列、バイト、および任意のオブジェクトなどのデータが含まれています。 C# 言語には、これらの型のキーワードが含まれています。 これらの型のサンプル セットを、一致する C# キーワードとともに以下に示します。

* <xref:System.Object?displayProperty=nameWithType> ([object](../csharp/language-reference/builtin-types/reference-types.md#the-object-type)): CLR 型システムの最も基本の基底クラス。 型階層のルートです。
* <xref:System.Int16?displayProperty=nameWithType> ([short](../csharp/language-reference/builtin-types/integral-numeric-types.md)): 16 ビットの符号付き整数型。 符号なしの <xref:System.UInt16> も存在します。
* <xref:System.Int32?displayProperty=nameWithType> ([int](../csharp/language-reference/builtin-types/integral-numeric-types.md)): 32 ビットの符号付き整数型。 符号なしの [UInt32](../csharp/language-reference/builtin-types/integral-numeric-types.md) も存在します。
* <xref:System.Single?displayProperty=nameWithType> ([float](../csharp/language-reference/builtin-types/floating-point-numeric-types.md)): 32 ビット浮動小数点型。
* <xref:System.Decimal?displayProperty=nameWithType> ([decimal](../csharp/language-reference/builtin-types/floating-point-numeric-types.md)): 128 ビットの 10 進数型。
* <xref:System.Byte?displayProperty=nameWithType> ([byte](../csharp/language-reference/builtin-types/integral-numeric-types.md)): メモリのバイトを表す符号なし 8 ビット整数。
* <xref:System.Boolean?displayProperty=nameWithType> ([bool](../csharp/language-reference/builtin-types/bool.md)): `true` または `false` を表すブール型。
* <xref:System.Char?displayProperty=nameWithType> ([char](../csharp/language-reference/builtin-types/char.md)): Unicode 文字を表す 16 ビットの数値型。
* <xref:System.String?displayProperty=nameWithType> ([string](../csharp/language-reference/builtin-types/reference-types.md#the-string-type)): 一連の文字を表します。 `char[]` とは異なりますが、`char` で各 `string` にインデックスを付けることができます。

## <a name="data-structures"></a>データ構造

.NET には、ほとんどの .NET アプリの主力となるデータ構造体のセットが含まれています。 これらはほとんどがコレクションですが、その他の型も含まれています。

* <xref:System.Array>: インデックスを使用してアクセスできる、厳密に型指定されたオブジェクトの配列を表します。 その構造ごとの固定サイズがあります。
* <xref:System.Collections.Generic.List%601>: インデックスを使用してアクセスできる、厳密に型指定されたオブジェクトのリストを表します。 必要に応じてサイズを自動調整します。
* <xref:System.Collections.Generic.Dictionary%602>: キーによってインデックスが作成される値のコレクションを表します。 値は、キーを使用してアクセスできます。 必要に応じてサイズを自動調整します。
* <xref:System.Uri>: URI (Uniform Resource Identifier) のオブジェクト表現を可能にし、URI の一部へ簡単にアクセスできるようにします。
* <xref:System.DateTime>: 通常、日付や時刻として表現される瞬間を表します。

## <a name="utility-apis"></a>ユーティリティ API

.NET には多くの重要なタスクの機能を提供するユーティリティ API のセットが含まれています。

* <xref:System.Net.Http.HttpClient>: URI で識別されるリソースに HTTP 要求を送信し、そのリソースから HTTP 応答を受信するための基底クラスを提供する API です。
* <xref:System.Xml.Linq.XDocument>: LINQ を使用して XML ドキュメントのロードと照会をするための API です。
* <xref:System.IO.StreamReader>: ファイルを読み取るための API です。
* <xref:System.IO.StreamWriter>: ファイルを書き込むための API です。

## <a name="app-model-apis"></a>アプリ モデル API

.NET で使用できる多くのアプリ モデルが、複数の企業から提供されています。

* [ASP.NET](https://www.asp.net): Web サイトとサービスを構築するための Web フレームワークを提供します。 Windows、Linux、macOS でサポートされます (ASP.NET のバージョンによって異なります)。
