---
title: .NET の文字列を使用するためのベスト プラクティス
description: .NET アプリケーションで文字列を効果的に使用する方法について説明します。
ms.date: 05/01/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- strings [.NET Framework],searching
- best practices,string comparison and sorting
- strings [.NET Framework],best practices
- strings [.NET Framework],basic string operations
- sorting strings
- strings [.NET Framework],sorting
- string comparison [.NET Framework],best practices
- string sorting
- comparing strings
- strings [.NET Framework],comparing
ms.assetid: b9f0bf53-e2de-4116-8ce9-d4f91a1df4f7
ms.openlocfilehash: c88776ea9d8ba17d86767b704e8b0eaff5b6cb89
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711481"
---
# <a name="best-practices-for-using-strings-in-net"></a>.NET の文字列を使用するためのベスト プラクティス

.NET には、ローカライズされたアプリケーションやグローバル化されたアプリケーションを開発するための広範なサポートが用意されており、文字列の並べ替えや表示などの一般的な操作を実行するときに、現在のカルチャの規則や特定のカルチャの規則を簡単に適用できるようになっています。 しかし、文字列の並べ替えや比較の操作は、必ずしもカルチャに依存するとは限りません。 たとえば、アプリケーションが内部で使用する文字列は、通常、すべてのカルチャで同じように処理される必要があります。 XML タグ、HTML タグ、ユーザー名、ファイル パス、システム オブジェクトの名前などのカルチャに依存しない文字列データがカルチャに依存するかのように解釈されると、アプリケーション コードで軽度のバグが発生したり、パフォーマンスが低下したり、場合によってはセキュリティの問題を引き起こしたりする可能性があります。

このトピックでは、.NET の文字列の並べ替え、比較、および大文字と小文字の区別のメソッドについて検討し、適切な文字列処理メソッドを選択するための推奨事項と、文字列処理メソッドに関する追加情報を紹介します。 また、数値データ、日時データなど、書式付きデータを表示および格納のために処理する方法についても説明します。

## <a name="recommendations-for-string-usage"></a>文字列の使用に関する推奨事項

.NET による開発で文字列を使用するときの簡単な推奨事項を次に示します。

- 文字列操作に対して文字列比較の規則を明示的に指定するオーバーロードを使用します。 そのためには、通常、<xref:System.StringComparison> 型のパラメーターを持つメソッド オーバーロードを呼び出します。
- カルチャに依存しない文字列照合の安全な既定の方法として、<xref:System.StringComparison.Ordinal?displayProperty=nameWithType> または <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> を使用して比較を行います。
- パフォーマンスを向上させるには、<xref:System.StringComparison.Ordinal?displayProperty=nameWithType> または <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> による比較を使用します。
- ユーザーに出力を表示する場合は、<xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType> に基づく文字列操作を使用します。
- 比較が言語的な意味を持たない場合 (記号としての比較など) は、<xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> に基づく文字列操作ではなく、非言語的な <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> 値または <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> 値を使用します。
- 比較のために文字列を正規化する場合は <xref:System.String.ToLowerInvariant%2A?displayProperty=nameWithType> メソッドではなく、<xref:System.String.ToUpperInvariant%2A?displayProperty=nameWithType> メソッドを使用します。
- 2 つの文字列が等価かどうかをテストするには、<xref:System.String.Equals%2A?displayProperty=nameWithType> メソッドのオーバーロードを使用します。
- <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドと <xref:System.String.CompareTo%2A?displayProperty=nameWithType> メソッドは、文字列を並べ替える場合に使用し、文字列の等価性を確認する場合には使用しません。
- 数値、日付など、文字列以外のデータをユーザー インターフェイスに表示するには、カルチャに依存する書式設定を使用します。 文字列以外のデータを文字列形式で保持するには、[インバリアント カルチャ](xref:System.Globalization.CultureInfo.InvariantCulture)を使用する書式設定を使用します。

文字列を使用する際に避ける必要があることを次に示します。

- 文字列操作に対して文字列比較の規則を明示的または暗黙的に指定しないオーバーロードは使用しないでください。
- ほとんどの場合、<xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> に基づく文字列操作は使用しないでください。 数少ない例外の 1 つは、言語的な意味を持つがカルチャには依存しないデータを永続化する場合です。
- 2 つの文字列が等価かどうかを確認する場合に、<xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドまたは <xref:System.String.CompareTo%2A> メソッドのオーバーロードで戻り値が 0 かどうかをテストする方法は使用しないでください。
- 数値データや日時データを文字列形式で保持する場合は、カルチャに依存する書式設定を使用しないでください。

## <a name="specifying-string-comparisons-explicitly"></a>文字列比較の明示的な指定

.NET の文字列操作メソッドは、ほとんどがオーバーロードされています。 通常は、既定の設定をそのまま使用する 1 つまたは複数のオーバーロードと、既定の設定を使用せずに文字列の比較または操作の正確な方法を定義するその他のオーバーロードがあります。 既定の設定に依存しないメソッドには、ほとんどの場合、 <xref:System.StringComparison>型のパラメーターが含まれています。これは、カルチャおよび大文字と小文字の区別によって文字列比較の規則を明示的に指定する列挙型です。 <xref:System.StringComparison> 列挙型のメンバーを次の表に示します。

|StringComparison のメンバー|説明|
|-----------------------------|-----------------|
|<xref:System.StringComparison.CurrentCulture>|現在のカルチャを使用して、大文字と小文字を区別する比較を実行します。|
|<xref:System.StringComparison.CurrentCultureIgnoreCase>|現在のカルチャを使用して、大文字と小文字を区別しない比較を実行します。|
|<xref:System.StringComparison.InvariantCulture>|インバリアント カルチャを使用して、大文字と小文字を区別する比較を実行します。|
|<xref:System.StringComparison.InvariantCultureIgnoreCase>|インバリアント カルチャを使用して、大文字と小文字を区別しない比較を実行します。|
|<xref:System.StringComparison.Ordinal>|序数に基づく比較を実行します。|
|<xref:System.StringComparison.OrdinalIgnoreCase>|大文字と小文字を区別しない、序数に基づく比較を実行します。|

たとえば、文字または文字列に一致する <xref:System.String> オブジェクト内の部分文字列のインデックスを返す <xref:System.String.IndexOf%2A> メソッドには、次の 9 つのオーバーロードがあります。

- <xref:System.String.IndexOf%28System.Char%29>、 <xref:System.String.IndexOf%28System.Char%2CSystem.Int32%29>、および <xref:System.String.IndexOf%28System.Char%2CSystem.Int32%2CSystem.Int32%29>。文字列内の文字の序数に基づく (大文字と小文字を区別し、カルチャに依存しない) 検索を既定で実行します。
- <xref:System.String.IndexOf%28System.String%29>、 <xref:System.String.IndexOf%28System.String%2CSystem.Int32%29>、および <xref:System.String.IndexOf%28System.String%2CSystem.Int32%2CSystem.Int32%29>。文字列内の部分文字列の、大文字と小文字を区別し、カルチャに依存した検索を既定で実行します。
- <xref:System.String.IndexOf%28System.String%2CSystem.StringComparison%29>、 <xref:System.String.IndexOf%28System.String%2CSystem.Int32%2CSystem.StringComparison%29>、および <xref:System.String.IndexOf%28System.String%2CSystem.Int32%2CSystem.Int32%2CSystem.StringComparison%29>。比較の形式を指定できる <xref:System.StringComparison> 型のパラメーターが含まれています。

次のような理由から、既定値を使用しないオーバーロードを選択することをお勧めします。

- 既定のパラメーターを持つオーバーロードには、序数に基づく比較を実行するもの (文字列インスタンスで <xref:System.Char> を検索するもの) と、カルチャに依存するもの (文字列インスタンスで文字列を検索するもの) があります。 どのメソッドがどの既定値を使用するのかを覚えておくのは容易ではなく、使用するオーバーロードを間違えやすくなります。
- メソッド呼び出しで既定値に依存するコードは、意図が不明確になります。 既定値に依存する次の例では、2 つの文字列の序数に基づく比較と言語に基づく比較のどちらを開発者が意図しているのかや、`protocol` と "http" の大文字と小文字が違っていた場合に等価性テストで `false` を返すかどうかがわかりにくくなっています。

     [!code-csharp[Conceptual.Strings.BestPractices#1](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/explicitargs1.cs#1)]
     [!code-vb[Conceptual.Strings.BestPractices#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/explicitargs1.vb#1)]

一般的には、既定値に依存しないメソッドを呼び出すことをお勧めします。そうすると、コードの意図が明確になります。 その結果、コードが読みやすくなるため、デバッグや保守も容易になります。 次の例では、前の例で発生した問題に対応します。 序数比較を使用することと、大文字と小文字の違いを無視することを指定します。

[!code-csharp[Conceptual.Strings.BestPractices#2](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/explicitargs1.cs#2)]
[!code-vb[Conceptual.Strings.BestPractices#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/explicitargs1.vb#2)]

## <a name="the-details-of-string-comparison"></a>文字列比較の詳細

文字列比較は、多くの文字列関連操作 (特に並べ替えおよび等価性テスト) の中核です。 文字列は、決まった順序で並べられています。たとえば、文字列の並べ替え済みリスト上で "my" が "string" の前にある場合、比較では必ず "my" が "string" 以下になります。 また、比較は等価性を暗黙的に定義します。 比較演算では、等価と見なされた文字列に対して 0 が返されます。 これは、どちらの文字列ももう一方の文字列より小さくないという意味に解釈するとわかりやすくなります。 文字列に関係する、意味のある操作のほとんどには、他の文字列との比較か、正しく定義された並べ替え操作の実行のいずれかまたは両方の処理が含まれています。

> [!NOTE]
> Windows オペレーティング システムの並べ替え操作と比較操作で使用される文字の重みに関する情報を含む一連のテキスト ファイルである[並べ替え重みテーブル](https://www.microsoft.com/download/details.aspx?id=10921) と、Linux と macOS 用の並べ替え重みテーブルの最新バージョンである [デフォルト Unicode 照合基本テーブル](https://www.unicode.org/Public/UCA/latest/allkeys.txt)をダウンロードできます。 Linux と macOS での並べ替え重みのテーブルの特定のバージョンは、システムにインストールされている [International Components for Unicode](http://site.icu-project.org/) ライブラリのバージョンによって異なります。 実装される ICU のバージョンと Unicode のバージョンに関する情報は、[ICU のダウンロード](http://site.icu-project.org/download)に関する記事を参照してください。

しかし、2 つの文字列の等価性や並べ替え順序を評価する場合、正しい結果は 1 つではありません。結果は、文字列の比較に使用される基準に依存するためです。 特に、序数に基づく文字列比較や、現在のカルチャまたは[インバリアント カルチャ](xref:System.Globalization.CultureInfo.InvariantCulture) (英語をベースとする、ロケールに依存しないカルチャ) の大文字と小文字の規則や並べ替えの規則に基づく文字列比較では、さまざまな結果が返される可能性があります。

さらに、文字列比較を、異なるバージョンの .NET を使用したり、異なるオペレーティング システムまたはバージョンが異なるオペレーティング システム上の .NET で実行したりすると、異なる結果が返る可能性があります。 詳細については、「[Strings and The Unicode Standard](xref:System.String#Unicode)」(文字列と Unicode 標準) を参照してください。

### <a name="string-comparisons-that-use-the-current-culture"></a>現在のカルチャを使用する文字列比較

文字列を比較するときの基準として現在のカルチャの規則が使用される場合があります。 現在のカルチャに基づく比較では、スレッドの現在のカルチャ (ロケール) が使用されます。 ユーザーがカルチャを設定していない場合は、コントロール パネルの **[地域のオプション]** ウィンドウの設定が既定で使用されます。 言語的な意味を持つデータや、カルチャに依存したユーザー操作を反映するデータに対しては、常に現在のカルチャに基づく比較を使用する必要があります。

しかし、.NET の比較や大文字と小文字の区別の動作は、カルチャによって変わります。 たとえば、開発されたコンピューターとは異なるカルチャのコンピューターでアプリケーションが実行された場合や、実行中のスレッドのカルチャが変更された場合などに、この変化が生じます。 これは意図的な動作ですが、多くの開発者にはまだあまり知られていません。 次の例は、英語 (米国) ("en-US") とスウェーデン語 ("sv-SE") のカルチャの並べ替え順序の違いを示しています。 並べ替えられた文字列配列で、"ångström"、"Windows"、および "Visual Studio" の位置が違っていることに注目してください。

[!code-csharp[Conceptual.Strings.BestPractices#3](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison1.cs#3)]
[!code-vb[Conceptual.Strings.BestPractices#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison1.vb#3)]

現在のカルチャを使用する、大文字と小文字を区別しない比較は、スレッドの現在のカルチャの大文字と小文字の区別の規則が無視される以外は、カルチャに依存した比較と同じです。 この動作も、並べ替え順序に影響する場合があります。

現在のカルチャのセマンティクスを使用する比較は、次のメソッドで既定で使用されます。

- <xref:System.StringComparison> パラメーターを含まない <xref:System.String.Compare%2A?displayProperty=nameWithType> のオーバーロード。
- <xref:System.String.CompareTo%2A?displayProperty=nameWithType> のオーバーロード。
- 既定の <xref:System.String.StartsWith%28System.String%29?displayProperty=nameWithType> メソッドと、 <xref:System.String.StartsWith%28System.String%2CSystem.Boolean%2CSystem.Globalization.CultureInfo%29?displayProperty=nameWithType> null `null`<xref:System.Globalization.CultureInfo> のオーバーロード。
- 既定の <xref:System.String.EndsWith%28System.String%29?displayProperty=nameWithType> メソッドと、`null`<xref:System.Globalization.CultureInfo> パラメーターを持つ <xref:System.String.EndsWith%28System.String%2CSystem.Boolean%2CSystem.Globalization.CultureInfo%29?displayProperty=nameWithType> メソッド。
- 検索パラメーターとして<xref:System.String.IndexOf%2A?displayProperty=nameWithType> を受け取る、 <xref:System.String> パラメーターを持たない <xref:System.StringComparison> のオーバーロード。
- 検索パラメーターとして <xref:System.String> を受け取る、<xref:System.StringComparison> パラメーターを持たない <xref:System.String.LastIndexOf%2A?displayProperty=nameWithType> のオーバーロード。

どのような場合でも、 <xref:System.StringComparison> パラメーターを持つオーバーロードを呼び出して、メソッド呼び出しの意図を明確にすることをお勧めします。

非言語的な文字列データが言語的に解釈されたり、特定のカルチャの文字列データが別のカルチャの規則で解釈されたりすると、軽度のバグやあまり軽度でないバグが発生する可能性があります。 その典型的な例が、トルコ語の I の問題です。

英語 (米国) を含むほぼすべてのラテン アルファベットでは、文字 "i" (\u0069) は "I" (\u0049) の小文字版です。 この大文字と小文字の規則は、このようなカルチャでプログラミングを行う人にとってはすぐに当たり前のことになります。 しかし、トルコ語 ("tr-TR") のアルファベットには、"i" の大文字版である "ドット付きの I" ("İ" (\u0130)) があります。 大文字にすると "I" になる小文字の "ドットなしの i" ("ı" (\u0131)) もあります。 この動作は、アゼルバイジャン語 ("az") のカルチャでも発生します。

したがって、"i" を大文字にしたり "I" を小文字にしたりする動作に関する前提は、すべてのカルチャで有効なわけではありません。 文字列比較ルーチンの既定のオーバーロードを使用すると、カルチャ間の差異の影響を受けることになります。 また、非言語的なデータを比較する場合も、既定のオーバーロードを使用すると望ましくない結果が返される可能性があります。たとえば次の例では、文字列 "file" と "FILE" の大文字と小文字を区別しない比較を実行しようとしています。

[!code-csharp[Conceptual.Strings.BestPractices#11](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/turkish1.cs#11)]
[!code-vb[Conceptual.Strings.BestPractices#11](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/turkish1.vb#11)]

この比較は、セキュリティが重要となる状況でカルチャが不注意に使用されると、次の例のような重大な問題を引き起こす可能性があります。 `IsFileURI("file:")` などのメソッド呼び出しは、現在のカルチャが英語 (米国) の場合は `true` を返しますが、現在のカルチャがトルコ語の場合は `false` を返します。 したがって、"FILE:" で始まる URI へのアクセスを大文字と小文字の区別なくブロックするセキュリティ対策は、トルコ語のシステムでは攻略される可能性があります。

[!code-csharp[Conceptual.Strings.BestPractices#12](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/turkish1.cs#12)]
[!code-vb[Conceptual.Strings.BestPractices#12](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/turkish1.vb#12)]

この例の "file:" は、カルチャに依存しない非言語的な識別子として解釈されるものなので、コードを次のように書き換える必要があります。

[!code-csharp[Conceptual.Strings.BestPractices#13](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/turkish1.cs#13)]
[!code-vb[Conceptual.Strings.BestPractices#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/turkish1.vb#13)]

### <a name="ordinal-string-operations"></a>序数に基づく文字列操作

メソッド呼び出しで <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> 値または <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> 値を指定すると、非言語的な比較が行われ、自然言語の特性は無視されます。 これらの <xref:System.StringComparison> 値を使用して呼び出されたメソッドでは、文字列操作の判断が、大文字と小文字の指定、またはカルチャでパラメーター化される同等の表ではなく、単純なバイト比較に基づいて行われます。 これにより、ほとんどの場合に文字列が意図されたとおりに解釈され、コードの実行速度と信頼性も向上します。

序数に基づく比較とは、各文字列の各バイトが言語的に解釈されずに比較される文字列比較です (たとえば、"windows" と "Windows" は一致しません)。 これは、実質的には C ランタイムの `strcmp` 関数の呼び出しです。 文字列が厳密に一致する必要がある状況や、慎重な照合ポリシーが求められる状況では、この比較を使用します。 また、序数に基づく比較は最も高速な比較演算でもあります。これは、結果を判定するときに言語の規則が適用されないためです。

.NET の文字列には、null 文字が埋め込まれる場合があります。 序数に基づく比較とカルチャに依存した比較 (インバリアント カルチャを使用する比較を含む) の最も明白な違いの 1 つは、文字列に埋め込まれた null 文字の処理に関連しています。 これらの文字は、<xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドや <xref:System.String.Equals%2A?displayProperty=nameWithType> メソッドを使用して、カルチャに依存した比較 (インバリアント カルチャを使用する比較を含む) を実行する場合には無視されます。 その結果、カルチャに依存した比較では、null 文字が埋め込まれた文字列と null 文字が埋め込まれていない文字列が等価と見なされる可能性があります。

> [!IMPORTANT]
> 埋め込まれた null 文字は、文字列比較メソッドでは無視されますが、文字列検索メソッド (<xref:System.String.Contains%2A?displayProperty=nameWithType>、<xref:System.String.EndsWith%2A?displayProperty=nameWithType>、<xref:System.String.IndexOf%2A?displayProperty=nameWithType>、<xref:System.String.LastIndexOf%2A?displayProperty=nameWithType>、<xref:System.String.StartsWith%2A?displayProperty=nameWithType> など) では無視されません。

次の例では、文字列 "Aa" と、"A" と "a" の間にいくつかの null 文字が埋め込まれた類似の文字列とのカルチャに依存した比較を実行して、2 つの文字列が等価と見なされることを示しています。

[!code-csharp[Conceptual.Strings.BestPractices#19](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/embeddednulls1.cs#19)]
 [!code-vb[Conceptual.Strings.BestPractices#19](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/embeddednulls1.vb#19)]

一方、次の例のように序数に基づく比較を使用すると、これらの文字列は等価とは見なされません。
  
[!code-csharp[Conceptual.Strings.BestPractices#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/embeddednulls2.cs#20)]
[!code-vb[Conceptual.Strings.BestPractices#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/embeddednulls2.vb#20)]

その次に慎重な方法は、大文字と小文字を区別しない序数に基づく比較です。 この比較では、大文字と小文字の区別のほとんどが無視されます (たとえば、"windows" と "Windows" は一致します)。 ASCII 文字を操作する場合、このポリシーは <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> と同等ですが、通常の ASCII の大文字と小文字の区別が無視されます。 したがって、[A, Z] (\u0041-\u005A) の任意の文字が [a,z] (\u0061-\007A) の対応する文字と一致します。 ASCII の範囲外の大文字と小文字の区別には、インバリアント カルチャのテーブルが使用されます。 次に例を示します。

[!code-csharp[Conceptual.Strings.BestPractices#4](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison2.cs#4)]
[!code-vb[Conceptual.Strings.BestPractices#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison2.vb#4)]

この比較は、次の比較と同等です (ただし、より高速です)。

[!code-csharp[Conceptual.Strings.BestPractices#5](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison2.cs#5)]
[!code-vb[Conceptual.Strings.BestPractices#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison2.vb#5)]

とはいえ、これらの比較はどちらも非常に高速です。

> [!NOTE]
> ファイル システム、レジストリのキーと値、および環境変数の文字列の動作は、<xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> によって最もよく表現されます。

<xref:System.StringComparison.Ordinal?displayProperty=nameWithType> と <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> は、どちらもバイナリ値を直接使用するため、照合に最適です。 比較の設定について確信を持てない場合は、この 2 つのいずれかの値を使用してください。 ただし、これらの値を使用するとバイトごとの比較が行われるため、言語的な順序 (英語の辞書のような順序) ではなくバイナリの順序で並べ替えが行われます。 したがって、結果をユーザーに表示すると、ほとんどの場合不自然に見えます。

序数に基づくセマンティクスは、<xref:System.StringComparison> 引数を含まない <xref:System.String.Equals%2A?displayProperty=nameWithType> のオーバーロード (等値演算子を含む) で既定で使用されます。 どのような場合でも、 <xref:System.StringComparison> パラメーターを持つオーバーロードを呼び出すことをお勧めします。

### <a name="string-operations-that-use-the-invariant-culture"></a>インバリアント カルチャを使用する文字列操作

インバリアント カルチャを使用する比較では、静的 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> プロパティから返される <xref:System.Globalization.CultureInfo.CompareInfo%2A> プロパティが使用されます。 この動作は、すべてのシステムで同じです。範囲外の文字は、等価のインバリアント文字と見なされる文字に変換されます。 このポリシーは、同じ文字列動作のセットを複数のカルチャにわたって保持する場合に便利ですが、予期しない結果になることもよくあります。

インバリアント カルチャを使用する、大文字と小文字を区別しない比較でも、静的 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> プロパティから返される静的 <xref:System.Globalization.CultureInfo.CompareInfo%2A> プロパティが比較情報として使用されます。 変換後の文字の大文字と小文字の違いは無視されます。

<xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> を使用する比較と <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> を使用する比較は、ASCII 文字列に対して同じように動作します。 ただし、<xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> では言語的な判断が下されるため、バイト セットとして解釈する必要がある文字列に対しては不適切になることがあります。 `CultureInfo.InvariantCulture.CompareInfo` オブジェクトのために <xref:System.String.Compare%2A> メソッドで特定の文字のセットが等価と解釈されることもあります。 たとえば、次の例が等価になるのは、インバリアント カルチャでは妥当です。

InvariantCulture: a + ̊ = å

LATIN SMALL LETTER A 文字 "a" (\u0061) は、COMBINING RING ABOVE 文字 "+ " ̊" (\u030a) の横にある場合、LATIN SMALL LETTER A WITH RING ABOVE 文字 "å" (\u00e5) として解釈されます。 この動作は、次の例に示すように、序数に基づく比較とは異なります。

[!code-csharp[Conceptual.Strings.BestPractices#15](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison3.cs#15)]
[!code-vb[Conceptual.Strings.BestPractices#15](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison3.vb#15)]

ファイル名やクッキーなど、"å" のような組み合わせが出現する可能性がある要素を解釈する場合にも、序数に基づく比較を使用するのが最も明確かつ適切な方法になります。

結局のところ、インバリアント カルチャには、比較に使用する際に便利なプロパティがほとんどありません。 インバリアント カルチャを使用すると、言語的な意味を持つ形で比較が行われるため、記号の完全な等価性は保証されません。その一方で、特定のカルチャでの表示にも適していません。 <xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> を比較に使用する数少ない理由の 1 つは、順序付けされたデータを複数のカルチャで同じように表示するために永続化できることです。 たとえば、表示する並べ替え済みの識別子のリストを含む大きなデータ ファイルがアプリケーションに付属している場合に、そのリストにエントリを追加するには、インバリアント スタイルの並べ替えを使用する挿入が必要になります。

## <a name="choosing-a-stringcomparison-member-for-your-method-call"></a>メソッド呼び出しに使用する StringComparison メンバーの選択

文字列のセマンティックなコンテキストと <xref:System.StringComparison> 列挙型のメンバーとの対応関係の概要を次の表に示します。

|データ|動作|対応する System.StringComparison<br /><br /> 値|
|----------|--------------|-----------------------------------------------------|
|大文字と小文字が区別される内部識別子。<br /><br /> XML や HTTP などの標準の、大文字と小文字が区別される識別子。<br /><br /> 大文字と小文字が区別されるセキュリティ関連の設定。|バイトが正確に一致する非言語的識別子。|<xref:System.StringComparison.Ordinal>|
|大文字と小文字が区別されない内部識別子。<br /><br /> XML や HTTP などの標準の、大文字と小文字が区別されない識別子。<br /><br /> ファイル パス。<br /><br /> レジストリのキーと値。<br /><br /> 環境変数。<br /><br /> リソース識別子 (ハンドル名など)。<br /><br /> 大文字と小文字が区別されないセキュリティ関連の設定。|大文字と小文字が区別されない、言語的な意味を持たない識別子 (ほとんどの Windows システム サービスで格納されるデータなど)。|<xref:System.StringComparison.OrdinalIgnoreCase>|
|永続化される、言語的な意味を持つデータの一部。<br /><br /> 一定の並べ替え順序を必要とする言語的なデータの表示。|カルチャに依存しないが、言語的な意味を持つデータ。|<xref:System.StringComparison.InvariantCulture><br /><br /> \- または -<br /><br /> <xref:System.StringComparison.InvariantCultureIgnoreCase>|
|ユーザーに表示されるデータ。<br /><br /> ほとんどのユーザー入力。|特定の言語の規則を必要とするデータ。|<xref:System.StringComparison.CurrentCulture><br /><br /> \- または -<br /><br /> <xref:System.StringComparison.CurrentCultureIgnoreCase>|

## <a name="common-string-comparison-methods-in-net"></a>.NET の一般的な文字列比較メソッド

以降では、文字列比較でよく使用されるメソッドについて説明します。

### <a name="stringcompare"></a>String.Compare

既定の解釈: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>

このメソッドは文字列解釈の中心的な操作となるため、メソッド呼び出しのすべてのインスタンスを調べて、文字列を現在のカルチャに従って解釈するべきか、カルチャから切り離して (記号として) 扱うべきかどうかを確認する必要があります。 ほとんどは後者であるため、その場合は代わりに <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> の比較を使用します。

<xref:System.Globalization.CultureInfo.CompareInfo%2A?displayProperty=nameWithType> プロパティから返される <xref:System.Globalization.CompareInfo?displayProperty=nameWithType> クラスにも、<xref:System.Globalization.CompareOptions> フラグ列挙体でさまざまな照合方法 (序数に基づく、空白文字を無視する、カナ型を無視するなど) を指定できる <xref:System.Globalization.CompareInfo.Compare%2A> メソッドが含まれています。

### <a name="stringcompareto"></a>String.CompareTo

既定の解釈: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>

このメソッドには、現時点では、<xref:System.StringComparison> 型を指定するオーバーロードはありません。 通常は、このメソッドを推奨される <xref:System.String.Compare%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> の形式に変換できます。

このメソッドは、 <xref:System.IComparable> インターフェイスと <xref:System.IComparable%601> インターフェイスを実装する型に実装されます。 このメソッドには <xref:System.StringComparison> パラメーターのオプションがないため、実装する型のコンストラクターで <xref:System.StringComparer> を指定できるようにするのが一般的です。 次の例では、クラス コンストラクターに <xref:System.StringComparer> パラメーターを含む `FileName` クラスを定義しています。 この <xref:System.StringComparer> オブジェクトは、その後、 `FileName.CompareTo` メソッドで使用されています。

[!code-csharp[Conceptual.Strings.BestPractices#6](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/api1.cs#6)]
[!code-vb[Conceptual.Strings.BestPractices#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/api1.vb#6)]

### <a name="stringequals"></a>String.Equals

既定の解釈: <xref:System.StringComparison.Ordinal?displayProperty=nameWithType>

<xref:System.String> クラスで等価性テストを実行するには、 <xref:System.String.Equals%2A> メソッド (静的メソッドまたはインスタンス メソッド) のオーバーロードを呼び出すか、静的等値演算子を使用します。 これらのオーバーロードと演算子では、序数に基づく比較が既定で使用されます。 しかし、序数に基づく比較を実行する場合でも、<xref:System.StringComparison> 型を明示的に指定するオーバーロードを呼び出すことをお勧めします。これにより、特定の文字列解釈のコードを検索しやすくなります。

### <a name="stringtoupper-and-stringtolower"></a>String.ToUpper と String.ToLower

既定の解釈: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>

これらのメソッドを使用するときには注意が必要です。というのも、文字列を大文字や小文字に強制的に変換する操作は、文字列を大文字と小文字の区別に関係なく比較するための小規模の正規化としてよく使用されるからです。 その場合は、大文字と小文字を区別しない比較を使用することを検討してください。

<xref:System.String.ToUpperInvariant%2A?displayProperty=nameWithType> メソッドと <xref:System.String.ToLowerInvariant%2A?displayProperty=nameWithType> メソッドを使用することもできます。 <xref:System.String.ToUpperInvariant%2A> は、大文字と小文字を正規化するための標準的な方法です。 <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> を使用して行われる比較は、動作の内容を見ると、両方の文字列引数に対して <xref:System.String.ToUpperInvariant%2A> を呼び出し、<xref:System.StringComparison.Ordinal?displayProperty=nameWithType> を使用して比較を行うという、2 つの呼び出しの組み合わせです。

特定のカルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトをメソッドに渡して、そのカルチャで大文字および小文字への変換を行うためのオーバーロードもあります。

### <a name="chartoupper-and-chartolower"></a>Char.ToUpper と Char.ToLower

既定の解釈: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>

これらのメソッドの動作は、上で説明した <xref:System.String.ToUpper%2A?displayProperty=nameWithType> メソッドおよび <xref:System.String.ToLower%2A?displayProperty=nameWithType> メソッドと同様です。

### <a name="stringstartswith-and-stringendswith"></a>String.StartsWith と String.EndsWith

既定の解釈: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>

これらのメソッドは、いずれもカルチャに依存した比較を既定で実行します。

### <a name="stringindexof-and-stringlastindexof"></a>String.IndexOf と String.LastIndexOf

既定の解釈: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>

これらのメソッドの既定のオーバーロードは、比較の実行方法が一貫していません。 <xref:System.Char> パラメーターを含むすべての <xref:System.String.IndexOf%2A?displayProperty=nameWithType> メソッドと <xref:System.String.LastIndexOf%2A?displayProperty=nameWithType> メソッドは、序数に基づく比較を実行します。一方、<xref:System.String> パラメーターを含む既定の <xref:System.String.IndexOf%2A?displayProperty=nameWithType> メソッドと <xref:System.String.LastIndexOf%2A?displayProperty=nameWithType> メソッドは、カルチャに依存した比較を実行します。

<xref:System.String.IndexOf%28System.String%29?displayProperty=nameWithType> メソッドまたは <xref:System.String.LastIndexOf%28System.String%29?displayProperty=nameWithType> メソッドを呼び出して、現在のインスタンスで検索する文字列を渡す場合は、<xref:System.StringComparison> 型を明示的に指定するオーバーロードを呼び出すことをお勧めします。 <xref:System.Char> 引数を含むオーバーロードでは、 <xref:System.StringComparison> 型を指定することはできません。

## <a name="methods-that-perform-string-comparison-indirectly"></a>間接的に文字列比較を実行するメソッド

文字列比較を中心的な操作とする非文字列メソッドの中には、 <xref:System.StringComparer> 型を使用するものがあります。 <xref:System.StringComparer> クラスには、<xref:System.StringComparer> のインスタンスを返す静的プロパティが 6 つ含まれています。これらのインスタンスの <xref:System.StringComparer.Compare%2A?displayProperty=nameWithType> メソッドは、次の種類の文字列比較を実行します。

- 現在のカルチャを使用する、カルチャに依存した文字列比較。 この <xref:System.StringComparer> オブジェクトは、<xref:System.StringComparer.CurrentCulture%2A?displayProperty=nameWithType> プロパティによって返されます。
- 現在のカルチャを使用する、大文字と小文字を区別しない比較。 この <xref:System.StringComparer> オブジェクトは、<xref:System.StringComparer.CurrentCultureIgnoreCase%2A?displayProperty=nameWithType> プロパティによって返されます。
- インバリアント カルチャの単語ベースの比較規則を使用する、カルチャに依存しない比較。 この <xref:System.StringComparer> オブジェクトは、<xref:System.StringComparer.InvariantCulture%2A?displayProperty=nameWithType> プロパティによって返されます。
- インバリアント カルチャの単語ベースの比較規則を使用する、大文字と小文字を区別しない、カルチャに依存しない比較。 この <xref:System.StringComparer> オブジェクトは、 <xref:System.StringComparer.InvariantCultureIgnoreCase%2A?displayProperty=nameWithType> プロパティによって返されます。
- 序数に基づく比較。 この <xref:System.StringComparer> オブジェクトは、<xref:System.StringComparer.Ordinal%2A?displayProperty=nameWithType> プロパティによって返されます。
- 大文字と小文字を区別しない、序数に基づく比較。 この <xref:System.StringComparer> オブジェクトは、<xref:System.StringComparer.OrdinalIgnoreCase%2A?displayProperty=nameWithType> プロパティによって返されます。

### <a name="arraysort-and-arraybinarysearch"></a>Array.Sort と Array.BinarySearch

既定の解釈: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>

データをコレクションに格納したり、永続化されたデータをファイルやデータベースからコレクションに読み取ったりするときに現在のカルチャを切り替えると、コレクション内のインバリアントが無効になる可能性があります。 <xref:System.Array.BinarySearch%2A?displayProperty=nameWithType> メソッドでは、配列内で検索する要素が既に並べ替えられていると見なされます。 <xref:System.Array.Sort%2A?displayProperty=nameWithType> メソッドは、配列内の文字列要素を並べ替えるために、<xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドを呼び出して個々の要素を順序付けます。 配列の並べ替えが行われてから内容の検索が行われるまでの間にカルチャが変更される場合、カルチャに依存した比較子を使用するのは危険です。 たとえば、次のコードでは、`Thread.CurrentThread.CurrentCulture` プロパティが暗黙的に指定した比較子で格納と取得の操作が行われます。 `StoreNames` の呼び出しと `DoesNameExist`の呼び出しの間にカルチャが変更されると (この 2 つのメソッドの呼び出しの間に配列の内容が永続化された場合には特に)、バイナリ サーチが失敗する可能性があります。

[!code-csharp[Conceptual.Strings.BestPractices#7](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect1.cs#7)]
 [!code-vb[Conceptual.Strings.BestPractices#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect1.vb#7)]

次の例は、推奨されるバリエーションを示しています。ここでは、配列の並べ替えと検索の両方に、同じ序数に基づく (カルチャに依存しない) 比較メソッドが使用されています。 コードの変更は、2 つの例の `Line A` および `Line B` というラベルが付いた行に反映されています。

[!code-csharp[Conceptual.Strings.BestPractices#8](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect1.cs#8)]
[!code-vb[Conceptual.Strings.BestPractices#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect1.vb#8)]

このデータを永続化して別のカルチャのシステムに移動したり、データをユーザーに表示するために並べ替えたりする場合は、<xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> を使用することを検討してください。そうすると、ユーザー出力のために言語的な操作を行っても、カルチャの変更による影響を受けることはありません。 次の例では、前の 2 つの例を変更して、配列の並べ替えと検索にインバリアント カルチャを使用しています。

[!code-csharp[Conceptual.Strings.BestPractices#9](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect1.cs#9)]
[!code-vb[Conceptual.Strings.BestPractices#9](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect1.vb#9)]

### <a name="collections-example-hashtable-constructor"></a>コレクションの例: Hashtable のコンストラクター

文字列の比較方法の影響を受ける操作の 2 例目は文字列のハッシュです。

次の例では、<xref:System.StringComparer.OrdinalIgnoreCase%2A?displayProperty=nameWithType> プロパティから返される <xref:System.StringComparer> オブジェクトを渡して <xref:System.Collections.Hashtable> オブジェクトをインスタンス化しています。 <xref:System.StringComparer> から派生するクラス <xref:System.StringComparer> は <xref:System.Collections.IEqualityComparer> インターフェイスを実装するため、その <xref:System.Collections.IEqualityComparer.GetHashCode%2A> メソッドを使用して、ハッシュ テーブルの文字列のハッシュ コードを計算しています。

[!code-csharp[Conceptual.Strings.BestPractices#10](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect2.cs#10)]
[!code-vb[Conceptual.Strings.BestPractices#10](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect2.vb#10)]

## <a name="displaying-and-persisting-formatted-data"></a>書式設定されたデータを表示および保持する

数値、日時など、文字列以外のデータをユーザーに表示するには、ユーザーのカルチャ設定を使用して書式設定します。 既定では、以下のすべてで、書式設定操作での現在のスレッド カルチャが使用されます。

- [C#](../../csharp/language-reference/tokens/interpolated.md) と [Visual Basic](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md) のコンパイラでサポートされている挿入文字列。
- [C#](../../csharp/language-reference/operators/addition-operator.md#string-concatenation) または [Visual Basic](../../visual-basic/programming-guide/language-features/operators-and-expressions/concatenation-operators.md ) の連結演算子を使用または <xref:System.String.Concat%2A?displayProperty=nameWithType> メソッドを直接呼び出す文字列連結操作。
- <xref:System.String.Format%2A?displayProperty=nameWithType> メソッド。
- 数値型と日時型の `ToString` メソッド。

指定されたカルチャまたは[インバリアント カルチャ](xref:System.Globalization.CultureInfo.InvariantCulture)の規則を使用することで文字列を書式設定することを明示的に指定するには、次を行うことができます。

- <xref:System.String.Format%2A?displayProperty=nameWithType> メソッドと `ToString` メソッドを使用している場合、`provider` パラメーター (<xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> または <xref:System.DateTime.ToString%28System.IFormatProvider%29?displayProperty=nameWithType> など) を持つオーバー ロードを呼び出し、それに <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> プロパティ、目的のカルチャを表す <xref:System.Globalization.CultureInfo> インスタンス、または <xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType> プロパティを渡します。

- 文字列連結の場合、コンパイラに暗黙の変換の実行を許可しないでください。 代わりに、`provider` パラメーターを持つ `ToString` オーバーロードを呼び出すことで、明示的な変換を実行します。 たとえば、次の C# コードでは、<xref:System.Double> 値を文字列に変換するときに、コンパイラでは現在のカルチャが暗黙的に使用されます。

  [!code-csharp[Implicit String Conversion](~/samples/snippets/standard/base-types/string-practices/cs/tostring.cs#1)]

  代わりに、次の C# コードのように、<xref:System.Double.ToString(System.IFormatProvider)?displayProperty=nameWithType> メソッドを呼び出して、変換に使用する書式指定規則を持つカルチャを明示的に指定することができます。

  [!code-csharp[Explicit String Conversion](~/samples/snippets/standard/base-types/string-practices/cs/tostring.cs#2)]

- 文字列補間の場合、挿入文字列を <xref:System.String> インスタンスに割り当てるのではなく、<xref:System.FormattableString> に割り当てます。 その後、その <xref:System.FormattableString.ToString?displayProperty=nameWithType> メソッドを呼び出して、現在のカルチャの規則を反映する結果の文字列を生成することも、<xref:System.FormattableString.ToString(System.IFormatProvider)?displayProperty=nameWithType> メソッドを呼び出して、指定したカルチャの規則を反映する結果の文字列を生成することもできます。 また、書式設定可能な文字列を静的 <xref:System.FormattableString.Invariant%2A?displayProperty=nameWithType> メソッドに渡して、インバリアント カルチャの規則を反映する結果の文字列を生成することもできます。 このアプローチの例を次に示します。 (この例の出力には en-US の現在のカルチャが反映されます)。

  [!code-csharp[String interpolation](~/samples/snippets/standard/base-types/string-practices/cs/formattable.cs)]
  [!code-vb[String interpolation](~/samples/snippets/standard/base-types/string-practices/vb/formattable.vb)]

文字列以外のデータは、バイナリ データまたは書式付きデータとして保持できます。 書式付きデータとして保存するには、`provider` パラメーターを含む書式指定メソッドのオーバーロードを呼び出し、そのパラメーターを <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> プロパティに渡す必要があります。 インバリアント カルチャは、カルチャとコンピューターに依存しない書式付きデータに一貫した書式を提供します。 これに対し、インバリアント カルチャ以外のカルチャを使用して書式設定するデータの保持には、さまざまな制限があります。

- カルチャが異なるシステムでデータを取得したり、現在のシステムのユーザーが現在のカルチャを変更してデータを取得しようとしたりすると、そのデータは使用できない可能性があります。
- 特定のコンピューターのカルチャのプロパティは、その標準の値とは異なる場合があります。 常に、ユーザーはカルチャに依存した表示設定をカスタマイズする可能性があります。 このため、ユーザーがカルチャの設定をカスタマイズすると、システムに保存されている書式付きデータを読み取ることができなくなる場合があります。 コンピューター間の書式付きデータの移植性がさらに制限される可能性があります。
- 数値や日時の書式設定を制御する国際的、地域的、または国内の標準は時間と共に変化するため、これらの変化は Windows オペレーティング システムの更新プログラムに組み込まれています。 書式設定の規則が変わると、以前の規則に従って書式設定されたデータを読み取ることができなくなる場合があります。

次に、カルチャに依存する書式設定を使用してデータを保持すると移植性が制限される例を示します。 この例では、日時の値の配列をファイルに保存します。 これらは、英語 (米国) のカルチャの規則を使用して書式設定されています。 現在のスレッド カルチャがフランス語 (スイス) に変更されると、アプリケーションは現在のカルチャの書式設定規則を使用して保存された値を読み取ることを試みます。 2 つのデータ項目の読み取りを試みると、<xref:System.FormatException> 例外がスローされます。日付の配列には、<xref:System.DateTime.MinValue> に等しい 2 つの間違った要素が含まれることになります。

[!code-csharp[Conceptual.Strings.BestPractices#21](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/persistence.cs#21)]
[!code-vb[Conceptual.Strings.BestPractices#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/persistence.vb#21)]

しかし、<xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> プロパティを <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> に置き換えて <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 呼び出しと <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 呼び出しを行うと、保持されている日時データは正常に復元されます。次にその出力を示します。

```console
06.05.1758 21:26
05.05.1818 07:19
22.04.1870 23:54
08.09.1890 06:47
18.02.1905 15:12
```

## <a name="see-also"></a>関連項目

- [文字列の操作](manipulating-strings.md)
