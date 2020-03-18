---
title: 言語機能とライブラリ型間のリレーションシップ | Microsoft Docs
description: 言語機能は多くの場合、実装のためにライブラリ型に依存します。 そのリレーションシップを理解します。
ms.date: 07/20/2017
ms.openlocfilehash: dfae7972af0a251a92700d7d33bd6f971eb1870e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61706027"
---
# <a name="relationships-between-language-features-and-library-types"></a>言語機能とライブラリ型間のリレーションシップ

C# 言語の定義は、標準ライブラリが特定の型とその型にアクセス可能な特定のメンバーを持つことを必要とします。 コンパイラは、多くの異なる言語機能にこれらの必要な型とメンバーを使用するコードを生成します。 これらの型やメンバーがまだ展開されていない環境のためのコードを記述する場合、必要に応じてその言語の新しいバージョンに必要な型が含まれている NuGet パッケージが利用できます。

この標準ライブラリ機能に対する依存関係は、C# 言語の最初のバージョンからその一部となっています。 そのバージョンでは、次の例が含まれています。

* <xref:System.Exception>: すべてのコンパイラーによって生成された例外に使用されます。
* <xref:System.String>: C# `string` 型は <xref:System.String> のシノニムです。
* <xref:System.Int32>: `int` のシノニムです。

その最初のバージョンは単純でした。コンパイラと標準ライブラリは共に出荷され、それぞれ 1 つのバージョンしかありませんでした。

C# の以降のバージョンでは、随時、依存関係に新しい型またはメンバーが追加されています。 例として、<xref:System.Runtime.CompilerServices.INotifyCompletion>、<xref:System.Runtime.CompilerServices.CallerFilePathAttribute>、<xref:System.Runtime.CompilerServices.CallerMemberNameAttribute> があります。 C# 7.0 では、 <xref:System.ValueTuple>タプル[言語機能を実装するために](../tuples.md)への依存関係が追加されます。

言語設計チームは、準拠している標準ライブラリで必要な型およびメンバーのアクセス領域を最小限に抑えることに取り組んでいます。 その目標は、新しいライブラリ機能が言語にシームレスに組み込まれているクリーン設計と両立させることです。 将来、標準ライブラリ内の新しい型とメンバーを必要とする C# の新バージョンが登場するでしょう。 自分の作業でこれらの依存関係を管理する方法を理解することが重要です。

## <a name="managing-your-dependencies"></a>依存関係の管理

C# コンパイラ ツールは現在、サポートされているプラットフォームの .NET ライブラリのリリース サイクルと切り離されています。 実際、.NET ライブラリごとに異なるリリース サイクルがあります。Windows の .NET Framework は、Windows Update としてリリースされ、.NET Core は個別のスケジュールで出荷されます。また、Xamarin バージョンのライブラリの更新は、各ターゲット プラットフォーム用の Xamarin ツールとともに出荷されます。

ほとんどの場合、これらの変更に気付くことはありません。 ただし、そのプラットフォーム上の .NET ライブラリ内にまだない機能を必要とする言語の新しいバージョンを使用している場合は、それらの新しい型を提供する NuGet パッケージを参照することになります。
お使いのアプリがサポートしているプラットフォームが新しいフレームワークのインストールで更新されるときに、この余分な参照を削除できます。

この分離は、対応するフレームワークがない可能性があるマシンを対象とする場合でも、新しい言語機能を使用できることを意味します。
