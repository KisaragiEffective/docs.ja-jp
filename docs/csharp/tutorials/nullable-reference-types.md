---
title: null 許容参照型を使用して設計する
description: この高度なチュートリアルでは、null 許容参照型の概要について説明します。 参照値で null がいつ許容されるかに関する設計意図を表すことで、コンパイラで null が許容されるようにします。
ms.date: 02/19/2019
ms.technology: csharp-null-safety
ms.custom: mvc
ms.openlocfilehash: b00050c1d151b95e330f94eb9393a4031e47d5a8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240068"
---
# <a name="tutorial-express-your-design-intent-more-clearly-with-nullable-and-non-nullable-reference-types"></a>チュートリアル: null 許容参照型と null 非許容参照型を使用して設計意図をもっと明確に示す

C# 8.0 には [null 許容参照型](../nullable-references.md)が導入されています。これは、null 許容値型が値型を補完するのと同じように、参照型を補完するものです。 型に **を追加することで、変数が**null 許容参照型`?`であることを宣言します。 たとえば、`string?` は、null が許容される `string` を表します。 これらの新しい型を使用して、一部の変数では*常に値を持つ必要があり*、他の変数では*値が欠落することも可能である*という設計意図をさらに明確に示すことができます。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - null 許容参照型と null 非許容参照型を設計に組み込む。
> - コード全体で null 許容参照型をチェックできるようにする。
> - コンパイラでこれらの設計上の決定が適用されるコードを記述する。
> - 自分の設計の中で null 許容参照機能を使用する。

## <a name="prerequisites"></a>前提条件

お使いのコンピューターを、.NET Core が実行されるように設定する必要があります。C# 8.0 コンパイラも実行されるようにします。 C# 8.0 コンパイラは、[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) または [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0) で使用できます。

このチュートリアルでは、.NET と、C# と Visual Studio または .NET Core CLI のいずれかに精通していることを前提としています。

## <a name="incorporate-nullable-reference-types-into-your-designs"></a>null 許容参照型と null 非許容参照型を設計に組み込む

このチュートリアルでは、アンケートの実行をモデル化するライブラリを構築します。 コードでは、null 許容参照型と null 非許容参照型の両方を使用して、現実世界の概念を表します。 アンケートの質問は null することはできません。 回答者が質問に答えたくない場合があります。 この場合、応答は `null` になる可能性があります。

このサンプルで記述するコードはそのような意図を表現し、コンパイラにその意図が適用されます。

## <a name="create-the-application-and-enable-nullable-reference-types"></a>アプリケーションを作成し、null 許容参照型を有効にする

新しいコンソール アプリケーションを作成します。Visual Studio を使用するか、コマンド ラインで `dotnet new console` を使用します。 アプリケーションに `NullableIntroduction` という名前を付けます。 アプリケーションを作成したら、プロジェクト全体が、有効な **null 許容注釈コンテキスト**でコンパイルされるように指定する必要があります。 *.csproj* ファイルを開き、`Nullable` 要素を `PropertyGroup` 要素に追加します。 値を `enable`に設定します。 C# 8.0 プロジェクトであっても、**null 許容参照型**機能を選択する必要があります。 これは、機能をオンにすると、既存の参照変数宣言が **null 非許容参照型**になるためです。 その決定は既存のコードで適切な null チェックが行われていない場合に問題を発見するのに役立ちますが、元の設計意図が正確に反映されない可能性があります。

```xml
<Nullable>enable</Nullable>
```

### <a name="design-the-types-for-the-application"></a>アプリケーション用の型を設計する

このアンケート アプリケーションでは、いくつかのクラスを作成する必要があります。

- 質問の一覧をモデル化するクラス。
- アンケートに応じてもらうために連絡した人物の一覧をモデル化するクラス。
- アンケートに応じた人物から得た回答をモデル化するクラス。

これらの型では、null 許容参照型と null 非許容参照型の両方を利用して、必要なメンバーと省略可能なメンバーを表現します。 null 許容参照型により、次の設計意図が明確に伝わります。

- アンケートの一部である質問を null にすることはできません。空の質問は意味がありません。
- 回答者を null にすることはできません。 回答者が参加を辞退している場合でも、連絡した人物を追跡したいからです。
- 質問に対する回答を null にすることができます。 回答者は、一部の質問またはすべての質問の回答を拒否できます。

これまで C# でプログラミングしている場合は、`null` 値を許容する参照型に慣れているため、null を許容しないインスタンスを宣言する機会がなかったかもしれません。

- 質問のコレクションは null を許容しないものにする必要があります。
- 回答者のコレクションは null を許容しないものにする必要があります。

コードを記述していくにつれて、参照の既定値としての null 非許容参照型によって、<xref:System.NullReferenceException> を引き起こす可能性がある一般的なミスを回避できることを理解できるでしょう。 このチュートリアルから学ぶことの 1 つは、`null` にすることができる変数とできない変数を決定することです。 この言語には、このような決定を表現するための構文が提供されていませんでした。 今、それが実現しています。

ビルドするアプリで、次の手順を行います。

1. アンケートを作成し、そこに質問を追加します。
1. アンケートの回答者の擬似乱数セットを作成します。
1. 回答されたアンケートのサイズが目標数に達するまで、回答者に連絡します。
1. アンケートの回答に関する重要な統計情報を書き込みます。

## <a name="build-the-survey-with-nullable-and-non-nullable-types"></a>null 許容型と null 非許容型を含むアンケートを作成する

最初に記述するコードによって、アンケートが作成されます。 アンケートの質問とアンケートの実行をモデル化するクラスを記述します。 アンケートには、回答の形式によって区別される 3 種類の質問があります (はい/いいえで回答するもの、番号で回答するもの、およびテキストで回答するもの)。 `public SurveyQuestion` クラスを作成します。

```csharp
namespace NullableIntroduction
{
    public class SurveyQuestion
    {
    }
}
```

コンパイラでは、有効な null 許容注釈コンテキスト内のコードについては、すべての参照型変数の宣言が **null 非許容**参照型として解釈されます。 次のコードに示すように、質問のテキストと質問の種類のプロパティを追加することで、最初の警告を確認できます。

```csharp
namespace NullableIntroduction
{
    public enum QuestionType
    {
        YesNo,
        Number,
        Text
    }

    public class SurveyQuestion
    {
        public string QuestionText { get; }
        public QuestionType TypeOfQuestion { get; }
    }
}
```

`QuestionText` を初期化していないため、コンパイラによって、null を許容しないプロパティが初期化されていないことを示す警告が発行されます。 この設計では、質問のテキストを null 以外にする必要があるため、初期化するためのコンストラクターを追加します。`QuestionType` 値も同様にします。 完成したクラス定義は、次のコードのようになります。

[!code-csharp[DefineQuestion](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyQuestion.cs)]

コンストラクターを追加すると、警告が解除されます。 コンストラクターの引数も、null 非許容参照型であるため、コンパイラによる警告は発行されません。

次に、`public` という名前の `SurveyRun` クラスを作成します。 次のコードに示すように、このクラスには、アンケートに質問を追加する `SurveyQuestion` オブジェクトとメソッドの一覧が含まれます。

```csharp
using System.Collections.Generic;

namespace NullableIntroduction
{
    public class SurveyRun
    {
        private List<SurveyQuestion> surveyQuestions = new List<SurveyQuestion>();

        public void AddQuestion(QuestionType type, string question) =>
            AddQuestion(new SurveyQuestion(type, question));
        public void AddQuestion(SurveyQuestion surveyQuestion) => surveyQuestions.Add(surveyQuestion);
    }
}
```

前と同じように、この一覧オブジェクトを null 以外の値に初期化する必要があります。そうしないとコンパイラによって警告が発行されます。 `AddQuestion` の 2 つ目のオーバーロードでは null のチェックは必要ないため、それが実行されることはありません。その変数は null 非許容であることが宣言されています。 その値は `null` になることはできません。

お使いのエディターで *Program.cs* に切り替え、`Main` の内容を次のコード行に置き換えます。

[!code-csharp[AddQuestions](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#AddQuestions)]

プロジェクト全体が、有効な null 許容注釈コンテキスト内にあるので、null 非許容参照型が必要なメソッドに `null` を渡すと、警告が発生します。 次の行を `Main` に追加して試してください。

```csharp
surveyRun.AddQuestion(QuestionType.Text, default);
```

## <a name="create-respondents-and-get-answers-to-the-survey"></a>回答者を作成し、アンケートに対する回答を取得する

次に、アンケートに対する回答を生成するコードを記述します。 このプロセスには、いくつかの小さいタスクが含まれます。

1. 回答者オブジェクトを生成するメソッドを作成します。 これらは、アンケートへの入力を求められる人物を表します。
1. 回答者にアンケートを依頼し、回答を収集するか、回答者が回答しなかったことを示すデータを収集することをシミュレートするロジックを構築します。
1. 十分な数の回答者がアンケートに回答するまで、これを繰り返します。

アンケートの回答を表すクラスが必要なので、ここでそれを追加します。 null 許容のサポートを有効にします。 次のコードに示すように、`Id` プロパティとそれを初期化するコンストラクターを追加します。

```csharp
namespace NullableIntroduction
{
    public class SurveyResponse
    {
        public int Id { get; }

        public SurveyResponse(int id) => Id = id;
    }
}
```

次に、`static` メソッドを追加し、ランダム ID を生成することで新しい参加者を作成します。

[!code-csharp[GenerateRespondents](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#Random)]

このクラスの主な役割は、アンケートの質問に対する参加者の回答を生成することです。 この役割には、いくつかの手順があります。

1. アンケートへの参加を依頼します。 回答者が同意しない場合は、応答の欠落 (つまり null) が返されます。
1. 各質問を表示し、回答を記録します。 回答も欠落する (つまり null になる) 可能性があります。

`SurveyResponse` クラスに次のコードを追加します。

[!code-csharp[AnswerSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#AnswerSurvey)]

アンケートの回答用のストレージは `Dictionary<int, string>?` であり、null が可能であることを示しています。 新しい言語機能を使用して、コンパイラーと後日コードを読む人の両方に対して、設計意図が宣言されています。 先に `surveyResponses` 値のチェックを行わずに `null` を逆参照した場合は、コンパイラの警告が表示されます。 `AnswerSurvey` メソッドで警告が表示されないのは、上記で `surveyResponses` 変数が null 以外の値に設定されたことをコンパイラが判断できるためです。

欠落している回答に対して `null` を使用すると、null 許容参照型を処理するための重要なポイントが強調表示されます。目標は、プログラムからすべての `null` 値を削除することではありません。 本当の目標は、記述しているコードで設計の意図が確実に表されるようにすることです。 欠落値は、コードでの表現に必要な概念です。 `null` 値は、これらの欠落値を表現する明確な方法です。 すべての `null` を削除しようとしても、`null` を使わずにそれらの欠落値を表すための他の何らかの方法を定義することになるだけです。

次に、`PerformSurvey` クラス内に `SurveyRun` メソッドを記述する必要があります。 `SurveyRun` クラスに次のコードを追加します。

[!code-csharp[PerformSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyRun.cs#PerformSurvey)]

ここでも、null を許容する `List<SurveyResponse>?` の選択によって、応答で null が可能であることが示されす。 これは、アンケートがまだ回答者に表示されていないことを示します。 十分な数の同意が得られるまで回答者が追加されることに注意してください。

アンケートを実行する最後の手順は、`Main` メソッドの最後にアンケートを実行する呼び出しを追加することです。

[!code-csharp[RunSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#RunSurvey)]

## <a name="examine-survey-responses"></a>アンケートの回答を調べる

最後の手順は、アンケートの結果を表示することです。 記述したクラスの多くにコードを追加します。 このコードでは、null 許容参照型と null 非許容参照型を区別する値を示します。 `SurveyResponse` クラスに次の 2 つの式形式メンバーを追加することから始めます。

[!code-csharp[ReportResponses](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#SurveyStatus)]

`surveyResponses` は null 許容参照型であるため、それを逆参照する前にチェックが必要です。 `Answer` メソッドからは null 非許容の文字列が返されます。そのため、null 合体演算子を使用して、不足している回答のケースをカバーする必要があります。

次に、次の 3 つの式形式メンバーを `SurveyRun` クラスに追加します。

[!code-csharp[ReportResults](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyRun.cs#RunReport)]

`AllParticipants` メンバーでは、`respondents` 変数は null の場合があるが、戻り値を null にすることはできないことを考慮する必要があります。 この式を `??` とその後ろの空のシーケンスを削除することで変更すると、コンパイラーによって、メソッドで `null` が返され、その戻り値のシグネチャで null 非許容型が返される可能性があることが警告されます。

最後に、次のループを `Main` メソッドの末尾に追加します。

[!code-csharp[DisplaySurveyResults](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#WriteAnswers)]

基になるインターフェースを非許容参照型を返すように設計しているため、このコードでは `null` のチェックは必要ありません。

## <a name="get-the-code"></a>コードの入手

[csharp/NullableIntroduction](https://github.com/dotnet/samples) フォルダーの [samples](https://github.com/dotnet/samples/tree/master/csharp/NullableIntroduction) リポジトリから、完成したチュートリアルのコードを取得できます。

null 許容参照型と null 非許容参照型の間で型宣言を変更することで試してください。 それによって生成される警告が変わることを確認して、`null` を間違って逆参照することがないようにしてください。

## <a name="next-steps"></a>次のステップ

既存のアプリケーションを null 許容参照型を使用するように移行することについてさらに詳しく学習します。
> [!div class="nextstepaction"]
> [Null 許容参照型を使用するようにアプリケーションをアップグレードする](upgrade-to-nullable-references.md)
