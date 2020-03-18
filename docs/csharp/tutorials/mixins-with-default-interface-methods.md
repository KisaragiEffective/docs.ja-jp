---
title: 既定のインターフェイス メソッドを使用して mixin 型を作成する
description: 既定のインターフェイス メンバーを使用すると、実装のためにオプションの既定の実装を使用してインターフェイスを拡張できます。
ms.technology: csharp-advanced-concepts
ms.date: 10/04/2019
ms.openlocfilehash: aaf8d34e27c9c56d95560656eb7a7b24b152c053
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240107"
---
# <a name="tutorial-mix-functionality-in-when-creating-classes-using-interfaces-with-default-interface-methods"></a>チュートリアル: 既定のインターフェイス メソッドでインターフェイスを使用してクラスを作成するときの機能の混合

.NET Core 3.0 上の C# 8.0 以降では、インターフェイスのメンバーを宣言するときに実装を定義できます。 この機能により、インターフェイスで宣言された機能の既定の実装を定義できるという新機能が提供されます。 クラスでは、機能をオーバーライドする場合、既定の機能を使用する場合、および個別の機能のサポートを宣言しない場合を選択できます。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> * 個別の機能を記述する実装を備えたインターフェイスを作成します。
> * 既定の実装を使用するクラスを作成します。
> * 既定の実装の一部またはすべてをオーバーライドするクラスを作成します。

## <a name="prerequisites"></a>必須コンポーネント

お使いのコンピューターを、.NET Core が実行されるように設定する必要があります。C# 8.0 コンパイラも実行されるようにします。 C# 8.0 コンパイラは [Visual Studio 2019 バージョン 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) または [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core) 以降から使用できます。

## <a name="limitations-of-extension-methods"></a>拡張メソッドの制限事項

インターフェイスの一部として表示される動作を実装する方法の 1 つとして、既定の動作を提供する[拡張メソッド](../programming-guide/classes-and-structs/extension-methods.md)を定義することがあります。 インターフェイスでは、そのインターフェイスを実装するクラスに対してより広い範囲の外部からのアクセスを提供すると同時に、最小セットのメンバーを宣言します。 たとえば、<xref:System.Linq.Enumerable> の拡張メソッドは、任意のシーケンスの実装を LINQ クエリのソースとして提供します。

拡張メソッドは、コンパイル時に変数の宣言型を使用して解決されます。 インターフェイスを実装するクラスを使用すると、すべての拡張メソッドに対してより適切な実装を提供できます。 コンパイラで実装を選択できるようにするには、変数宣言が実装する型と一致している必要があります。 コンパイル時の型がインターフェイスと一致する場合、メソッドの呼び出しは拡張メソッドに解決されます。 拡張メソッドに関するもう 1 つの問題は、拡張メソッドを含むクラスにアクセスできる場所であれば、これらのメソッドにアクセスできることです。 クラスでは、拡張メソッドで宣言された機能を提供する必要があるかどうかを宣言できません。

C# 8.0 以降では、既定の実装をインターフェイス メソッドとして宣言できます。 その後は、すべてのクラスで自動的に既定の実装が使用されます。 より適切な実装を提供できるクラスでは、インターフェイス メソッドの定義をより適切なアルゴリズムでオーバーライドできます。 ある意味では、この手法は[拡張メソッド](../programming-guide/classes-and-structs/extension-methods.md)を使用する方法と似ています。

この記事では、既定のインターフェイスの実装で新しいシナリオを実現する方法について説明します。

## <a name="design-the-application"></a>アプリケーションを設計する

ホーム オートメーション アプリケーションについて考えてみましょう。 家庭内で使用される可能性がある照明とインジケーターには、おそらくさまざまな種類があります。 すべての照明が、オン/オフを切り替え、現在の状態を報告する API をサポートする必要があります。 一部の照明とインジケーターでは、次のような他の機能がサポートされている場合があります。

- 照明をオンにして、タイマーの後にオフにする。
- 一定の期間、照明を点滅させる。

これらの拡張機能の一部は、最小セットをサポートするデバイスでエミュレートできます。 これは、既定の実装を提供することを示します。 より多くの機能が組み込まれているデバイスの場合、デバイス ソフトウェアではネイティブ機能を使用します。 他の照明については、インターフェイスを実装し、既定の実装を使用することを選択できます。

このシナリオでは、拡張メソッドよりも既定のインターフェイス メンバーの方が適したソリューションです。 クラス作成者は、実装するインターフェイスを制御できます。 選択したインターフェイスはメソッドとして利用できます。 また、既定のインターフェイス メソッドは既定で仮想であるため、メソッドのディスパッチでは常にクラス内の実装が選択されます。

これらの違いを示すコードを作成してみましょう。

## <a name="create-interfaces"></a>インターフェイスを作成する

まず、すべての照明の動作を定義するインターフェイスを作成します。

[!code-csharp[Declare base interface](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/UnusedExampleCode.cs?name=SnippetILightInterfaceV1)]

基本的な天井の照明器具では、次のコードに示すようにこのインターフェイスを実装する場合があります。

[!code-csharp[First overhead light](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/UnusedExampleCode.cs?name=SnippetOverheadLightV1)]

このチュートリアルでは、IoT デバイスを駆動していませんが、コンソールにメッセージを書き込んでそのアクティビティをエミュレートします。 家を自動化せずにコードを調べることができます。

次に、タイムアウト後に自動的にオフにできる照明のインターフェイスを定義します。

[!code-csharp[pure Timer interface](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/UnusedExampleCode.cs?name=SnippetPureTimerInterface)]

天井の照明に基本的な実装を追加することもできますが、おすすめのソリューションは、このインターフェイス定義を変更して `virtual` の既定の実装を提供することです。

[!code-csharp[Timer interface](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/ITimerLight.cs?name=SnippetTimerLightFinal)]

この変更を追加することで、`OverheadLight` クラスで、インターフェイスのサポートを宣言してタイマー関数を実装できます。

```csharp
public class OverheadLight : ITimerLight { }
```

照明の種類によっては、より高度なプロトコルがサポートされている場合があります。 次のコードに示すように、`TurnOnFor` の独自の実装を提供できます。

[!code-csharp[Override the timer function](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/HalogenLight.cs?name=SnippetHalogenLight)]

仮想クラス メソッドのオーバーライドとは異なり、`HalogenLight` クラスの `TurnOnFor` の宣言では、`override` キーワードは使用されません。

## <a name="mix-and-match-capabilities"></a>機能の混在と対応付け

より高度な機能を導入すると、既定のインターフェイス メソッドの利点が明らかになります。 インターフェイスを使用すると、機能を混在させて対応付けることができます。 また、各クラスの作成者は、既定の実装とカスタム実装のいずれかを選択できるようになります。 点滅する照明の既定の実装を持つインターフェイスを追加してみましょう。

[!code-csharp[Define the blinking light interface](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/IBlinkingLight.cs?name=SnippetBlinkingLight)]

既定の実装では、任意の照明を点滅させることができます。 天井の照明には、既定の実装を使用して、タイマーと点滅の両方の機能を追加できます。

[!code-csharp[Use the default blink function](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/OverheadLight.cs?name=SnippetOverheadLight)]

新しい照明の種類である `LEDLight` では、タイマー関数と点滅関数の両方を直接サポートします。 この照明のスタイルでは、`ITimerLight` と `IBlinkingLight` の両方のインターフェイスを実装し、`Blink` メソッドをオーバーライドします。

[!code-csharp[Override the blink function](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/LEDLight.cs?name=SnippetLEDLight)]

`ExtraFancyLight` は、点滅とタイマーの両方の関数を直接サポートしている可能性があります。

[!code-csharp[Override the blink and timer function](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/ExtraFancyLight.cs?name=SnippetExtraFancyLight)]

以前に作成した `HalogenLight` は、点滅をサポートしていません。 そのため、サポートされているインターフェイスの一覧に `IBlinkingLight` を追加しないでください。

## <a name="detect-the-light-types-using-pattern-matching"></a>パターン マッチングを使用して照明の種類を検出する

次に、テスト コードを書いてみましょう。 C# の[パターン マッチング](../pattern-matching.md)機能を利用して、サポートされているインターフェイスを調べることで、照明の機能を決定することができます。  次のメソッドでは、各照明のサポートされている機能を実行します。

[!code-csharp[Test a light's capabilities](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/Program.cs?name=SnippetTestLightFunctions)]

`Main` メソッドの次のコードでは、各照明の種類を順番に作成し、その照明をテストします。

[!code-csharp[Test a light's capabilities](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/Program.cs?name=SnippetMainMethod)]

## <a name="how-the-compiler-determines-best-implementation"></a>コンパイラで最適な実装を決定する方法

このシナリオは、実装のない基底インターフェイスを示しています。 `ILight` インターフェイスにメソッドを追加すると、新たな複雑さが生じます。 既定のインターフェイス メソッドを管理する言語規則により、複数の派生インターフェイスを実装する具象クラスへの影響が最小限に抑えられます。 元のインターフェイスを新しいメソッドで拡張して、その使用方法がどのように変わるかを説明します。 すべてのインジケーター照明では、その電源の状態を列挙値として報告できます。

[!code-csharp[Enumeration for power status](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/ILight.cs?name=SnippetPowerStatus)]

既定の実装では AC 電源が想定されています。

[!code-csharp[Report a default power status](~/samples/snippets/csharp/tutorials/mixins-with-interfaces/ILight.cs?name=SnippetILightInterface)]

`ExtraFancyLight` が `ILight` インターフェイスと `ITimerLight` および `IBlinkingLight` 両方の派生インターフェイスのサポートを宣言していても、これらの変更は正常にコンパイルされます。 `ILight` インターフェイスで宣言される "最も近い" 実装は 1 つのみです。 オーバーライドを宣言した任意のクラスは、1 つの "最も近い" 実装になります。 上記のクラスの例では、他の派生インターフェイスのメンバーをオーバーライドしています。

複数の派生インターフェイスで同じメソッドをオーバーライドすることは避けてください。 このようにすると、クラスで両方の派生インターフェイスが実装されるたびに、あいまいなメソッド呼び出しが作成されます。 コンパイラでは、より適切な 1 つのメソッドを選択できないため、エラーが発行されます。 たとえば、`IBlinkingLight` と `ITimerLight` の両方で `PowerStatus` のオーバーライドが実装されている場合、`OverheadLight` にはより具体的なオーバーライドを用意する必要があります。 そうしないと、コンパイラでは 2 つの派生インターフェイスで実装を選択できません。 通常、インターフェイスの定義を小さく保ち、1 つの機能に集中することで、この状況を回避できます。 このシナリオでは、照明の各機能は独自のインターフェイスです。複数のインターフェイスはクラスからのみ継承されます。

このサンプルでは、クラスに混在させることができる個別の機能を定義できる 1 つのシナリオを示します。 サポートされる機能のセットを宣言するには、クラスがサポートするインターフェイスを宣言します。 仮想の既定のインターフェイス メソッドを使用すると、クラスでは、任意の、またはすべてのインターフェイス メソッドに対して異なる実装を使用または定義できます。 この言語機能によって、構築している実際のシステムをモデル化する新しい方法が提供されます。 既定のインターフェイス メソッドでは、これらの機能の仮想実装を使用して、さまざまな機能を混在させて対応付けることができる関連クラスをより明確に表現できるようになります。
