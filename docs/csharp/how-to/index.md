---
title: 方法に関する記事 (C# ガイド)
description: 簡単なヒントと、焦点を絞った短いコード サンプルのコレクション
ms.date: 12/20/2017
ms.openlocfilehash: e6cb657726b82a1710bbcd596fe48037b5c26352
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397546"
---
# <a name="how-to-c"></a>方法 (C#)

C# ガイドの方法に関するセクションでは、一般的な質問に対する簡単な回答が見つかります。 場合によっては、記事が複数のセクションで表示されることもあります。 複数の検索パスで見つけやすいようにしました。

## <a name="general-c-concepts"></a>一般的な C# の概念

C# の開発者には一般的な方法といえるヒントやコツがいくつかあります。

- [オブジェクト初期化子を使用してオブジェクトを初期化する](../programming-guide/classes-and-structs/how-to-initialize-objects-by-using-an-object-initializer.md)。
- [メソッドに構造体を渡す場合とクラスを渡す場合の違いについて理解する](../programming-guide/classes-and-structs/how-to-know-the-difference-passing-a-struct-and-passing-a-class-to-a-method.md)。
- [演算子のオーバーロードを使用する](../language-reference/operators/operator-overloading.md)。
- [カスタム拡張メソッドを実装して呼び出す](../programming-guide/classes-and-structs/how-to-implement-and-call-a-custom-extension-method.md)。
- C# のプログラマでも[ Visual Basic の `My` 名前空間を使用する場合がある](../programming-guide/namespaces/how-to-use-the-my-namespace.md)。
- [拡張メソッドを使用して `enum` 型の新しいメソッドを作成する](../programming-guide/classes-and-structs/how-to-create-a-new-method-for-an-enumeration.md)。

### <a name="class-and-struct-members"></a>クラスと構造体のメンバー

クラスと構造体を作成してプログラムを実装します。 次の手法は、クラスまたは構造体を作成するときによく使用されます。

- [自動的に実装されたプロパティを宣言する](../programming-guide/classes-and-structs/how-to-implement-a-lightweight-class-with-auto-implemented-properties.md)。
- [読み取り/書き込みのプロパティを宣言して使用する](../programming-guide/classes-and-structs/how-to-declare-and-use-read-write-properties.md)。
- [定数を定義する](../programming-guide/classes-and-structs/how-to-define-constants.md)。
- [文字列出力を提供するために `ToString` メソッドをオーバーライドする](../programming-guide/classes-and-structs/how-to-override-the-tostring-method.md)。
- [抽象プロパティを定義する](../programming-guide/classes-and-structs/how-to-define-abstract-properties.md)。
- [XML ドキュメント機能を使用してコードを文書化する](../programming-guide/xmldoc/how-to-use-the-xml-documentation-features.md)。
- [インターフェイス メンバーを明示的に実装して](../programming-guide/interfaces/how-to-explicitly-implement-interface-members.md)パブリック インターフェイスを簡潔に保つ。
- [2 つのインターフェイスのメンバーを明示的に実装する](../programming-guide/interfaces/how-to-explicitly-implement-members-of-two-interfaces.md)。

### <a name="working-with-collections"></a>コレクションの操作

次の記事は、データのコレクションの操作に役立ちます。

- [コレクション初期化子を使用してディクショナリを初期化する](../programming-guide/classes-and-structs/how-to-initialize-a-dictionary-with-a-collection-initializer.md)。

## <a name="working-with-strings"></a>文字列の処理

文字列は、文字列の表示または操作に使用される基本的なデータ型です。 次の記事で、文字列に関する一般的なプラクティスを説明しています。

- [文字列を比較する](compare-strings.md)。
- [文字列の内容を変更する](modify-string-contents.md)。
- [文字列が数値を表すかどうかを判断する](../programming-guide/strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)。
- [`String.Split` を使用して文字列を区切る](parse-strings-using-split.md)。
- [複数の文字列を 1 つに結合する](concatenate-multiple-strings.md)。
- [文字列内のテキストを検索する](search-strings.md)。

## <a name="convert-between-types"></a>型の変換

オブジェクトを別の型に変換しなければならない場合があります。

- [文字列が数値を表すかどうかを判断する](../programming-guide/strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)。
- [16 進数を表す文字列と数値を変換する](../programming-guide/types/how-to-convert-between-hexadecimal-strings-and-numeric-types.md)。
- [文字列を `DateTime` に変換する](../../standard/base-types/parsing-datetime.md)。
- [バイト配列を int に変換する](../programming-guide/types/how-to-convert-a-byte-array-to-an-int.md)。
- [文字列を数値に変換する](../programming-guide/types/how-to-convert-a-string-to-a-number.md)。
- [パターン マッチングで `as` と `is` の演算子を使用して異なる型に安全にキャストする](../how-to/safely-cast-using-pattern-matching-is-and-as-operators.md)。
- [カスタムの型変換を定義する](../language-reference/operators/user-defined-conversion-operators.md)。
- [型が null 許容値型であるかを判断する](../language-reference/builtin-types/nullable-value-types.md#how-to-identify-a-nullable-value-type)。
- [null 許容値型と null 非許容値型の間で変換する](../language-reference/builtin-types/nullable-value-types.md#conversion-from-a-nullable-value-type-to-an-underlying-type)。

## <a name="equality-and-ordering-comparisons"></a>等価比較と順序付け比較

等価に関する独自のルールを定義する、またはその型のオブジェクト間の自然な順序を定義する型を作成することができます。

- [参照に基づく等価性をテストする](../programming-guide/statements-expressions-operators/how-to-test-for-reference-equality-identity.md)。
- [型の値に基づく等価性を定義する](../programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type.md)。

## <a name="exception-handling"></a>例外処理

.NET プログラムは、例外がスローされたことによってメソッドの作業が完了しなかったことを報告します。 次の記事では、例外の操作について説明します。

- [`try` および `catch` を使用して例外を処理する](../programming-guide/exceptions/how-to-handle-an-exception-using-try-catch.md)。
- [`finally` 句を使用してリソースをクリーンアップする](../programming-guide/exceptions/how-to-execute-cleanup-code-using-finally.md)。
- [非 CLS (共通言語仕様) の例外から回復する](../programming-guide/exceptions/how-to-catch-a-non-cls-exception.md)。

## <a name="delegates-and-events"></a>デリゲートおよびイベント

デリゲートおよびイベントは、弱く結合されたコード ブロックを伴うストラテジ向けの機能を提供します。

- [デリゲートを宣言し、インスタンス化して使用する](../programming-guide/delegates/how-to-declare-instantiate-and-use-a-delegate.md)。
- [マルチキャスト デリゲートを結合する](../programming-guide/delegates/how-to-combine-delegates-multicast-delegates.md)。

イベントは、通知を発行またはサブスクライブするためのメカニズムを提供します。

- [イベントのサブスクリプションとサブスクリプションの解除](../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md)。
- [インターフェイスで宣言されたイベントを実装する](../programming-guide/events/how-to-implement-interface-events.md)。
- [コードがイベントを発行するときに .NET Framework ガイドラインに準拠する](../programming-guide/events/how-to-publish-events-that-conform-to-net-framework-guidelines.md)。
- [派生クラスから基底クラスで定義されているイベントを発生させる](../programming-guide/events/how-to-raise-base-class-events-in-derived-classes.md)。
- [カスタム イベント アクセサーを実装する](../programming-guide/events/how-to-implement-custom-event-accessors.md)。

## <a name="linq-practices"></a>LINQ のプラクティス

LINQ では、LINQ クエリ式パターンをサポートするすべてのデータ ソースのクエリを実行するコードを記述できます。 次の記事は、パターンの理解と、さまざまなデータ ソースの操作に役立ちます。

- [コレクションのクエリを実行する](../programming-guide/concepts/linq/how-to-query-an-arraylist-with-linq.md)。
- [クエリでラムダ式を使用する](../programming-guide/statements-expressions-operators/how-to-use-lambda-expressions-in-a-query.md)。
- [クエリ式で `var` を使用する](../programming-guide/classes-and-structs/how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md)。
- [クエリから要素のプロパティのサブセットを返す](../programming-guide/classes-and-structs/how-to-return-subsets-of-element-properties-in-a-query.md)。
- [複雑なフィルターを使用してクエリを記述する](../programming-guide/concepts/linq/how-to-write-queries-with-complex-filtering.md)。
- [データ ソースの要素を並べ替える](../programming-guide/concepts/linq/how-to-sort-elements.md)。
- [複数のキーに基づいて要素を並べ替える](../programming-guide/concepts/linq/how-to-sort-elements-on-multiple-keys.md)。
- [プロジェクションの型を制御する](../programming-guide/concepts/linq/how-to-control-the-type-of-a-projection.md)。
- [ソース シーケンス内の値の出現箇所をカウントする](../programming-guide/concepts/linq/how-to-count-occurrences-of-a-word-in-a-string-linq.md)。
- [中間値を計算する](../programming-guide/concepts/linq/how-to-calculate-intermediate-values.md)。
- [複数のソースからデータをマージする](../programming-guide/concepts/linq/how-to-populate-object-collections-from-multiple-sources-linq.md)。
- [2 つのシーケンスの差集合を見つける](../programming-guide/concepts/linq/how-to-find-the-set-difference-between-two-lists-linq.md)。
- [空のクエリ結果をデバッグする](../programming-guide/concepts/linq/how-to-debug-empty-query-results-sets.md)。
- [LINQ クエリにカスタム メソッドを追加する](../programming-guide/concepts/linq/how-to-add-custom-methods-for-linq-queries.md)。

## <a name="multiple-threads-and-async-processing"></a>複数のスレッドおよび非同期処理

最新のプログラムでは、多くの場合、非同期操作を使用します。 次の記事は、これらの手法の使用方法を理解するのに役立ちます。

- [`System.Threading.Tasks.Task.WhenAll` を使用して非同期のパフォーマンスを向上させる](../programming-guide/concepts/async/how-to-extend-the-async-walkthrough-by-using-task-whenall.md)。
- [`async` および `await` を使用して複数の Web 要求を並行して作成する](../programming-guide/concepts/async/how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md)。
- [スレッド プールを使用する](../../standard/threading/the-managed-thread-pool.md#using-the-thread-pool)。

## <a name="command-line-args-to-your-program"></a>プログラムのコマンド ライン引数

通常、C# のプログラムにはコマンド ライン引数が含まれます。 次の記事では、そのようなコマンド ライン引数にアクセスして処理する方法について説明します。

- [`for` を使用してすべてのコマンド ライン引数を取得する](../programming-guide/main-and-command-args/how-to-display-command-line-arguments.md)。
