---
title: C# のコーディング規則 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- coding conventions, C#
- Visual C#, coding conventions
- C# language, coding conventions
ms.assetid: f4f60de9-d49b-4fb6-bab1-20e19ea24710
ms.openlocfilehash: 77b173a420f26834855e0bdca3c8d04406ac65d4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398377"
---
# <a name="c-coding-conventions-c-programming-guide"></a>C# のコーディング規則 (C# プログラミング ガイド)

コーディング規則には、次の目的があります。  
  
- コードの見た目が統一されるため、コードを読むときに、レイアウトではなく内容に重点を置くことができます。  
  
- これにより、経験に基づいて推測することで、コードをより迅速に理解することができます。  
  
- コードのコピー、変更、および保守が容易になります。  
  
- コーディング規約により、C# のベスト プラクティスがわかります。  

この記事のガイドラインは、サンプルおよびドキュメントを開発するために Microsoft によって使用されます。  
  
## <a name="naming-conventions"></a>命名規則  
  
- [using ディレクティブ](../../language-reference/keywords/using-directive.md)が含まれていない簡単な例では、名前空間の修飾を使用します。 プロジェクトに名前空間が既定でインポートされていることがわかっている場合は、その名前空間の各名前を完全修飾する必要はありません。 次の例に示すように、修飾名が長すぎて 1 行に収まらない場合は、ドット (.) の後で改行できます。  
  
     [!code-csharp[csProgGuideCodingConventions#1](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#1)]  
  
- 他のガイドラインに合わせて、Visual Studio デザイナーのツールを使用して作成されたオブジェクトの名前を変更する必要はありません。  
  
## <a name="layout-conventions"></a>レイアウト規則  

コードの構造を強調する書式が使用され、コードが読みやすくなっているのが、優れたレイアウトです。 マイクロソフトの例とサンプルは、次の規則に準拠しています。  
  
- コード エディターの既定の設定 (スマートインデント、4 文字インデント、タブを空白として保存) を使用します。 詳細については、「[[オプション]、[テキスト エディター]、[C#]、[書式設定]](/visualstudio/ide/reference/options-text-editor-csharp-formatting)」を参照してください。  
  
- 1 つの行には 1 つのステートメントのみを記述します。  
  
- 1 つの行には 1 つの宣言のみを記述します。  
  
- 継続行にインデントが自動的に設定されない場合は、1 タブストップ (4 つの空白) 分のインデントを設定します。  
  
- メソッド定義とプロパティ定義の間に少なくとも 1 行の空白行を追加します。  
  
- 次のコードに示すように、式に句を作成するときはかっこを使用します。  
  
     [!code-csharp[csProgGuideCodingConventions#2](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#2)]  
  
## <a name="commenting-conventions"></a>コメント規則  
  
- コメントは、コード行の末尾ではなく別の行に記述します。  
  
- コメントのテキストは大文字で開始します。  
  
- コメントのテキストはピリオドで終了します。  
  
- 次の例に示すように、コメント デリミター (//) とコメント テキストの間に空白を 1 つ挿入します。  
  
     [!code-csharp[csProgGuideCodingConventions#3](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#3)]  
  
- アスタリスクを整形したブロックでコメントを囲まないようにします。  
  
## <a name="language-guidelines"></a>言語ガイドライン  

以降のセクションでは、コード例とサンプルを準備する際に C# チームが従っている方法について説明します。  
  
### <a name="string-data-type"></a>文字列型 (String)  
  
- 次のコードに示すように、短い文字列を連結するときは[文字列補間](../../language-reference/tokens/interpolated.md)を使用します。  
  
     [!code-csharp[csProgGuideCodingConventions#6](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#6)]  
  
- ループ内で文字列を追加する場合 (特に大量のテキストを処理する場合) は、<xref:System.Text.StringBuilder> オブジェクトを使用します。  
  
     [!code-csharp[csProgGuideCodingConventions#7](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#7)]  
  
### <a name="implicitly-typed-local-variables"></a>暗黙的に型指定されるローカル変数  
  
- 変数の型が割り当ての右側から明らかである場合、または厳密な型が重要でない場合は、ローカル変数の[暗黙の型指定](../classes-and-structs/implicitly-typed-local-variables.md)を使用します。  
  
     [!code-csharp[csProgGuideCodingConventions#8](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#8)]  
  
- 割り当ての右側から型が明らかではない場合、[var](../../language-reference/keywords/var.md) を使用しないでください。  
  
     [!code-csharp[csProgGuideCodingConventions#9](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#9)]  
  
- 変数の型を指定するときに変数名に頼らないでください。 変数名が正しくない場合があります。  
  
     [!code-csharp[csProgGuideCodingConventions#10](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#10)]  
  
- `var`dynamic[ の代わりに ](../../language-reference/builtin-types/reference-types.md) を使用しないようにしてください。  
  
- [for](../../language-reference/keywords/for.md) ループでループ変数の型を決定するときは、暗黙の型指定が使用されます。  
  
     次の例では、`for` ステートメントで暗黙の型指定を使用しています。  
  
     [!code-csharp[csProgGuideCodingConventions#7](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#7)]  

- [foreach](../../language-reference/keywords/foreach-in.md) ループでループ変数の型を決定するときは、暗黙の型指定は使用されません。

     次の例では、`foreach` ステートメントで暗黙の型指定が使用されています。

     [!code-csharp[csProgGuideCodingConventions#12](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#12)]

     > [!NOTE]
     > 反復可能コレクションの要素の型を誤って変更しないように注意してください。 たとえば、<xref:System.Linq.IQueryable?displayProperty=nameWithType> ステートメントで <xref:System.Collections.IEnumerable?displayProperty=nameWithType> から `foreach` に切り替えるのは簡単ですが、これを行うとクエリの結果が変更されます。

### <a name="unsigned-data-type"></a>Unsigned データ型  
  
通常は、unsigned 型ではなく `int` を使用します。 C# では `int` を使用するのが一般的です。`int` を使用すると、他のライブラリと対話しやすくなります。  
  
### <a name="arrays"></a>配列  
  
宣言行で配列を初期化するときは簡潔な構文を使用します。  
  
[!code-csharp[csProgGuideCodingConventions#13](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#13)]  
  
### <a name="delegates"></a>デリゲート  
  
デリゲート型のインスタンスを作成するときは簡潔な構文を使用します。  
  
[!code-csharp[csProgGuideCodingConventions#14](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#14)]  
  
[!code-csharp[csProgGuideCodingConventions#15](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#15)]  
  
### <a name="try-catch-and-using-statements-in-exception-handling"></a>例外処理における try-catch ステートメントと using ステートメント  
  
- ほとんどの例外処理には、[try-catch](../../language-reference/keywords/try-catch.md) ステートメントを使用します。  
  
     [!code-csharp[csProgGuideCodingConventions#16](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#16)]  
  
- C# の [using ステートメント](../../language-reference/keywords/using-statement.md)を使用して、コードを簡潔にします。 [try-finally](../../language-reference/keywords/try-finally.md) ステートメントを使用するときに `finally` ブロックのコードが <xref:System.IDisposable.Dispose%2A> メソッドの呼び出しだけである場合は、`using` ステートメントを代わりに使用します。  
  
     [!code-csharp[csProgGuideCodingConventions#17](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#17)]  
  
### <a name="-and-124124-operators"></a>&& 演算子および &#124;&#124; 演算子  
  
例外を回避し、不要な比較をスキップしてパフォーマンスを向上させるには、比較を実行する場合、次の例に示すように [&&](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-and-operator-) の代わりに [&](../../language-reference/operators/boolean-logical-operators.md#logical-and-operator-) を、[&#124;](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-or-operator-) の代わりに [&#124;&#124;](../../language-reference/operators/boolean-logical-operators.md#logical-or-operator-) を使用します。  
  
[!code-csharp[csProgGuideCodingConventions#18](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#18)]  
  
### <a name="new-operator"></a>new 演算子  
  
- 次の宣言に示すように、暗黙の型指定を使用してオブジェクトのインスタンス化を簡潔な形式にします。  
  
     [!code-csharp[csProgGuideCodingConventions#19](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#19)]  
  
     前の行は次の宣言に相当します。  
  
     [!code-csharp[csProgGuideCodingConventions#20](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#20)]  
  
- オブジェクト初期化子を使用してオブジェクトの作成を簡略化します。  
  
     [!code-csharp[csProgGuideCodingConventions#21](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#21)]  
  
### <a name="event-handling"></a>イベント処理  
  
後で削除する必要のないイベント ハンドラーを定義する場合は、ラムダ式を使用します。  
  
[!code-csharp[csProgGuideCodingConventions#22](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#22)]  
  
[!code-csharp[csProgGuideCodingConventions#23](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#23)]  
  
### <a name="static-members"></a>静的メンバー  
  
[静的](../../language-reference/keywords/static.md)メンバーは、クラス名 (*ClassName.StaticMember*) を使用して呼び出します。 こうすることで、静的アクセスが明確になり、コードがよりわかりやすくなります。  派生クラスの名前を持つ基本クラスに定義された静的メンバーを指定しないでください。  このコードをコンパイルすると、コードが読みやすくなくなり、派生クラスに同じ名前の静的メンバーを追加すると、将来的にコードが中断する場合があります。  
  
### <a name="linq-queries"></a>LINQ クエリ  
  
- クエリ変数にはわかりやすい名前を使用します。 次の例では、シアトル在住の顧客に `seattleCustomers` を使用しています。  
  
     [!code-csharp[csProgGuideCodingConventions#25](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#25)]  
  
- エイリアスを使用して、匿名型のプロパティ名の大文字と小文字の使用が正しい Pascal 形式になるようにします。  
  
     [!code-csharp[csProgGuideCodingConventions#26](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#26)]  
  
- 結果のプロパティ名があいまいになる場合は、プロパティ名を変更します。 たとえば、クエリで顧客名と販売店 ID を返す場合、クエリ結果で `Name` と `ID` をそのまま使用するのではなく、これらの名前を変更し、`Name` が顧客の名前であり、`ID` が販売店の ID であることを明確にします。  
  
     [!code-csharp[csProgGuideCodingConventions#27](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#27)]  
  
- クエリ変数と範囲変数の宣言で暗黙の型指定を使用します。  
  
     [!code-csharp[csProgGuideCodingConventions#25](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#25)]  
  
- 前の例に示すように、クエリ句を [from](../../language-reference/keywords/from-clause.md) 句の下に配置します。  
  
- [where](../../language-reference/keywords/where-clause.md) 句を他のクエリ句より先に使用し、それ以降のクエリ句では、フィルター化されたデータセットが処理されるようにします。  
  
     [!code-csharp[csProgGuideCodingConventions#29](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#29)]  
  
- 内部コレクションにアクセスするには、`from`join[ 句ではなく複数の ](../../language-reference/keywords/join-clause.md) 句を使用します。 たとえば、`Student` オブジェクトのコレクションがあり、各オブジェクトに試験の点数のコレクションが含まれているとします。 次のクエリを実行すると、90 点より高い点数とその点数を取った学生の姓が返されます。  
  
     [!code-csharp[csProgGuideCodingConventions#30](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#30)]  
  
## <a name="security"></a>Security  

「[安全なコーディングのガイドライン](../../../standard/security/secure-coding-guidelines.md)」のガイドラインに従ってください。  
  
## <a name="see-also"></a>参照

- [Visual Basic のコーディング規則](../../../visual-basic/programming-guide/program-structure/coding-conventions.md)
- [安全なコーディングのガイドライン](../../../standard/security/secure-coding-guidelines.md)
