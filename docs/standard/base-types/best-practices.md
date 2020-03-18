---
title: .NET の正規表現に関するベスト プラクティス
description: .NET での効率的で効果的な正規表現の作成方法について説明します。
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- .NET Framework regular expressions, best practices
- regular expressions, best practices
ms.assetid: 618e5afb-3a97-440d-831a-70e4c526a51c
ms.openlocfilehash: 9b09f5a2505888c6154a58a3512c94c51f89295b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77124423"
---
# <a name="best-practices-for-regular-expressions-in-net"></a>.NET の正規表現に関するベスト プラクティス

.NET の正規表現エンジンは、リテラル テキストの比較と照合ではなくパターン一致に基づいてテキストを処理する、完全な機能を備えた強力なツールです。 ほとんどの場合は、すばやく効率的にパターン一致が実行されますが、 処理が非常に遅く見えることもあります。 極端なケースでは、比較的小さな入力の処理に何時間も何日もかかり、応答しなくなったように見えることさえあります。

ここでは、正規表現で最適なパフォーマンスを確保するためのいくつかのベスト プラクティスの概要を説明します。

## <a name="consider-the-input-source"></a>入力ソースを考慮に入れる

一般に、正規表現で受け入れられる入力には、制約のある入力と制約のない入力の 2 種類があります。 制約のある入力とは、あらかじめ定義された形式に従っている、既知のソースまたは信頼できるソースからのテキストです。 制約のない入力とは、あらかじめ定義された形式や予想される形式に従っていない可能性のある、不確実なソース (Web ユーザーなど) からのテキストです。

正規表現パターンは、有効な入力に一致するように記述されるのが一般的です。 開発者はまず、対象となるテキストを調査して、そのテキストに一致する正規表現パターンを記述します。 記述が完了すると、そのパターンを複数の有効な入力項目でテストして、修正や改善が必要かどうかを確認します。 想定されるすべての有効な入力に一致するようになったら、そのパターンは運用環境で使用する準備が整ったと見なされ、リリースされるアプリケーションに含めることができます。 こうして作成された正規表現パターンは、制約のある入力との照合には適していますが、 制約のない入力との照合に適しているとは言えません。

制約のない入力と照合する正規表現は、次の 3 種類のテキストを効率的に処理できなければなりません。

- 正規表現パターンに一致するテキスト。

- 正規表現パターンに一致しないテキスト。

- 正規表現パターンにほぼ一致するテキスト。

制約のある入力を処理するために記述された正規表現で特に問題となるのは、最後の種類のテキストです。 その正規表現が広範な[バックトラッキング](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)にも依存している場合、一見何の問題もないように見えるテキストの処理に極端に長い時間 (場合によっては何時間も何日も) が費やされる可能性があります。

> [!WARNING]
> 次の例では、過度なバックトラッキングを生じる傾向があり、有効な電子メール アドレスを拒否する可能性がある正規表現を使用します。 電子メールの検証ルーチンで使用しないでください。 電子メール アドレスを検証する正規表現を使用する場合は、「[方法:文字列が有効な電子メール形式であるかどうかを検証する](../../../docs/standard/base-types/how-to-verify-that-strings-are-in-valid-email-format.md)」をご覧ください。

例として、電子メール アドレスのエイリアスを検証するための正規表現について見てみましょう。このような正規表現はよく使用されますが、きわめて大きな問題もはらんでいます。 有効と見なされる電子メール アドレスを処理するために、`^[0-9A-Z]([-.\w]*[0-9A-Z])*$` という正規表現を記述したとします。有効な電子メール アドレスは、英数字で始まり、その後に 0 個以上の文字 (英数字、ピリオド、またはハイフン) が続きます。 また、正規表現は英数字で終了する必要があります。 この正規表現は、次の例に示すように、有効な入力は簡単に処理できますが、有効に近い入力を処理するときに極端に処理効率が低下します。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/design2.cs#1)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/design2.vb#1)]

この出力を見るとわかるように、有効な電子メール エイリアスは、その長さに関係なくほとんど同じ時間で処理されます。 一方、有効に近い電子メール アドレスでは、長さが 5 文字を超えると、文字列の文字が 1 文字増えるたびに処理時間が約 2 倍に増加します。 つまり、有効に近い文字列の長さが 28 文字になると処理に 1 時間以上かかり、33 文字になるとほぼ 1 日かかることになります。

この正規表現は、一致する入力の形式ばかりを念頭に置いて開発されていて、パターンに一致しない入力のことが考慮されていません。 そのため、制約のない入力が正規表現パターンにほぼ一致する場合に、パフォーマンスが大幅に低下する可能性があります。

この問題を解決する方法を以下に示します。

- パターンを開発するときには、バックトラッキングが正規表現エンジンのパフォーマンスに与える影響を考慮に入れます。特に、制約のない入力を処理する正規表現ではこれが重要です。 詳細については、このトピックの「[バックトラッキングを管理する](#take-charge-of-backtracking)」を参照してください。

- 有効な入力だけでなく、無効な入力と有効に近い入力も使用して、正規表現を徹底的にテストします。 特定の正規表現に対する入力をランダムに生成するには、Microsoft Research の正規表現調査ツール [Rex](https://www.microsoft.com/research/project/rex-regular-expression-exploration/) を使用できます。

## <a name="handle-object-instantiation-appropriately"></a>オブジェクトのインスタンス化を適切に処理する

.NET の正規表現オブジェクト モデルの中核となるのは、正規表現エンジンを表す <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> クラスです。 <xref:System.Text.RegularExpressions.Regex> エンジンの使用方法は、多くの場合、正規表現のパフォーマンスを左右する最大の要因になります。 正規表現を定義するときには、正規表現エンジンと正規表現パターンを密に結合する必要があります。 この結合のプロセスは、正規表現パターンをコンストラクターに渡して <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化する場合も、分析する文字列と共に正規表現パターンを渡して静的メソッドを呼び出す場合も、必然的にコストが高くなります。

> [!NOTE]
> 解釈される正規表現とコンパイルされる正規表現を使用する場合のパフォーマンスへの暗黙的な影響に関する詳細な議論については、「[Optimizing Regular Expression Performance, Part II:Taking Charge of Backtracking](https://docs.microsoft.com/archive/blogs/bclteam/optimizing-regular-expression-performance-part-ii-taking-charge-of-backtracking-ron-petrusha)」(正規表現のパフォーマンスの最適化、パート II: バックトラッキングの管理) を BCL チームのブログ上で参照してください。

正規表現エンジンを特定の正規表現パターンと結合し、そのエンジンを使用してテキストを照合するには、次のようにいくつかの方法があります。

- パターン一致を実行する静的メソッド (<xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%29?displayProperty=nameWithType> など) を呼び出します。 この場合は、正規表現オブジェクトをインスタンス化する必要はありません。

- <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化し、解釈される正規表現のパターン一致を実行するインスタンス メソッドを呼び出します。 これは、正規表現エンジンを正規表現パターンにバインドするための既定の方法です。 <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化するときに、`options` フラグを含む <xref:System.Text.RegularExpressions.RegexOptions.Compiled> 引数を指定しないと、この方法が使用されます。

- <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化し、コンパイルされた正規表現のパターン一致を実行するインスタンス メソッドを呼び出します。 <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化するときに、`options` フラグを含む <xref:System.Text.RegularExpressions.RegexOptions.Compiled> 引数を指定した場合、正規表現オブジェクトはコンパイル済みのパターンを表します。

- 特定の正規表現パターンと密に結合された専用の <xref:System.Text.RegularExpressions.Regex> オブジェクトを作成し、コンパイルして、スタンドアロンのアセンブリに保存します。 これを行うには、<xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType> メソッドを呼び出します。

正規表現の一致メソッドを呼び出す方法は、アプリケーションのパフォーマンスに大きな影響を与える可能性があります。 以降では、アプリケーションのパフォーマンスを改善するために、静的メソッドの呼び出し、解釈される正規表現、およびコンパイルされる正規表現を使い分ける方法について説明します。

> [!IMPORTANT]
> メソッド呼び出しの形式 (静的、解釈、コンパイル) は、メソッド呼び出しで同じ正規表現が繰り返し使用される場合や、アプリケーションで正規表現オブジェクトが多用される場合に、パフォーマンスに影響を与えます。

### <a name="static-regular-expressions"></a>静的正規表現

静的正規表現メソッドは、正規表現オブジェクトを同じ正規表現で繰り返しインスタンス化する代わりの方法として推奨されます。 正規表現オブジェクトで使用される正規表現パターンとは異なり、静的メソッドの呼び出しで使用されるパターンの場合は、そのオペレーション コードまたはコンパイルされた Microsoft Intermediate Language (MSIL) が正規表現エンジンによって内部にキャッシュされます。

たとえば、ユーザー入力を検証するために別のメソッドを頻繁に呼び出すイベント ハンドラーがあったとします。 これを反映したコードを次の例に示します。この例では、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Click> イベントを使用して `IsValidCurrency` というメソッドを呼び出しています。このメソッドは、ユーザーが通貨記号に続けて 1 文字以上の 10 進数の数字を入力したかどうかを確認します。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/static1.cs#2)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/static1.vb#2)]

次の例は、`IsValidCurrency` メソッドのきわめて非効率的な実装を示しています。 この例では、このメソッドが呼び出されるたびに <xref:System.Text.RegularExpressions.Regex> オブジェクトが同じパターンでインスタンス化されます。 そのため、メソッドが呼び出されるたびに正規表現パターンを再コンパイルしなければならなくなります。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/static1.cs#3)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/static1.vb#3)]

この非効率的なコードは、静的な <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%29?displayProperty=nameWithType> メソッドの呼び出しに置き換える必要があります。 これにより、パターン一致メソッドを呼び出すたびに <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化する必要がなくなり、正規表現エンジンがキャッシュからコンパイル済みの正規表現を取得できるようになります。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/static2.cs#4)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/static2.vb#4)]

既定では、最近使用された静的正規表現パターンが 15 個までキャッシュされます。 アプリケーションで多数の静的正規表現をキャッシュする必要がある場合は、<xref:System.Text.RegularExpressions.Regex.CacheSize%2A?displayProperty=nameWithType> プロパティを設定してキャッシュのサイズを調整できます。

この例で使用されている正規表現 `\p{Sc}+\s*\d+` は、入力文字列が通貨記号と 1 文字以上の 10 進数の数字で構成されているかどうかを確認します。 このパターンは、次の表に示すように定義されます。

|パターン|説明|
|-------------|-----------------|
|`\p{Sc}+`|Unicode の Symbol, Currency カテゴリの 1 個以上の文字と一致します。|
|`\s*`|0 個以上の空白文字と一致します。|
|`\d+`|1 個以上の 10 進数と一致します。|

### <a name="interpreted-vs-compiled-regular-expressions"></a>解釈される正規表現とコンパイルされる正規表現

<xref:System.Text.RegularExpressions.RegexOptions.Compiled> オプションを指定して正規表現エンジンにバインドされていない正規表現パターンは、解釈されます。 正規表現オブジェクトがインスタンス化されるとき、正規表現エンジンは、その正規表現を一連のオペレーション コードに変換します。 インスタンス メソッドが呼び出されると、オペレーション コードが MSIL に変換され、JIT コンパイラによって実行されます。 同様に、静的正規表現メソッドが呼び出され、その正規表現がキャッシュに見つからない場合、正規表現エンジンは、その正規表現を一連のオペレーション コードに変換し、キャッシュに格納します。 それらのオペレーション コードは、その後、JIT コンパイラで実行できるように MSIL に変換されます。 解釈される正規表現では、実行時間が長くなる代わりに、スタートアップ時間が短縮されます。 このため、正規表現を使用するメソッドの呼び出し回数が少ない場合、または正確な回数はわからなくても少ないと予想される場合に適しています。 メソッド呼び出しの回数が増えるにつれて、スタートアップ時間の短縮というメリットよりも、実行速度の低下というデメリットの方が大きくなります。

<xref:System.Text.RegularExpressions.RegexOptions.Compiled> オプションを指定して正規表現エンジンにバインドされた正規表現パターンは、コンパイルされます。 つまり、正規表現オブジェクトがインスタンス化されるとき、または静的正規表現メソッドが呼び出され、その正規表現がキャッシュに見つからないとき、正規表現エンジンは、その正規表現を一連の中間的なオペレーション コードに変換し、さらに MSIL に変換します。 メソッドが呼び出されると、その MSIL が JIT コンパイラによって実行されます。 コンパイルされる正規表現では、解釈される正規表現とは対照的に、スタートアップ時間が長くなる一方で、個々のパターン一致メソッドの実行時間は短くなります。 結果として、正規表現のコンパイルによって得られるパフォーマンス上のメリットは、正規表現メソッドが呼び出される回数に比例して大きくなります。

要約すると、特定の正規表現について正規表現メソッドを呼び出す回数が比較的少ない場合は、解釈される正規表現を使用することをお勧めします。 特定の正規表現について正規表現メソッドを呼び出す回数が比較的多い場合は、コンパイルされる正規表現を使用することをお勧めします。 解釈される正規表現で実行速度の低下がスタートアップ時間の短縮を上回るしきい値や、コンパイルされる正規表現でスタートアップ時間の増加が実行速度の向上を上回るしきい値については、正確な値を特定するのは困難です。 これは、正規表現の複雑さや、処理される個々のデータなど、さまざまな要因に依存します。 特定のアプリケーションのシナリオにおいて、解釈される正規表現とコンパイルされる正規表現のどちらで最適なパフォーマンスが得られるかを特定するには、<xref:System.Diagnostics.Stopwatch> クラスを使用して実行時間を比較します。

次の例では、Theodore Dreiser の『*The Financier*』の最初の 10 個の文を読み取る場合とすべての文を読み取る場合について、コンパイルされる正規表現と解釈される正規表現のパフォーマンスを比較しています。 出力を見るとわかるように、正規表現の一致メソッドの呼び出しが 10 回だけの場合は、解釈される正規表現の方がコンパイルされる正規表現よりパフォーマンスが高くなります。 しかし、多数の呼び出し (この場合は 13,000 回以上) が行われる場合は、コンパイルされる正規表現の方がパフォーマンスが高くなります。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/compare1.cs#5)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/compare1.vb#5)]

この例で使用する正規表現パターン `\b(\w+((\r?\n)|,?\s))*\w+[.?:;!]` は、次の表に示すように定義されています。

|パターン|説明|
|-------------|-----------------|
|`\b`|ワード境界から照合を開始します。|
|`\w+`|1 つ以上の単語文字に一致します。|
|<code>(\r?\n)&#124;,?\s)</code>|0 ～ 1 個の復帰とそれに続く改行文字、または 0 ～ 1 個のコンマとそれに続く空白文字に一致します。|
|<code>(\w+((\r?\n)&#124;,?\s))*</code>|1 個以上の単語文字の後に 0 ～ 1 個の復帰と改行文字または 0 ～ 1 個のコンマと空白文字が続くパターンの 0 回以上の出現に一致します。|
|`\w+`|1 つ以上の単語文字に一致します。|
|`[.?:;!]`|ピリオド、疑問符、コロン、セミコロン、または感嘆符に一致します。|

### <a name="regular-expressions-compiled-to-an-assembly"></a>正規表現: アセンブリにコンパイル済み

.NET では、コンパイル済みの正規表現を含むアセンブリを作成することもできます。 これにより、パフォーマンスの低下を招く正規表現のコンパイルを、実行時ではなくデザイン時に行うことができます。 ただし、いくつかの追加の作業も伴います。正規表現を事前に定義して、アセンブリにコンパイルする必要があります。 これにより、アセンブリの正規表現を使用するソース コードをコンパイルするときに、コンパイラがそのアセンブリを参照できるようになります。 アセンブリに含まれるコンパイル済みの各正規表現は、<xref:System.Text.RegularExpressions.Regex> の派生クラスによって表されます。

正規表現をアセンブリにコンパイルするには、<xref:System.Text.RegularExpressions.Regex.CompileToAssembly%28System.Text.RegularExpressions.RegexCompilationInfo%5B%5D%2CSystem.Reflection.AssemblyName%29?displayProperty=nameWithType> メソッドを呼び出して、コンパイルする正規表現を表す <xref:System.Text.RegularExpressions.RegexCompilationInfo> オブジェクトの配列と、作成するアセンブリに関する情報を含む <xref:System.Reflection.AssemblyName> オブジェクトを渡します。

次のような状況では、正規表現をアセンブリにコンパイルすることをお勧めします。

- コンポーネント開発者が、再利用できる正規表現のライブラリを作成する場合。

- 正規表現のパターン一致メソッドが呼び出される回数を特定できない場合 (1 ～ 2 回から数千～数万回の範囲)。 別個のアセンブリにコンパイルされた正規表現では、コンパイルされる正規表現や解釈される正規表現とは違って、メソッド呼び出しの回数に関係なく一貫したパフォーマンスが得られます。

コンパイル済みの正規表現を使用してパフォーマンスを最適化する場合は、アセンブリの作成、正規表現エンジンの読み込み、およびそのパターン一致メソッドの実行にリフレクションを使用しないようにする必要があります。 そのためには、正規表現パターンを動的に構築しないこと、およびパターン一致オプション (大文字と小文字を区別しないパターン一致など) をアセンブリの作成時に指定することが要求されます。 さらに、アセンブリを作成するコードを、正規表現を使用するコードから分離する必要もあります。

次の例は、コンパイル済みの正規表現を含むアセンブリを作成する方法を示しています。 この例では、`RegexLib.dll` という 1 つの正規表現クラスを含む `SentencePattern` という名前のアセンブリを作成しています。このクラスには、「[解釈される正規表現とコンパイルされる正規表現](#interpreted-vs-compiled-regular-expressions)」セクションで使用した、文に一致する正規表現パターンが含まれています。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/compile1.cs#6)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/compile1.vb#6)]

この例を実行可能ファイルにコンパイルして実行すると、`RegexLib.dll` という名前のアセンブリが作成されます。 正規表現は、`Utilities.RegularExpressions.SentencePattern` から派生する <xref:System.Text.RegularExpressions.Regex> という名前のクラスによって表されます。 次の例では、このコンパイル済みの正規表現を使用して、Theodore Dreiser の『*The Financier*』のテキストから文を抽出しています。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/compile2.cs#7)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/compile2.vb#7)]

## <a name="take-charge-of-backtracking"></a>バックトラッキングを管理する

通常、正規表現エンジンは入力文字列内を直線的に進んで、入力文字列を正規表現パターンと比較します。 しかし、正規表現パターン内で不定量指定子 (`*`、`+`、`?` など) が使用されていると、パターン全体に対する一致を検索するために、それまでに見つかった部分的な一致を放棄して、以前に保存した状態に戻る場合があります。 このプロセスをバックトラッキングと呼びます。

> [!NOTE]
> バックトラッキングの詳細については、「[正規表現の動作の詳細](../../../docs/standard/base-types/details-of-regular-expression-behavior.md)」と「[バックトラッキング](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)」を参照してください。 バックトラッキングに関する詳細な議論については、「[Optimizing Regular Expression Performance, Part II:Taking Charge of Backtracking](https://docs.microsoft.com/archive/blogs/bclteam/optimizing-regular-expression-performance-part-ii-taking-charge-of-backtracking-ron-petrusha)」(正規表現のパフォーマンスの最適化、パート II: バックトラッキングの管理) を BCL チームのブログ上で参照してください。

バックトラッキングのサポートにより、正規表現はより強力かつ柔軟になります。 同時に、正規表現エンジンの動作を正規表現の開発者が制御することにもなります。 この責任を認識していない開発者によるバックトラッキングの誤用や過度なバックトラッキングへの依存が、多くの場合、正規表現のパフォーマンスを低下させる最大の要因になっています。 最悪のシナリオでは、入力文字列が 1 文字増えるたびに実行時間が倍増することもあります。 実際、バックトラッキングを過度に使用すると、入力が正規表現パターンにほぼ一致する場合に、プログラム的に無限ループと同等の状態に陥りやすくなります。そのような状態では、正規表現エンジンによる比較的短い入力文字列の処理に何時間も何日もかかることがあります。

照合に必要でないにもかかわらずバックトラッキングを使用した代償として、アプリケーションのパフォーマンスが低下することもよくあります。 例として、`\b\p{Lu}\w*\b` という正規表現について見てみましょう。この正規表現は、次の表に示すように、大文字で始まるすべての単語に一致します。

|パターン|説明|
|-|-|
|`\b`|ワード境界から照合を開始します。|
|`\p{Lu}`|大文字に一致します。|
|`\w*`|0 個以上の単語に使用される文字に一致します。|
|`\b`|ワード境界で照合を終了します。|

ワード境界は単語文字と同じではなく、単語文字のサブセットでもないため、正規表現エンジンが単語文字の照合中にワード境界を越える可能性はありません。 したがって、この正規表現では、バックトラッキングが照合の全体的な成功に寄与することはありません。バックトラッキングを使用すると、正規表現エンジンが単語文字の照合に成功するたびに状態を保存しなければならなくなるため、パフォーマンスを低下させるだけです。

バックトラッキングが不要であることがわかった場合は、アトミック グループとして知られる `(?>subexpression)` 言語要素を使用して無効にすることができます。 次の例では、2 つの正規表現を使用して入力文字列を解析しています。 1 つはバックトラッキングに依存する `\b\p{Lu}\w*\b`、 もう 1 つはバックトラッキングを無効にする `\b\p{Lu}(?>\w*)\b` です。 出力を見るとわかるように、どちらも結果は同じになります。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/backtrack2.cs#10)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/backtrack2.vb#10)]

正規表現パターンと入力テキストの照合にバックトラッキングが欠かせない場合もよくあります。 ただし、過度なバックトラッキングが発生すると、極端にパフォーマンスが低下して、アプリケーションが応答しなくなったように見えることがあります。 この状況が発生するのは、たとえば、量指定子が入れ子になっていて、外側の部分式に一致するテキストが内側の部分式に一致するテキストのサブセットになっている場合です。

> [!WARNING]
> 過度なバックトラッキングの回避に加えて、タイムアウト機能を使用して、過度なバックトラッキングによって、正規表現のパフォーマンスが極端に低下するのを防ぐ必要があります。 詳しくは、「[タイムアウト値を使用する](#use-time-out-values)」をご覧ください。

例として、部品番号に一致するように作られた `^[0-9A-Z]([-.\w]*[0-9A-Z])*\$$` という正規表現パターンについて見てみましょう。この部品番号は、少なくとも 1 文字の英数字で構成されます。 追加の文字では、英数字、ハイフン、アンダースコア、およびピリオドが許容されますが、最後の文字は英数字でなければなりません。 部品番号の終わりはドル記号で示されます。 この正規表現パターンは、量指定子が入れ子になっているうえに、部分式 `[0-9A-Z]` が部分式 `[-.\w]*` のサブセットであるため、パフォーマンスが極端に低下する可能性があります。

このような場合に正規表現のパフォーマンスを最適化するには、入れ子になった量指定子を削除して、外側の部分式をゼロ幅の先読みアサーションまたは後読みアサーションに置き換えます。 先読みアサーションと後読みアサーションはアンカーなので、入力文字列内でポインターを移動させることなく、先読みまたは後読みによって、指定された条件が満たされているかどうかを確認します。 たとえば、この部品番号の正規表現は `^[0-9A-Z][-.\w]*(?<=[0-9A-Z])\$$` として書き直すことができます。 この正規表現パターンは、次の表に示すように定義されます。

|パターン|説明|
|-------------|-----------------|
|`^`|入力文字列の先頭から照合を開始します。|
|`[0-9A-Z]`|英数字 1 文字に一致します。 これは、部品番号に最低限必要な文字です。|
|`[-.\w]*`|任意の単語文字、ハイフン、またはピリオドの 0 回以上の出現に一致します。|
|`\$`|ドル記号に一致します。|
|`(?<=[0-9A-Z])`|末尾のドル記号を先読みし、前の文字が英数字であることを確認します。|
|`$`|入力文字列の末尾で照合を終了します。|

次の例では、この正規表現を使用して、部品番号を含む配列を照合しています。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/backtrack4.cs#11)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/backtrack4.vb#11)]

.NET の正規表現言語には、入れ子になった量指定子を取り除くために使用できる次の言語要素が含まれています。 詳細については、「 [グループ化構成体](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)」を参照してください。

|言語要素|説明|
|----------------------|-----------------|
|`(?=` `subexpression` `)`|ゼロ幅の肯定先読みです。 現在の位置からの先読みにより、`subexpression` が入力文字列に一致するかどうかを確認します。|
|`(?!` `subexpression` `)`|ゼロ幅の否定先読みです。 現在の位置からの先読みにより、`subexpression` が入力文字列に一致しないかどうかを確認します。|
|`(?<=` `subexpression` `)`|ゼロ幅の肯定後読みです。 現在の位置からの後読みにより、`subexpression` が入力文字列に一致するかどうかを確認します。|
|`(?<!` `subexpression` `)`|ゼロ幅の否定後読みです。 現在の位置からの後読みにより、`subexpression` が入力文字列に一致しないかどうかを確認します。|

## <a name="use-time-out-values"></a>タイムアウト値を使用する

正規表現が正規表現パターンにほぼ一致する入力を処理する場合は、パフォーマンスに大きな影響を与える過度なバックトラッキングに依存することがよくあります。 慎重にバックトラッキングの使用を検討し、ほぼ一致する入力に対して正規表現をテストすることに加えて、過度なバックトラッキングが発生した場合にその影響を確実に最小限に抑えるために、必ずタイムアウト値を設定する必要があります。

正規表現のタイムアウト間隔は、タイムアウトする前に正規表現エンジンが単一の一致を検索する期間を定義します。既定のタイムアウト間隔は <xref:System.Text.RegularExpressions.Regex.InfiniteMatchTimeout?displayProperty=nameWithType> で、正規表現がタイムアウトしないことを意味します。次のようにこの値をオーバーライドし、タイムアウト間隔を定義できます。

- <xref:System.Text.RegularExpressions.Regex> コンストラクターを呼び出すことによって、<xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29?displayProperty=nameWithType> オブジェクトをインスタンス化するときに、タイムアウト値を指定します。

- <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29?displayProperty=nameWithType>、<xref:System.Text.RegularExpressions.Regex.Replace%28System.String%2CSystem.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29?displayProperty=nameWithType> など、`matchTimeout` パラメーターを含む静的パターン一致メソッドを呼び出します。

- <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType> メソッドを呼び出すことによって作成されるコンパイルされる正規表現の場合、<xref:System.TimeSpan> 型のパラメーターを持つコンストラクターを呼び出します。

タイムアウト間隔を定義していて、その間隔の終了時に一致が見つからない場合、正規表現メソッドは <xref:System.Text.RegularExpressions.RegexMatchTimeoutException> 例外をスローします。 例外ハンドラーで、タイムアウト間隔を延長して一致を再試行する、一致操作を破棄して一致なしと見なす、または一致操作を破棄して今後の分析のために例外情報を記録することを選択できます。

次の例では、テキスト ドキュメントの単語の数と 1 つの単語に含まれる平均文字数を計算するために、タイムアウト間隔が 350 ミリ秒の正規表現をインスタンス化する `GetWordData` メソッドを定義します。 一致操作がタイムアウトした場合、タイムアウト間隔は 350 ミリ秒ずつ延長され、<xref:System.Text.RegularExpressions.Regex> オブジェクトが再インスタンス化されます。 新しいタイムアウト間隔が 1 秒を超える場合、メソッドは呼び出し元に例外を再スローします。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/timeout1.cs#12)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/timeout1.vb#12)]

## <a name="capture-only-when-necessary"></a>必要なときにのみキャプチャする

.NET の正規表現では、数多くのグループ化構成体がサポートされています。これらを使用すると、正規表現パターンを 1 つ以上の部分式にグループ化することができます。 .NET の正規表現言語で最もよく使用されるグループ化構成体は、番号付きのキャプチャ グループを定義する `(`*subexpression*`)` と、名前付きのキャプチャ グループを定義する `(?<`*name*`>`*subexpression*`)` です。 グループ化構成体は、前方参照を作成したり、量指定子を適用する部分式を定義したりするのに欠かせません。

しかし、これらの言語要素の使用にはコストが伴います。 まず、<xref:System.Text.RegularExpressions.GroupCollection> プロパティから返される <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> オブジェクトに、最新の名前のないキャプチャまたは名前付きキャプチャが設定されます。また、1 つのグループ化構成体によって入力文字列の複数の部分文字列がキャプチャされた場合は、特定のキャプチャ グループの <xref:System.Text.RegularExpressions.CaptureCollection> プロパティから返される <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> オブジェクトに複数の <xref:System.Text.RegularExpressions.Capture> オブジェクトが設定されます。

正規表現内で量指定子を適用できるようにするためだけにグループ化構成体が使用されていて、それらの部分式によってキャプチャされたグループがその後に使用されていない場合もよくあります。 例として、文全体をキャプチャするために作られた正規表現 `\b(\w+[;,]?\s?)+[.?!]` について見てみましょう。 次の表は、この正規表現パターン内の言語要素と、<xref:System.Text.RegularExpressions.Match> オブジェクトの <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> コレクションおよび <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> コレクションに対する影響を示しています。

|パターン|説明|
|-------------|-----------------|
|`\b`|ワード境界から照合を開始します。|
|`\w+`|1 つ以上の単語文字に一致します。|
|`[;,]?`|0 個または 1 個のコンマまたはセミコロンに一致します。|
|`\s?`|0 個または 1 個の空白文字と一致します。|
|`(\w+[;,]?\s?)+`|1 個以上の単語文字の後に省略可能なコンマまたはセミコロンと省略可能な空白文字が続くパターンの 1 回以上の出現に一致します。 これにより、最初のキャプチャ グループが定義されます。このグループは、複数の単語文字の組み合わせ (つまり単語) の後に省略可能な区切り記号が続くパターンが、正規表現エンジンが文の終わりに到達するまで繰り返されるようにするために必要です。|
|`[.?!]`|ピリオド、疑問符、または感嘆符に一致します。|

次の例に示すように、一致が見つかると、<xref:System.Text.RegularExpressions.GroupCollection> オブジェクトと <xref:System.Text.RegularExpressions.CaptureCollection> オブジェクトの両方に一致からのキャプチャが設定されます。 ここでは、`(\w+[;,]?\s?)` 量指定子を適用できるようにするためにキャプチャ グループ `+` が使用されているため、正規表現パターンが文の各単語に一致します。 そうでない場合は、文の最後の単語に一致します。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/group1.cs#8)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/group1.vb#8)]

量指定子を適用するためだけに部分式を使用していて、キャプチャされたテキストは特に必要ないという場合は、グループ キャプチャを無効にする必要があります。 たとえば、`(?:subexpression)` 言語要素をグループに適用すると、そのグループでは、一致した部分文字列がキャプチャされなくなります。 次の例では、前の例の正規表現パターンが `\b(?:\w+[;,]?\s?)+[.?!]` に変更されています。 出力を見るとわかるように、これにより、<xref:System.Text.RegularExpressions.GroupCollection> コレクションと <xref:System.Text.RegularExpressions.CaptureCollection> コレクションが設定されなくなります。

[!code-csharp[Conceptual.RegularExpressions.BestPractices#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/group2.cs#9)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/group2.vb#9)]

キャプチャを無効にするには次のような方法があります。

- `(?:subexpression)` 言語要素を使用します。 この要素をグループに適用すると、そのグループでは、一致した部分文字列がキャプチャされなくなります。 入れ子になったグループによる部分文字列のキャプチャは無効になりません。

- <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture> オプションを使用します。 これにより、正規表現パターン内の名前のないキャプチャ (暗黙的なキャプチャ) がすべて無効になります。 このオプションを使用した場合は、`(?<name>subexpression)` 言語要素を使用して定義した名前付きグループに一致する部分文字列のみがキャプチャされます。 <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture> フラグは、`options` クラス コンストラクターの <xref:System.Text.RegularExpressions.Regex> パラメーターか、`options` の静的な一致メソッドの <xref:System.Text.RegularExpressions.Regex> パラメーターに渡すことができます。

- `n` 言語要素の `(?imnsx)` オプションを使用します。 これにより、正規表現パターンでこの要素が出現する位置以降の名前のないキャプチャ (暗黙的なキャプチャ) がすべて無効になります。 パターンの末尾に到達するか、`(-n)` オプションによって名前のないキャプチャ (暗黙的なキャプチャ) が有効になるまで、キャプチャは無効のままです。 詳細については、「 [その他の構成体](../../../docs/standard/base-types/miscellaneous-constructs-in-regular-expressions.md)」を参照してください。

- `n` 言語要素の `(?imnsx:subexpression)` オプションを使用します。 これにより、`subexpression` 内の名前のないキャプチャ (暗黙的なキャプチャ) がすべて無効になります。 入れ子になった名前のない (暗黙的な) キャプチャ グループによるキャプチャも無効になります。

## <a name="related-topics"></a>関連トピック

|Title|説明|
|-----------|-----------------|
|[正規表現の動作の詳細](../../../docs/standard/base-types/details-of-regular-expression-behavior.md)|.NET の正規表現エンジンの実装について検討します。 正規表現の柔軟性に焦点を当てて、正規表現エンジンの効率的かつ堅牢な動作を確保するための開発者の責任について説明します。|
|[バックトラッキング](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)|バックトラッキングの概要と、正規表現のパフォーマンスに与える影響について説明し、バックトラッキングの代わりに使用できる言語要素について検討します。|
|[正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)|.NET の正規表現言語の言語要素について説明します。各言語要素の詳細な説明へのリンクも含まれています。|
