---
title: カルチャを認識しない文字列操作
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- culture, culture-insensitive string operations
- case-sensitive comparisons
- globalization [.NET Framework], culture-insensitive string operations
- strings [.NET Framework], culture-insensitive string operations
- localization [.NET Framework], culture-insensitive string operations
- world-ready applications, culture-insensitive string operations
- culture-sensitive string operations
- culture-insensitive string operations
ms.assetid: e6e2bb94-a95d-44e2-b68c-cfdd1db77784
ms.openlocfilehash: 06c46033936e16355b8d2eb6650e8731a04af6e9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73141281"
---
# <a name="culture-insensitive-string-operations"></a>カルチャを認識しない文字列操作

カルチャを認識する文字列操作は、カルチャごとにユーザーに結果を表示するようデザインされたアプリケーションを作成する場合に役立ちます。 既定では、カルチャを認識するメソッドは、使用するカルチャを現在のスレッドの <xref:System.Globalization.CultureInfo.CurrentCulture%2A> プロパティから取得します。

ただし、カルチャを認識する文字列操作は、必ずしも望ましい動作ではありません。 結果をカルチャに依存させない場合に、カルチャを認識する操作を使用すると、カスタムの大文字と小文字の対応規則および並べ替え規則を使用するカルチャでのアプリケーション コードの実行が失敗することがあります。 たとえば、「[.NET の文字列を使用するためのベスト プラクティス](../../../docs/standard/base-types/best-practices-strings.md#string-comparisons-that-use-the-current-culture)」の記事の「[現在のカルチャを使用する文字列比較](../../../docs/standard/base-types/best-practices-strings.md)」を参照してください。

文字列操作でカルチャに依存するかどうかは、アプリケーションで結果をどのように使用するかに基づいて決定する必要があります。 ユーザーに結果を表示する場合は、通常、カルチャを認識する文字列操作を実行する必要があります。 たとえば、アプリケーションがローカライズされた文字列を並べ替えてリスト ボックスに表示する場合は、カルチャを認識する並べ替えを実行します。

内部的に使用される文字列に対する操作は、通常、カルチャを認識しないようにします。 一般的に、アプリケーションがファイル名、永続形式、エンド ユーザーに表示されないシンボル情報などの処理を実行する場合には、文字列操作の結果がカルチャごとに変わらないようにする必要があります。 たとえば、XML タグとして認識されるかどうかを調べるために文字列を比較するアプリケーションでは、比較操作でカルチャを認識しないでください。 また、セキュリティに関する決定が文字列の比較操作や大文字と小文字の変更操作の結果に基づいて行われる場合は、この操作がカルチャを認識しないようにして、操作の結果が <xref:System.Globalization.CultureInfo.CurrentCulture%2A> の値に影響されないようにする必要があります。

開発するアプリケーションにローカリゼーションとグローバリゼーションに対応するためのコードが含まれているかどうかに関係なく、.NET Framework のメソッドは既定ではカルチャを認識する結果を取得することに注意してください。 このトピックの目的は、アプリケーションがこれらのメソッドを使用してカルチャを認識しない結果を取得するための正しい方法を示すことです。

## <a name="see-also"></a>参照

- [グローバライズとローカライズ](../../../docs/standard/globalization-localization/index.md)
