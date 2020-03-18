---
title: My 名前空間の拡張
ms.date: 07/20/2015
f1_keywords:
- vb.AddingMyExtensions
helpviewer_keywords:
- My namespace [Visual Basic], customizing
- My namespace
- My namespace [Visual Basic], extending
ms.assetid: 808e8617-b01c-4135-8b21-babe87389e8e
ms.openlocfilehash: 2a7b0b84061fccd9a333a68e4a19477bd19ca4ff
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74330310"
---
# <a name="extending-the-my-namespace-in-visual-basic"></a>Visual Basic での `My` 名前空間の拡張

Visual Basic の `My` 名前空間は、.NET Framework の機能を簡単に活用できるようにするプロパティとメソッドを公開します。 `My` 名前空間により、一般的なプログラミングの問題が簡略化されるため、多くの場合、単一のコード行に対して困難なタスクを減らすことができます。 さらに、`My` 名前空間は完全に拡張可能であるため、`My` の動作をカスタマイズし、その階層に新しいサービスを追加して、特定のアプリケーションのニーズに適合させることができます。 このトピックでは、`My` 名前空間の既存のメンバーをカスタマイズする方法と、独自のカスタムクラスを `My` 名前空間に追加する方法について説明します。

## <a name="customizing-existing-my-namespace-members"></a>既存の `My` 名前空間のメンバーのカスタマイズ

Visual Basic の `My` 名前空間は、アプリケーションやコンピューターなどに関してよく使用される情報を公開します。 `My` 名前空間内のオブジェクトの完全な一覧については、「[マイリファレンス](../../language-reference/keywords/my-reference.md)」を参照してください。 アプリケーションのニーズに合うように、`My` 名前空間の既存のメンバーをカスタマイズすることが必要になる場合があります。 読み取り専用ではない、`My` 名前空間内のオブジェクトのすべてのプロパティは、カスタム値に設定できます。

たとえば、`My.User` オブジェクトを頻繁に使用して、アプリケーションを実行しているユーザーの現在のセキュリティコンテキストにアクセスするとします。 ただし、会社はカスタムユーザーオブジェクトを使用して、会社内のユーザーに追加の情報と機能を公開しています。 このシナリオでは、次の例に示すように、`My.User.CurrentPrincipal` プロパティの既定値を独自のカスタムプリンシパルオブジェクトのインスタンスに置き換えることができます。

[!code-vb[VbVbcnExtendingMy#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#1)]

`My.User` オブジェクトの `CurrentPrincipal` プロパティを設定すると、アプリケーションの実行に使用する id が変更されます。 `My.User` オブジェクトは、新たに指定されたユーザーに関する情報を返します。
  
## <a name="adding-members-to-my-objects"></a>`My` オブジェクトへのメンバーの追加

`My.Application` および `My.Computer` から返される型は `Partial` クラスとして定義されます。 したがって、`MyApplication` または `MyComputer`という名前の `Partial` クラスを作成することによって、`My.Application` および `My.Computer` オブジェクトを拡張できます。 クラスを `Private` クラスにすることはできません。 `My` 名前空間の一部としてクラスを指定した場合は、`My.Application` オブジェクトまたは `My.Computer` オブジェクトに含まれるプロパティおよびメソッドを追加できます。

次の例では、`DnsServerIPAddresses` という名前のプロパティを `My.Computer` オブジェクトに追加します。

[!code-vb[VbVbcnExtendingMy#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class2.vb#2)]

## <a name="adding-custom-objects-to-the-my-namespace"></a>`My` 名前空間へのカスタムオブジェクトの追加

`My` 名前空間には、多くの一般的なプログラミングタスクのためのソリューションが用意されていますが、`My` 名前空間が対応していないタスクが発生する場合があります。 たとえば、アプリケーションがユーザーデータのカスタムディレクトリサービスにアクセスする場合や、アプリケーションが既定で Visual Basic にインストールされていないアセンブリを使用する場合があります。 `My` 名前空間を拡張して、環境に固有の一般的なタスクにカスタムソリューションを含めることができます。 `My` 名前空間は、増大するアプリケーションのニーズに合わせて新しいメンバーを追加するために簡単に拡張できます。 また、Visual Basic テンプレートとして、`My` 名前空間拡張を他の開発者にデプロイすることもできます。
  
### <a name="adding-members-to-the-my-namespace"></a>`My` 名前空間へのメンバーの追加

`My` は他の名前空間と同じ名前空間であるため、モジュールを追加して `My`の `Namespace` を指定するだけで、最上位レベルのプロパティを追加できます。 次の例に示すように、`HideModuleName` 属性を使用してモジュールに注釈を設定します。 `HideModuleName` 属性を指定すると、`My` 名前空間のメンバーを表示するときに、IntelliSense によってモジュール名が表示されなくなります。

[!code-vb[VbVbcnExtendingMy#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#3)]

`My` 名前空間にメンバーを追加するには、必要に応じて、モジュールにプロパティを追加します。 `My` 名前空間に追加された各プロパティについて、`ThreadSafeObjectProvider(Of T)`型のプライベートフィールドを追加します。ここで、型はカスタムプロパティによって返される型です。 このフィールドは、`GetInstance` メソッドを呼び出すことによってプロパティによって返されるスレッドセーフなオブジェクトインスタンスを作成するために使用されます。 その結果、拡張プロパティにアクセスする各スレッドは、返された型の独自のインスタンスを受け取ります。 次の例では、`SampleExtension` 型の `SampleExtension` という名前のプロパティを `My` 名前空間に追加します。

[!code-vb[VbVbcnExtendingMy#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#4)]

## <a name="adding-events-to-custom-my-objects"></a>カスタム `My` オブジェクトへのイベントの追加

`My.Application` オブジェクトを使用して、`My` 名前空間の `MyApplication` 部分クラスを拡張することによって、カスタム `My` オブジェクトのイベントを公開できます。 Windows ベースのプロジェクトの場合は、**ソリューションエクスプローラー**でプロジェクトのの **[マイプロジェクト**] ノードをダブルクリックします。 Visual Basic**プロジェクトデザイナー**で、 **[アプリケーション]** タブをクリックし、 **[アプリケーションイベントの表示]** ボタンをクリックします。 *Applicationevents*という名前の新しいファイルが作成されます。 `MyApplication` クラスを拡張するための次のコードが含まれています。

[!code-vb[VbVbcnExtendingMy#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#5)]

カスタム `My` オブジェクトのイベントハンドラーを追加するには、`MyApplication` クラスにカスタムイベントハンドラーを追加します。 カスタムイベントを使用すると、イベントハンドラーの追加、削除、またはイベントの発生時に実行されるコードを追加できます。 カスタムイベントの `AddHandler` コードは、ユーザーがイベントを処理するためにコードを追加した場合にのみ実行されることに注意してください。 たとえば、前のセクションの `SampleExtension` オブジェクトに、のカスタムイベントハンドラーを追加する `Load` イベントがあるとします。 次のコード例では、`My.SampleExtension.Load` イベントが発生したときに呼び出される `SampleExtensionLoad` という名前のカスタムイベントハンドラーを示します。 新しい `My.SampleExtensionLoad` イベントを処理するコードを追加すると、このカスタムイベントコードの `AddHandler` の部分が実行されます。 `MyApplication_SampleExtensionLoad` メソッドは、`My.SampleExtensionLoad` イベントを処理するイベントハンドラーの例を示すために、コード例に含まれています。 *Applicationevents .vb*ファイルを編集するときに、コードエディターの上にある左側のドロップダウンリストで **[アプリケーションイベント**] オプションを選択すると、`SampleExtensionLoad` イベントが使用できることに注意してください。

[!code-vb[VbVbcnExtendingMy#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#6)]

## <a name="design-guidelines"></a>設計ガイドライン

`My` 名前空間の拡張機能を開発するときは、次のガイドラインに従って、拡張コンポーネントのメンテナンスコストを最小限に抑えることができます。

- **拡張ロジックのみを含めます。** `My` 名前空間拡張に含まれるロジックには、`My` 名前空間で必要な機能を公開するために必要なコードのみを含める必要があります。 拡張機能は、ソースコードとしてユーザープロジェクトに存在するため、拡張機能コンポーネントを更新するとメンテナンスコストが高くなり、可能な場合は回避する必要があります。
- **プロジェクトの前提を最小化します。** `My` 名前空間の拡張機能を作成する場合は、一連の参照、プロジェクトレベルのインポート、または特定のコンパイラ設定 (たとえば、`Option Strict` オフ) を想定しないでください。 代わりに、`Global` キーワードを使用して、依存関係を最小限にし、すべての型参照を完全修飾します。 また、拡張機能のエラーを最小限に抑えるために、拡張機能が `Option Strict` でコンパイルされることを確認します。
- **拡張コードを分離します。** コードを1つのファイルに配置すると、拡張機能が Visual Studio 項目テンプレートとして簡単に展開できるようになります。 詳細については、このトピックで後述する「拡張機能のパッケージ化と展開」を参照してください。 すべての `My` 名前空間拡張コードを1つのファイルまたはプロジェクト内の別のフォルダーに配置すると、ユーザーは `My` 名前空間の拡張機能を見つけることができます。

## <a name="designing-class-libraries-for-my"></a>`My` のクラスライブラリの設計

ほとんどのオブジェクトモデルの場合と同様に、設計パターンの中には `My` 名前空間で適切に機能するものもあれば、そうでないものもあります。 `My` 名前空間の拡張機能を設計するときは、次の原則を考慮してください。

- **ステートレスメソッド。** `My` 名前空間のメソッドは、特定のタスクに完全なソリューションを提供する必要があります。 メソッドに渡されたパラメーター値が、特定のタスクを完了するために必要なすべての入力を提供していることを確認します。 リソースへのオープン接続など、以前の状態に依存するメソッドを作成しないようにします。
- **グローバルインスタンス。** `My` 名前空間で保持される状態は、プロジェクトに対してグローバルです。 たとえば、`My.Application.Info` は、アプリケーション全体で共有される状態をカプセル化します。
- **単純なパラメーターの型。** 複雑なパラメーターの型を避けることで、シンプルなものを維持します。 代わりに、パラメーター入力を受け取らないか、文字列、プリミティブ型などの単純な入力型を受け取るメソッドを作成します。
- **ファクトリメソッド。** 一部の型は、インスタンス化が困難な場合があります。 ファクトリメソッドを `My` 名前空間の拡張機能として指定すると、このカテゴリに分類される型をより簡単に検出して使用できます。 適切に機能するファクトリメソッドの例としては、`My.Computer.FileSystem.OpenTextFileReader`があります。 .NET Framework で使用できるストリームの種類はいくつかあります。 具体的には、テキストファイルを指定することによって、使用するストリームをユーザーが理解できるように `OpenTextFileReader` します。

これらのガイドラインでは、クラスライブラリの一般的な設計原則は使用できません。 これらは、Visual Basic と `My` 名前空間を使用している開発者向けに最適化された推奨事項です。 クラスライブラリを作成するための一般的な設計原則については、「[フレームワークのデザインガイドライン](../../../standard/design-guidelines/index.md)」を参照してください。

## <a name="packaging-and-deploying-extensions"></a>拡張機能のパッケージ化と配置

Visual Studio プロジェクトテンプレートには `My` 名前空間拡張を含めることができます。また、拡張機能をパッケージ化して Visual Studio 項目テンプレートとして配置することもできます。 `My` 名前空間拡張を Visual Studio 項目テンプレートとしてパッケージ化すると、Visual Basic によって提供される追加機能を利用できます。 これらの機能を使用すると、プロジェクトが特定のアセンブリを参照するときに拡張機能を含めることができます。また、Visual Basic プロジェクトデザイナーの **[マイ拡張**] ページを使用して、ユーザーが `My` 名前空間拡張機能を明示的に追加できるようになります。

`My` 名前空間拡張をデプロイする方法の詳細については、「[カスタムマイ拡張機能のパッケージ化と配置](packaging-and-deploying-custom-my-extensions.md)」を参照してください。

## <a name="see-also"></a>参照

- [カスタム My 拡張のパッケージ化と配置](packaging-and-deploying-custom-my-extensions.md)
- [Visual Basic アプリケーション モデルの拡張](extending-the-visual-basic-application-model.md)
- [My で利用可能なオブジェクトのカスタマイズ](customizing-which-objects-are-available-in-my.md)
- [[マイ拡張] ページ (プロジェクト デザイナー)](/visualstudio/ide/reference/my-extensions-page-project-designer-visual-basic)
- [[アプリケーション] ページ (プロジェクト デザイナー) (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)
- [Partial](../../language-reference/modifiers/partial.md)
