---
title: グローバリゼーション
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- globalization [.NET Framework], about globalization
- global applications, globalization
- international applications [.NET Framework], globalization
- world-ready applications, globalization
- application development [.NET Framework], globalization
- culture, globalization
ms.assetid: 4e919934-6b19-42f2-b770-275a4fae87c9
ms.openlocfilehash: 953d8d3055dff48cd943b748771f20803a4d6573
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73120901"
---
# <a name="globalization"></a>グローバリゼーション

グローバリゼーションとは、さまざまな文化圏 (カルチャ) のユーザー向けに、ローカライズされたインターフェイスと、その地域に合ったデータをサポートするような、国際対応アプリの設計と開発をいいます。 設計フェーズに着手する前に、アプリでサポートするカルチャを決定してください。 アプリは既定値として 1 つのカルチャまたは地域を対象としますが、別のカルチャまたは地域のユーザーに簡単に拡張できるようにアプリを設計および作成できます。

開発者には、自分のカルチャによって形成されるユーザー インターフェイスとデータに関する前提があります。 たとえば、米国の英語圏の開発者は、日付と時刻のデータを形式 `MM/dd/yyyy hh:mm:ss` の文字列としてシリアル化することは、十分理にかなっていると考えます。 ただし、別のカルチャのシステムでその文字列を逆シリアル化すると、<xref:System.FormatException> 例外がスローされるか、正しくないデータが生成される可能性があります。 グローバリゼーションにより、このようなカルチャ固有の前提を識別し、それがアプリの設計またはコードに影響しないようにすることができます。

この記事では、グローバライズされたアプリで文字列、日付と時刻の値、および数値を処理するときに考慮する必要がある重要な問題および使用できるベスト プラクティスについて説明します。

## <a name="strings"></a>文字列

カルチャまたは地域ごとに異なる文字と文字セットを使用し、異なる方法で並べ替える可能性があるため、文字と文字列の処理は、グローバリゼーションで主な焦点となります。 このセクションでは、グローバライズされたアプリで文字列を使用するための推奨事項について説明します。

### <a name="use-unicode-internally"></a>Unicode を内部で使用する

既定では、.NET は Unicode 文字列を使用します。 Unicode 文字列は、ゼロ個以上の <xref:System.Char> オブジェクトで構成され、それぞれが UTF-16 コード単位を表します。 世界中で使用されているすべての文字セットのほとんどすべての文字に対して、Unicode 表現があります。

Windows オペレーティング システムを含む、多くのアプリケーションとオペレーティング システムでは、文字セットを表すためにコード ページを使用できます。 コード ページには、通常、0x00 から 0x7F までの標準 ASCII 値が含まれ、0x80 から 0xFF までの残りの値に他の文字をマップします。 0x80 から 0xFF までの値の解釈は特定のコード ページによって異なります。 このため、可能な場合は、グローバライズされたアプリでコード ページを使用しないようにします。

次の例では、システムの既定のコード ページとデータが保存されたコード ページとが異なる場合にコード ページのデータを解釈する危険性を示しています (このシナリオをシミュレートするため、例では、異なるコード ページを明示的に指定します)。最初に、ギリシャ文字の大文字で構成される配列を定義します。 コード ページ 737 (MS-DOS ギリシャ語とも呼ばれます) を使用してそれらをバイト配列にエンコードし、バイト配列をファイルに保存します。 ファイルを取得し、そのバイト配列をコード ページ 737 を使用してデコードした場合、元の文字が復元されます。 ただし、ファイルを取得して、そのバイト配列をコード ページ 1252 (または Windows-1252。ラテン語アルファベットの文字を表します) を使用してデコードした場合、元の文字は失われます。

[!code-csharp[Conceptual.Globalization#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/codepages1.cs#1)]
[!code-vb[Conceptual.Globalization#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/codepages1.vb#1)]

Unicode を使用することで、同じコード単位を必ず同じ文字にマップでき、同じ文字を必ず同じバイト配列にマップできます。

### <a name="use-resource-files"></a>リソース ファイルを使用する

単一のカルチャまたは地域を対象とするアプリを開発する場合でも、ユーザー インターフェイスに表示される文字列などのリソースを格納するためにリソース ファイルを使用する必要があります。 リソースを直接コードに追加しないでください。 リソース ファイルの使用には、次の利点があります。

- すべての文字列が単一の場所に存在します。 ソース コード全体を検索して、特定の言語またはカルチャに合わせて変更する文字列を特定する必要はありません。

- 文字列を複製する必要がありません。 リソース ファイルを使用しない開発者は、多くの場合、複数のソース コード ファイルで同じ文字列を定義します。 この重複により、文字列を変更するときに 1 つ以上のインスタンスを見落とす可能性が高くなります。

- イメージ、バイナリ データなどの文字列以外のリソースを個別のスタンドアロン ファイルに格納するのではなく、リソース ファイルに格納できるので、それらが簡単に取得できます。

ローカライズされたアプリを作成する場合、リソース ファイルを使用することには特に利点があります。 サテライト アセンブリにリソースを配置すると <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> プロパティで定義されているユーザーの現在の UI カルチャに基づいて、共通言語ランタイムが自動的にカルチャに応じたリソースを選択します。 適切なカルチャ固有のリソースを提供し、<xref:System.Resources.ResourceManager> オブジェクトを正しくインスタンス化するか、厳密に型指定されたリソース クラスを使用すると、ランタイムは適切なリソースの取得の詳細を処理します。

リソース ファイルの作成の詳細については、[リソース ファイルの作成](../../../docs/framework/resources/creating-resource-files-for-desktop-apps.md)に関する記事を参照してください。 サテライト アセンブリの作成と展開の詳細については、[サテライト アセンブリの作成](../../../docs/framework/resources/creating-satellite-assemblies-for-desktop-apps.md)と[リソースのパッケージ化と展開](../../../docs/framework/resources/packaging-and-deploying-resources-in-desktop-apps.md)に関するページを参照してください。

### <a name="search-and-compare-strings"></a>文字列の検索と比較を行う

文字列は、できるだけ全体を 1 つのまとまりとして扱い、個々の文字の連続として処理しない必要があります。 これは、部分文字列を並べ替えたり、検索したりするときに、組み合わせ文字の解析に関する問題を防ぐうえで特に重要です。

> [!TIP]
> <xref:System.Globalization.StringInfo> クラスを使用して、文字列の個別の文字ではなく、テキスト要素を操作できます。

文字列の検索と比較でよくある間違いは、それぞれが <xref:System.Char> オブジェクトによって表される文字のコレクションとして文字列を処理することです。 実際に、1 つの文字が 1 つ、2 つ、またはそれ以上の <xref:System.Char> オブジェクトによって形成される場合があります。 このような文字は、アルファベットが、Unicode 基本ラテン文字の範囲 (U+0021 ～ U+007E) 外にある文字で構成されるカルチャの文字列に最もよくみられます。 次の例では、文字列で LATIN CAPITAL LETTER A WITH GRAVE 文字 (U+00C0) のインデックスを検索します。 ただし、この文字は、1 つのコード単位 (U+00C0) または複合文字 (2 つのコード単位:U+0021 と U+007E) という 2 つの方法で表現できます。 この場合、この文字は、文字列インスタンスで 2 つの <xref:System.Char> オブジェクト (U+0021 と U+007E) によって表されます。 このコード例では、<xref:System.String.IndexOf%28System.Char%29?displayProperty=nameWithType> オーバーロードと <xref:System.String.IndexOf%28System.String%29?displayProperty=nameWithType> オーバーロードを呼び出して、文字列インスタンスでのこの文字の位置を検索しますが、この 2 つは異なる結果を返します。 最初のメソッド呼び出しでは <xref:System.Char> 引数を指定しているので、序数に基づく比較が実行され、一致を見つけることができません。 2 番目の呼び出しでは <xref:System.String> 引数を指定しているので、カルチャに依存した比較が実行され、一致が見つかります。

[!code-csharp[Conceptual.Globalization#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/search1.cs#18)]
[!code-vb[Conceptual.Globalization#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/search1.vb#18)]

<xref:System.StringComparison> メソッド、<xref:System.String.IndexOf%28System.String%2CSystem.StringComparison%29?displayProperty=nameWithType> メソッドなど <xref:System.String.LastIndexOf%28System.String%2CSystem.StringComparison%29?displayProperty=nameWithType> パラメーターを含むオーバーロードを呼び出して、この例のあいまいさ (異なる結果を返すメソッドの 2 つの類似オーバーロードの呼び出し) の一部を回避できます。

ただし、検索が常にカルチャに依存しているとは限りません。 検索の目的がセキュリティ上の決定をするか、またはリソースへのアクセスを許可または拒否することである場合、次のセクションで説明するように序数に基づく比較を実行する必要があります。

### <a name="test-strings-for-equality"></a>文字列の等価性をテストする

2 つの文字列が並べ替え順序で並ぶ方法を確認するのではなく、2 つの文字列の等価性をテストする場合は、<xref:System.String.Compare%2A?displayProperty=nameWithType>、<xref:System.Globalization.CompareInfo.Compare%2A?displayProperty=nameWithType> などの文字列比較メソッドの代わりに <xref:System.String.Equals%2A?displayProperty=nameWithType> メソッドを使用します。

等価性の比較は、通常、条件付きでリソースにアクセスするために実行します。 たとえば、パスワードを確認したり、ファイルがあることを確認したりするために等価性の比較を実行する場合があります。 このような非言語的な比較は、カルチャに依存するのではなく、常に序数に基づいて実行する必要があります。 一般に、パスワードなどの文字列の場合は <xref:System.String.Equals%28System.String%2CSystem.StringComparison%29?displayProperty=nameWithType> の値を使用して、ファイル名、URI などの文字列の場合は <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> の値を使用して、インスタンス <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> メソッドまたは静的 <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> メソッドを呼び出す必要があります。

等価性の比較では、<xref:System.String.Equals%2A?displayProperty=nameWithType> メソッドを呼び出すのではなく、検索したり、部分文字列を比較したりすることがあります。 場合によっては、部分文字列検索を使用して、その部分文字列が別の文字列と等しいかどうかを確認できます。 この比較の目的が非言語的である場合、検索はカルチャに依存するのではなく、序数に基づいて実行する必要があります。

次の例は、非言語的なデータに対してカルチャに依存した検索を実行することの危険性を示しています。 `AccessesFileSystem` メソッドは、部分文字列 "FILE" で始まる URI のファイル システムのアクセスを禁止するように設計されています。 これを実行するため、カルチャに依存し、大文字と小文字を区別しない比較を実行して、URI の先頭と文字列 "FILE" を比較します。 ファイル システムにアクセスする URI は "FILE:" または "file:" で始まる可能性があるため、"i" (U+0069) は常に "I" (U+0049) と等価の小文字表現であるという暗黙の前提があります。 ただし、トルコ語およびアゼルバイジャン語には、"i" の大文字として "İ" (U+0130) があります。 このような相違があるため、カルチャに依存した比較を使用すると、ファイル システムのアクセスを禁止する必要がある場合でもそのアクセスが許可されます。

[!code-csharp[Conceptual.Globalization#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/equals1.cs#12)]
[!code-vb[Conceptual.Globalization#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/equals1.vb#12)]

次の例に示すように、大文字と小文字を無視する序数に基づく比較を実行して、この問題を回避できます。

[!code-csharp[Conceptual.Globalization#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/equals2.cs#13)]
[!code-vb[Conceptual.Globalization#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/equals2.vb#13)]

### <a name="order-and-sort-strings"></a>文字列の順序と並べ替え

通常、ユーザー インターフェイスの表示順序が指定された文字列は、カルチャに基づいて並べ替える必要があります。 ほとんどの場合、このような文字列比較は、文字列を並べ替える <xref:System.Array.Sort%2A?displayProperty=nameWithType>、<xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType> などのメソッドを呼び出すと、.NET によって暗黙的に処理されます。 既定では、文字列は、現在のカルチャの並べ替え規則を使用して並べ替えられます。 次の例では、文字列の配列を英語 (米国) カルチャとスウェーデン語 (スウェーデン) カルチャの規則を使用して並べ替えたときの違いを示しています。

[!code-csharp[Conceptual.Globalization#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/sort1.cs#14)]
[!code-vb[Conceptual.Globalization#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/sort1.vb#14)]

カルチャに依存した文字列比較は、各カルチャの <xref:System.Globalization.CompareInfo> プロパティによって返される <xref:System.Globalization.CultureInfo.CompareInfo%2A?displayProperty=nameWithType> オブジェクトによって定義されます。 <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッド オーバーロードを使用するカルチャに依存した文字列比較では、<xref:System.Globalization.CompareInfo> オブジェクトも使用します。

.NET は、文字列データのカルチャに依存した並べ替えを実行するためにテーブルを使用します。 並べ替えのウェイトと文字列の正規化に関するデータが入ったこれらのテーブルの内容は、特定のバージョンの .NET によって実装される Unicode 標準のバージョンによって決まります。 次の表は、特定のバージョンの .NET Framework と .NET Core によって実装される Unicode のバージョンです。 サポートされている Unicode バージョンの一覧は、文字の比較と並べ替えに対してのみ適用されます。カテゴリ別での Unicode 文字の分類には適用されません。 詳細については、記事「<xref:System.String>」 の「Strings and The Unicode Standard」 (文字列と Unicode 標準) セクションを参照してください。

|.NET Framework のバージョン|オペレーティング システム|Unicode バージョン|
|----------------------------|----------------------|---------------------|
|.NET Framework 2.0|すべてのオペレーティング システム|Unicode 4.1|
|.NET Framework 3.0|すべてのオペレーティング システム|Unicode 4.1|
|.NET Framework 3.5|すべてのオペレーティング システム|Unicode 4.1|
|.NET Framework 4|すべてのオペレーティング システム|Unicode 5.0|
|Windows 7 の .NET Framework 4.5 以降|Unicode 5.0|
|Windows 8 以降のオペレーティング システムの .NET Framework 4.5 以降|Unicode 6.3.0|
|.NET Core (すべてのバージョン)|基になるオペレーティング システムでサポートされている Unicode 標準のバージョンによって異なります。|

.NET Framework 4.5 以降と .NET Core のすべてのバージョンでは、文字列の比較と並べ替えはオペレーティング システムによって異なります。 Windows 7 で実行される NET Framework 4.5 以降は、Unicode 5.0 を実装する独自のテーブルからデータを取得します。 Windows 8 以降で実行される NET Framework 4.5 以降は、Unicode 6.3 を実装するオペレーティング システムのテーブルからデータを取得します。 .NET Core では、サポートされている Unicode のバージョンは基になるオペレーティング システムによって異なります。 カルチャに依存した並べ替えが実行されたデータをシリアル化する場合は、<xref:System.Globalization.SortVersion> クラスを使用して、.NET およびオペレーティング システムの並べ替え順序と一致するように、シリアル化されたデータをいつ並べ替える必要があるかを判断できます。 例については、<xref:System.Globalization.SortVersion> クラスに関するトピックを参照してください。

広範なカルチャ固有の並べ替えを文字列データに対して実行するアプリの場合、<xref:System.Globalization.SortKey> クラスを使用して文字列を比較できます。 並べ替えキーは、特定の文字列のアルファベット順、大文字と小文字の区別、発音の区別など、カルチャ固有の並べ替えウェイトを反映しています。 並べ替えキーを使用した比較はバイナリであるため、<xref:System.Globalization.CompareInfo> オブジェクトを暗黙的または明示的に使用する比較よりも高速です。 <xref:System.Globalization.CompareInfo.GetSortKey%2A?displayProperty=nameWithType> メソッドに文字列を渡すことによって、特定の文字列のカルチャ固有の並べ替えキーを作成します。

次の例は、前の例と似ています。 ただし、暗黙的に <xref:System.Array.Sort%28System.Array%29?displayProperty=nameWithType> メソッドを呼び出す <xref:System.Globalization.CompareInfo.Compare%2A?displayProperty=nameWithType> メソッドを呼び出すのではなく、インスタンス化して、<xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> メソッドに渡す、並べ替えキーを比較する <xref:System.Array.Sort%60%601%28%60%600%5B%5D%2CSystem.Collections.Generic.IComparer%7B%60%600%7D%29?displayProperty=nameWithType> 実装を定義しています。

[!code-csharp[Conceptual.Globalization#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/sortkey1.cs#15)]
[!code-vb[Conceptual.Globalization#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/sortkey1.vb#15)]

### <a name="avoid-string-concatenation"></a>文字列の連結を回避する

可能な限り、文字列を実行時に連結して使う方法は避けてください。 連結される文字列は、他のローカライズ言語には当てはまらない、アプリの元の言語の語順に依存することが多く、ローカライズ作業が困難になります。

## <a name="handle-dates-and-times"></a>日付と時刻を処理する

日付と時刻の値を処理する方法は、その値をユーザー インターフェイスに表示するのか、保持するのかによって異なります。 このセクションでは、両方の使用を検討します。 また、日付と時刻を操作するときにタイム ゾーンの相違と算術演算を処理する方法についても説明します。

### <a name="display-dates-and-times"></a>日付と時刻を表示する

通常、日付と時刻をユーザー インターフェイスに表示するときには、<xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> プロパティと、<xref:System.Globalization.DateTimeFormatInfo> プロパティによって返される `CultureInfo.CurrentCulture.DateTimeFormat` オブジェクトで定義されているユーザーのカルチャの書式指定規則を使用する必要があります。 次のメソッドのいずれかを使用して日付の書式を設定すると、現在のカルチャの書式指定規則が自動的に使用されます。

- パラメーターなしの <xref:System.DateTime.ToString?displayProperty=nameWithType> メソッド

- 書式指定文字列を含む <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> メソッド

- パラメーターなしの <xref:System.DateTimeOffset.ToString?displayProperty=nameWithType> メソッド

- 書式指定文字列を含む <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType>

- [複合書式指定](../../../docs/standard/base-types/composite-formatting.md)機能 (日付と共に使用する場合)

次の例では、2012 年 10 月 11 日の日の出と日没のデータを 2 回表示します。 最初に、現在のカルチャをクロアチア語 (クロアチア) に設定し、次に英語 (英国) に設定します。 どちらの場合も、日付と時刻はそのカルチャに適した書式で表示されます。

[!code-csharp[Conceptual.Globalization#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates1.cs#2)]
[!code-vb[Conceptual.Globalization#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates1.vb#2)]

### <a name="persist-dates-and-times"></a>日付と時刻を保持する

日付と時刻のデータはカルチャによって異なる可能性がある書式で保持しないでください。 これは、データの破損または実行時例外が発生する一般的なプログラミング エラーです。 次の例では、英語 (米国) カルチャの書式指定規則を使用して文字列として 2 つの日付 (2013 年 1 月 9 日と 2013 年 8 月 18 日) をシリアル化します。 データが英語 (米国) カルチャの規則を使用して取得および解析されると、正常に復元されます。 ただし、英語 (イギリス) カルチャの規則を使用して取得および解析されると、最初の日付は 9 月 1 日と間違って解釈され、グレゴリオ暦には 18 番目の月がないため 2 番目の日付は解析されません。

[!code-csharp[Conceptual.Globalization#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates2.cs#3)]
[!code-vb[Conceptual.Globalization#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates2.vb#3)]

この問題は、次の 3 つのうちいずれかの方法で回避できます。

- 文字列ではなくバイナリ形式で日付と時刻をシリアル化します。

- ユーザーのカルチャに関係なく同じであるカスタム書式指定文字列を使用して日付と時刻の文字列表現を保存および解析します。

- インバリアント カルチャの書式指定規則を使用して文字列を保存します。

次の例では、最後のアプローチを示します。 この例では、静的 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> プロパティによって返されるインバリアント カルチャの書式指定規則を使用します。

[!code-csharp[Conceptual.Globalization#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates3.cs#4)]
[!code-vb[Conceptual.Globalization#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates3.vb#4)]

### <a name="serialization-and-time-zone-awareness"></a>シリアル化とタイム ゾーンへの対応

日付と時刻の値は、一般的な時刻 ("店舗は 2013 年 1 月 2 日の午前 9 時に開店します") から特定の時点 ("生年月日:2013 年 1 月 2 日午前 6 時 32 分 00 秒") まで、多様に解釈できます。 時刻の値が特定の時点を表し、それをシリアル化された値から復元する場合、ユーザーの地理的場所またはタイム ゾーンに関係なく同じ特定の時点を表すことを確認する必要があります。

この問題を説明する例を次に示します。 この例では、1 つのローカル日付と時刻の値を、3 つの[標準書式](../../../docs/standard/base-types/standard-date-and-time-format-strings.md) (一般の日付と長い形式の時刻の "G"、並べ替え可能な日付と時刻の "s"、およびラウンド トリップする日付と時刻の "o") の文字列として、およびバイナリ形式で保存します。

[!code-csharp[Conceptual.Globalization#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates4.cs#10)]
[!code-vb[Conceptual.Globalization#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates4.vb#10)]

シリアル化されたシステムと同じタイム ゾーンのシステムでデータを復元すると、出力に示すように、逆シリアル化された日付と時刻の値は正確に元の値を反映しています。

```console
'3/30/2013 6:00:00 PM' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00.0000000-07:00' --> 3/30/2013 6:00:00 PM Local

3/30/2013 6:00:00 PM Local
```

ただし、別のタイム ゾーンのシステムでデータを復元する場合は、"o" (ラウンド トリップ) 標準書式指定文字列を使用して書式設定された日付と時刻の値だけがタイム ゾーン情報を維持して、同じ時点を表します。 日付と時刻のデータがロマンス標準時ゾーンのシステムで復元されたときの出力を次に示します。

```console
'3/30/2013 6:00:00 PM' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00.0000000-07:00' --> 3/31/2013 3:00:00 AM Local

3/30/2013 6:00:00 PM Local
```

データが逆シリアル化されるシステムのタイム ゾーンに関係なく 1 つの時点を表す日付と時刻の値を正確に反映するには、次のいずれかの方法を使用できます。

- "o" (ラウンド トリップ) 標準書式指定文字列を使用して値を文字列として保存します。 その後、ターゲット システムで逆シリアル化します。

- 値を UTC に変換し、"r" (RFC1123) 標準書式指定文字列を使用して文字列として保存します。 その後、ターゲット システムで逆シリアル化し、現地時刻に変換します。

- 値を UTC に変換し、"u" (世界共通の並べ替え可能な日付と時刻) 標準書式指定文字列を使用して文字列として保存します。 その後、ターゲット システムで逆シリアル化し、現地時刻に変換します。

- 値を UTC に変換し、バイナリ形式で保存します。 その後、ターゲット システムで逆シリアル化し、現地時刻に変換します。

各方法の例を次に示します。

[!code-csharp[Conceptual.Globalization#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates8.cs#11)]
[!code-vb[Conceptual.Globalization#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates8.vb#11)]

データを太平洋標準時ゾーンのシステムでシリアル化し、ロマンス標準時ゾーンのシステムで逆シリアル化すると、次の出力が表示されます。

```console
'2013-03-30T18:00:00.0000000-07:00' --> 3/31/2013 3:00:00 AM Local
'Sun, 31 Mar 2013 01:00:00 GMT' --> 3/31/2013 3:00:00 AM Local
'2013-03-31 01:00:00Z' --> 3/31/2013 3:00:00 AM Local

3/31/2013 3:00:00 AM Local
```

詳細については、「[タイム ゾーン間での時刻の変換](../../../docs/standard/datetime/converting-between-time-zones.md)」を参照してください。

### <a name="perform-date-and-time-arithmetic"></a>日付と時刻の演算を実行する

<xref:System.DateTime> 型と <xref:System.DateTimeOffset> 型は、算術演算をサポートします。 2 つの日付の値の差を計算したり、日付の値に特定の時間間隔を加算または減算したりできます。 ただし、日付と時刻の値に対する算術演算では、タイム ゾーンとタイム ゾーン調整規則が考慮されません。 このため、時点を表す値に対して日付と時刻の算術を実行すると、不正確な結果が返されることがあります。

たとえば、太平洋標準時から太平洋夏時間への切り替えは、3 月の第 2 日曜日、2013 年の場合は 3 月 10 日に行われます。 次の例に示すように、太平洋標準時ゾーンのシステムで 2013 年 3 月 9 日午前 10 時 30 分の 48 時間後の日時を計算すると、 結果は 2013 年 3 月 11 日午前 10 時 30 分となり、その間の時間調整は考慮されません。

[!code-csharp[Conceptual.Globalization#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates5.cs#8)]
[!code-vb[Conceptual.Globalization#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates5.vb#8)]

日付と時刻の値に対する算術演算によって正確な結果を生成するには、次の手順を実行します。

1. ソース タイム ゾーンの時刻を UTC に変換します。

2. 算術演算を実行します。

3. 結果が日付と時刻の値である場合、UTC からソース タイム ゾーンの時刻に変換します。

次の例は前の例と似ていますが、この 3 つの手順を実行することで、2013 年 3 月 9 日午前 10 時 30 分に 48 時間が正しく追加されています。

[!code-csharp[Conceptual.Globalization#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates6.cs#9)]
[!code-vb[Conceptual.Globalization#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates6.vb#9)]

詳細については、「[日付と時刻を使用した算術演算の実行](../../../docs/standard/datetime/performing-arithmetic-operations.md)」を参照してください。

### <a name="use-culture-sensitive-names-for-date-elements"></a>日付要素にカルチャに依存した名前を使用する

アプリによっては、月または曜日の名前を表示することが必要になる場合があります。 これを実行するために、次のようなコードが一般に使用されます。

[!code-csharp[Conceptual.Globalization#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/monthname1.cs#19)]
[!code-vb[Conceptual.Globalization#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/monthname1.vb#19)]

ただし、このコードは曜日の名前を必ず英語で返します。 多くの場合、月の名前を抽出するコードには、さらに柔軟性がありません。 一般に、このようなコードでは特定の言語の月の名前を使用した 12 か月の暦を前提としています。

次の例に示すように、[カスタム日時書式指定文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)または <xref:System.Globalization.DateTimeFormatInfo> オブジェクトのプロパティを使用すると、ユーザーのカルチャの曜日または月の名前を反映する文字列を簡単に抽出できます。 この例では、現在のカルチャをフランス語 (フランス) に変更し、2013 年 7 月 1 日の曜日の名前と月の名前を表示します。

[!code-csharp[Conceptual.Globalization#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/monthname2.cs#20)]
[!code-vb[Conceptual.Globalization#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/monthname2.vb#20)]

## <a name="numeric-values"></a>数値

数値の処理は、数値をユーザー インターフェイスに表示するのか、保持するのかによって異なります。 このセクションでは、両方の使用を検討します。

> [!NOTE]
> 解析操作と書式設定操作では、.NET は基本ラテン文字 0 から 9 (U+0030 から U+0039) だけを数字として認識します。

### <a name="display-numeric-values"></a>数値を表示する

通常、数値をユーザー インターフェイスに表示するときには、<xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> プロパティと、<xref:System.Globalization.NumberFormatInfo> プロパティによって返される `CultureInfo.CurrentCulture.NumberFormat` オブジェクトで定義されているユーザーのカルチャの書式指定規則を使用する必要があります。 次のメソッドのいずれかを使用して日付の書式を設定すると、現在のカルチャの書式指定規則が自動的に使用されます。

- 任意の数値型のパラメーターなしの `ToString` メソッド

- 書式指定文字列を引数として含む任意の数値型の `ToString(String)` メソッド

- [複合書式指定](../../../docs/standard/base-types/composite-formatting.md)機能 (数値と共に使用する場合)

次の例では、パリ (フランス) の毎月の平均気温を表示します。 この例では、最初にデータを表示する前に現在のカルチャをフランス語 (フランス) に設定し、次に英語 (米国) に設定します。 どちらの場合も、月の名前と気温はそのカルチャに適した形式で表示されます。 この 2 つのカルチャでは、気温の値に使用する小数点記号が異なります。 また、この例では、"MMMM" カスタム日時書式指定文字列を使用して月の正式名を表示し、<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A?displayProperty=nameWithType> 配列で最も長い月の名前の長さを確認することで、結果の文字列の月の名前に適切な領域を割り当てます。

[!code-csharp[Conceptual.Globalization#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/numbers1.cs#5)]
[!code-vb[Conceptual.Globalization#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/numbers1.vb#5)]

### <a name="persist-numeric-values"></a>数値を保持する

数値データはカルチャ固有の書式で保持しないでください。 これは、データの破損または実行時例外が発生する一般的なプログラミング エラーです。 次の例では、10 個のランダムな浮動小数点数を生成し、英語 (米国) カルチャの書式指定規則を使用して文字列としてシリアル化します。 データが英語 (米国) カルチャの規則を使用して取得および解析されると、正常に復元されます。 ただし、この数値をフランス語 (フランス) カルチャの規則を使用して取得および解析すると、使用する小数点記号が異なるため、数値はいずれも解析できません。

[!code-csharp[Conceptual.Globalization#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/numbers2.cs#6)]
[!code-vb[Conceptual.Globalization#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/numbers2.vb#6)]

この問題を回避するには、次の方法のいずれかを使用します。

- ユーザーのカルチャに関係なく同じであるカスタム書式指定文字列を使用して数値の文字列表現を保存および解析します。

- <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> プロパティによって返されるインバリアント カルチャの書式指定規則を使用して文字列として数値を保存します。

- 文字列形式ではなく、バイナリで数値をシリアル化します。

次の例では、最後のアプローチを示します。 この例では、<xref:System.Double> 値の配列をシリアル化し、英語 (米国) カルチャとフランス語 (フランス) カルチャの書式指定規則を使用して逆シリアル化して、表示します。

[!code-csharp[Conceptual.Globalization#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/numbers3.cs#7)]
[!code-vb[Conceptual.Globalization#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/numbers3.vb#7)]

通貨値のシリアル化は特殊なケースです。 通貨値は、それが表現されている通貨の単位に依存するため、独立した数値として扱う意味はほとんどありません。 ただし、通貨値を、通貨記号を含んだ書式設定された文字列として保存する場合、次の例に示すように、既定のカルチャが異なる通貨記号を使用するシステムで逆シリアル化できません。

[!code-csharp[Conceptual.Globalization#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/currency1.cs#16)]
[!code-vb[Conceptual.Globalization#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/currency1.vb#16)]

代わりに、カルチャの名前など、カルチャに関する情報と共に数値をシリアル化して、値とその通貨記号が現在のカルチャと関係なく逆シリアル化できるようにする必要があります。 次の例では、2 つメンバーを使用して `CurrencyValue` 構造体を定義することでこれを実行しています。2 つのメンバーは、<xref:System.Decimal> 値とその値が属するカルチャの名前です。

[!code-csharp[Conceptual.Globalization#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/currency2.cs#17)]
[!code-vb[Conceptual.Globalization#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/currency2.vb#17)]

## <a name="work-with-culture-specific-settings"></a>カルチャ固有の設定を使用する

.NET では、特定のカルチャまたは地域は <xref:System.Globalization.CultureInfo> クラスによって表されます。 そのプロパティの一部は、カルチャのある側面に関する特定の情報を提供するオブジェクトを返します。

- <xref:System.Globalization.CultureInfo.CompareInfo%2A?displayProperty=nameWithType> プロパティは、カルチャによる文字列の比較方法および並べ替え方法に関する情報を含んだ <xref:System.Globalization.CompareInfo> オブジェクトを返します。

- <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> プロパティは、日付と時刻のデータの書式設定で使用されるカルチャ固有の情報を提供する <xref:System.Globalization.DateTimeFormatInfo> オブジェクトを返します。

- <xref:System.Globalization.CultureInfo.NumberFormat%2A?displayProperty=nameWithType> プロパティは、数値データの書式設定で使用されるカルチャ固有の情報を提供する <xref:System.Globalization.NumberFormatInfo> オブジェクトを返します。

- <xref:System.Globalization.CultureInfo.TextInfo%2A?displayProperty=nameWithType> プロパティは、カルチャの書記体系に関する情報を提供する <xref:System.Globalization.TextInfo> オブジェクトを返します。

一般に、特定の <xref:System.Globalization.CultureInfo> プロパティおよび関連するオブジェクトの値について何も想定しないでください。 代わりに、次の理由により、カルチャ固有のデータは変更される可能性があると考える必要があります。

- 個々のプロパティ値は、データが修正された、より優れたデータが使用可能になった、カルチャ固有の規則が変更されたなどの理由で、時間と共に変更または修正される可能性があります。

- 個々のプロパティ値は、.NET のバージョンまたはオペレーティング システムのバージョン間で異なる場合があります。

- .NET では置換カルチャをサポートしています。 これにより、既存の標準カルチャを補足または既存の標準カルチャを完全に置き換える新しいカスタム カルチャを定義できます。

- Windows システムのユーザーは、コントロール パネルの **[地域と言語]** アプリを使用してカルチャ固有の設定をカスタマイズできます。 <xref:System.Globalization.CultureInfo> オブジェクトをインスタンス化するときに、<xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29?displayProperty=nameWithType> コンストラクターを呼び出すことでこのようなユーザーによるカスタマイズを反映するかどうかを決定できます。 通常は、エンド ユーザーのアプリでは、ユーザー設定を尊重して、ユーザー自身が予期する形式でデータを表示する必要があります。

## <a name="see-also"></a>関連項目

- [グローバライズとローカライズ](../../../docs/standard/globalization-localization/index.md)
- [文字列を使用するためのベスト プラクティス](../../../docs/standard/base-types/best-practices-strings.md)
