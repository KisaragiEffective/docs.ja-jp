---
title: System.string との比較
ms.date: 12/13/2019
description: Sqlite ライブラリと system.string ライブラリの違いの一部について説明します。
ms.openlocfilehash: 076bbc6f746cf9296c96ec73047397a21a3b2558
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75900711"
---
# <a name="comparison-to-systemdatasqlite"></a>System.string との比較

2005では、Robert Simpson は ADO.NET 2.0 用の SQLite プロバイダーであるを作成しました。 2010では、SQLite チームはプロジェクトのメンテナンスと開発を引き継ぎました。 Mono チームは2007のコードを Mono としてフォークしたことにも注意してください。 SQLite には長い歴史があり、Visual Studio ツールを使用して完全な機能を備えた安定した ADO.NET プロバイダーに進化しました。 新しいリリースでは、.NET Framework のすべてのバージョンと互換性のあるアセンブリを、バージョン2.0 および 3.5 .NET Compact Framework に戻すことができます。

.NET Core の最初のバージョン (2016 でリリース) は、.NET の単一、軽量、最新、およびクロスプラットフォームの実装でした。 古くなった Api と Api は、意図的に削除されています。 ADO.NET には、データセット Api (DataTable および DataAdapter を含む) は含まれていませんでした。

Entity Framework チームは、システムのデータコードベースについてよく理解していました。 EF チームのメンバーである brice Lambson は、以前は SQLite チームが Entity Framework バージョン5および6のサポートを追加しました。 また、.NET Core が計画されていたときと同じように、他のユーザーが SQLite ADO.NET プロバイダーの実装を試しています。 詳しい説明を終えた後、Entity Framework チームは Brice のプロトタイプに基づいて、Microsoft のデータを作成することにしました。 これにより、.NET Core の目標に合わせて、新しい軽量で最新の実装を作成できます。

ここでは、最新の例として、[ユーザー定義関数](user-defined-functions.md)を作成するためのコードを、次のコードに示します。

```csharp
// System.Data.SQLite
connection.BindFunction(
    new SQLiteFunctionAttribute("ceiling", 1, FunctionType.Scalar),
    (Func<object[], object>)((object[] args) => Math.Ceiling((double)((object[])args[1])[0])),
    null);

// Microsoft.Data.Sqlite
connection.CreateFunction(
    "ceiling",
    (double arg) => Math.Ceiling(arg));
```

2017では、.NET Core 2.0 の戦略が変更されました。 .NET Core を成功させるには、.NET Framework との互換性が不可欠であると判断されました。 データセット Api など、削除された Api の多くが再び追加されました。 他の多くの場合と同様に、このブロックが解除されたことにより、そのデータを .NET Core に移植することもできます。 ただし、現在のところ、軽量で最新の Microsoft. Data. Sqlite の目標は残っています。 ADO.NET によって実装されていない ADO.NET Api の詳細については、「[制限事項](adonet-limitations.md)」を参照してください。

新しい機能が追加されたときに、データ sqlite の設計が考慮されます。 2つの間の変更を最小限に抑えるために、可能な場合は、これらの間の変更を最小限に抑えることをお試しください。

## <a name="data-types"></a>データの種類

Microsoft のデータ sqlite と system.string の最大の違いは、データ型の処理方法です。 「[データ型](types.md)」で説明されているように、quirkiness は、任意の文字列を列の型として指定できるようにする sqlite の基になるを非表示にしません。また、整数、実数、テキスト、BLOB の4つのプリミティブ型のみを持ちます。

データ SQLite は、列の型を .NET 型に直接マップするために、追加のセマンティクスを適用します。 これにより、プロバイダーはより厳密に型指定された感覚を持つことになりますが、いくつかの大まかな境界があります。 たとえば、SELECT ステートメントで式の列の型を指定するために、新しい SQL ステートメント (型) を導入する必要がありました。

## <a name="connection-strings"></a>接続文字列

Microsoft Data Sqlite には、[接続文字列](connection-strings.md)キーワードがより多く含まれています。 次の表に、代わりに使用できる代替方法を示します。

| キーワード          | 代替                                         |
| ---------------- | --------------------------------------------------- |
| キャッシュのサイズ       | `PRAGMA cache_size = <pages>` の送信                  |
| 既定のタイムアウト  | SqliteConnection で DefaultTimeout プロパティを使用する |
| FailIfMissing    | `Mode=ReadWrite` を使用してください。                                |
| FullUri          | Data Source キーワードを使用する                         |
| ジャーナルモード     | `PRAGMA journal_mode = <mode>` の送信                 |
| レガシ形式    | `PRAGMA legacy_file_format = 1` の送信                |
| 最大ページ数   | `PRAGMA max_page_count = <pages>` の送信              |
| ページ サイズ        | `PRAGMA page_size = <bytes>` の送信                   |
| 読み取り専用        | `Mode=ReadOnly` を使用してください。                                 |
| Synchronous      | `PRAGMA synchronous = <mode>` の送信                  |
| URI              | Data Source キーワードを使用する                         |
| UseUTF16Encoding | `PRAGMA encoding = 'UTF-16'` の送信                   |

## <a name="authorization"></a>認証

Sqlite の認証コールバックを公開する API がありません。 この機能に関するフィードバックを提供するには、問題[#13835](https://github.com/dotnet/efcore/issues/13835)を使用します。

## <a name="data-change-notifications"></a>データ変更通知

Sqlite のデータ変更通知を公開する API がありません。 この機能に関するフィードバックを提供するには、問題[#13827](https://github.com/dotnet/efcore/issues/13827)を使用します。

## <a name="virtual-table-modules"></a>仮想テーブルモジュール

Microsoft Data Sqlite には、仮想テーブルモジュールを作成するための API がありません。 この機能に関するフィードバックを提供するには、問題[#13823](https://github.com/dotnet/efcore/issues/13823)を使用します。

## <a name="see-also"></a>関連項目

* [データ型](types.md)
* [接続文字列](connection-strings.md)
* [暗号化](encryption.md)
* [ADO.NET の制限事項](adonet-limitations.md)
* [Dapper の制限事項](dapper-limitations.md)
