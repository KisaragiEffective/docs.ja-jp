---
title: .NET での例外の処理とスロー
ms.date: 06/19/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- exceptions [.NET], handling
- runtime, exceptions
- filtering exceptions
- errors [.NET], exceptions
- exceptions [.NET], throwing
- exceptions [.NET]
- common language runtime, exceptions
ms.assetid: f99a1d29-a2a8-47af-9707-9909f9010735
ms.openlocfilehash: 8e78b2a8d7a815637e143eeb88bcfb51ded33771
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75741346"
---
# <a name="handling-and-throwing-exceptions-in-net"></a>.NET での例外の処理とスロー

アプリケーションは、実行中に発生するエラーを一貫した方法で処理できなければなりません。 .NET では、一貫した方法でアプリケーションにエラーを通知するためのモデルが用意されています。 .NET 操作では、例外をスローすることによって障害の発生を示します。

## <a name="exceptions"></a>例外

例外とは、プログラムを実行することによって発生するエラー状態または予期しない動作のことです。 例外がスローされる原因として、コードまたは呼び出したコード (たとえば共有ライブラリ) 内に障害がある、オペレーティング システム リソースを使用できない、予期しない状態 (たとえば検証できないコード) をランタイムが検出したなどがあります。 アプリケーションは、他の状態からではなく、これらの状態のうちのいくつかから回復できます。 ほとんどのアプリケーション例外から回復できますが、ほとんどのランタイム例外からは回復できません。

.NET では、例外は、<xref:System.Exception?displayProperty=nameWithType> クラスから継承されるオブジェクトです。 例外は問題が発生したコード領域からスローされます。 例外は、アプリケーションが処理するかプログラムが終了するまで、スタックに渡されます。

## <a name="exceptions-vs-traditional-error-handling-methods"></a>例外:従来のエラー処理メソッド

言語のエラー処理モデルは従来、エラーを検出してそれに対応したハンドラーを見つける言語固有の方法か、オペレーティング システムが備えているエラー処理機構のいずれかを使用していました。 .NET が例外処理を実装する方法は、次の利点をもたらします。

- 例外のスローと処理は、.NET プログラミング言語では同じように機能します。

- 例外を処理するための特定の言語構文を必要とせず、各言語が独自の構文を定義できます。

- 例外は、プロセス間、さらにはコンピューターの境界を越えてスローできます。

- プログラムの信頼性を高めるための例外処理コードをアプリケーションに追加できます。

例外には、リターン コードなどの他のエラー通知メソッドに優る利点があります。 例外がスローされ、それを処理しないと、ランタイムによってアプリケーションが終了されるため、エラーが見過ごされることはありません。 無効な値は、エラーのリターン コードの確認に失敗したコードの結果として、システムを経由した伝達を続行しません。

## <a name="common-exceptions"></a>一般的な例外

次の表は、一般的な例外とそれらの原因の例をいくつか示しています。

| 例外の種類 | [説明] | 例 |
| -------------- | ----------- | ------- |
| <xref:System.Exception> | すべての例外の基底クラスです。 | なし (この例外の派生クラスを使用)。 |
| <xref:System.IndexOutOfRangeException> | 配列のインデックスが誤っている場合にのみ、ランタイムによってスローされます。 | 次のように、配列に対して配列の有効範囲外のインデックスを付けた場合。 <br /> `arr[arr.Length+1]` |
| <xref:System.NullReferenceException> | null オブジェクトが参照された場合にのみ、ランタイムによってスローされます。 | `object o = null;` <br /> `o.ToString();` |
| <xref:System.InvalidOperationException> | 無効な状態の場合にメソッドによってスローされます。 | 基になるコレクションから項目を削除した後での、`Enumerator.MoveNext()` の呼び出しです。 |
| <xref:System.ArgumentException> | すべての引数の例外の基底クラスです。 | なし (この例外の派生クラスを使用)。 |
| <xref:System.ArgumentNullException> | null の引数を許可しないメソッドによってスローされます。 | `String s = null;` <br /> `"Calculate".IndexOf(s);`|
| <xref:System.ArgumentOutOfRangeException> | 引数が特定の範囲内にあることを検査するメソッドによってスローされます。 | `String s = "string";` <br /> `s.Substring(s.Length+1);` |

## <a name="see-also"></a>参照

- [Exception クラスとプロパティ](exception-class-and-properties.md)
- [方法: Try ブロックと Catch ブロックを使用して例外をキャッチする](how-to-use-the-try-catch-block-to-catch-exceptions.md)
- [方法: catch ブロックで特定の例外を使用する](how-to-use-specific-exceptions-in-a-catch-block.md)
- [方法: 例外を明示的にスローする](how-to-explicitly-throw-exceptions.md)
- [方法: ユーザー定義の例外を作成する](how-to-create-user-defined-exceptions.md)
- [ユーザー フィルター例外ハンドラーの使用](using-user-filtered-exception-handlers.md)
- [方法: finally ブロックを使用する](how-to-use-finally-blocks.md)
- [COM 相互運用の例外の処理](handling-com-interop-exceptions.md)
- [例外の推奨事項](best-practices-for-exceptions.md)
- [ランタイム時の例外についてすべての開発者が知っておくべきこと](https://github.com/dotnet/runtime/blob/master/docs/design/coreclr/botr/exceptions.md)
