---
title: Windows フォームデザイナーのデザイン時エラー
titleSuffix: ''
ms.date: 09/09/2019
f1_keywords:
- DTELErrorList
- WhyDTELPage
helpviewer_keywords:
- errors [Windows Forms Designer]
- design-time errors [Windows Forms Designer]
ms.assetid: ad408380-825a-46d8-9a4a-531b130b88ce
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 2a39c76d011c6d129f91647fabe3f129245b9466
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746086"
---
# <a name="windows-forms-designer-error-page"></a>Windows フォームデザイナーのエラーページ

コードのエラー、サードパーティのコンポーネント、または他の場所でエラーが発生したために Windows フォームデザイナーの読み込みに失敗した場合は、デザイナーではなくエラーページが表示されます。 このエラーページは、必ずしもデザイナーのバグを示しているわけではありません。 このバグは、分離コードページ内のどこかにある可能性があります。これは、フォーム名 \<> という名前です。Designer.cs。 エラーは、コードページのエラーの場所に移動するためのリンクを含む、折りたたみ可能な黄色のバーに表示されます。

![Windows フォームデザイナーのエラーページ](media/windows-forms-designer-error-page-collapsed.png)

エラーを無視してデザイナーの読み込みを続行するには、 **[無視して続行]** をクリックします。 この操作によって予期しない動作が発生する可能性があります。たとえば、コントロールがデザインサーフェイスに表示されないことがあります。

## <a name="instances-of-this-error"></a>このエラーのインスタンス

黄色のエラーバーが展開されると、エラーの各インスタンスが一覧表示されます。 多くのエラーの種類には、 *[プロジェクト名]* *[フォーム名]* 行: *[行番号]* 列: *[列番号]* の形式の正確な場所が含まれています。 コールスタックがエラーに関連付けられている場合は、 **[呼び出し履歴の表示]** リンクをクリックすると、呼び出し履歴が表示されます。 呼び出し履歴を調べると、エラーの解決に役立つ場合があります。

![Windows フォームデザイナー展開されたエラー](media/windows-forms-designer-error-page-expanded.png)

> [!NOTE]
>
> - Visual Basic アプリの場合、[デザイン時エラー] ページには複数のエラーが表示されませんが、同じエラーの複数のインスタンスが表示されることがあります。
> - アプリC++の場合、エラーにはコードの場所のリンクがありません。

## <a name="help-with-this-error"></a>このエラーに関するヘルプ

エラーのヘルプトピックが表示されている場合は、 **MSDN のヘルプ**リンクをクリックして、docs.microsoft.com のヘルプページに直接移動します。

## <a name="forum-posts-about-this-error"></a>このエラーに関するフォーラムの投稿

[**このエラーに関連する投稿を MSDN フォーラムで検索**する] リンクをクリックして、Microsoft Developer Network フォーラムに移動します。 [Windows フォームデザイナー](https://social.msdn.microsoft.com/Forums/windows/home?forum=winformsdesigner)または[Windows フォーム](https://social.msdn.microsoft.com/Forums/windows/home?category=windowsforms)フォーラムを具体的に検索することもできます。

## <a name="design-time-errors"></a>デザイン時エラー

ここでは、発生する可能性のあるエラーの一部を示します。

### <a name="identifier-name-is-not-a-valid-identifier"></a>'\<識別子名 > ' は有効な識別子ではありません

このエラーは、フィールド、メソッド、イベント、またはオブジェクトの名前が不適切であることを示します。

### <a name="name-already-exists-in-project-name"></a>'\<name > ' は '\<プロジェクト名 > ' に既に存在します

エラーメッセージ: "'\<名前 > ' は、'\<プロジェクト名 > ' に既に存在します。 一意の名前を入力してください。 "

プロジェクトに既に存在する継承されたフォームの名前を指定しました。 このエラーを修正するには、継承されたフォームに一意の名前を付けます。

### <a name="toolbox-tab-name-is-not-a-toolbox-category"></a>'\<ツールボックスタブ名 > ' はツールボックスカテゴリではありません

サードパーティのデザイナーが、存在しないツールボックスのタブにアクセスしようとしました。 コンポーネントのベンダにお問い合わせください。

### <a name="a-requested-language-parser-is-not-installed"></a>要求された言語パーサーはインストールされていません。

エラーメッセージ: "要求された言語パーサーがインストールされていません。 言語パーサー名は '{0}' です。 "

Visual Studio によって、ファイルの種類に登録されているデザイナーの読み込みが試行されましたが、読み込めませんでした。 これは、セットアップ中に発生したエラーが原因である可能性があります。 修正のために使用している言語のベンダにお問い合わせください。

### <a name="a-service-required-for-generating-and-parsing-source-code-is-missing"></a>ソース コードの生成、解析に必要なサービスがありません。

これは、サードパーティのコンポーネントに問題があります。 コンポーネントのベンダにお問い合わせください。

### <a name="an-exception-occurred-while-trying-to-create-an-instance-of-object-name"></a>'\<オブジェクト名 > ' のインスタンスを作成しようとして例外が発生しました

エラーメッセージ: "\<オブジェクト名 > ' のインスタンスを作成しようとして例外が発生しました。 例外は "\<例外文字列\>" でした。

サードパーティのデザイナーによって、Visual Studio がオブジェクトを作成するように要求しましたが、オブジェクトでエラーが発生しました。 コンポーネントのベンダにお問い合わせください。

### <a name="another-editor-has-document-name-open-in-an-incompatible-mode"></a>別のエディターで '\<ドキュメント名 > ' が互換性のないモードで開かれています

エラーメッセージ: "別のエディターで '\<ドキュメント名 > ' が互換性のないモードで開かれています。 エディターを閉じて、この操作をもう一度お試しください。 "

このエラーは、別のエディターで既に開かれているファイルを開こうとした場合に発生します。 ファイルが既に開いているエディターが表示されます。 このエラーを修正するには、ファイルが開いているエディターを閉じて、もう一度やり直してください。

### <a name="another-editor-has-made-changes-to-document-name"></a>別のエディターによって '\<ドキュメント名 > ' が変更されました

変更を有効にするには、デザイナーを閉じてから開き直します。 通常、変更が加えられると、Visual Studio によってデザイナーが自動的に再読み込みされます。 ただし、サードパーティのコンポーネントデザイナーなど、他のデザイナーでは、再読み込み動作がサポートされていない場合があります。 この場合、Visual Studio では、デザイナーを閉じて再度開くように求められます。

### <a name="another-editor-has-the-file-open-in-an-incompatible-mode"></a>別のエディターによって互換性のないモードでファイルが開かれています。

エラーメッセージ: "別のエディターで、互換性のないモードでファイルが開かれています。 エディターを閉じて、この操作をもう一度お試しください。 "

このメッセージは、"別のエディターに '\<ドキュメント名 > ' 互換性のないモードで開く" と似ていますが、Visual Studio はファイル名を特定できません。 このエラーを修正するには、ファイルが開いているエディターを閉じて、もう一度やり直してください。

### <a name="array-rank-rank-in-array-is-too-high"></a>配列のランク '\<ランク ' > ' が大きすぎます

Visual Studio では、デザイナーによって解析されるコードブロック内の1次元配列のみがサポートされます。 多次元配列は、この領域の外側で有効です。

### <a name="assembly-assembly-name-could-not-be-opened"></a>アセンブリ '\<アセンブリ名 > ' を開けませんでした

エラーメッセージ: "アセンブリ '\<アセンブリ名 > ' を開けませんでした。 ファイルがまだ存在していることを確認してください。 "

このエラーメッセージは、開くことができなかったファイルを開こうとしたときに発生します。 ファイルが存在し、有効なアセンブリであることを確認してください。

### <a name="bad-element-type-this-serializer-expects-an-element-of-type-type-name"></a>要素の型が正しくありません。 このシリアライザーには '\<type name > ' 型の要素が必要です

これは、サードパーティのコンポーネントに問題があります。 コンポーネントのベンダにお問い合わせください。

### <a name="cannot-access-the-visual-studio-toolbox-at-this-time"></a>現在 Visual Studio の Toolbox にアクセスできません。

Visual Studio によってツールボックスが呼び出されましたが、これは使用できませんでした。 このエラーが表示された場合は、[[問題の報告](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)] を使用して問題をログに記録してください。

### <a name="cannot-bind-an-event-handler-to-the-event-name-event-because-it-is-read-only"></a>イベントハンドラーを '\<イベント名 > ' イベントにバインドできません。このイベントは読み取り専用です。

このエラーは、基本クラスから継承されたコントロールにイベントを接続しようとした場合によく発生します。 コントロールのメンバー変数がプライベートである場合、Visual Studio はイベントをメソッドに接続できません。 プライベートに継承されたコントロールには、追加のイベントをバインドすることはできません。

### <a name="cannot-create-a-method-name-for-the-requested-component-because-it-is-not-a-member-of-the-design-container"></a>デザイン コンテナーのメンバーでないため、要求されたコンポーネントのメソッド名を作成できません。

Visual Studio が、デザイナーにメンバー変数を持たないコンポーネントにイベントハンドラーを追加しようとしました。 コンポーネントのベンダにお問い合わせください。

### <a name="cannot-name-the-object-name-because-it-is-already-named-name"></a>オブジェクト '\<name > ' に名前を付けることはできません。既に '\<name > ' という名前が付けられています

これは、Visual Studio シリアライザーでの内部エラーです。 これは、シリアライザーがオブジェクトに2回名前を指定しようとしたことを示します。これはサポートされていません。 このエラーが表示された場合は、[[問題の報告](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)] を使用して問題をログに記録してください。

### <a name="cannot-remove-or-destroy-inherited-component-component-name"></a>継承されたコンポーネント '\<コンポーネント名 > ' を削除または破棄できません

継承されたコントロールは、継承するクラスの所有権を持ちます。 継承されたコントロールに対する変更は、コントロールの生成元のクラスで行う必要があります。 そのため、名前を変更したり、破棄したりすることはできません。

### <a name="category-toolbox-tab-name-does-not-have-a-tool-for-class-class-name"></a>カテゴリ '\<ツールボックスタブ名 > ' には、クラス '\<クラス名 > ' のツールがありません

デザイナーが特定の [ツールボックス] タブのクラスを参照しようとしましたが、このクラスは存在しません。 コンポーネントのベンダにお問い合わせください。

### <a name="class-class-name-has-no-matching-constructor"></a>クラス '\<クラス名 > ' に一致するコンストラクターがありません

サードパーティのデザイナーが Visual Studio に対して、存在しないコンストラクターに特定のパラメーターを持つオブジェクトを作成するように要求しました。 コンポーネントのベンダにお問い合わせください。

### <a name="code-generation-for-property-property-name-failed"></a>プロパティ '\<プロパティ名 > ' のコード生成に失敗しました

これは、エラーの一般的なラッパーです。 このメッセージに付随するエラー文字列は、エラーメッセージに関する詳細情報を提供し、より具体的なヘルプトピックへのリンクを提供します。 このエラーを修正するには、エラーメッセージに示されたエラーをこのエラーに追加します。

### <a name="component-component-name-did-not-call-containeradd-in-its-constructor"></a>コンポーネント '\<コンポーネント名 > ' が Container. Add () をコンストラクターで呼び出しませんでした

これは、読み込んだコンポーネントまたはフォームに配置したコンポーネントのエラーです。 これは、コンポーネントがそれ自体をコンテナーコントロールに追加しなかったことを示します (別のコントロールまたはフォームであるかどうか)。 デザイナーは引き続き機能しますが、実行時にコンポーネントに問題がある可能性があります。

このエラーを解決するには、コンポーネントのベンダに問い合わせてください。 または、作成したコンポーネントの場合は、コンポーネントのコンストラクターで `IContainer.Add` メソッドを呼び出します。

### <a name="component-name-cannot-be-empty"></a>コンポーネント名を空にすることはできません。

このエラーは、コンポーネントの名前を空の値に変更しようとした場合に発生します。

### <a name="could-not-access-the-variable-variable-name-because-it-has-not-been-initialized-yet"></a>変数 '\<変数名 > ' はまだ初期化されていないため、アクセスできませんでした

このエラーは、2つのシナリオが原因で発生する可能性があります。 サードパーティのコンポーネントベンダーが、配布されたコントロールまたはコンポーネントに問題があるか、または記述したコードにコンポーネント間の再帰的な依存関係があります。

このエラーを修正するには、コードに再帰的な依存関係がないことを確認します。 このような問題がない場合は、エラーメッセージのテキストを正確に確認し、コンポーネントのベンダに問い合わせてください。

### <a name="could-not-find-type-type-name"></a>型 '\<型名 > ' が見つかりませんでした

エラーメッセージ: "型名 '\<型名 > ' が見つかりませんでした。 この型を含むアセンブリが参照されていることを確認してください。 この型が開発プロジェクトの一部である場合は、プロジェクトが正常にビルドされていることを確認してください。 "

このエラーは、参照が見つからなかったために発生しました。 エラーメッセージに示されている型が参照されていること、および型が必要とするすべてのアセンブリも参照されていることを確認します。 多くの場合、問題は、ソリューション内のコントロールがビルドされていないことです。 ビルドするには、 **[ビルド]** メニューの **[ソリューションのビルド]** を選択します。 それ以外の場合、コントロールが既にビルドされている場合は、ソリューションエクスプローラーの **[参照]** または **[依存関係]** フォルダーの右クリックメニューから手動で参照を追加します。

### <a name="could-not-load-type-type-name"></a>型 '\<型名 > ' を読み込めませんでした

エラーメッセージ: "型 '\<型名 > ' を読み込めませんでした。 この型を含むアセンブリがプロジェクト参照に追加されていることを確認してください。 "

Visual Studio がイベント処理メソッドを接続しようとしましたが、メソッドのパラメーターの型が1つ以上見つかりませんでした。 これは通常、参照がないことが原因で発生します。 このエラーを修正するには、型を含む参照をプロジェクトに追加してから、操作をやり直してください。

### <a name="could-not-locate-the-project-item-templates-for-inherited-components"></a>継承コンポーネントのプロジェクト項目テンプレートが見つかりませんでした。

Visual Studio での継承されたフォームのテンプレートは使用できません。 このエラーが表示された場合は、[[問題の報告](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)] を使用して問題をログに記録してください。

### <a name="delegate-class-class-name-has-no-invoke-method-is-this-class-a-delegate"></a>デリゲートクラス '\<クラス名 > ' に invoke メソッドがありません。 このクラスはデリゲートですか?

Visual Studio でイベントハンドラーを作成しようとしましたが、イベントの種類に問題があります。 これは、イベントが CLS に準拠していない言語で作成された場合に発生する可能性があります。 コンポーネントのベンダにお問い合わせください。

### <a name="duplicate-declaration-of-member-member-name"></a>メンバー '\<メンバー名 > ' の宣言が重複しています

このエラーは、メンバー変数が2回宣言されているために発生します (たとえば、コードで `Button1` という名前の2つのコントロールが宣言されている場合)。 名前は、継承されたフォーム全体で一意である必要があります。 また、大文字と小文字のみを区別して名前を変えることはできません。

### <a name="error-reading-resources-from-the-resource-file-for-the-culture-culture-name"></a>カルチャ '\<カルチャ名 > ' のリソースファイルからリソースを読み取り中にエラーが発生します

このエラーは、プロジェクト内に無効な .resx ファイルがある場合に発生する可能性があります。

このエラーを修正するには、次のようにします。

1. ソリューションエクスプローラーの **[すべてのファイルを表示]** ボタンをクリックして、ソリューションに関連付けられている .resx ファイルを表示します。
2. .Resx ファイルを右クリックし、 **[開く]** を選択して、XML エディターで .resx ファイルを読み込みます。
3. .Resx ファイルを手動で編集して、エラーに対処します。

### <a name="error-reading-resources-from-the-resource-file-for-the-default-culture-culture-name"></a>既定のカルチャ '\<カルチャ名 > ' のリソースファイルからリソースを読み取り中にエラーが発生します

このエラーは、既定のカルチャのプロジェクトに無効な .resx ファイルがある場合に発生する可能性があります。

このエラーを修正するには、次のようにします。

1. ソリューションエクスプローラーの **[すべてのファイルを表示]** ボタンをクリックして、ソリューションに関連付けられている .resx ファイルを表示します。
2. .Resx ファイルを右クリックし、 **[開く]** を選択して、XML エディターで .resx ファイルを読み込みます。
3. .Resx ファイルを手動で編集して、エラーに対処します。

### <a name="failed-to-parse-method-method-name"></a>メソッド '\<メソッド名 > ' を解析できませんでした

エラーメッセージ: "メソッド名 > '\<メソッドを解析できませんでした。 パーサーから次のエラーが報告されました: '\<エラー文字列 > '。 考えられるエラーについては、タスク一覧を参照してください。 "

これは、解析中に発生した問題の一般的なエラーメッセージです。 これらのエラーは、多くの場合、構文エラーによって発生します。 エラーに関連する特定のメッセージについては、タスク一覧を参照してください。

### <a name="invalid-component-name-component-name"></a>コンポーネント名が無効です: '\<コンポーネント名 > '

コンポーネントの名前を、その言語の無効な値に変更しようとしました。 このエラーを修正するには、その言語の名前付け規則に準拠するようにコンポーネントの名前を指定します。

### <a name="the-type-class-name-is-made-of-several-partial-classes-in-the-same-file"></a>型 '\<クラス名 > ' は、同じファイル内の複数の部分クラスで構成されています

[Partial](../../../csharp/language-reference/keywords/partial-type.md)キーワードを使用して複数のファイルでクラスを定義する場合、各ファイルに定義できる部分定義は1つだけです。

このエラーを修正するには、クラスの部分定義を1つだけ残して、ファイルからすべて削除します。

### <a name="the-assembly-assembly-name-could-not-be-found"></a>アセンブリ '\<アセンブリ名 > ' が見つかりませんでした

エラーメッセージ: "アセンブリ '\<アセンブリ名 > ' が見つかりませんでした。 アセンブリが参照されていることを確認します。 アセンブリが現在の開発プロジェクトの一部である場合は、プロジェクトがビルドされていることを確認します。

このエラーは、"型 '\<型名 > ' が見つかりませんでした" と似ていますが、通常、このエラーはメタデータ属性が原因で発生します。 このエラーを修正するには、属性で使用されているすべてのアセンブリが参照されていることを確認します。

### <a name="the-assembly-name-assembly-name-is-invalid"></a>アセンブリ名 '\<アセンブリ名 > ' が無効です

コンポーネントが特定のアセンブリを要求しましたが、コンポーネントによって指定された名前は有効なアセンブリ名ではありません。 コンポーネントのベンダにお問い合わせください。

### <a name="the-base-class-class-name-cannot-be-designed"></a>基底クラス '\<クラス名 > ' をデザインできません

Visual Studio によってクラスが読み込まれましたが、クラスの実装者がデザイナーを提供していないため、クラスをデザインできません。 クラスがデザイナーをサポートしている場合は、コンパイラエラーなど、デザイナーでの表示に問題が発生する問題がないことを確認してください。 また、クラスへのすべての参照が正しいこと、およびすべてのクラス名のスペルが正しいことを確認します。 それ以外の場合、クラスがデザイン可能でない場合は、コードビューで編集します。

### <a name="the-base-class-class-name-could-not-be-loaded"></a>基底クラス '\<クラス名 > ' を読み込むことができませんでした

クラスがプロジェクト内で参照されていないため、Visual Studio はこのクラスを読み込むことができません。 このエラーを修正するには、プロジェクトのクラスへの参照を追加し、Windows フォームデザイナーウィンドウを閉じてから再度開きます。

### <a name="the-class-class-name-cannot-be-designed-in-this-version-of-visual-studio"></a>クラス '\<クラス名 > ' をこのバージョンの Visual Studio でデザインすることはできません

このコントロールまたはコンポーネントのデザイナーは、Visual Studio と同じ型をサポートしていません。 コンポーネントのベンダにお問い合わせください。

### <a name="the-class-name-is-not-a-valid-identifier-for-this-language"></a>クラス名が、この言語に有効な識別子ではありません。

ユーザーによって作成されているソースコードのクラス名が、使用されている言語に対して無効です。 このエラーを修正するには、言語の要件に準拠するようにクラスの名前を指定します。

### <a name="the-component-cannot-be-added-because-it-contains-a-circular-reference-to-reference-name"></a>'\<参照名 > ' への循環参照が含まれているため、このコンポーネントを追加できません

コントロールまたはコンポーネントをそれ自体に追加することはできません。 もう1つの状況は、Form1 の別のインスタンスを作成するフォーム (例、Form1) の InitializeComponent メソッドにコードがある場合です。

### <a name="the-designer-cannot-be-modified-at-this-time"></a>現在デザイナーを変更することはできません。

このエラーは、エディター内のファイルが読み取り専用としてマークされている場合に発生します。 ファイルが読み取り専用に設定されていないこと、およびアプリケーションが実行されていないことを確認します。

### <a name="the-designer-could-not-be-shown-for-this-file-because-none-of-the-classes-within-it-can-be-designed"></a>このファイルのデザイナーに、デザインできるクラスがないため、デザイナーを表示できませんでした。

このエラーは、Visual Studio がデザイナーの要件を満たす基底クラスを見つけられない場合に発生します。 フォームとコントロールは、デザイナーをサポートする基本クラスから派生する必要があります。 継承されたフォームまたはコントロールから派生している場合は、プロジェクトがビルドされていることを確認します。

### <a name="the-designer-for-base-class-class-name-is-not-installed"></a>基底クラス '\<class name > ' のデザイナーがインストールされていません

クラスのデザイナーを読み込むことができませんでした。 このエラーが表示された場合は、[[問題の報告](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)] を使用して問題をログに記録してください。

### <a name="the-designer-must-create-an-instance-of-type-type-name-but-it-cant-because-the-type-is-declared-as-abstract"></a>デザイナーは型 '\<type name > ' のインスタンスを作成する必要がありますが、型が abstract として宣言されているため、これを行うことはできません。

このエラーは、デザイナーに渡されるオブジェクトの基底クラスが[abstract](../../../csharp/language-reference/keywords/abstract.md)であり、許可されていないために発生しました。

### <a name="the-file-could-not-be-loaded-in-the-designer"></a>ファイルをデザイナーに読み込めませんでした。

このファイルの基底クラスは、デザイナーをサポートしていません。 この問題を回避するには、コードビューを使用してファイルを操作します。 ソリューションエクスプローラーでファイルを右クリックし、 **[コードの表示]** を選択します。

### <a name="the-language-for-this-file-does-not-support-the-necessary-code-parsing-and-generation-services"></a>このファイルの言語は、サービスを解析して生成するのに必要なコードをサポートしません。

エラーメッセージ: "このファイルの言語は、必要なコード解析および生成サービスをサポートしていません。 開いているファイルがプロジェクトのメンバーであることを確認してから、もう一度ファイルを開いてください。

このエラーは、通常、デザイナーをサポートしていないプロジェクト内のファイルを開こうとしたことが原因で発生します。

### <a name="the-language-parser-class-class-name-is-not-implemented-properly"></a>言語パーサークラス '\<クラス名 > ' が正しく実装されていません

エラーメッセージ: "言語パーサークラス '\<クラス名 > ' が正しく実装されていません。 更新されたパーサーモジュールについては、ベンダーにお問い合わせください。 "

使用中の言語が、正しい基底クラスから派生していないデザイナークラスを登録しています。 使用している言語のベンダにお問い合わせください。

### <a name="the-name-name-is-already-used-by-another-object"></a>名前 '\<name > ' は、既に別のオブジェクトによって使用されています

これは、Visual Studio シリアライザーでの内部エラーです。 このエラーが表示された場合は、[[問題の報告](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)] を使用して問題をログに記録してください。

### <a name="the-object-object-name-does-not-implement-the-icomponent-interface"></a>オブジェクト '\<オブジェクト名 > ' は IComponent インターフェイスを実装していません

Visual Studio はコンポーネントを作成しようとしましたが、作成されたオブジェクトは <xref:System.ComponentModel.IComponent> インターフェイスを実装していません。 修正方法については、コンポーネントベンダにお問い合わせください。

### <a name="the-object-object-name-returned-null-for-the-property-property-name-but-this-is-not-allowed"></a>オブジェクト '\<オブジェクト名 > ' は、プロパティ '\<プロパティ名 > ' に対して null を返しましたが、これは許可されていません

常にオブジェクトを返す必要がある .NET プロパティがいくつかあります。 たとえば、フォームの**controls**コレクションは、コントロールがない場合でも、常にオブジェクトを返す必要があります。

このエラーを修正するには、エラーで指定されたプロパティが null でないことを確認します。

### <a name="the-serialization-data-object-is-not-of-the-proper-type"></a>シリアル化データ オブジェクトの種類が正しくありません。

シリアライザーによって提供されるデータオブジェクトは、現在使用されているシリアライザーに一致する型のインスタンスではありません。 コンポーネントのベンダにお問い合わせください。

### <a name="the-service-service-name-is-required-but-could-not-be-located"></a>サービス '\<サービス名 > ' が必要ですが、見つかりませんでした

エラーメッセージ: "サービス '\<サービス名 > ' が必要ですが、見つかりませんでした。 Visual Studio のインストールに問題がある可能性があります。 "

Visual Studio で必要なサービスが使用できません。 そのデザイナーをサポートしていないプロジェクトを読み込もうとしている場合は、コードエディターを使用して必要な変更を行います。 それ以外の場合、このエラーが表示された場合は、[[問題の報告](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)] を使用して問題をログに記録してください。

### <a name="the-service-instance-must-derive-from-or-implement-interface-name"></a>サービスインスタンスは、'\<インターフェイス名 > ' から派生するか、実装する必要があります

このエラーは、コンポーネントまたはコンポーネントデザイナーが、インターフェイスとオブジェクトを必要とする**Addservice**メソッドを呼び出したが、指定されたオブジェクトが指定されたインターフェイスを実装していないことを示します。 コンポーネントのベンダにお問い合わせください。

### <a name="the-text-in-the-code-window-could-not-be-modified"></a>コード ウィンドウのテキストを変更できませんでした。

エラーメッセージ: "コードウィンドウ内のテキストを変更できませんでした。 ファイルが読み取り専用ではなく、十分なディスク領域があることを確認してください。 "

このエラーは、ディスク領域またはメモリの問題が原因で Visual Studio がファイルを編集できない場合、またはファイルが読み取り専用としてマークされている場合に発生します。

### <a name="the-toolbox-enumerator-object-only-supports-retrieving-one-item-at-a-time"></a>Toolbox 列挙子オブジェクトは、一度に 1 項目のみの取得をサポートします。

このエラーが表示された場合は、[[問題の報告](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)] を使用して問題をログに記録してください。

### <a name="the-toolbox-item-for-component-name-could-not-be-retrieved-from-the-toolbox"></a>'\<コンポーネント名 > ' のツールボックス項目をツールボックスから取得できませんでした

エラーメッセージ: "\<コンポーネント名 > ' のツールボックス項目をツールボックスから取得できませんでした。 ツールボックス項目を含むアセンブリが正しくインストールされていることを確認します。 ツールボックス項目で次のエラーが発生しました: \<エラー文字列 >。 "

問題のコンポーネントが、Visual Studio にアクセスしたときに例外をスローしました。 コンポーネントのベンダにお問い合わせください。

### <a name="the-toolbox-item-for-toolbox-item-name-could-not-be-retrieved-from-the-toolbox"></a>'\<ツールボックス項目名 > ' のツールボックス項目をツールボックスから取得できませんでした

エラーメッセージ: "\<ツールボックス項目名 > ' のツールボックス項目をツールボックスから取得できませんでした。 ツールボックスから項目を削除してから、もう一度追加してみてください。 "

このエラーは、ツールボックス項目内のデータが破損した場合、またはコンポーネントのバージョンが変更された場合に発生します。 ツールボックスから項目を削除してから再度追加してみてください。

### <a name="the-type-type-name-could-not-be-found"></a>型 '\<型名 > ' が見つかりませんでした

エラーメッセージ: "型 '\<型名 > ' が見つかりませんでした。 型を含むアセンブリが参照されていることを確認します。 アセンブリが現在の開発プロジェクトの一部である場合は、プロジェクトがビルドされていることを確認します。

デザイナーの読み込み中に、Visual Studio が型を見つけることができませんでした。 型を含むアセンブリが参照されていることを確認します。 アセンブリが現在の開発プロジェクトの一部である場合は、プロジェクトがビルドされていることを確認します。

### <a name="the-type-resolution-service-may-only-be-called-from-the-main-application-thread"></a>型の解決サービスはメイン アプリケーション スレッドからのみ呼び出されます。

Visual Studio が、間違ったスレッドから必要なリソースにアクセスしようとしました。 このエラーは、デザイナーの作成に使用されたコードが、メインアプリケーションスレッド以外のスレッドからの型解決サービスを呼び出したときに表示されます。 このエラーを修正するには、正しいスレッドからサービスを呼び出すか、コンポーネントベンダに問い合わせてください。

### <a name="the-variable-variable-name-is-either-undeclared-or-was-never-assigned"></a>変数 '\<変数名 > ' は宣言されていないか、割り当てられていません。

ソースコードに、宣言または割り当てられていない**Button1**などの変数への参照が含まれています。 変数が割り当てられていない場合、このメッセージはエラーではなく警告として表示されます。

### <a name="there-is-already-a-command-handler-for-the-menu-command-menu-command-name"></a>メニューコマンド '\<メニューコマンド名 > ' のコマンドハンドラーが既に存在します

このエラーは、サードパーティのデザイナーが、既にハンドラーを持つコマンドをコマンドテーブルに追加した場合に発生します。 コンポーネントのベンダにお問い合わせください。

### <a name="there-is-already-a-component-named-component-name"></a>'\<コンポーネント名 > ' という名前のコンポーネントは既に存在します

エラーメッセージ: "\<コンポーネント名 > ' という名前のコンポーネントは既に存在します。 コンポーネントには一意の名前を指定する必要があり、名前の大文字と小文字は区別されません。 名前は、継承されたクラスのコンポーネントの名前と競合することもできません。 "

このエラーメッセージは、プロパティウィンドウ内のコンポーネントの名前が変更された場合に発生します。 このエラーを修正するには、すべてのコンポーネント名が一意であり、大文字と小文字が区別されず、継承されたクラスのコンポーネントの名前と競合しないようにします。

### <a name="there-is-already-a-toolbox-item-creator-registered-for-the-format-format-name"></a>'\<format name > ' という形式で登録されたツールボックスアイテムクリエーターが既に存在します

サードパーティのコンポーネントは、[ツールボックス] タブ上のアイテムへのコールバックを作成しましたが、そのアイテムには既にコールバックが含まれていました。 コンポーネントのベンダにお問い合わせください。

### <a name="this-language-engine-does-not-support-a-codemodel-with-which-to-load-a-designer"></a>この言語エンジンは、デザイナーを読み込む CodeModel をサポートしません。

このメッセージは、"このファイルの言語は、必要なコード解析および生成サービスをサポートしていません" と似ていますが、このメッセージには内部登録の問題が含まれます。 このエラーが表示された場合は、[[問題の報告](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)] を使用して問題をログに記録してください。

### <a name="type-type-name-does-not-have-a-constructor-with-parameters-of-types-parameter-type-names"></a>型 '\<型名\>' には、型 '\<パラメーター型名のパラメーターを持つコンストラクターがありません > '

パラメーターが一致する[コンストラクター](../../../csharp/programming-guide/classes-and-structs/constructors.md)が見つかりませんでした。 これは、必要な型以外の型を持つコンストラクターを指定したことが原因である可能性があります。 たとえば、**ポイント**コンストラクターは2つの整数を受け取ることができます。 Float を指定した場合、このエラーが発生します。

このエラーを修正するには、別のコンストラクターを使用するか、コンストラクターによって提供されるものと一致するようにパラメーター型を明示的にキャストします。

### <a name="unable-to-add-reference-reference-name-to-the-current-application"></a>参照 '\<参照名 > ' を現在のアプリケーションに追加できません

エラーメッセージ: "参照 '\<参照名 > ' を現在のアプリケーションに追加できません。 '\<参照名 > ' の別のバージョンが既に参照されていないことを確認してください。

Visual Studio は参照を追加できません。 このエラーを修正するには、参照の別のバージョンがまだ参照されていないことを確認します。

### <a name="unable-to-check-out-the-current-file"></a>現在のファイルをチェックアウトできません。

エラーメッセージ: "現在のファイルをチェックアウトできません。 ファイルがロックされているか、ファイルを手動でチェックアウトする必要がある可能性があります。

このエラーは、現在ソースコード管理にチェックインされているファイルを変更した場合に発生します。 通常、Visual Studio では、ユーザーがファイルをチェックアウトできるように、[ファイルのチェックアウト] ダイアログボックスが表示されます。 今回は、ファイルがチェックアウトされていませんでした。チェックアウト中のマージの競合が原因である可能性があります。 このエラーを修正するには、ファイルがロックされていないことを確認してから、手動でファイルをチェックアウトしてみてください。

### <a name="unable-to-find-page-named-options-dialog-box-tab-name"></a>'\<オプションダイアログボックスのタブ名 > ' という名前のページが見つかりません

このエラーは、コンポーネントデザイナーが、存在しない名前を使用して [オプション] ダイアログボックスからページへのアクセスを要求した場合に発生します。 コンポーネントのベンダにお問い合わせください。

### <a name="unable-to-find-property-property-name-on-page-options-dialog-box-tab-name"></a>プロパティ '\<プロパティ名 > ' がページ '\<オプションダイアログボックスのタブ名 > ' に見つかりません

このエラーは、コンポーネントデザイナーが [オプション] ダイアログボックスからページの特定の値へのアクセスを要求したが、その値が存在しない場合に発生します。 コンポーネントのベンダにお問い合わせください。

### <a name="visual-studio-cannot-open-a-designer-for-the-file-because-the-class-within-it-does-not-inherit-from-a-class-that-can-be-visually-designed"></a>Visual Studio 内のクラスが、画面上でデザインできるクラスから継承されていないため、ファイルのデザイナーを開けません。

Visual Studio はクラスをロードしましたが、そのクラスのデザイナーをロードできませんでした。 Visual Studio では、デザイナーがファイルの最初のクラスを使用する必要があります。 このエラーを修正するには、ファイルの最初のクラスになるようにクラスコードを移動してから、デザイナーを再度読み込みます。

### <a name="visual-studio-cannot-save-or-load-instances-of-the-type-type-name"></a>Visual Studio では、型 '\<型名 > ' のインスタンスを保存または読み込むことができません

これは、サードパーティのコンポーネントに問題があります。 コンポーネントのベンダにお問い合わせください。

### <a name="visual-studio-is-unable-to-open-document-name-in-design-view"></a>Visual Studio は、'\<ドキュメント名 > ' を開くことができませんデザインビュー

エラーメッセージ: "Visual Studio は、デザインビューで '\<ドキュメント名 > ' を開くことができません。 ファイルの種類に対してパーサーがインストールされていません。 "

このエラーは、プロジェクトの言語がデザイナーをサポートしていないことを示します。また、[ファイルを開く] ダイアログボックスまたはソリューションエクスプローラーからファイルを開こうとすると発生します。 代わりに、コードビューでファイルを編集します。

### <a name="visual-studio-was-unable-to-find-a-designer-for-classes-of-type-type-name"></a>'\<type name > ' 型のクラスのデザイナーが見つかりませんでした

Visual Studio によってクラスが読み込まれましたが、クラスをデザインすることはできません。 代わりに、クラスを右クリックして **[コードの表示]** を選択し、コードビューでクラスを編集します。

## <a name="see-also"></a>参照

- [デザイナーを使用した Windows フォームコントロールの開発](developing-windows-forms-controls-at-design-time.md)
- [Windows フォームデザイナーフォーラム](https://social.msdn.microsoft.com/Forums/windows/home?forum=winformsdesigner)
