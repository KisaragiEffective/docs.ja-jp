---
title: 破壊的変更の種類
description: .NET により開発者のために .NET のバージョン間での互換性が維持される方法、および破壊的変更と見なされる変更の種類について説明します。
ms.date: 01/28/2021
ms.topic: conceptual
ms.openlocfilehash: a1a55a95cedca4aecba875bb44b952173327606d
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740619"
---
# <a name="changes-that-affect-compatibility"></a>互換性に影響を与える変更点

.NET の歴史を通じて、.NET のバージョン間および異なる実装間で高いレベルの互換性を維持することに力が注がれてきました。 .NET 5 (および .NET Core) 以降のバージョンは、.NET Framework と比較すると新しいテクノロジと見なすことができますが、2 つの大きな要因により、.NET のこの実装が .NET Framework と大きく異なるものになることはできません。

- 開発者の多くは、.NET Framework アプリケーションをもともと開発していたか、現在も開発を続けています。 .NET の実装間には一貫した動作が期待されます。

- .NET Standard ライブラリ プロジェクトを使用すると、開発者は .NET Framework と .NET 5 (および .NET Core) 以降のバージョンで共有される共通の API をターゲットとするライブラリを作成できます。 開発者は、.NET 5 アプリケーションで使用されるライブラリが、.NET Framework アプリケーションで使用される同じライブラリと同じように動作するはずであると期待します。

.NET の実装間の互換性と共に、開発者は .NET の特定の実装のバージョン間についても高いレベルの互換性を期待します。 特に、以前のバージョンの .NET Core 用に書かれたコードは、.NET 5 以降のバージョンでもシームレスに動作する必要があります。 実際、多くの開発者は、新しくリリースされたバージョンの .NET にある新しい API が、それらの API が導入されたプレリリース バージョンとも互換性があると期待します。

この記事では、互換性に影響する変更と、.NET チームが各種の変更を評価する方法について説明します。 .NET チームが考えられる破壊的変更にどのように対処しているかの理解は、特に[既存の .NET API](https://github.com/dotnet/runtime) の動作を変更する pull request をオープンする開発者に役立ちます。

以下のセクションでは、.NET API に対して行われた変更のカテゴリと、それがアプリケーションの互換性に与える影響について説明します。 変更は許可 ✔️ されているか、未許可 ❌ であるか、または以前の動作の予測可能性、明確性、一貫性の程度の判断と評価が必要 ❓ であるかのいずれかです。

> [!NOTE]
>
> - .NET ライブラリの変更がどのように評価されるかのガイドとして利用できるだけでなく、ライブラリ開発者はこれらの基準を使用して、複数の .NET の実装とバージョンをターゲットとするライブラリの変更を評価することもできます。
> - 上位互換性と下位互換性などの互換性カテゴリの詳細については、[カテゴリ](categories.md)に関するページを参照してください。

## <a name="modifications-to-the-public-contract"></a>パブリック コントラクトの変更

このカテゴリの変更によって、型のパブリック セキュリティ、外部からのアクセスが変わります。 このカテゴリの変更のほとんどは、"*後方互換性*" に違反しているため、許可されません (以前のバージョンの API で開発されたアプリケーションが新しいバージョン上で再コンパイルすることなく実行できること)。

### <a name="types"></a>型

- ✔️ **許可:インターフェイスが既に基本データ型で実装されている場合に、型からインターフェイスの実装を削除する**

- ❓ **判断が必要:型に新しいインターフェイスの実装を追加する**

  既存のクライアントに悪影響を及ぼさないため、これは許容できる変更です。 新しい実装が許容できる状態を維持するには、型に対するすべての変更が、ここで定義された許容可能な変更の境界内で機能する必要があります。 下位レベルでは消費できないコードやデータを生成するデザイナーやシリアライザーの能力に直接影響するインターフェイスを追加する場合は、細心の注意が必要です。 たとえば、<xref:System.Runtime.Serialization.ISerializable> インターフェイスです。

- ❓ **判断が必要:新しい基底クラスを導入する**

  新しい [abstract](../../csharp/language-reference/keywords/abstract.md) メンバーを導入しない場合、または既存の型のセマンティクスまたは動作を変更しない場合は、既存の 2 つの型の間の階層に型を導入できます。 たとえば、.NET Framework 2.0 では、<xref:System.Data.Common.DbConnection> クラスが <xref:System.Data.SqlClient.SqlConnection> の新しい基底クラスになりました。これは、以前は直接 <xref:System.ComponentModel.Component> から派生していました。

- ✔️ **許可:あるアセンブリから別のアセンブリに型を移動する**

  *以前の* アセンブリは、新しいアセンブリを指す <xref:System.Runtime.CompilerServices.TypeForwardedToAttribute> でマークする必要があります。

- ✔️ **許可:[struct](../../csharp/language-reference/builtin-types/struct.md) 型を `readonly struct` 型に変更する**

  `readonly struct` 型の `struct` 型への変更は許可されていません。

- ✔️ **許可: *アクセス可能な* (パブリックまたは保護された) コンストラクターがない場合に [sealed](../../csharp/language-reference/keywords/sealed.md) または [abstract](../../csharp/language-reference/keywords/abstract.md) キーワードを型に追加する**

- ✔️ **許可:型の可視性を拡張する**

- ❌ **未許可:型の名前空間または名前を変更する**

- ❌ **未許可:パブリック型の名前変更または削除する**

   これで、名前が変更された、または削除された型を使用するすべてのコードは互換性がなくなります。

- ❌ **未許可:列挙型の基になる型を変更する**

   これは、コンパイル時と動作の破壊的変更であり、属性の引数が解析できなくなる可能性があるバイナリの破壊的変更もあります。

- ❌ **未許可:以前はアンシールドだった型をシールする**

- ❌ **未許可:インターフェイスの一連の基本データ型にインターフェイスを追加する**

   あるインターフェイスが以前は実装していなかったインターフェイスを実装すると、そのインターフェイスの元のバージョンを実装していたすべての型は互換性がなくなります。

- ❓ **判断が必要:一連の基底クラスからクラスを削除する、または実装済みの一連のインターフェイスからインターフェイスを削除する**

  インターフェイス削除の規則には 1 つの例外があります。削除したインターフェイスから派生するインターフェイスの実装を追加することができます。 たとえば、型またはインターフェイスが <xref:System.ComponentModel.IComponent> を実装し、それが <xref:System.IDisposable> を実装している場合は、<xref:System.IDisposable> を削除できます。

- ❌ **未許可:`readonly struct` 型を [struct](../../csharp/language-reference/builtin-types/struct.md) 型に変更する**

  ただし、`struct` 型から `readonly struct` 型への変更は許可されています。

- ❌ **未許可:[struct](../../csharp/language-reference/builtin-types/struct.md) 型を `ref struct` 型に、またはその逆に変更する**

- ❌ **未許可:型の可視性を下げる**

   ただし、型の可視性を上げることは許可されています。

### <a name="members"></a>メンバー

- ✔️ **許可:[virtual](../../csharp/language-reference/keywords/sealed.md) ではないメンバーの可視性を拡張する**

- ✔️ **許可: *アクセス可能な* (パブリックまたは保護された) コンストラクターを持たない、または型が [シールされている](../../csharp/language-reference/keywords/sealed.md)パブリック型に抽象メンバーを追加する**

  ただし、アクセス可能な (パブリックまたは保護された) コンストラクターを持ち、`sealed` ではない型に抽象メンバーを追加することは許可されていません。

- ✔️ **許可:型にアクセス可能な (パブリックまたは保護された) コンストラクターがない、または型が [シールされている](../../csharp/language-reference/keywords/sealed.md)場合に、[保護された](../../csharp/language-reference/keywords/protected.md)メンバーの可視性を制限する**

- ✔️ **許可:削除された型より上位の層のクラスにメンバーを移動する**

- ✔️ **許可:オーバーライドを追加または削除する**

  オーバーライドを導入すると、[base](../../csharp/language-reference/keywords/base.md) を呼び出すときに以前のコンシューマーでオーバーライドがスキップされる場合があります。

- ✔️ **許可:以前クラスにコンストラクターがなかった場合に、パラメーターなしのコンストラクターと共に、クラスにコンストラクターを追加する**

   ただし、パラメーターなしのコンストラクターを追加 "*せずに*" 以前にコンストラクターがなかったクラスにコンストラクターを追加することは許可されていません。

- ✔️ **許可:メンバーを [abstract](../../csharp/language-reference/keywords/abstract.md) から [virtual](../../csharp/language-reference/keywords/virtual.md) に変更する**

- ✔️ **許可:戻り値を `ref readonly` から `ref` に変更する (仮想メソッドまたはインターフェイスを除く)**

- ✔️ **許可:フィールドの静的な型が可変値型ではない場合に、フィールドから [readonly](../../csharp/language-reference/keywords/readonly.md)を削除する**

- ✔️ **許可:以前定義されていなかった新しいイベントを呼び出す**

- ❓ **判断が必要:新しいインスタンス フィールドを型に追加する**

   この変更はシリアル化に影響があります。

- ❌ **未許可:パブリック メンバーまたはパラメーターを名前変更または削除する**

   これで、名前が変更された、または削除されたメンバーまたはパラメーターを使用するすべてのコードは互換性がなくなります。

   これは、列挙型メンバーを名前変更または削除する場合も、プロパティからゲッターまたはセッターを削除または名前変更する場合も含まれます。

- ❌ **未許可:インターフェイスにメンバーを追加する**

- ❌ **未許可:パブリック定数または列挙型メンバーの値を変更する**

- ❌ **未許可:プロパティ、フィールド、パラメーター、または戻り値の型を変更する**

- ❌ **未許可:パラメーターの順序を追加、削除、または変更する**

- ❌ **未許可:[in](../../csharp/language-reference/keywords/in.md)、[out](../../csharp/language-reference/keywords/out.md)、または [ref](../../csharp/language-reference/keywords/ref.md) キーワードをパラメーターに追加または削除する**

- ❌ **未許可:パラメーター名を変更する (大文字と小文字の変更も含む)**

  これは 2 つの理由で破壊的と見なされます。

  - これは、Visual Basic の遅延バインディング機能や C# の[動的](../../csharp/language-reference/builtin-types/reference-types.md#the-dynamic-type)など、遅延バインディングのシナリオの互換性がなくなります。

  - 開発者が[名前付き引数](../../csharp/programming-guide/classes-and-structs/named-and-optional-arguments.md#named-arguments)を使用すると、[ソースの互換性](categories.md#source-compatibility)がなくなります。

- ❌ **未許可:`ref` の戻り値から `ref readonly` の戻り値に変更する**

- ❌️ **未許可:仮想メソッドまたはインターフェイス上で戻り値を `ref readonly` から `ref` に変更する**

- ❌ **未許可:メンバーから [abstract](../../csharp/language-reference/keywords/abstract.md) を追加または削除する**

- ❌ **未許可:メンバーから [virtual](../../csharp/language-reference/keywords/virtual.md) キーワードを削除する**

  C# コンパイラでは [callvirt](<xref:System.Reflection.Emit.OpCodes.Callvirt>) 中間言語 (IL) 命令を発行して非仮想メソッドを呼び出す傾向があるので (`callvirt` は null チェックを行いますが、通常の呼び出しでは行われません)、多くの場合、これは破壊的変更ではありませんが、この動作はいくつかの理由で一定ではありません。
  - .NET がターゲットにしている言語は C# だけではありません。

  - ターゲット メソッドが非仮想で、おそらく null ではない場合 ([?. null 反映演算子](../../csharp/language-reference/operators/member-access-operators.md#null-conditional-operators--and-)を介してアクセスされるメソッドなど) は常に、C# コンパイラでは `callvirt` を通常の呼び出しに最適化しようとします。

  メソッドを仮想化することは、多くの場合、コンシューマー コードから最終的に非仮想的に呼び出されることを示します。

- ❌ **未許可:メンバーに [virtual](../../csharp/language-reference/keywords/virtual.md) キーワードを追加する**

- ❌ **未許可:仮想メンバーを抽象化する**

  [仮想メンバー](../../csharp/language-reference/keywords/virtual.md)には、派生クラスによってオーバーライド "*できる*" メソッド実装が用意されています。 [抽象メンバー](../../csharp/language-reference/keywords/abstract.md)には実装がないのでオーバーライドする "*必要があります*"。

- ❌ **未許可:アクセス可能な (パブリックまたは保護された) コンストラクターを持ち、[シール](../../csharp/language-reference/keywords/sealed.md)されていないパブリック型に抽象メンバーを追加する**

- ❌ **未許可:メンバーに [static](../../csharp/language-reference/keywords/static.md) キーワードを追加または削除する**

- ❌ **未許可:既存のオーバーロードを排除し、異なる動作を定義するオーバーロードを追加する**

  これで、以前の過負荷にバインドされていた既存のクライアントは互換性がなくなります。 たとえば、クラスに <xref:System.UInt32> を受け取る単一バージョンのメソッドがある場合、既存のコンシューマーは <xref:System.Int32> 値を渡すときにそのオーバーロードに正常にバインドします。 ただし、<xref:System.Int32> を受け取るオーバーロードを追加した場合、再コンパイル時または遅延バインディングの使用時に、コンパイラでは新しいオーバーロードにバインドされるようになりました。 異なる動作結果になる場合、これは破壊的変更です。

- ❌ **未許可:パラメーターなしのコンストラクターを追加せずに、以前にコンストラクターがなかったクラスにコンストラクターを追加する**

- ❌️ **未許可:フィールドに [readonly](../../csharp/language-reference/keywords/readonly.md) を追加する**

- ❌ **未許可:メンバーの可視性を下げる**

   これには、*アクセス可能な* (`public` または `protected`) コンストラクターがあり、型が [シールド](../../csharp/language-reference/keywords/sealed.md)*されていない* 場合に、[保護された](../../csharp/language-reference/keywords/protected.md)メンバーの可視性を下げることも含まれます。 そうでない場合は、保護されたメンバーの可視性を下げることができます。

   メンバーの可視性を上げることは許可されています。

- ❌ **未許可:メンバーの型を変更する**

   メソッドの戻り値、プロパティまたはフィールドの型は変更できません。 たとえば、<xref:System.Object> を返すメソッドのシグネチャを <xref:System.String> を返すように変更することはできません。その逆も同様です。

- ❌ **未許可:以前は状態がなかった構造体にフィールドを追加する**

  限定代入規則では、変数型がステートレス構造体である限り、未初期化変数の使用が許可されます。 構造体がステートフルになると、コードは未初期化データになる可能性があります。 これは、ソースの破壊的変更とバイナリの破壊的変更の両方になる可能性があります。

- ❌ **未許可:以前に発生したことのない既存のイベントを発生させる**

## <a name="behavioral-changes"></a>動作の変更

### <a name="assemblies"></a>アセンブリ

- ✔️ **許可:同じプラットフォームがまだサポートされているときにアセンブリを移植可能にする**

- ❌ **未許可:アセンブリの名前を変更する**
- ❌ **未許可:アセンブリの公開キーを変更する**

### <a name="properties-fields-parameters-and-return-values"></a>プロパティ、フィールド、パラメーター、および戻り値

- ✔️ **許可:プロパティ、フィールド、戻り値、または [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) パラメーターの値を派生型に変更する**

  たとえば、型 <xref:System.Object> を返すメソッドからは <xref:System.String> インスタンスが返される可能性があります (ただし、メソッドのシグネチャは変更できません)。

- ✔️ **許可:メンバーが [virtual](../../csharp/language-reference/keywords/virtual.md) ではない場合、プロパティまたはパラメーターに許容される値の範囲を広げる**

  メソッドに渡すことができるか、メンバーから返される値の範囲は拡張できますが、パラメーターまたはメンバー型は拡張できません。 たとえば、メソッドに渡される値は 0-124 から 0-255 に拡張できますが、パラメーターの型を <xref:System.Byte> から <xref:System.Int32> に変更することはできません。

- ❌ **未許可:メンバーが [virtual](../../csharp/language-reference/keywords/virtual.md) の場合、プロパティまたはパラメーターに許容される値の範囲を広げる**

   この変更で、既存のオーバーライドされたメンバーは互換性がなくなり、値の拡張された範囲に対しては正しく機能しなくなります。

- ❌ **未許可:プロパティまたはパラメーターの許容される値の範囲を狭める**

- ❌ **未許可:プロパティ、フィールド、戻り値、または [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) パラメーターの戻り値の範囲を広げる**

- ❌ **未許可:プロパティ、フィールド、メソッドの戻り値、または [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) パラメーターの戻り値を変更する**

- ❌ **未許可:プロパティ、フィールド、またはパラメーターの既定値を変更する**

- ❌ **未許可:数値の戻り値の有効桁数を変更する**

- ❓ **判断が必要:入力の解析と新しい例外のスローを変更する (解析の動作がドキュメントで指定されていない場合でも)**

### <a name="exceptions"></a>例外

- ✔️ **許可:既存の例外ではなく派生した例外をスローする**

  新しい例外は既存の例外のサブクラスなので、以前の例外処理コードは引き続き例外を処理します。 たとえば、.NET Framework 4 では、カルチャが見つからなかった場合、カルチャの作成および取得メソッドは <xref:System.ArgumentException> ではなく <xref:System.Globalization.CultureNotFoundException> をスローするようになりました。 <xref:System.Globalization.CultureNotFoundException> は <xref:System.ArgumentException> から派生しているので、これは許容される変更です。

- ✔️ **許可:<xref:System.NotSupportedException>、<xref:System.NotImplementedException>、<xref:System.NullReferenceException> よりも具体的な例外をスローする**

- ✔️ **許可:回復不能と見なされる例外をスローする**

  おそらく、回復不能な例外はキャッチされませんが、代わりに高レベルの catch-all ハンドラーによって処理されます。 そのため、これらの明示的な例外をキャッチするコードをユーザーが持つことは期待されません。 回復不能な例外は次のとおりです。

  - <xref:System.AccessViolationException>
  - <xref:System.ExecutionEngineException>
  - <xref:System.Runtime.InteropServices.SEHException>
  - <xref:System.StackOverflowException>

- ✔️ **許可:新しいコード パスで新しい例外をスローする**

  例外は、新しいパラメーター値または状態で実行される新しいコードパスにのみ適用されます。これは、以前のバージョンをターゲットとする既存のコードでは実行できません。

- ✔️ **許可:より堅牢な動作または新しいシナリオを有効にするために例外を削除する**

  たとえば、以前は正の値のみを処理し、それ以外の場合は<xref:System.ArgumentOutOfRangeException> をスローする `Divide` メソッドは、例外をスローせずに負の値と正の値の両方をサポートするように変更できます。

- ✔️ **許可:エラー メッセージのテキストを変更する**

  開発者は、エラー メッセージのテキストに頼るべきではありません。これはユーザーのカルチャに基づいて変わることもあります。

- ❌ **未許可:上記以外の場合に例外をスローする**

- ❌ **未許可:上記以外の場合の例外を削除する**

### <a name="attributes"></a>属性

- ✔️ **許可:観測可能 *ではない* 属性の値を変更する**

- ❌ **未許可:監視可能 *な* 属性の値を変更する**

- ❓ **判断が必要:属性を削除する**

  ほとんどの場合、属性 (<xref:System.NonSerializedAttribute> など) を削除することは破壊的変更です。

## <a name="platform-support"></a>プラットフォームのサポート

- ✔️ **許可:以前はサポートされていなかったプラットフォーム上の操作をサポートする**

- ❌ **未許可:以前にプラットフォームでサポートされていた操作に対して、特定のサービス パックのサポートを停止する、または必須にする**

## <a name="internal-implementation-changes"></a>内部的な実装の変更

- ❓ **判断が必要:内部型のセキュリティを変更する**

   このような変更は一般に許可されていますが、非公開のリフレクションは互換性がなくなります。 一般的なサードパーティ製ライブラリや多数の開発者が内部 API に依存している場合など、状況によってはこのような変更が許容されないことがあります。

- ❓ **判断が必要:メンバーの内部的な実装を変更する**

  このような変更は一般に許可されていますが、非公開のリフレクションは互換性がなくなります。 顧客コードが非公開のリフレクションに頻繁に依存している場合や、変更によって意図しない副作用が生じる場合など、状況によってはこのような変更が許容されないことがあります。

- ✔️ **許可:操作のパフォーマンスを向上させる**

   操作のパフォーマンスを変更する機能は不可欠ですが、そのような変更により、現在の操作速度に依存するコードと互換性がなくなる可能性があります。 これは、非同期操作のタイミングに依存するコードに特に当てはまります。 パフォーマンスの変更は、該当する API の他の動作には影響しません。する場合は、破壊的変更になります。

- ✔️ **許可:間接的に (そして多くの場合は低下するように) 操作のパフォーマンスを変更する**

  該当する変更が他の何らかの理由で破壊的と分類されていない場合、これは許容されます。 多くの場合、余分な操作を含む可能性があるアクション、または新しい機能を追加するアクションを実行する必要があります。 これはほとんど常にパフォーマンスに影響しますが、該当する API を想定どおりに機能させるためには不可欠なことがあります。

- ❌ **未許可:同期 API から非同期に変更する (およびその逆)**

## <a name="code-changes"></a>コード変更

- ✔️ **許可:パラメーターに [params](../../csharp/language-reference/keywords/params.md) を追加する**

- ❌ **未許可:[struct](../../csharp/language-reference/builtin-types/struct.md) を [class](../../csharp/language-reference/keywords/class.md) に変更する (およびその逆)**

- ❌ **未許可:[checked](../../csharp/language-reference/keywords/virtual.md) キーワードをコード ブロックに追加する**

   この変更により、以前に実行されたコードから <xref:System.OverflowException> がスローされることがあるため、許容されません。

- ❌ **未許可:パラメーターから [params](../../csharp/language-reference/keywords/params.md) を削除する**

- ❌ **未許可:イベントの発生順序を変更する**

  開発者はイベントが同じ順序で発生すると合理的に想定する可能性があります。開発者のコードはイベントが発生する順序に依存していることがよくあります。

- ❌ **未許可:特定のアクションでイベントが発生しないようにする**

- ❌ **未許可:指定したイベントが呼び出される回数を変更する**

- ❌ **未許可:列挙型に <xref:System.FlagsAttribute> を追加する**
