---
title: 規模が大きく、応答性の高い .NET Framework アプリの作成
ms.date: 03/30/2017
ms.assetid: 123457ac-4223-4273-bb58-3bc0e4957e9d
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: c320d004b05e58fc7c239cd8c1f3bcec84ad8f78
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937907"
---
# <a name="writing-large-responsive-net-framework-apps"></a>規模が大きく、応答性の高い .NET Framework アプリの作成

この記事では、大規模な .NET Framework アプリや、ファイルやデータベースなど大量のデータを処理するアプリのパフォーマンス改善のヒントを説明します。 説明するヒントは C# および Visual Basic コンパイラを マネージド コードで作成し直した際に得られたものです。この記事では C# コンパイラでの実際の例をいくつか紹介します。 
  
.NET Framework は、生産性の高いアプリ開発環境です。 強力で安全な言語と豊富なライブラリにより、アプリの開発生産性が高くなります。 ただし、高い生産性には責任が伴います。 .NET Framework のあらゆる機能を使用するするのであれば、必要に応じてコードのパフォーマンスを調整できるようにしておく必要があります。 
  
## <a name="why-the-new-compiler-performance-applies-to-your-app"></a>新しいコンパイラのパフォーマンスがアプリに適用される理由  
 .NET コンパイラ プラットフォーム ("Roslyn") チームは、コードのモデリングと分析、ツールの開発、そして Visual Studio でのより充実したコード対応エクスペリエンスの実現のための新たな API を提供するため、C# および Visual Basic コンパイラをマネージド コードで再作成しました。 コンパイラの全体的な書き直しと、新しいコンパイラでの Visual Studio 機能の構築を通して、大規模 .NET Framework アプリや、大量データを処理するアプリに適用できる有用なパフォーマンス情報が明らかになりました。 C# コンパイラについてのこのような情報や例を活用するために、コンパイラについて理解しておく必要はありません。 
  
 Visual Studio ではコンパイラ API を使用して、ユーザーに人気のある IntelliSense 機能 (識別子とキーワードの色づけ、構文の入力候補一覧、エラーを示す波線、パラメーターのヒント、コードの問題、コードアクションなど) を作成します。 Visual Studio では、開発者がコードを入力するときや変更するときにこのヘルプが表示されます。コンパイラがコード開発者による編集内容をモデル化している間も、Visual Studio は応答性を維持する必要があります。 
  
 エンド ユーザはアプリを操作するときに、アプリが応答するものであると期待します。 入力またはコマンドの処理がブロックされることがあってはなりません。 ヘルプはすぐにポップアップ表示され、またユーザーが入力を続けるときには閉じる必要があります。 処理時間がかかっている UI スレッドがあり、これが原因でアプリの動作が遅くなっていると判断される場合でも、アプリがその UI スレッドをブロックしないようにする必要があります。 
  
 Roslyn コンパイラの詳細については、 [.NET COMPILER PLATFORM SDK](../../csharp/roslyn-sdk/index.md)を参照してください。
  
## <a name="just-the-facts"></a>.NET Framework についての事実  
 パフォーマンスを調整し、応答性のある .NET Framework アプリを作成する際には、次に説明する事実を考慮してください。 
  
### <a name="fact-1-dont-prematurely-optimize"></a>事実 1: 不完全な最適化は行わない  
 必要以上に複雑なコードを記述すると、保守、デバッグ、細かな調整に伴うコストが発生します。 経験豊富なプログラマは、コーディングの問題の解決方法を直観的に把握し、より効率的なコードを記述します。 しかし、コードの最適化が不完全になることがあります。 たとえば、単純な配列で十分な場合にハッシュ テーブルを使用したり、単に値を再計算する代わりに、メモリ リークが発生する恐れのある複雑なキャッシュを使用したりします。 経験豊富なプログラマであっても、パフォーマンスを確認するテストを実施し、問題がある場合にはコードを分析してください。 
  
### <a name="fact-2-if-youre-not-measuring-youre-guessing"></a>事実 2: 測定していないのであれば、それは推測である  
 プロファイルと測定は嘘をつきません。 プロファイルから、CPU の負荷が上限になっているかどうか、またはディスク I/O でブロックされているかどうかがわかります。 プロファイルは、割り当てるメモリのサイズと種類、[ガベージ コレクション](../../standard/garbage-collection/index.md) (GC) の処理に CPU が長い時間を取られていないか示します。 
  
 アプリでの主要な顧客エクスペリエンスやシナリオについてパフォーマンスの目標を設定し、パフォーマンスを測定するテストを作成してください。 テスト失敗の調査には科学的な手法を使用します。ガイドとなるプロファイルを使用して、どのような問題が発生しているかを仮定し、実験やコード変更によってその仮定を検証します。 定期的にテストを実施して、時間の経過と共にベースライン パフォーマンス測定を確立します。これにより、パフォーマンス後退を引き起こしている変更を切り分けることができます。 パフォーマンス測定を厳密に実施することで、不要なコード更新に時間をかけることを回避できます。 
  
### <a name="fact-3-good-tools-make-all-the-difference"></a>事実 3: 優れたツールには大きな効果がある  
 優れたツールを使用すれば、最も大きなパフォーマンスの問題 (CPU、メモリ、またはディスク) の詳細を迅速に確認し、このようなボトルネックを引き起こしているコードを特定できます。 Microsoft には、 [Visual Studio Profiler](/visualstudio/profiling/beginners-guide-to-performance-profiling)や[perfview](https://www.microsoft.com/download/details.aspx?id=28567)などのさまざまなパフォーマンスツールが付属しています。 
  
 PerfView は、ディスク I/O、GC イベント、メモリなどの深刻な問題に取り組む際に役立つ極めて強力な無償のツールです。 パフォーマンスに関連する [Windows イベント トレーシング](../wcf/samples/etw-tracing.md) (ETW) イベントをキャプチャし、アプリ別、プロセス別、スタック別、およびスレッド別に情報を容易に確認できます。 PerfView は、アプリによって割り当てられるメモリの種類と量、そしてメモリの割り当てにどの関数またはコール スタックがどの程度関与しているのかを示します。 詳細については、ツールに付属している詳しいヘルプ トピック、デモ、ビデオ (Channel 9 の [PerfView チュートリアル](https://channel9.msdn.com/Series/PerfView-Tutorial) など) を参照してください。 
  
### <a name="fact-4-its-all-about-allocations"></a>事実 4: すべては割り当てで決まる  
 応答性の高い .NET Framework アプリ開発の要となるのはアルゴリズム (例: バブル ソートの代わりにクイック ソートを使用) であると思うかもしれませんが、それは正しくありません。 応答性の高いアプリを開発する上で最も重要なのは、メモリの割り当てです。これは特に、アプリが非常に大規模であり大量データを処理する場合に該当します。 
  
 新しいコンパイラ API の応答性の高い IDE 機能のほとんどの開発作業には、割り当てを回避し、キャッシュ ストラテジを管理することが関連していました。 PerfView トレースから、新しい C# および Visual Basic コンパイラのパフォーマンスはほとんど CPU とは関連していないことが判明しています。 これらのコンパイラは、数十万行から数百万行のコード行の読み取り、メタデータの読み取り、または生成されたコードの出力の時点では I/O と関連しています。 UI スレッドの遅延の原因は、ほぼガベージ コレクションにあります。 .NET Framework GC は、パフォーマンスのために高度に調整されており、その処理のほとんどはアプリ コードの実行中に同時に実行されます。 ただし、1 回の割り当てによって負荷の高い [gen2](../../standard/garbage-collection/fundamentals.md) コレクションが実行され、これによってすべてのスレッドが停止されることがあります。 
  
## <a name="common-allocations-and-examples"></a>一般的な割り当てと例  
 このセクションの例の式では、小規模な割り当てについては特に問題になりません。 ただし、大きなアプリが式をかなりの回数にわたって実行すると、数百メガバイトまたはギガバイトのメモリが割り当てられることがあります。 たとえば、開発者によるエディターへの入力をシミュレーションする 1 分間のテストを実行したところ、ギガバイト単位のメモリが割り当てられたため、パフォーマンス チームは入力に関するシナリオに重点を置いて取り組みました。 
  
### <a name="boxing"></a>ボックス化  
 通常はスタックまたはデータ構造体内にある値型がオブジェクトでラップされると、[ボックス化](../../csharp/programming-guide/types/boxing-and-unboxing.md)が発生します。 つまり、データを保持するオブジェクトを割り当て、そのオブジェクトを指すポインタを返します。 .NET Framework では、メソッドのシグネチャや格納場所の種類が原因で、値がボックス化されることがあります。 オブジェクト内で値型がラップされると、メモリ割り当てが行われます。 多くのボックス化操作を実行すると、アプリへメガバイト単位またはギガバイト単位のメモリが割り当てられ、アプリがさらに多くの GC の原因になります。 .NET Framework と言語コンパイラでは可能な限りボックス化を回避しますが、場合によっては、ほとんど予期していない状況でボックス化が行われることがあります。 
  
 PerｆView でボックス化を確認するには、トレースを開き、アプリのプロセス名の下の [GC Heap Alloc Stacks (GC ヒープ割り当てスタック)] を参照します (PerfView はすべてのプロセスについて報告する点に注意してください)。 <xref:System.Int32?displayProperty=nameWithType> や <xref:System.Char?displayProperty=nameWithType> のような型が割り当ての下に表示される場合は、値型をボックス化しています。 このような型のいずれか 1 つを選択すると、型がボックス化されているスタックと関数が表示されます。 
  
 **例 1: string メソッドと値型引数**  
  
 このサンプル コードは、過剰であり不要である可能性のあるボックス化を示します。  
  
```csharp  
public class Logger  
{  
    public static void WriteLine(string s) { /*...*/ }  
}  
  
public class BoxingExample  
{  
    public void Log(int id, int size)  
    {  
        var s = string.Format("{0}:{1}", id, size);  
        Logger.WriteLine(s);  
    }  
}  
```  
  
 このコードはログ機能を提供するので、アプリは `Log` 関数を頻繁に (数百万回) 呼び出すことがあります。 ここで問題となるのは、`string.Format` の呼び出しが <xref:System.String.Format%28System.String%2CSystem.Object%2CSystem.Object%29> オーバーロードに解決されることです。 
  
 このオーバーロードでは、.NET Framework が `int` 値をオブジェクトにボックス化し、このメソッド呼び出しに渡す必要があります。 部分的な修正として、`id.ToString()` と `size.ToString()` を呼び出し、すべての文字列 (オブジェクト) を `string.Format` 呼び出しに渡す方法があります。 `ToString()` を呼び出すと文字列が割り当てられますが、この割り当ては `string.Format` 内部で行われます。 
  
 この基本的な `string.Format` 呼び出しは単なる文字列の連結であると考え、代わりに次のようなコードを記述することがあります。  
  
```csharp  
var s = id.ToString() + ':' + size.ToString();  
```  
  
 ただしこのコード行は <xref:System.String.Concat%28System.Object%2CSystem.Object%2CSystem.Object%29> にコンパイルされるため、ボックス化割り当てが行われます。 .NET Framework は `Concat` を呼び出すために文字リテラルをボックス化する必要があります。  
  
 **例 1 の修正**  
  
 完全な修正策は単純です。 文字リテラルを文字列リテラルに置き換えるだけです。この場合、文字列は既にオブジェクトであるためボックス化は発生しません。  
  
```csharp  
var s = id.ToString() + ":" + size.ToString();  
```  
  
 **例 2: 列挙型のボックス化**  
  
 この例では、特にディクショナリ検索操作で列挙型が頻繁に使用されることが原因で、新しい C# および Visual Basic コンパイラでメモリが大量に割り当てられます。 
  
```csharp  
public enum Color  
{  
    Red, Green, Blue  
}  
  
public class BoxingExample  
{  
    private string name;  
    private Color color;  
    public override int GetHashCode()  
    {  
        return name.GetHashCode() ^ color.GetHashCode();  
    }  
}  
```  
  
 これは、非常に微妙な問題です。 PerfView ではこれは <xref:System.Enum.GetHashCode> によるボックス化として報告されます。これは、実装上の理由からこのメソッドが列挙型の基になる表現をボックス化するためです。 PerfView で詳しく調べると、<xref:System.Enum.GetHashCode> への呼び出しごとに、ボックス化割り当てが 2 つあることが分かります。 コンパイラと .NET Framework がそれぞれ 1 つずつ挿入します。 
  
 **例 2 の修正**  
  
 両方の割り当てを容易に回避できます。このためには、<xref:System.Enum.GetHashCode> を呼び出す前に基となる表現にキャストします。  
  
```csharp  
((int)color).GetHashCode()  
```  
  
 列挙型に対するボックス化のもう 1 つの一般的な原因は、<xref:System.Enum.HasFlag%28System.Enum%29?displayProperty=nameWithType> メソッドです。 <xref:System.Enum.HasFlag%28System.Enum%29> に渡す引数をボックス化する必要があります。 ほとんどの場合、<xref:System.Enum.HasFlag%28System.Enum%29?displayProperty=nameWithType> の呼び出しをビット演算テストに置き換える方法のほうが簡単であり、また割り当てが発生しません。 
  
 パフォーマンスに関する 1 番目の事実 (不完全な最適化は行わない) に留意し、このような方法でコード全体を作成し直さないでください。 このようなボックス化にかかるコストに注意してください。また、コードの変更は、アプリのプロファイリングとホットスポットの検出の後に行ってください。 
  
### <a name="strings"></a>文字列  
 文字列操作は、割り当てが発生する最も大きな原因の 1 つであり、PerfView で上位 5 件の割り当てに表示されることがよくあります。 プログラムはシリアル化、JSON、および REST API に文字列を使用します。 列挙型を使用できない場合は、システムとの相互運用のためにプログラム定数として文字列を使用できます。 文字列がパフォーマンスに大きく影響していることがプロファイリングによって明らかになる場合は、<xref:System.String> メソッド (<xref:System.String.Format%2A>、<xref:System.String.Concat%2A>、<xref:System.String.Split%2A>、<xref:System.String.Join%2A>、<xref:System.String.Substring%2A> など) の呼び出しを探します。 <xref:System.Text.StringBuilder> を使用すると、複数の要素から 1 つの文字列を作成するコストを回避できます。ただし <xref:System.Text.StringBuilder> オブジェクトの割り当てでさえも、管理する必要があるボトルネックとなることがあります。 
  
 **例 3: 文字列操作**  
  
 C# コンパイラには、書式設定された XML Doc コメントのテキストを書き込む次のコードがありました。  
  
```csharp  
public void WriteFormattedDocComment(string text)  
{  
    string[] lines = text.Split(new[] { "\r\n", "\r", "\n" },  
                                StringSplitOptions.None);  
    int numLines = lines.Length;  
    bool skipSpace = true;  
    if (lines[0].TrimStart().StartsWith("///"))  
    {  
        for (int i = 0; i < numLines; i++)  
        {  
            string trimmed = lines[i].TrimStart();  
            if (trimmed.Length < 4 || !char.IsWhiteSpace(trimmed[3]))  
            {  
                skipSpace = false;  
                break;  
            }  
        }  
        int substringStart = skipSpace ? 4 : 3;  
        for (int i = 0; i < numLines; i++)  
            WriteLine(lines[i].TrimStart().Substring(substringStart));  
    }  
    else { /* ... */ }  
```  
  
 このコードによって大量の文字列操作が実行されていることがわかります。 このコードはライブラリ メソッドを使用して、行を個々の文字列に分割し、空白をトリミングし、引数 `text` が XML ドキュメント コメントであるかどうかを確認し、行から部分文字列を抽出します。 
  
 `WriteFormattedDocComment` 内部の最初の行では、`text.Split` が呼び出されるたびに、3 つの要素からなる新しい配列が引数として割り当てられます。 コンパイラは、この配列を割り当てるコードを毎回出力する必要があります。 これは、<xref:System.String.Split%2A> による配列の格納先で、この配列が他のコードによって変更される可能性があるかどうかをコンパイラが認識できないためです。変更される場合は、後の `WriteFormattedDocComment` 呼び出しに影響があります。 <xref:System.String.Split%2A> の呼び出しでは、`text` のすべての行に文字列が割り当てられ、操作を実行するために他のメモリが割り当てられます。 
  
 `WriteFormattedDocComment` には <xref:System.String.TrimStart%2A> メソッド呼び出しが 3 つあります。 このうちの 2 つは、処理と割り当てが重複する内側のループ内にあります。 さらに悪いことに、引数なしで <xref:System.String.TrimStart%2A> メソッドを呼び出すと、文字列結果の他に空の配列 (`params` パラメーター) が割り当てられます。 
  
 最後に、<xref:System.String.Substring%2A> メソッドが呼び出されます。このメソッドは通常、新しい文字列を割り当てます。 
  
 **例 3 の修正**  
  
 前述の例と異なり、小規模な編集ではこれらの割り当てを修正できません。 さかのぼって問題を調べ、異なる方法で対処する必要があります。 たとえば、`WriteFormattedDocComment()` の引数が、このメソッドに必要な情報がすべて含まれている文字列の場合、コードでは複数の部分文字列を割り当てる代わりに、その分多くのインデックス作成処理を実行できます。 
  
 コンパイラのパフォーマンス チームは、このような割り当てに対処するため次のようなコードを使用しました。  
  
```csharp  
private int IndexOfFirstNonWhiteSpaceChar(string text, int start) {  
    while (start < text.Length && char.IsWhiteSpace(text[start])) start++;  
    return start;  
}  
  
private bool TrimmedStringStartsWith(string text, int start, string prefix) {  
    start = IndexOfFirstNonWhiteSpaceChar(text, start);  
    int len = text.Length - start;  
    if (len < prefix.Length) return false;  
    for (int i = 0; i < len; i++)  
    {  
        if (prefix[i] != text[start + i]) return false;  
    }  
    return true;  
}  
  
// etc... 
```  
  
 `WriteFormattedDocComment()` の最初のバージョンでは、配列、複数の部分文字列、トリミングされた部分文字列と空の `params` 配列が割り当てられました。 また、"///" もチェックされています。 修正後のコードは、インデックス作成のみを使用し、割り当てを行いません。 このメソッドは、空白ではない最初の文字を検索し、文字を文字でチェックして、文字列が "///" で始まるかどうかを確認します。 新しいコードでは、<xref:System.String.TrimStart%2A> ではなく `IndexOfFirstNonWhiteSpaceChar` を使用して、空白以外の文字が出現する最初のインデックス (指定した開始インデックスの後) を返します。 この修正は完全ではありませんが、完全な解決策として類似の修正を適用する方法がわかります。 コード全体でこの方法を適用することで、`WriteFormattedDocComment()` 内のすべての割り当てを削除できます。 
  
 **例 4: StringBuilder**  
  
 この例は <xref:System.Text.StringBuilder> オブジェクトを使用します。 次の関数は、ジェネリック型の完全な型名を生成します。  
  
```csharp  
public class Example  
{  
    // Constructs a name like "SomeType<T1, T2, T3>"  
    public string GenerateFullTypeName(string name, int arity)  
    {  
        StringBuilder sb = new StringBuilder();  
  
        sb.Append(name);  
        if (arity != 0)  
        {  
            sb.Append("<");  
            for (int i = 1; i < arity; i++)  
            {  
                sb.Append("T"); sb.Append(i.ToString()); sb.Append(", ");  
            }  
            sb.Append("T"); sb.Append(i.ToString()); sb.Append(">");  
        }  
  
        return sb.ToString();  
    }  
}  
```  
  
 新しい <xref:System.Text.StringBuilder> インスタンスを作成する行に注目します。 このコードが原因で、`sb.ToString()` の割り当てと <xref:System.Text.StringBuilder> 実装内での内部割り当てが発生しますが、文字列の結果が必要な場合はこれらの割り当てを制御できません。 
  
 **例 4 の修正**  
  
 `StringBuilder` オブジェクトの割り当てを修正するには、このオブジェクトをキャッシュします。 破棄される可能性がある 1 つのインスタンスをキャッシュするだけでも、パフォーマンスが大幅に改善されることがあります。 これは関数の新しい実装であり、新しい最初の行と最後の行を除くすべてのコードを省略しています。  
  
```csharp  
// Constructs a name like "MyType<T1, T2, T3>"  
public string GenerateFullTypeName(string name, int arity)  
{  
    StringBuilder sb = AcquireBuilder();  
    /* Use sb as before */  
    return GetStringAndReleaseBuilder(sb);  
}  
```  
  
 重要なのは、新しい `AcquireBuilder()` および `GetStringAndReleaseBuilder()` 関数です。  
  
```csharp  
[ThreadStatic]  
private static StringBuilder cachedStringBuilder;  
  
private static StringBuilder AcquireBuilder()  
{  
    StringBuilder result = cachedStringBuilder;  
    if (result == null)  
    {  
        return new StringBuilder();  
    }  
    result.Clear();  
    cachedStringBuilder = null;  
    return result;  
}  
  
private static string GetStringAndReleaseBuilder(StringBuilder sb)  
{  
    string result = sb.ToString();  
    cachedStringBuilder = sb;  
    return result;  
}  
```  
  
 新しいコンパイラはスレッド処理を使用するため、このような実装は thread-static フィールド (<xref:System.ThreadStaticAttribute> 属性) を使用して <xref:System.Text.StringBuilder> をキャッシュします。このため `ThreadStatic` 宣言を使用せずに済ませることができます。 thread-static フィールドには、このコードを実行する各スレッドの一意の値が格納されます。 
  
 `AcquireBuilder()` は、キャッシュされた <xref:System.Text.StringBuilder> インスタンスがある場合はそのインスタンスを返します。その後、そのインスタンスをクリアしてフィールドまたはキャッシュを Null に設定します。 それ以外の場合は、`AcquireBuilder()` は新規インスタンスを作成して返し、フィールドまたはキャッシュは Null に設定したままにします。 
  
 <xref:System.Text.StringBuilder> の処理が完了したら、`GetStringAndReleaseBuilder()` を呼び出して文字列結果を取得し、フィールドまたはキャッシュに <xref:System.Text.StringBuilder> インスタンスを保存し、結果を返します。 実行時にこのコードに再び入り、複数の <xref:System.Text.StringBuilder> オブジェクトが作成される可能性があります (ただし複数オブジェクトが作成されることはほとんどありません)。 このコードは最後に解放された <xref:System.Text.StringBuilder> インスタンスだけを、後で使用できるように保存します。 この単純なキャッシュ対策によって、新しいコンパイラでの割り当てが大幅に減少しました。 .NET Framework と MSBuild ("MSBuild") の一部では、パフォーマンス改善のために類似の手法が使用されています。 
  
 サイズ制限があるため、この単純なキャッシュ対策は適切なキャッシュ設計に準拠しています。 ただし、元のバージョンよりもコードが増えたため、保守コストも増加します。 このキャッシュ対策を取り入れるのは、パフォーマンスの問題を検出し、PerfView で <xref:System.Text.StringBuilder> 割り当てがその問題の大きな原因であることが示されている場合だけにしてください。 
  
### <a name="linq-and-lambdas"></a>LINQ とラムダ  
ラムダ式と組み合わせた統合言語クエリ (LINQ) は、生産性機能の一例です。 ただし、その用途は、時間の経過と共にパフォーマンスに大きな影響を与える可能性があり、コードの書き直しが必要になる場合があります。
  
 **例 5: ラムダ、List\<T>、および IEnumerable\<T>**  
  
 この例では、[LINQ と関数スタイルのコード](https://docs.microsoft.com/archive/blogs/charlie/anders-hejlsberg-on-linq-and-functional-programming)を利用し、与えられた名前文字列で、コンパイラのモデルで記号を探します。  
  
```csharp  
class Symbol {  
    public string Name { get; private set; }  
    /*...*/  
}  
  
class Compiler {  
    private List<Symbol> symbols;  
    public Symbol FindMatchingSymbol(string name)  
    {  
        return symbols.FirstOrDefault(s => s.Name == name);  
    }  
}  
```  
  
 新しいコンパイラと、このコンパイラ上に構築された IDE 機能は `FindMatchingSymbol()` をかなり頻繁に呼び出します。この関数を記述する 1 行のコードには、割り当てがいくつか隠されています。 このような割り当てを調べるには、まず関数を記述する 1 行のコードを 2 行に分割します。  
  
```csharp  
Func<Symbol, bool> predicate = s => s.Name == name;  
     return symbols.FirstOrDefault(predicate);  
```  
  
 最初の行では、[ラムダ式](../../csharp/programming-guide/statements-expressions-operators/lambda-expressions.md)`s => s.Name == name` ローカル変数 `name`に[対して閉じ](https://docs.microsoft.com/archive/blogs/ericlippert/what-are-closures)ます。 つまり、このコードは `predicate` が保持している[デリゲート](../../csharp/language-reference/builtin-types/reference-types.md#the-delegate-type)にオブジェクトを割り当てる以外に、`name` の値をキャプチャする環境を保持する静的クラスを割り当てます。 コンパイラは次のようなコードを生成します。  
  
```csharp  
// Compiler-generated class to hold environment state for lambda  
private class Lambda1Environment  
{  
    public string capturedName;  
    public bool Evaluate(Symbol s)  
    {  
        return s.Name == this.capturedName;  
    }  
}  
  
// Expanded Func<Symbol, bool> predicate = s => s.Name == name;  
Lambda1Environment l = new Lambda1Environment() { capturedName = name };  
var predicate = new Func<Symbol, bool>(l.Evaluate);  
```  
  
 ここでは 2 つの `new` 割り当て (環境クラスに対する割り当てとデリゲートに対する割り当て) が明示的に行われます。 
  
 次に `FirstOrDefault`. の呼び出しを調べます。 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 型に対するこの拡張メソッドでも割り当てが発生します。 `FirstOrDefault` が <xref:System.Collections.Generic.IEnumerable%601> オブジェクトを最初の引数として受け取るため、この呼び出しを拡張して次のコードのようにできます (このコードは説明のため単純化されています)。  
  
```csharp  
// Expanded return symbols.FirstOrDefault(predicate) ... 
     IEnumerable<Symbol> enumerable = symbols;  
     IEnumerator<Symbol> enumerator = enumerable.GetEnumerator();  
     while(enumerator.MoveNext())  
     {  
         if (predicate(enumerator.Current))  
             return enumerator.Current;  
     }  
     return default(Symbol);  
```  
  
 `symbols` 変数の型は <xref:System.Collections.Generic.List%601> です。 <xref:System.Collections.Generic.List%601> コレクション型は <xref:System.Collections.Generic.IEnumerable%601> を実装し、<xref:System.Collections.Generic.IEnumerator%601> が <xref:System.Collections.Generic.List%601>を使用して実装する列挙子 (`struct` インターフェイス) を適切に定義します。 クラスの代わりに構造体を使用すると、通常ヒープ割り当てが回避されます。ヒープ割り当ては、ガベージ コレクションのパフォーマンスに影響することがあります。 通常、列挙子は言語の `foreach` ループで使用されます。このループは、コール スタックで返される列挙子構造を使用します。 オブジェクトのスペースを確保するためにコール スタック ポインターをインクリメントしても、GC はヒープ割り当てのような影響を受けません。 
  
 拡張 `FirstOrDefault` 呼び出しの場合、このコードは`GetEnumerator()` に対して <xref:System.Collections.Generic.IEnumerable%601> を呼び出す必要があります。 `symbols` を `enumerable` 型の `IEnumerable<Symbol>` 変数に割り当てると、実際のオブジェクトが <xref:System.Collections.Generic.List%601> であるという情報が失われます. つまり、コードが `enumerable.GetEnumerator()` で列挙子をフェッチするときには、.NET Framework は返される構造体をボックス化し、`enumerator` 変数に割り当てる必要があります。 
  
 **例 5 の修正**  
  
 この修正では、`FindMatchingSymbol` を次のように書き直し、1 つのコード行を 6 行のコード行に置き換えます。この 6 行のコードは簡潔であり、読んで理解しやすく、また保守が容易です。  
  
```csharp  
public Symbol FindMatchingSymbol(string name)  
    {  
        foreach (Symbol s in symbols)  
        {  
            if (s.Name == name)  
                return s;  
        }  
        return null;  
    }  
```  
  
 このコードは LINQ 拡張メソッド、ラムダ、列挙子を使用しないため、割り当ての問題は発生しません。 `symbols` コレクションが <xref:System.Collections.Generic.List%601> であることをコンパイラが認識でき、結果列挙子 (構造体) を適切な型のローカル変数にバインドしてボックス化を回避できるため、割り当てが発生しません。 この関数の元のバージョンは、C# の高い性能と .NET Framework の生産性を示す最適な例でした。 この新しく効率性が高いバージョンは、保守のために複雑なコードを追加することなく、このような品質を保持します。 
  
### <a name="async-method-caching"></a>非同期のメソッド キャッシュ  

次の例は、キャッシュされた結果を[非同期](../../csharp/programming-guide/concepts/async/index.md)メソッドで使用しようとすると発生する一般的な問題を示します。
  
 **例 6: 非同期メソッドでのキャッシュ**  
  
 新しい C# および Visual Basic コンパイラ上に構築された Visual Studio IDE 機能は、構文ツリーを頻繁にフェッチします。コンパイラは、Visual Studio の応答性を維持するために非同期を使用します。 構文ツリーを取得する目的で作成するコードの最初のバージョンを次に示します。  
  
```csharp  
class SyntaxTree { /*...*/ }  
  
class Parser { /*...*/  
    public SyntaxTree Syntax { get; }  
    public Task ParseSourceCode() { /*...*/ }  
}  
  
class Compilation { /*...*/  
    public async Task<SyntaxTree> GetSyntaxTreeAsync()  
    {  
        var parser = new Parser(); // allocation  
        await parser.ParseSourceCode(); // expensive  
        return parser.Syntax;  
    }  
}  
```  
  
 `GetSyntaxTreeAsync()` を呼び出すと `Parser` がインスタンス化され、コードが解析され、<xref:System.Threading.Tasks.Task> オブジェクトが返されることがわかります。`Task<SyntaxTree>`. コストがかかる部分は、`Parser` インスタンスの割り当てとコードの解析です。 この関数は <xref:System.Threading.Tasks.Task> を返すので、呼び出し元はユーザー入力に応答するために解析作業を待ち、UI スレッドを解放できます。 
  
 Visual Studio の複数の機能が同じ構文ツリーを取得しようとすることがあるため、解析結果をキャッシュする次のコードを記述すると、時間と割り当てを節約できます。 ただしこのコードでは割り当てが発生します。  
  
```csharp  
class Compilation { /*...*/  
  
    private SyntaxTree cachedResult;  
  
    public async Task<SyntaxTree> GetSyntaxTreeAsync()  
    {  
        if (this.cachedResult == null)  
        {  
            var parser = new Parser(); // allocation  
            await parser.ParseSourceCode(); // expensive  
            this.cachedResult = parser.Syntax;  
        }  
        return this.cachedResult;  
    }  
}  
```  
  
 キャッシュに関する新しいコードには `SyntaxTree` という名前の `cachedResult` フィールドがあります。 このフィールドが Null の場合、`GetSyntaxTreeAsync()` が処理を行い、キャッシュに結果が保存されます。 `GetSyntaxTreeAsync()` は `SyntaxTree` オブジェクトを返します。 ここで問題となるのは、`async` 型の `Task<SyntaxTree>` 関数があり、`SyntaxTree` 型の値を返す場合、コンパイラが、(`Task<SyntaxTree>.FromResult()` を使用して) 結果を保持するために Task を割り当てるコードを出力することです。 Task は完了済みとしてマークされ、結果が即時に利用可能になります。 新しいコンパイラのコードでは、既に完了している <xref:System.Threading.Tasks.Task> オブジェクトが頻繁に発生するため、このような割り当てを修正すると応答性が著しく向上することがよくありました。 
  
 **例 6 の修正**  
  
 完了した <xref:System.Threading.Tasks.Task> の割り当てを削除するには、完了した結果を含むタスクオブジェクトをキャッシュします。  
  
```csharp  
class Compilation { /*...*/  
  
    private Task<SyntaxTree> cachedResult;  
  
    public Task<SyntaxTree> GetSyntaxTreeAsync()  
    {  
        return this.cachedResult ??   
               (this.cachedResult = GetSyntaxTreeUncachedAsync());  
    }  
  
    private async Task<SyntaxTree> GetSyntaxTreeUncachedAsync()  
    {  
        var parser = new Parser(); // allocation  
        await parser.ParseSourceCode(); // expensive  
        return parser.Syntax;  
    }  
}  
```  
  
 このコードは `cachedResult` の型を `Task<SyntaxTree>` に変更し、`async` からの元のコードを保持する `GetSyntaxTreeAsync()` ヘルパー関数を採用しています。 `GetSyntaxTreeAsync()` は [null 合体演算子](../../csharp/language-reference/operators/null-coalescing-operator.md)を使用して、`cachedResult` が null でない場合にそれを返します。 `cachedResult` が null の場合、`GetSyntaxTreeAsync()` は `GetSyntaxTreeUncachedAsync()` を呼び出し、結果をキャッシュします。 `GetSyntaxTreeAsync()` は、通常のコードのようには、`GetSyntaxTreeUncachedAsync()` 呼び出しを待たないことに注意してください。 待機しないということは、`GetSyntaxTreeUncachedAsync()` がその <xref:System.Threading.Tasks.Task> オブジェクトを返す場合、`GetSyntaxTreeAsync()` が即時に<xref:System.Threading.Tasks.Task> を返すことになります。 この時点で、キャッシュされた結果は <xref:System.Threading.Tasks.Task> であるため、キャシュされた結果を返すための割り当ては行われません。 
  
### <a name="additional-considerations"></a>その他の考慮事項  
 大きなアプリまたは大量データを処理するアプリで発生する可能性がある問題に関するその他の点を次に説明します。 
  
 **ディクショナリ**  
  
 ディクショナリは多くのプログラムで随所に使用されており、非常に便利で本質的に効率的です。 ただし、使い方が不適切なことがよくあります。 Visual Studio と新しいコンパイラでは、分析から、多くのディクショナリでは含まれている要素が 1つだけであるか、何も含まれていないことが判明しました。 x86 マシンでは、空の <xref:System.Collections.Generic.Dictionary%602> には 10 個のフィールドがあり、ヒープで 48 バイトを占有しています。 ディクショナリが役立つのは、一定時間内の検索のマッピングまたは関連データ構造体が必要な場合です。 ただし要素の数が少ない場合は、ディクショナリを使用するとスペースを無駄に使用することになります。 代わりに、たとえば `List<KeyValuePair\<K,V>>` の繰り返し検索も、同じ速度で行えます。 ディクショナリにデータを読み込み、ディクショナリから読み取るだけのためにディクショナリを使用する場合 (非常に一般的なパターン)、N(log(N)) ルックアップで並べ替えた配列を使用すると、使用している要素の数に応じて、ほぼ同程度の速度を得ることができます。 
  
 **クラスと構造体**  
  
 クラスと構造体は、アプリを調整する上で従来型のスペース/時間のトレードオフを生じさせます。 フィールドが含まれていないクラスでは、x86 マシンで 12 バイトのオーバーヘッドが生じますが、クラス インスタンスを指すポインタをだけを受け取るので、クラスの受け渡しにかかるコストは低くなります。 ボックス化されていない構造体の場合、ヒープ割り当ては行われませんが、大きな構造体を関数の引数または戻り値として渡すと、構造体のすべてのデータ メンバーをアトミックにコピーするために、CPU 時間がかかります。 構造体を返すプロパティ呼び出しの繰り返しに注意し、過剰なデータ コピーを行わないようにするため、プロパティの値をローカル変数にキャッシュします。 
  
 **キャッシュ**  
  
 一般的なパフォーマンス向上のためのテクニックは、結果をキャッシュすることです。 ただし、サイズ制限や破棄ポリシーが設定されていないキャッシュはメモリ リークとなる可能性があります。 大量のデータを処理するときに、キャッシュに大量のメモリを維持すると、ガベージ コレクションによって、キャッシュしたルックアップによるメリットがオーバーライドされます。 
  
 この記事では、特に大規模システムや大量のデータを処理するシステムにおいて、アプリの応答性に影響する可能性があるパフォーマンスのボトルネックの症状にどのように注意したらよいかを説明しました。 一般的な問題には、ボックス化、文字列操作、LINQ およびラムダ、非同期方式でのキャッシュ、サイズ制限または破棄ポリシーのないキャッシュ、不適切なディクショナリの使用、構造体の受け渡しなどがあります。 アプリの調整に関する 4 つの事実に注意してください。  
  
- 不完全な最適化は行わない - 生産的に作業し、問題を見つけたらアプリを調整します。 
  
- プロファイルは嘘をつかない - 測定していないのであれば、推測にすぎません。 
  
- 優れたツールには大きな効果がある - PerfView をダウンロードして試してみてください。 
  
- すべては割り当てで決まる - コンパイラ プラットフォーム チームは、新しいコンパイラのパフォーマンスの向上のため、この分野に最も多くの時間をかけました。 
  
## <a name="see-also"></a>関連項目

- [このトピックのプレゼンテーションビデオ](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B333)
- [パフォーマンス プロファイリングのビギナーズ ガイド](/visualstudio/profiling/beginners-guide-to-performance-profiling)
- [パフォーマンス](index.md)
- [.NET のパフォーマンスに関するヒント](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973839(v%3dmsdn.10))
- [Channel 9 PerfView のチュートリアル](https://channel9.msdn.com/Series/PerfView-Tutorial)
- [.NET Compiler Platform SDK](../../csharp/roslyn-sdk/index.md)
- [GitHub の dotnet/roslyn リポジトリ](https://github.com/dotnet/roslyn)
