---
title: キーワード リファレンス
description: すべてのF#言語キーワードに関する情報へのリンクを検索します。
ms.date: 11/04/2019
ms.openlocfilehash: 64bb680a0861f4b8287f887ea67edb6fcf4f88a6
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976618"
---
# <a name="keyword-reference"></a>キーワード リファレンス

このトピックには、すべてF#の言語キーワードに関する情報へのリンクが含まれています。

## <a name="f-keyword-table"></a>F#キーワードテーブル

次の表は、 F#すべてのキーワードをアルファベット順に示しています。簡単な説明と、詳細情報が記載されている関連トピックへのリンクが記載されています。

|キーワード|Link|説明|
|-------|----|-----------|
|`abstract`|[メンバー](./members/index.md)<br /><br />[抽象クラス](abstract-classes.md)|宣言されている型に実装が存在しないか、または仮想であり、既定のが実装されているメソッドを示します。|
|`and`|[`let` バインド](./functions/let-bindings.md)<br /><br />[レコード](records.md)<br /><br />[メンバー](./members/index.md)<br /><br />[制約](./generics/constraints.md)|相互に再帰的なバインドとレコード、プロパティ宣言、およびジェネリックパラメーターに対する複数の制約を使用して使用されます。|
|`as`|[クラス](classes.md)<br /><br />[パターン一致](Pattern-Matching.md)|現在のクラスオブジェクトにオブジェクト名を指定するために使用します。 パターン一致でパターン全体に名前を付けるためにも使用されます。|
|`assert`|[アサーション](assertions.md)|デバッグ中にコードを検証するために使用されます。|
|`base`|[クラス](classes.md)<br /><br />[継承](inheritance.md)|基底クラスオブジェクトの名前として使用されます。|
|`begin`|[冗語構文](verbose-syntax.md)|Verbose 構文では、コードブロックの開始を示します。|
|`class`|[クラス](classes.md)|Verbose 構文では、クラス定義の開始を示します。|
|`default`|[メンバー](./members/index.md)|抽象メソッドの実装を示します。仮想メソッドを作成するために、抽象メソッドの宣言と共に使用されます。|
|`delegate`|[デリゲート](delegates.md)|デリゲートを宣言するために使用されます。|
|`do`|[do バインド](./functions/do-bindings.md)<br /><br />[ループ: `for...to` 式](loops-for-to-expression.md)<br /><br />[ループ: `for...in` 式](loops-for-in-expression.md)<br /><br />[ループ: `while...do` 式](loops-while-do-expression.md)|ループ構造または命令型コードの実行に使用されます。|
|`done`|[冗語構文](verbose-syntax.md)|詳細な構文では、ループ式のコードブロックの末尾を示します。|
|`downcast`|[キャストと変換](casting-and-conversions.md)|継承チェーンの下位にある型に変換するために使用します。|
|`downto`|[ループ: `for...to` 式](loops-for-to-expression.md)|`for` 式で、逆にカウントするときに使用します。|
|`elif`|[条件式: `if...then...else`](conditional-expressions-if-then-else.md)|条件分岐で使用されます。 `else if`の短い形式。|
|`else`|[条件式: `if...then...else`](conditional-expressions-if-then-else.md)|条件分岐で使用されます。|
|`end`|[構造体](structures.md)<br /><br />[判別共用体](discriminated-unions.md)<br /><br />[レコード](records.md)<br /><br />[型拡張](type-extensions.md)<br /><br />[冗語構文](verbose-syntax.md)|型定義と型拡張機能では、メンバー定義のセクションの末尾を示します。<br /><br />Verbose 構文で、`begin` キーワードで始まるコードブロックの末尾を指定するために使用されます。|
|`exception`|[例外処理](./exception-handling/index.md)<br /><br />[例外の種類](./exception-handling/exception-types.md)|例外の種類を宣言するために使用します。|
|`extern`|[外部関数](./functions/external-functions.md)|宣言されたプログラム要素が別のバイナリまたはアセンブリで定義されていることを示します。|
|`false`|[プリミティブ型](basic-types.md)|ブール型のリテラルとして使用されます。|
|`finally`|[例外: `try...finally` 式](./exception-handling/the-try-finally-expression.md)|`try` と共に使用して、例外が発生したかどうかに関係なく実行されるコードのブロックを導入します。|
|`fixed`|[固定](fixed.md)|ガベージコレクションが行われないように、スタックにポインターを "ピン留め" するために使用されます。|
|`for`|[ループ: `for...to` 式](loops-for-to-expression.md)<br /><br />[ループ: for...in 式](loops-for-in-expression.md)|ループ構造で使用されます。|
|`fun`|[ラムダ式: `fun` キーワード](./functions/lambda-expressions-the-fun-keyword.md)|ラムダ式で使用されます。匿名関数とも呼ばれます。|
|`function`|[match 式](match-expressions.md)<br /><br />[ラムダ式: 楽しいキーワード](./functions/lambda-expressions-the-fun-keyword.md)|`fun` キーワードの代わりとして使用され、1つの引数にパターンマッチングがあるラムダ式で `match` 式を使用します。|
|`global`|[名前空間](namespaces.md)|最上位レベルの .NET 名前空間を参照するために使用されます。|
|`if`|[条件式: `if...then...else`](conditional-expressions-if-then-else.md)|条件分岐構造で使用されます。|
|`in`|[ループ: for...in 式](loops-for-in-expression.md)<br /><br />[冗語構文](verbose-syntax.md)|バインドから式を分離するために、シーケンス式および詳細構文で使用されます。|
|`inherit`|[継承](inheritance.md)|基底クラスまたは基本インターフェイスを指定するために使用します。|
|`inline`|[関数](./functions/index.md)<br /><br />[インライン関数](./functions/inline-functions.md)|呼び出し元のコードに直接統合する必要がある関数を示すために使用されます。|
|`interface`|[インターフェイス](interfaces.md)|インターフェイスの宣言と実装に使用されます。|
|`internal`|[アクセス制御](access-control.md)|メンバーがアセンブリ内に表示されているが、外部では参照できないことを指定するために使用します。|
|`lazy`|[遅延式](lazy-expressions.md)|結果が必要な場合にのみ実行される式を指定するために使用します。|
|`let`|[`let` バインド](./functions/let-bindings.md)|値または関数に名前を関連付ける、またはバインドするために使用されます。|
|`let!`|[非同期ワークフロー](asynchronous-workflows.md)<br /><br />[コンピュテーション式](computation-expressions.md)|非同期ワークフローで名前を非同期計算の結果にバインドするために、または計算型の結果に名前をバインドするために使用されるその他の計算式で使用されます。|
|`match`|[match 式](match-expressions.md)|値をパターンと比較することによって分岐するために使用されます。|
|`match!`|[コンピュテーション式](computation-expressions.md#match)|コンピュテーション式への呼び出しと、その結果に一致するパターンをインライン化するために使用されます。|
|`member`|[メンバー](./members/index.md)|オブジェクト型のプロパティまたはメソッドを宣言するために使用します。|
|`module`|[モジュール](modules.md)|名前を関連する型、値、および関数のグループに関連付けて、他のコードと論理的に分離するために使用されます。|
|`mutable`|[let バインド](./functions/let-bindings.md)|変数、つまり変更可能な値を宣言するために使用されます。|
|`namespace`|[名前空間](namespaces.md)|名前を関連する型およびモジュールのグループに関連付けて、他のコードから論理的に分離するために使用されます。|
|`new`|[コンストラクター](./members/constructors.md)<br /><br />[制約](./generics/constraints.md)|を作成するか、オブジェクトを作成できるコンストラクターを宣言、定義、または呼び出すために使用します。<br /><br />ジェネリックパラメーター制約でも、型が特定のコンストラクターを持つ必要があることを示すために使用されます。|
|`not`|[シンボルと演算子のリファレンス](./symbol-and-operator-reference/index.md)<br /><br />[制約](./generics/constraints.md)|実際にはキーワードではありません。 ただし、`not struct` の組み合わせは、ジェネリックパラメーターの制約として使用されます。|
|`null`|[null 値](./values/null-values.md)<br /><br />[制約](./generics/constraints.md)|オブジェクトが存在しないことを示します。<br /><br />ジェネリックパラメーター制約でも使用されます。|
|`of`|[判別共用体](discriminated-unions.md)<br /><br />[デリゲート](delegates.md)<br /><br />[例外の種類](./exception-handling/exception-types.md)|判別共用体で、値のカテゴリの型、およびデリゲートおよび例外の宣言を示すために使用されます。|
|`open`|[インポート宣言: `open` キーワード](import-declarations-the-open-keyword.md)|名前空間またはモジュールの内容を修飾なしで使用できるようにするために使用します。|
|`or`|[シンボルと演算子のリファレンス](./symbol-and-operator-reference/index.md)<br /><br />[制約](./generics/constraints.md)|ブール条件と共に boolean `or` 演算子として使用されます。 これは、`||` に相当します。<br /><br />メンバーの制約でも使用されます。|
|`override`|[メンバー](./members/index.md)|基本バージョンとは異なる抽象メソッドまたは仮想メソッドのバージョンを実装するために使用されます。|
|`private`|[アクセス制御](access-control.md)|メンバーへのアクセスを、同じ型またはモジュール内のコードに限定します。|
|`public`|[アクセス制御](access-control.md)|型の外部からメンバーにアクセスできるようにします。|
|`rec`|[関数](./functions/index.md)|関数が再帰的であることを示すために使用されます。|
|`return`|[非同期ワークフロー](Asynchronous-Workflows.md)<br /><br />[コンピュテーション式](computation-expressions.md)|コンピュテーション式の結果として指定する値を示すために使用します。|
|`return!`|[コンピュテーション式](computation-expressions.md)<br /><br />[非同期ワークフロー](asynchronous-workflows.md)|評価時に、含んでいるコンピュテーション式の結果を提供するコンピュテーション式を示すために使用されます。|
|`select`|[クエリ式](query-expressions.md)|クエリ式で、抽出するフィールドまたは列を指定するために使用されます。 これはコンテキストキーワードであることに注意してください。これは、実際には予約語ではなく、適切なコンテキストでキーワードと同じように動作することを意味します。|
|`static`|[メンバー](./members/index.md)|型のインスタンスを使用せずに呼び出すことができるメソッドまたはプロパティ、または型のすべてのインスタンス間で共有される値メンバーを示すために使用されます。|
|`struct`|[構造体](structures.md)<br /><br /> [タプル](tuples.md)<br/><br/>[制約](./generics/constraints.md)|構造体型を宣言するために使用されます。<br /><br/>構造体のタプルを指定するために使用します。<br/><br />ジェネリックパラメーター制約でも使用されます。<br /><br />モジュール定義での OCaml の互換性のために使用されます。|
|`then`|[条件式: `if...then...else`](conditional-expressions-if-then-else.md)<br /><br />[コンストラクター](./members/constructors.md)|条件式で使用されます。<br /><br />オブジェクトの構築後に副作用を実行するためにも使用されます。|
|`to`|[ループ: `for...to` 式](loops-for-to-expression.md)|範囲を示すために `for` ループで使用されます。|
|`true`|[プリミティブ型](basic-types.md)|ブール型のリテラルとして使用されます。|
|`try`|[例外: 試行...with 式](./exception-handling/the-try-with-expression.md)<br /><br />[例外: 試行...finally 式](./exception-handling/the-try-finally-expression.md)|例外を生成する可能性のあるコードブロックを導入するために使用されます。 `with` または `finally`と共に使用します。|
|`type`|[F# の型](fsharp-types.md)<br /><br />[クラス](classes.md)<br /><br />[レコード](records.md)<br /><br />[構造体](structures.md)<br /><br />[列挙体](enumerations.md)<br /><br />[判別共用体](discriminated-unions.md)<br /><br />[型略称](type-abbreviations.md)<br /><br />[測定単位](units-of-measure.md)|クラス、レコード、構造体、判別共用体、列挙型、測定単位、または型略称を宣言するために使用されます。|
|`upcast`|[キャストと変換](casting-and-conversions.md)|継承チェーンの上位にある型に変換するために使用します。|
|`use`|[リソースの管理: `use` キーワード](resource-management-the-use-keyword.md)|リソースを解放するために `Dispose` を呼び出す必要がある値には、`let` の代わりに使用されます。|
|`use!`|[コンピュテーション式](computation-expressions.md)<br /><br />[非同期ワークフロー](asynchronous-workflows.md)|非同期ワークフローや、リソースを解放するために `Dispose` を呼び出す必要がある値に対して、`let!` の代わりに使用されます。|
|`val`|[明示的なフィールド: `val` キーワード](./members/explicit-fields-the-val-keyword.md)<br /><br />[シグネチャ](signature-files.md)<br /><br />[メンバー](./members/index.md)|制限された状況で、値を示すため、または型でメンバーを宣言するために、シグネチャで使用されます。|
|`void`|[プリミティブ型](basic-types.md)|.NET `void` の種類を示します。 他の .NET 言語と相互運用するときに使用します。|
|`when`|[制約](./generics/constraints.md)|パターンに一致する場合、ブール条件 (*場合*によっては) に使用され、ジェネリック型パラメーターの制約句を導入します。|
|`while`|[ループ: `while...do` 式](loops-while-do-expression.md)|では、ループ構造が導入されています。|
|`with`|[match 式](match-expressions.md)<br /><br />[オブジェクト式](object-expressions.md)<br /><br />[レコード式のコピーと更新](copy-and-update-record-expressions.md)<br /><br />[型拡張](type-extensions.md)<br /><br />[例外: `try...with` 式](./exception-handling/the-try-with-expression.md)|パターン一致式で `match` キーワードと共に使用されます。 オブジェクト式、レコードコピー式、および型拡張でも使用され、メンバー定義を導入したり、例外ハンドラーを導入したりします。|
|`yield`|[リスト](lists.md)、[配列](arrays.md)、[シーケンス](sequences.md)|シーケンスの値を生成するために、リスト、配列、またはシーケンス式で使用されます。 通常は、ほとんどの状況で暗黙的に使用されるため、を省略できます。|
|`yield!`|[コンピュテーション式](computation-expressions.md)<br /><br />[非同期ワークフロー](asynchronous-workflows.md)|コンピュテーション式で使用され、指定されたコンピュテーション式の結果を、それを含むコンピュテーション式の結果のコレクションに追加します。|

次のトークンは、OCaml F#言語のキーワードであるため、で予約されています。

- `asr`
- `land`
- `lor`
- `lsl`
- `lsr`
- `lxor`
- `mod`
- `sig`

`--mlcompatibility` コンパイラオプションを使用する場合、上記のキーワードを識別子として使用できます。

次のトークンは、今後F#言語を拡張するためのキーワードとして予約されています。

- `atomic`
- `break`
- `checked`
- `component`
- `const`
- `constraint`
- `constructor`
- `continue`
- `eager`
- `event`
- `external`
- `functor`
- `include`
- `method`
- `mixin`
- `object`
- `parallel`
- `process`
- `protected`
- `pure`
- `sealed`
- `tailcall`
- `trait`
- `virtual`
- `volatile`

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](index.md)
- [シンボルと演算子のリファレンス](./symbol-and-operator-reference/index.md)
- [コンパイラ オプション](compiler-options.md)
