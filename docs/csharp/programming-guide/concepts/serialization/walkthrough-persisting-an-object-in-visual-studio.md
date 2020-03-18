---
title: 'チュートリアル: C# を使用してオブジェクトを永続化する'
ms.date: 04/26/2018
ms.openlocfilehash: 85c5d1b711180eda5734d5860d996242c6bc89d1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79167571"
---
# <a name="walkthrough-persisting-an-object-using-c"></a>チュートリアル: C\# を使用してオブジェクトを永続化する

シリアル化によってインスタンス間でオブジェクトのデータを永続化すると、値を保存しておき、次にそのオブジェクトをインスタンス化するときに、その値を取得することができます。

このチュートリアルでは、基本的な `Loan` オブジェクトを作成し、そのデータをファイルに永続化します。 その後、オブジェクトを再作成するときに、そのファイルからデータを取得します。

> [!IMPORTANT]
> 次の例では、ファイルが存在しない場合は、新しいファイルが作成されます。 アプリケーションでファイルを作成する必要がある場合、そのアプリケーションには、フォルダーに対する `Create` アクセス許可が必要です。 アクセス許可は、アクセス制御リストを使用して設定します。 ファイルが既に存在する場合、アプリケーションに必要なのは下位の `Write` アクセス許可だけです。 可能な場合は、(フォルダーに対して Create アクセス許可を付与するのではなく) 配置時にファイルを作成し、1 つのファイルに対してのみ `Read` アクセス許可を付与する方が安全です。 また、ルート フォルダーや Program Files フォルダーにデータを書き込むより、ユーザー フォルダーに書き込む方が安全です。

> [!IMPORTANT]
> この例では、バイナリ形式のファイルにデータを格納します。 この形式は、パスワードやクレジット カード情報などの重要情報には使用しないでください。

## <a name="prerequisites"></a>前提条件

- ビルドして実行するには、[.NET Core SDK](https://dotnet.microsoft.com/download) をインストールします。

- コード エディターをまだインストールしていなければ、お気に入りのエディターをインストールしてください。

> [!TIP]
> コード エディターをインストールする必要がありますか。 [Visual Studio](https://visualstudio.com/downloads) をお試しください。

- この例では C# 7.3 が必要です。 「[C# 言語のバージョンの選択](../../../language-reference/configure-language-version.md)」を参照してください

オンラインで [.NET サンプルの GitHub リポジトリ](https://github.com/dotnet/samples/tree/master/csharp/serialization)にアクセスしてサンプル コードを確認することができます。

## <a name="creating-the-loan-object"></a>loan オブジェクトを作成する

まず、`Loan` クラスとそのクラスを使用するコンソール アプリケーションを作成します。

1. 新しいアプリケーションを作成します。 「`dotnet new console -o serialization`」と入力して `serialization`という名前のサブディレクトリに新しいコンソール アプリケーションを作成します。
1. エディターでアプリケーションを開き、`Loan.cs` という名前の新しいクラスを追加します。
1. `Loan` クラスに次のコードを追加します。

[!code-csharp[Loan class definition](../../../../../samples/snippets/csharp/serialization/Loan.cs#1)]

また、`Loan` クラスを使用するアプリケーションも作成する必要があります。

## <a name="serialize-the-loan-object"></a>loan オブジェクトをシリアル化する

1. `Program.cs`を開きます。 次のコードを追加します。

[!code-csharp[Create a loan object](../../../../../samples/snippets/csharp/serialization/Program.cs#1)]

`PropertyChanged` イベントのイベント ハンドラーを追加し、`Loan` オブジェクトを変更して変更を表示する処理を行う数行を追加します。 この追加は次のコードで確認できます。

[!code-csharp[Listening for the PropertyChanged event](../../../../../samples/snippets/csharp/serialization/Program.cs#2)]

この時点で、コードを実行して現在の出力を確認することができます。

```console
New customer value: Henry Clay
7.5
7.1
```

このアプリケーションを繰り返し実行すると、常に同じ値が書き込まれます。 プログラムを実行するたびに新しい Loan オブジェクトが作成されます。 実際には利率は定期的に変わりますが、アプリケーションを実行するたびに変わるとは限りません。 シリアル化コードとは、アプリケーションのインスタンス間で最新の利率を保持することを意味します。 次の手順では、Loan クラスにシリアル化を追加して、利率を保持できるようにします。

## <a name="using-serialization-to-persist-the-object"></a>シリアル化を使用したオブジェクトの永続化

Loan クラスの値を永続化するには、まず、クラスを `Serializable` 属性でマークする必要があります。 Loan クラス定義の上に次のコードを追加します。

[!code-csharp[Loan class definition](../../../../../samples/snippets/csharp/serialization/Loan.cs#2)]

<xref:System.SerializableAttribute> は、クラス内のすべての要素がファイルに永続化できることをコンパイラに示します。 `PropertyChanged` イベントは保存する必要があるオブジェクト グラフの一部を表していないので、シリアル化しないようにします。 シリアル化すると、そのイベントにアタッチされているすべてのオブジェクトがシリアル化されます。 <xref:System.NonSerializedAttribute> は、`PropertyChanged` イベント ハンドラーのフィールド宣言に追加できます。

[!code-csharp[Disable serialization for the event handler](../../../../../samples/snippets/csharp/serialization/Loan.cs#3)]

C# 7.3 以降、`field` のターゲット値を使用して、自動実装プロパティのバッキング フィールドに属性をアタッチできるようになりました。 次のコードでは `TimeLastLoaded` プロパティを追加し、シリアル化可能ではないとマークします。

[!code-csharp[Disable serialization for an auto-implemented property](../../../../../samples/snippets/csharp/serialization/Loan.cs#4)]

次に、LoanApp アプリケーションにシリアル化コードを追加します。 クラスをシリアル化してファイルに書き込むには、<xref:System.IO> 名前空間と <xref:System.Runtime.Serialization.Formatters.Binary> 名前空間を使用します。 次のコードのように、必要な名前空間への参照を追加すると、完全修飾名の入力が不要になります。

[!code-csharp[Adding namespaces for serialization](../../../../../samples/snippets/csharp/serialization/Program.cs#3)]

次の手順では、オブジェクトの作成時にファイルからオブジェクトを逆シリアル化するコードを追加します。 次のコードのように、シリアル化されたデータのファイル名を定数としてクラスに追加します。

[!code-csharp[Define the name of the saved file](../../../../../samples/snippets/csharp/serialization/Program.cs#4)]

次に、`TestLoan` オブジェクトを作成する行の後に次のコードを追加します。

[!code-csharp[Read from a file if it exists](../../../../../samples/snippets/csharp/serialization/Program.cs#5)]

まず、ファイルが存在することを確認する必要があります。 ファイルが存在する場合は、バイナリ ファイルを読み取る <xref:System.IO.Stream> クラスと、ファイルを変換する <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> クラスを作成します。 ストリーム型を Loan オブジェクト型に変換する必要もあります。

次に、クラスをファイルにシリアル化するコードを追加する必要があります。 `Main` メソッドの既存のコードの後に次のコードを追加します。

[!code-csharp[Save the existing Loan object](../../../../../samples/snippets/csharp/serialization/Program.cs#6)]

この時点で、アプリケーションを再度ビルドして実行できます。 初めて実行すると、利率は 7.5 から始まり、7.1 に変更されます。 いったんアプリケーションを閉じて、再び実行します。 利率を変更するコードの前でも、保存済みのファイルが読み込まれ、利率は 7.1 であるというメッセージがアプリケーションから出力されます。

## <a name="see-also"></a>参照

- [シリアル化 (C#)](index.md)
- [C# プログラミングガイド](../..//index.md)
