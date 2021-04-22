---
title: コード コントラクト
description: 事前条件、事後条件、およびオブジェクト不変条件を .NET コードで指定できるコード コントラクトについて説明します。
ms.date: 09/05/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Code contracts
ms.assetid: 84526045-496f-489d-8517-a258cf76f040
ms.openlocfilehash: 60f794373af75bd3f745c224e0a8c7a84192e4c4
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904144"
---
# <a name="code-contracts"></a>コード コントラクト

コード コントラクトを使用すると、事前条件、事後条件、およびオブジェクト不変条件をコードで指定できます。 事前条件とは、メソッドやプロパティに入るときに満たされている必要がある要件です。 事後条件は、メソッドやプロパティのコードが終了するときの予測を表します。 オブジェクト不変条件は、正しい状態のクラスに対して予期される状態を表します。

コード コントラクトには、コードをマーク付けするためのクラス、コンパイル時の分析のための静的アナライザー、およびランタイム アナライザーが含まれます。 コード コントラクトのクラスは <xref:System.Diagnostics.Contracts> 名前空間にあります。

コード コントラクトには次のような利点があります。

- テストの強化: コード コントラクトでは、コントラクトの静的検証、ランタイム チェック、およびドキュメントの生成がサポートされます。

- 自動テスト ツール: コード コントラクトを使用すると、事前条件を満たさない無意味なテスト引数をフィルターで除外して、より意味のある単体テストを生成できます。

- 静的検証: 静的チェックにより、コントラクト違反がないかどうかをプログラムを実行せずに確認できます。 暗黙的なコントラクト (null の逆参照、配列の範囲など) と明示的なコントラクトがチェックされます。

- リファレンス ドキュメント: ドキュメント生成機能により、既存の XML ドキュメント ファイルにコントラクトの情報が追加されます。 生成されるドキュメントのページにコントラクトのセクションが含まれるようにするための、[Sandcastle](https://github.com/EWSoftware/SHFB) で使用できるスタイル シートも用意されています。

コントラクトは、すべての .NET Framework 言語ですぐに使用できます。特別なパーサーやコンパイラを記述する必要はありません。 実行するコード コントラクト分析のレベルを指定できる Visual Studio アドインや、 コントラクトの形式が正しいかどうかを確認したり (型チェックおよび名前解決)、コンパイルされた形式 (MSIL (Microsoft Intermediate Language) 形式) のコントラクトを生成したりできるアナライザーも用意されています。 Visual Studio でコントラクトを作成する場合は、Visual Studio の標準の IntelliSense も利用できます。

コントラクト クラスのほとんどのメソッドは、条件付きでコンパイルされます。したがって、それらのメソッドの呼び出しは、`#define` ディレクティブを使用して CONTRACTS_FULL という特別なシンボルを定義した場合にのみコンパイラで生成されます。 CONTRACTS_FULL を使用すると、コードで `#ifdef` ディレクティブを使用せずにコントラクトを記述して、コントラクトを含むものと含まないものなど、さまざまなビルドを生成できます。

コード コントラクトを使用するためのツールおよび詳細な手順については、Visual Studio Marketplace サイトの [Code Contracts](https://marketplace.visualstudio.com/items?itemName=RiSEResearchinSoftwareEngineering.CodeContractsforNET) を参照してください。

## <a name="preconditions"></a>Preconditions

事前条件を指定するには、<xref:System.Diagnostics.Contracts.Contract.Requires%2A?displayProperty=nameWithType> メソッドを使用します。 事前条件では、メソッドが呼び出される状態を指定します。 通常は、有効なパラメーター値を指定するために使用されます。 事前条件で参照されるすべてのメンバーは、アクセス レベルが少なくともメソッド自体と同じである必要があります。そうでない場合、メソッドのすべての呼び出し元がその事前条件を理解できない場合があります。 また、条件に副作用がないようにする必要もあります。 事前条件が満たされなかった場合の実行時の動作は、ランタイム アナライザーによって決定されます。

たとえば、次の事前条件は、パラメーター `x` が null 以外である必要があることを表しています。

```csharp
Contract.Requires(x != null);
```

事前条件が満たされなかった場合に特定の例外をスローする必要がある場合には、次のように、<xref:System.Diagnostics.Contracts.Contract.Requires%2A> のジェネリック オーバーロードを使用します。

```csharp
Contract.Requires<ArgumentNullException>(x != null, "x");
```

### <a name="legacy-requires-statements"></a>レガシ requires ステートメント

ほとんどのコードには、`if`-`then`-`throw` コードの形式のパラメーター検証が含まれています。 以下の場合は、それらのステートメントがコントラクト ツールで事前条件として認識されます。

- それらのステートメントがメソッド内で他のステートメントより前にある場合。

- それらのステートメント全体の後に <xref:System.Diagnostics.Contracts.Contract> メソッドの明示的な呼び出し (<xref:System.Diagnostics.Contracts.Contract.Requires%2A>、<xref:System.Diagnostics.Contracts.Contract.Ensures%2A>、<xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A>、または <xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A> のいずれかのメソッドの呼び出しなど) がある場合。

`if`-`then`-`throw` ステートメントがこのような形式になっている場合、それらのステートメントはレガシ `requires` ステートメントとして認識されます。 `if`-`then`-`throw` というシーケンスの後に他のコントラクトがない場合は、<xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A?displayProperty=nameWithType> メソッドでコードを終了します。

```csharp
if (x == null) throw new ...
Contract.EndContractBlock(); // All previous "if" checks are preconditions
```

上のテストの条件は否定の事前条件であることに注意してください。 (実際の事前条件は `x != null` です)。否定の事前条件には多くの制限があり、上の例のように記述する必要があります (`else` 句が含まれていてはならない、`then` 句の本体は 1 つの `throw` ステートメントでなければならないなど)。 `if` テストには純粋性と可視性の両方の規則が適用されますが (「[使用方法のガイドライン](#usage_guidelines)」を参照)、`throw` 式に適用されるのは純粋性の規則だけです。 ただし、スローされる例外の型には、そのコントラクトが存在するメソッドと同じレベルの可視性が必要です。

## <a name="postconditions"></a>実行後の状態

事後条件は、メソッドの終了時の状態についてのコントラクトで、 メソッドが終了する直前にチェックされます。 事後条件が満たされなかった場合の実行時の動作は、ランタイム アナライザーによって決定されます。

事後条件では、事前条件とは違って、可視性のレベルがメソッドより低いメンバーも参照できます。 事後条件でプライベートの状態を使用して指定した情報は、クライアントが理解できなかったり利用できなかったりする場合もありますが、そのためにクライアントがメソッドを正しく使用できなくなることはありません。

### <a name="standard-postconditions"></a>標準の事後条件

標準の事後条件を指定するには、<xref:System.Diagnostics.Contracts.Contract.Ensures%2A> メソッドを使用します。 事後条件は、メソッドが正常に終了した場合に `true` になる必要がある条件を表します。

```csharp
Contract.Ensures(this.F > 0);
```

### <a name="exceptional-postconditions"></a>例外の事後条件

例外の事後条件とは、メソッドによって特定の例外がスローされた場合に `true` になる必要がある事後条件です。 これらの事後条件を指定するには、次の例で示されているように、<xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A?displayProperty=nameWithType> メソッドを使用します。

```csharp
Contract.EnsuresOnThrow<T>(this.F > 0);
```

引数は、`T` のサブタイプである例外がスローされた場合に `true` になる必要がある条件です。

例外型の中には、例外の事後条件で使用するのが困難なものもあります。 たとえば、`T` に対して <xref:System.Exception> 型を使用する場合は、指定する条件が、スローされる例外の型に関係なく (スタック オーバーフローなどの制御不可能な例外の場合でも) メソッドによって保証される必要があります。 例外の事後条件は、メンバーが呼び出されたときにスローされる可能性がある特定の例外に対してのみ使用するようにしてください (<xref:System.TimeZoneInfo> メソッドの呼び出しに対して <xref:System.InvalidTimeZoneException> がスローされる場合など)。

### <a name="special-postconditions"></a>特殊な事後条件

次のメソッドは、事後条件の中でのみ使用できます。

- 事後条件でメソッドの戻り値を参照するには、`Contract.Result<T>()` という式を使用します。`T` はメソッドの戻り値の型に置き換えられます。 コンパイラが型を推論できない場合は明示的に指定する必要があります。 たとえば、C# コンパイラでは、引数を受け取らないメソッドの型は推論できないため、`Contract.Ensures(0 <Contract.Result<int>())` という事後条件を使用する必要があります。戻り値の型が `void` のメソッドの事後条件では `Contract.Result<T>()` を参照できません。

- 事後条件の前の状態の値は、メソッドまたはプロパティの開始時の式の値を表します。 使用される式は `Contract.OldValue<T>(e)` で、`T` は `e` の型です。 その型をコンパイラが推論できる場合は、このジェネリック型引数を省略できます (たとえば、この式は引数を受け取るため、C# コンパイラでは常に型が推論されます)。`e` に使用できる値と、古い式を使用できるコンテキストについては、いくつかの制限があります。 まず、古い式に別の古い式を含めることはできません。 また、最も重要な点として、古い式で参照する値は、メソッドの事前条件の状態に存在する必要があります。 つまり、古い式は、メソッドの事前条件が `true` であれば評価できる式である必要があります。 この規則のいくつかの例を次に示します。

  - 値がメソッドの事前条件の状態に存在する必要があります。 オブジェクトのフィールドを参照するには、オブジェクトが常に null 以外であることが事前条件によって保証される必要があります。

  - 古い式でメソッドの戻り値を参照することはできません。

      ```csharp
      Contract.OldValue(Contract.Result<int>() + x) // ERROR
      ```

  - 古い式で `out` パラメーターを参照することはできません。

  - 量指定子の範囲がメソッドの戻り値に依存する場合は、量指定子のバインド変数に依存する古い式は使用できません。

      ```csharp
      Contract.ForAll(0, Contract.Result<int>(), i => Contract.OldValue(xs[i]) > 3); // ERROR
      ```

  - 古い式で <xref:System.Diagnostics.Contracts.Contract.ForAll%2A> または <xref:System.Diagnostics.Contracts.Contract.Exists%2A> の呼び出しの匿名デリゲートのパラメーターを参照することはできません (メソッド呼び出しのインデクサーまたは引数として使用されている場合を除く)。

      ```csharp
      Contract.ForAll(0, xs.Length, i => Contract.OldValue(xs[i]) > 3); // OK
      Contract.ForAll(0, xs.Length, i => Contract.OldValue(i) > 3); // ERROR
      ```

  - 古い式の値が匿名デリゲートのパラメーターに依存している場合、古い式をその匿名デリゲートの本体に含めることはできません (匿名デリゲートが <xref:System.Diagnostics.Contracts.Contract.ForAll%2A> メソッドまたは <xref:System.Diagnostics.Contracts.Contract.Exists%2A> メソッドの引数である場合を除く)。

      ```csharp
      Method(... (T t) => Contract.OldValue(... t ...) ...); // ERROR
      ```

  - コントラクトはメソッド本体の前にあるため、`Out` パラメーターは問題になります。ほとんどのコンパイラでは、事後条件で `out` パラメーターを参照することは許可されていません。 この問題を解決するために、<xref:System.Diagnostics.Contracts.Contract> クラスには <xref:System.Diagnostics.Contracts.Contract.ValueAtReturn%2A> メソッドが用意されています。このメソッドを使用すると、`out` パラメーターに基づく事後条件を使用できます。

      ```csharp
      public void OutParam(out int x)
      {
          Contract.Ensures(Contract.ValueAtReturn(out x) == 3);
          x = 3;
      }
      ```

      <xref:System.Diagnostics.Contracts.Contract.OldValue%2A> メソッドと同様に、コンパイラが型を推論できる場合はジェネリック型パラメーターを省略できます。 このメソッドの呼び出しは、コントラクト リライターによって `out` パラメーターの値に置き換えられます。 <xref:System.Diagnostics.Contracts.Contract.ValueAtReturn%2A> メソッドは事後条件でしか使用できません。 このメソッドの引数は、`out` パラメーターか、構造体の `out` パラメーターのフィールド (構造体コンストラクターの事後条件でフィールドを参照する場合にも便利です) でなければなりません。

      > [!NOTE]
      > 現在、コード コントラクト分析ツールでは、`out` パラメーターが正しく初期化されているかどうかはチェックされません。事後条件に含まれていても無視されます。 したがって、前の例で、コントラクトの後の行で `x` に整数を代入せずにこの値をそのまま使用しても、コンパイラで適切なエラーが生成されません。 ただし、CONTRACTS_FULL プリプロセッサ シンボルが定義されていないビルド (リリース ビルドなど) では、コンパイラでエラーが生成されます。

## <a name="invariants"></a>インバリアント

オブジェクト不変条件とは、そのオブジェクトがクライアントに表示される場合に常にクラスの各インスタンスに対して true になる必要があり、 オブジェクトが正しいと見なされる条件を表します。

インバリアントなメソッドは、<xref:System.Diagnostics.Contracts.ContractInvariantMethodAttribute> 属性でマークされることによって識別されます。 インバリアントなメソッドには、<xref:System.Diagnostics.Contracts.Contract.Invariant%2A> メソッドの一連の呼び出し以外のコードが含まれていない必要があります。それらの呼び出しでは、次の例のように、個々の不変式を指定します。

```csharp
[ContractInvariantMethod]
protected void ObjectInvariant ()
{
    Contract.Invariant(this.y >= 0);
    Contract.Invariant(this.x > this.y);
    ...
}
```

不変式は、CONTRACTS_FULL プリプロセッサ シンボルによって条件付きで定義され、 ランタイム チェックで各パブリック メソッドの最後にチェックされます。 不変式が同じクラスのパブリック メソッドを参照している場合は、通常ならそのパブリック メソッドの最後に実行される不変式のチェックが無効になり、 そのクラスの一番外側のメソッド呼び出しの最後にのみチェックが実行されます。 別のクラスのメソッドの呼び出しのためにクラスへの再入がなされる場合も同様です。 オブジェクト ファイナライザーと <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> の実装については、インバリアントはチェックされません。

<a name="usage_guidelines"></a>

## <a name="usage-guidelines"></a>使用方法のガイドライン

### <a name="contract-ordering"></a>コントラクトの順序

メソッドのコントラクトを記述する際に使用する必要がある要素の順序を次の表に示します。

|`If-then-throw statements`|下位互換性のあるパブリックな事前条件。|
|-|-|
|<xref:System.Diagnostics.Contracts.Contract.Requires%2A>|すべてのパブリックな事前条件。|
|<xref:System.Diagnostics.Contracts.Contract.Ensures%2A>|すべてのパブリックな (標準の) 事後条件。|
|<xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A>|すべてのパブリックな例外の事後条件。|
|<xref:System.Diagnostics.Contracts.Contract.Ensures%2A>|すべてのプライベートな/内部の (標準の) 事後条件。|
|<xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A>|すべてのプライベートな/内部の例外の事後条件。|
|<xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A>|`if`-`then`-`throw` スタイルの事前条件を他のコントラクトなしで使用する場合は、<xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A> を呼び出して、前の if チェックがすべて事前条件であることを示します。|

<a name="purity"></a>

### <a name="purity"></a>純粋性

コントラクトの中で呼び出されるメソッドは、すべて純粋である (既存の状態を更新しない) 必要があります。 メソッドに入った後に作成されたオブジェクトは、そのピュア メソッドによって変更できます。

コード コントラクト ツールでは、現在、次のコード要素が純粋と見なされます。

- <xref:System.Diagnostics.Contracts.PureAttribute> でマークされたメソッド。

- <xref:System.Diagnostics.Contracts.PureAttribute> でマークされた型 (この属性はその型のすべてのメソッドに適用されます)。

- プロパティの get アクセサー。

- 演算子 (名前が "op" で始まり、1 つか 2 つのパラメーターを持ち、戻り値の型が void ではない静的メソッド)。

- 完全修飾名が "System.Diagnostics.Contracts.Contract"、"System.String"、"System.IO.Path"、または "System.Type" で始まるメソッド。

- 呼び出されたデリゲート (デリゲート型自体に <xref:System.Diagnostics.Contracts.PureAttribute> 属性が設定されている場合)。 デリゲート型の <xref:System.Predicate%601?displayProperty=nameWithType> と <xref:System.Comparison%601?displayProperty=nameWithType> は純粋と見なされます。

<a name="visibility"></a>

### <a name="visibility"></a>表示

コントラクトで参照されるすべてのメンバーには、少なくとも親のメソッドと同じレベルの可視性が必要です。 たとえば、パブリック メソッドの事前条件でプライベート フィールドを参照することはできません。そのようなコントラクトは、クライアントがメソッドの呼び出しの前に検証できません。 ただし、そのフィールドが <xref:System.Diagnostics.Contracts.ContractPublicPropertyNameAttribute> でマークされている場合は、これらの規則から除外されます。

## <a name="example"></a>例

次に、コード コントラクトの使用例を示します。

[!code-csharp[ContractExample#1](../../../samples/snippets/csharp/VS_Snippets_CLR/contractexample/cs/program.cs#1)]
[!code-vb[ContractExample#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/contractexample/vb/program.vb#1)]
