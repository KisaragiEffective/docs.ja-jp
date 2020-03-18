---
title: エラーの種類
ms.date: 07/20/2015
helpviewer_keywords:
- exceptions, types
- errors [Visual Basic], types
- errors [Visual Basic], logic
- errors [Visual Basic], syntax
- logic errors [Visual Basic], Visual Basic
- run-time errors [Visual Basic], types of errors
- syntax errors [Visual Basic], Visual Basic
ms.assetid: 3048aabf-8c97-4e13-9150-853769cb5f6f
ms.openlocfilehash: 04320c7a2fd27749e6de24f0ad21cc51c86ddda2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345156"
---
# <a name="error-types-visual-basic"></a>エラーの種類 (Visual Basic)
Visual Basic では、エラーは構文エラー、実行時エラー、および論理エラーの3つのカテゴリのいずれかに分類されます。

## <a name="syntax-errors"></a>構文エラー
 *構文エラー*は、コードの記述中に表示されるエラーです。 Visual Studio を使用している場合、コード**エディター**ウィンドウでコードを入力すると、Visual Basic によってコードがチェックされ、単語のスペルミスや言語要素の不適切な使用などの誤りが発生した場合に警告が表示されます。 コマンドラインからコンパイルした場合、Visual Basic には、構文エラーに関する情報を含むコンパイラエラーが表示されます。 構文エラーは、最も一般的なエラーの種類です。 コーディング環境では、問題が発生するとすぐに簡単に修正できます。

> [!NOTE]
> `Option Explicit` ステートメントは、構文エラーを回避するための1つの手段です。 これにより、アプリケーションで使用されるすべての変数が事前に宣言されます。 そのため、これらの変数がコード内で使用されている場合は、すべての活字エラーが即座にキャッチされ、修正できます。

## <a name="run-time-errors"></a>実行時エラー
 *実行時エラー*は、コードをコンパイルして実行した後にのみ表示されるエラーです。 このようなコードには、構文エラーがないにもかかわらず、実行できないと思われるコードが含まれます。 たとえば、ファイルを開くためのコード行が正しく記述されているとします。 ただし、ファイルが存在しない場合、アプリケーションはファイルを開くことができず、例外がスローされます。 ほとんどのランタイムエラーは、問題のあるコードを書き直すか、[例外処理](../../language-reference/statements/try-catch-finally-statement.md)を使用して再コンパイルし、再実行することによって修正できます。
  
## <a name="logic-errors"></a>ロジックエラー
 *ロジックエラー*は、アプリケーションが使用された後に表示されるエラーです。 ほとんどの場合、開発者によって作成された仮定に誤りがあるか、またはユーザーの操作に対する応答として望ましくないまたは予期しない結果になります。 たとえば、誤って入力されたキーによってメソッドに誤った情報が提供されることがあります。そうでない場合は、常に有効な値がメソッドに渡されると想定できます。 [例外処理](../../language-reference/statements/try-catch-finally-statement.md)を使用してロジックエラーを処理することもできますが (たとえば、引数が `Nothing` であるかどうかをテストし、<xref:System.ArgumentNullException>をスローするなど)、ほとんどの場合、ロジックのエラーを修正し、アプリケーションを再コンパイルすることによって対処する必要があります。

## <a name="see-also"></a>参照

- [Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)
- [デバッガーの基本事項](/visualstudio/debugger/debugger-feature-tour)
