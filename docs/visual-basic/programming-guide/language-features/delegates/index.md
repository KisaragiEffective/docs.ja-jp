---
title: デリゲート
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [Visual Basic]
- Visual Basic code, delegates
ms.assetid: 410b60dc-5e60-4ec0-bfae-426755a2ee28
ms.openlocfilehash: 15b4cb0a038429c5fe67d3e013818a7a2170abcc
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345232"
---
# <a name="delegates-visual-basic"></a>デリゲート (Visual Basic)

デリゲートは、メソッドを参照するオブジェクトです。 デリゲートは他のプログラミング言語で使用される関数ポインターに似ているため、"*タイプ セーフ関数ポインター*" と説明されることがあります。 ただし、関数ポインターとは異なり、Visual Basic デリゲートはクラス <xref:System.Delegate?displayProperty=nameWithType>に基づく参照型です。 デリゲートは、共有メソッド (特定のクラスのインスタンスがなくても呼び出すことのできるメソッド) とインスタンス メソッドの両方を参照できます。

## <a name="delegates-and-events"></a>デリゲートとイベント

デリゲートは、呼び出し側プロシージャと呼び出されるプロシージャの間の媒介手段が必要な状況で役立ちます。 たとえば、イベントを発生させるオブジェクトが、異なる状況で別個のイベント ハンドラーを呼び出せるようにする必要がある場合があります。 しかし、イベントを発生させるオブジェクトは、どのイベント ハンドラーが特定のイベントを処理するかをあらかじめ把握できません。 Visual Basic を使用すると、`AddHandler` ステートメントを使用するときにデリゲートを作成することによって、イベントハンドラーをイベントに動的に関連付けることができます。 実行時に、デリゲートによって適切なイベント ハンドラーに呼び出しが転送されます。

独自のデリゲートを作成することもできますが、ほとんどの場合 Visual Basic デリゲートが作成され、詳細が処理されます。 たとえば、`Event` ステートメントは、`<EventName>EventHandler` ステートメントを含むクラスの入れ子のクラスとして、`Event` という名前のデリゲート クラスをイベントと同じシグネチャを使用して暗黙的に定義します。 `AddressOf` ステートメントは、特定のプロシージャを参照するデリゲートのインスタンスを暗黙的に作成します。 次の 2 つのコード行は同等です。 最初の行では、`EventHandler` のインスタンスが明示的に作成され、メソッド `Button1_Click` への参照が引数として渡されます。 2 番目の行は、より便利な方法で同じことを実行します。

[!code-vb[VbVbalrDelegates#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#6)]

コンパイラがデリゲートの種類をコンテキストから判断できる場合は、デリゲートの簡単な作成方法を使用できます。

## <a name="declaring-events-that-use-an-existing-delegate-type"></a>既存の種類のデリゲートを使用するイベントの宣言

状況によっては、基になるデリゲートとして既存のデリゲートを使用するイベントを宣言する必要が生じることがあります。 その方法を次の構文に示します。

[!code-vb[VbVbalrDelegates#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#7)]

これは、同じハンドラーに複数のイベントをルーティングする場合に役立ちます。

## <a name="delegate-variables-and-parameters"></a>デリゲート変数とパラメーター

デリゲートは、フリースレッド処理や、実行時に異なるバージョンの関数を呼び出す必要があるプロシージャなど、イベントに関連しない他のタスクにも使用できます。

たとえば、自動車の名前のリスト ボックスを含む、項目別広告のアプリケーションがあるとします。 広告はタイトル(通常は自動車の型式) で並べ替えられます。 問題が発生する可能性があるのは、型式の前に年式が含まれている自動車があるときです。 問題になるのは、リスト ボックスに組み込まれた並べ替え機能の並べ替えの基準が文字コードだけであることです。このため、日付で始まるすべての広告が最初に配置され、その後に型式で始まる広告が配置されます。

この問題を解決するために、クラスに並べ替えプロシージャを作成できます。このプロシージャは、ほとんどのリスト ボックスに対応する標準的なアルファベット順の並べ替えを使用しますが、実行時に自動車広告用のカスタム並べ替えプロシージャに切り替えることができます。 このプロシージャを作成するには、デリゲートを使用して実行時にカスタム並べ替えプロシージャを並べ替えクラスに渡します。

## <a name="addressof-and-lambda-expressions"></a>AddressOf とラムダ式

各デリゲート クラスでは、オブジェクト メソッドの仕様を渡すコンストラクターを定義します。 デリゲート コンス トラクターに渡す引数は、メソッドへの参照、またはラムダ式である必要があります。

メソッドへの参照を指定するには、次の構文を使用します。

`AddressOf` [`expression`.]`methodName`

コンパイル時の `expression` の型は、シグネチャがデリゲート クラスのシグネチャと同じで、指定された名前のメソッドを持つクラスまたはインターフェイスである必要があります。 `methodName` は、共有メソッドまたはインスタンス メソッドのいずれかにできます。 クラスの既定メソッドに対してデリゲートを作成する場合も、`methodName` は省略できません。

ラムダ式を指定するには、次の構文を使用します。

`Function` ([`parm` As `type`, `parm2` As `type2`, ...]) `expression`

次の例では、デリゲートの参照を指定するために使用される `AddressOf` 式とラムダ式を示します。

[!code-vb[VbVbalrDelegates#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class2.vb#15)]

関数のシグネチャは、デリゲート型のシグネチャと一致している必要があります。 ラムダ式について詳しくは、「[ラムダ式](../../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)」をご覧ください。 ラムダ式のその他の例と `AddressOf` のデリゲートへの代入については、「[厳密でないデリゲート変換](../../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)」を参照してください。

## <a name="related-topics"></a>関連トピック

|タイトル|説明|
|-----------|-----------------|
|[方法: デリゲート メソッドを呼び出す](../../../../visual-basic/programming-guide/language-features/delegates/how-to-invoke-a-delegate-method.md)|メソッドをデリゲートに関連付け、デリゲートからそのメソッドを呼び出す方法の例を示します。|
|方法 : [Visual Basic でプロシージャを別のプロシージャに渡す](../../../../visual-basic/programming-guide/language-features/delegates/how-to-pass-procedures-to-another-procedure.md)|デリゲートを使用してプロシージャを別のプロシージャに渡す方法を示します。|
|[厳密でないデリゲート変換](../../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)|シグネチャが同じでない場合でも Sub や関数をデリゲートやハンドラーに割り当てる方法を説明します。|
|[イベント](../../../../visual-basic/programming-guide/language-features/events/index.md)|Visual Basic のデリゲートの概要について説明します。|
