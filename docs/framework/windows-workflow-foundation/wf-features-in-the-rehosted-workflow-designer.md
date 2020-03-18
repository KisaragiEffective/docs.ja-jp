---
title: 再ホストされたワークフロー デザイナーにおける Workflow Foundation 4.5 の新機能のサポート
ms.date: 03/30/2017
ms.assetid: 1a4a4038-d8e6-41dd-99ea-93bd76286772
ms.openlocfilehash: 1c554c60bf2e50a8eb89764a21ad15b95343b182
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937730"
---
# <a name="support-for-new-workflow-foundation-45-features-in-the-rehosted-workflow-designer"></a>再ホストされたワークフロー デザイナーにおける Workflow Foundation 4.5 の新機能のサポート
.NET Framework 4.5 の Windows Workflow Foundation (WF) では、ワークフローデザイナーのエクスペリエンスに対するいくつかの機能強化など、多くの新機能が導入されました。 このトピックでは、このような新機能のうち、再ホストされたデザイナーでサポートされている機能と現時点ではサポートされていない機能について詳しく説明します。

> [!NOTE]
> .NET Framework 4.5 で導入された新しい Windows Workflow Foundation (WF) 機能の一覧については、「 [.net 4.5 における Windows Workflow Foundation の新](whats-new-in-wf-in-dotnet.md)機能」を参照してください。

## <a name="activities"></a>活動
 組み込みのアクティビティ ライブラリには、既存のアクティビティ用の新しいアクティビティと新しい機能が含まれています。 これらの新しいアクティビティはすべて、再ホストされたデザイナーでサポートされています。 これらの新しいアクティビティの詳細については、「 [.net 4.5 の Windows Workflow Foundation の新機能](whats-new-in-wf-in-dotnet.md)」の「[アクティビティ](whats-new-in-wf-in-dotnet.md#BKMK_NewActivities)」セクションを参照してください。

## <a name="c-expressions"></a>C# の式
 .NET Framework 4.5 より前では、ワークフロー内のすべての式は Visual Basic でのみ記述できました。 .NET Framework 4.5 では、Visual Basic 式は Visual Basic を使用して作成されたプロジェクトにのみ使用されます。 Visual C# プロジェクトでは、式に C# が使用されるようになりました。 Visual Studio 2012 でワークフローを作成する場合、完全C#に機能する式エディターには、文法の強調表示や intellisense などの機能が用意されています。 以前のバージョンで作成された、Visual Basic の式を使用する C# ワークフロー プロジェクトは引き続き動作します。

> [!WARNING]
> C# の式は、再ホストされたデザイナーではサポートされていません。

## <a name="new-designer-capabilities"></a>デザイナーの新機能

### <a name="designer-search"></a>デザイナーでの検索
 .NET Framework 4.5 で導入された [[クイック検索](whats-new-in-wf-in-dotnet.md#BKMK_QuickFind)] 機能と[[フォルダーを](whats-new-in-wf-in-dotnet.md#BKMK_FindInFiles)使用して検索] 機能は、再ホストされたデザイナーではサポートされていません。 `Toolbox` による検索は、再ホストされたデザイナーでもサポートされています。 これらの機能の詳細については、「[デザイナー検索](whats-new-in-wf-in-dotnet.md#BKMK_DesignerSearch)」を参照してください。

> [!WARNING]
> [[クイック検索](whats-new-in-wf-in-dotnet.md#BKMK_QuickFind)] と[[フォルダーを検索](whats-new-in-wf-in-dotnet.md#BKMK_FindInFiles)する] は、再ホストされたデザイナーではサポートされていません。

### <a name="delete-context-menu-item-in-variable-and-argument-designer"></a>変数デザイナーと引数デザイナーのコンテキスト メニューの [削除]
 .NET Framework 4 では、変数と引数は、キーボードを使用してデザイナーでのみ削除できました。 .NET Framework 4.5 以降では、コンテキストメニューを使用して変数と引数を削除できます。 この機能は、再ホストされたデザイナーでサポートされています。

 変数デザイナーと引数デザイナーのコンテキスト メニューを次のスクリーンショットに示しています。

 ![変数/引数デザイナーのコンテキスト メニュー](./media/wf-features-in-the-rehosted-workflow-designer/designer-context-menu.png)

### <a name="auto-surround-with-sequence"></a>ブロックの自動挿入シーケンス
 ワークフローまたは特定のコンテナー アクティビティ (<xref:System.Activities.Statements.NoPersistScope> など) には Body アクティビティを 1 つしか含めることができないため、2 つ目のアクティビティを追加するには、開発者が最初のアクティビティを削除し、<xref:System.Activities.Statements.Sequence> アクティビティを追加してから、シーケンス アクティビティに両方のアクティビティを追加する必要がありました。 .NET Framework 4.5 以降では、デザイナー画面に2つ目のアクティビティを追加すると、両方のアクティビティをラップする `Sequence` アクティビティが自動的に作成されます。 この機能は、再ホストされたデザイナーでサポートされています。

 次のスクリーンショットは、`WriteLine` の `Body` 内の `NoPersistScope` アクティビティを示しています。

 ![NoPersistScope アクティビティの本体の WriteLine アクティビティ。](./media/wf-features-in-the-rehosted-workflow-designer/auto-surround-write-line-activity.png)

 次のスクリーンショットは、2 つ目の `WriteLine` を 1 つ目の下にドロップしたときに `Body` 内に自動的に作成された `Sequence` アクティビティを示しています。

 ![NoPersistScope の本体に自動的に作成されたシーケンス。](./media/wf-features-in-the-rehosted-workflow-designer/auto-surround-sequence-activity.png)

### <a name="pan-mode"></a>パン モード
 デザイナーで大規模なワークフロー内をより簡単に移動するには、パン モードを有効にすると、開発者は、スクロール バーを使用する必要なく、ワークフローの表示される部分をクリックおよびドラッグして移動できるようになります。 パン モードをアクティブ化するボタンは、デザイナーの右下隅にあります。 この機能は、再ホストされたデザイナーでサポートされています。

 次のスクリーンショットは、ワークフロー デザイナーの右下隅にあるパン ボタンを示しています。

 ![ワークフローデザイナーで [パン] ボタンが強調表示されています。](./media/wf-features-in-the-rehosted-workflow-designer/pan-button-workflow-designer.png)

 マウスの中央ボタンまたは Space キーを使用して、ワークフロー デザイナーをパンすることもできます。

### <a name="multi-select"></a>複数選択
 複数のアクティビティを同時に選択できます。これを行うには、複数のアクティビティを囲むようにドラッグするか (パン モードが無効な場合)、Ctrl キーを押したまま目的のアクティビティを 1 つずつクリックします。 この機能は、再ホストされたデザイナーでサポートされています。

 選択した複数のアクティビティは、デザイナー内でドラッグ アンド ドロップすることも、コンテキスト メニューを使用して操作することもできます。

### <a name="outline-view-of-workflow-items"></a>ワークフロー項目のアウトライン表示
 階層ワークフローを移動しやすくするため、ワークフローのコンポーネントはツリー スタイルのアウトライン表示で示されます。 アウトライン ビューは、**ドキュメントアウトライン** ビューに表示されます。 このビューを Visual Studio で開くには、上部のメニューで **[表示]** 、 **[その他のウィンドウ]** 、 **[ドキュメントアウトライン]** の順に選択するか、Ctrl W キーを押しながら U キーを押します。 アウトライン表示でノードをクリックすると、ワークフロー デザイナーの対応するアクティビティに移動し、アウトライン表示が更新されて、デザイナーで選択されているアクティビティが表示されます。 この機能は、再ホストされたデザイナーでサポートされています。

 [はじめにチュートリアル](getting-started-tutorial.md)の完成したワークフローの次のスクリーンショットは、シーケンシャルワークフローを含むアウトラインビューを示しています。

 ![Visual Studio のシーケンシャルワークフローを含むアウトラインビューのスクリーンショット](./media/wf-features-in-the-rehosted-workflow-designer/outline-view-in-workflow-designer.jpg)

### <a name="more-control-of-visibility-of-shell-bar-and-header-items"></a>シェル バーおよびヘッダー項目の可視性の詳細な制御
 再ホストされたデザイナーでは、標準 UI コントロールの中に、特定のワークフローにとって意味がないものもあれば、無効になっているものもあります。 .NET Framework 4 では、このカスタマイズはデザイナーの下部にあるシェルバーでのみサポートされています。 .NET Framework 4.5 では、デザイナーの上部にあるシェルヘッダー項目の表示を調整するには、適切な <xref:System.Activities.Presentation.View.ShellHeaderItemsVisibility> 値を使用して <xref:System.Activities.Presentation.View.DesignerView.WorkflowShellHeaderItemsVisibility%2A> を設定します。

### <a name="auto-connect-and-auto-insert-in-flowchart-and-state-machine-workflows"></a>フローチャートおよびステート マシンのワークフローの自動接続と自動挿入
 .NET Framework 4 では、フローチャートワークフロー内のノード間の接続を手動で追加する必要がありました。 .NET Framework 4.5 では、フローチャートおよびステートマシンノードには、アクティビティをツールボックスからデザイナー画面にドラッグしたときに表示される自動接続ポイントがあります。 アクティビティをこれらのポイントのうち 1 つにドロップすると、アクティビティが必要な接続と共に自動的に追加されます。

 次のスクリーンショットは、アクティビティがツールボックスからドラッグされるときに表示されるアタッチ ポイントを示します。

 ![自動接続ポイントを示すフローチャートの開始ノード](./media/wf-features-in-the-rehosted-workflow-designer/auto-connect-points-start-node.png)

 アクティビティは、フローチャート ノードと状態の間の接続にドラッグすることで、その他 2 つのノード間にノードを自動挿入することもできます。 次のスクリーンショットは、アクティビティをツールボックスからドラッグ アンド ドロップできる、強調表示された接続線を示しています。

 ![アクティビティをドロップするための自動挿入ハンドル](./media/wf-features-in-the-rehosted-workflow-designer/auto-insert-connecting-line.png)

 自動接続と自動挿入は、再ホストされたデザイナーでサポートされています。

### <a name="designer-annotations"></a>デザイナー注釈
 より大規模なワークフローの開発を容易にするため、デザイン プロセスを追跡できるよう注釈の追加がサポートされるようになりました。 注釈は、アクティビティ、状態、フローチャート ノード、変数、および引数に追加できます。 次のスクリーンショットは、デザイナーに注釈を追加するためのコンテキスト メニューを示しています。

 ![表記を追加するためのメニューを表示するスクリーンショット。](./media/wf-features-in-the-rehosted-workflow-designer/designer-annotations-context-menu.png)

 デザイナー注釈は、再ホストされたデザイナーでサポートされています。

### <a name="define-and-consume-activitydelegate-objects-in-the-designer"></a>デザイナーでの ActivityDelegate オブジェクトの定義と使用
 .NET Framework 4 のアクティビティは、ワークフローの他の部分がワークフローの実行と対話できる実行ポイントを公開するためにオブジェクト <xref:System.Activities.ActivityDelegate> 使用されていますが、通常、これらの実行ポイントを使用するにはかなりの量のコードが必要です。 このリリースでは、開発者はワークフロー デザイナーを使用してアクティビティ デリゲートを定義および使用できます。 詳細については、「[方法: ワークフローデザイナーでアクティビティデリゲートを定義および使用する](/visualstudio/workflow-designer/how-to-define-and-consume-activity-delegates-in-the-workflow-designer)」を参照してください。

 アクティビティ デリゲートは、再ホストされたデザイナーでサポートされています。

### <a name="build-time-validation"></a>ビルド時の検証
 .NET Framework 4 では、ワークフロープロジェクトのビルド中に、ワークフローの検証エラーがビルドエラーとしてカウントされませんでした。 つまり、ワークフローの検証エラーが発生した場合でも、ワークフロー プロジェクトのビルドは成功している可能性があります。 .NET Framework 4.5 では、ワークフローの検証エラーによってビルドが失敗します。

> [!WARNING]
> ビルド時の検証は、再ホストされたデザイナーではサポートされていません。  
  
### <a name="design-time-background-validation"></a>デザイン時バックグラウンド検証  
 .NET Framework 4 では、ワークフローがフォアグラウンドプロセスとして検証されました。これにより、複雑な、または時間のかかる検証プロセスで UI がブロックされる可能性があります。 現在、ワークフローの検証はバックグラウンド スレッドで実行されるため、UI がブロックされることはありません。  
  
 デザイン時バックグラウンド検証は、再ホストされたデザイナーでサポートされています。  
  
### <a name="view-state-located-in-a-separate-location-in-xaml-files"></a>XAML ファイル内で別々の場所にあるビューステート  
 .NET Framework 4 では、ワークフローのビューステート情報は、さまざまな場所にある XAML ファイルに格納されます。 これは、XAML を直接読み取ったり、ビューステート情報を削除するコードを記述したりする開発者にとっては不便です。 .NET Framework 4.5 では、XAML ファイル内のビューステート情報が XAML ファイル内の個別の要素としてシリアル化されます。  開発者は、アクティビティのビューステート情報を簡単に見つけて編集したり、ビューステートを完全に削除したりできます。  
  
 この機能は、再ホストされたワークフロー デザイナーでサポートされています。  
  
### <a name="opt-in-for-workflow-45-features-in-rehosted-designer"></a>再ホストされたデザイナーでの Workflow 4.5 機能のオプトイン  
 旧バージョンとの互換性を維持するために、再ホストされたデザイナーでは、.NET Framework 4.5 に含まれるいくつかの新機能が既定で有効になっていません。 これは、再ホストされたデザイナーを使用する既存のアプリケーションが、最新バージョンに更新することで壊れないようにするためです。 再ホストされたデザイナーで新機能を有効にするには、<xref:System.Activities.Presentation.DesignerConfigurationService.TargetFrameworkName%2A> を ".Net Framework 4.5" に設定するか、<xref:System.Activities.Presentation.DesignerConfigurationService> の各メンバーを設定して各機能を有効にします。  
  
## <a name="new-workflow-development-models"></a>新しいワークフロー開発モデル  
 このリリースには、フローチャートおよびシーケンシャル ワークフロー開発モデルに加えて、ステート マシンのワークフロー、およびコントラクト優先ワークフロー サービスが含まれています。  
  
### <a name="state-machine-workflows"></a>ステート マシンのワークフロー  
 ステートマシンのワークフローは、 [Microsoft .NET Framework 4 Platform Update 1](https://docs.microsoft.com/archive/blogs/endpoint/microsoft-net-framework-4-platform-update-1)の .NET Framework 4.0.1 の一部として導入されました。 この更新プログラムには、開発者がステート マシンのワークフローを作成できるようにする、いくつかの新しいクラスとアクティビティが含まれていました。 これらのクラスとアクティビティは .NET Framework 4.5 に対して更新されました。 更新プログラムは、下記のものなどです。  
  
1. 状態にブレークポイントを設定する機能。  
  
2. ワークフロー デザイナーで遷移をコピーして貼り付ける機能。  
  
3. トリガーを共有する遷移の作成に対するデザイナーのサポート。  
  
4. ステート マシンのワークフロー作成に使用するアクティビティ (<xref:System.Activities.Statements.StateMachine><xref:System.Activities.Statements.State>、<xref:System.Activities.Statements.Transition> など)。  
  
 次のスクリーンショットは、[はじめにチュートリアル](getting-started-tutorial.md)の手順[「ステートマシンワークフローを作成する方法](how-to-create-a-state-machine-workflow.md)」の完成したステートマシンワークフローを示しています。  
  
 ![完成したステートマシンのワークフローを示す図。](./media/wf-features-in-the-rehosted-workflow-designer/complete-state-machine-workflow.jpg)  
  
 ステートマシンワークフローの作成の詳細については、「[ステートマシンワークフロー](state-machine-workflows.md)」を参照してください。 ステート マシンのワークフローは、再ホストされたデザイナーでサポートされています。  
  
### <a name="contract-first-workflow-development"></a>コントラクト優先ワークフローの開発  
 コントラクト優先ワークフロー開発ツールを使用すると、開発者は code first でコントラクトをデザインできます。 Visual Studio で数回クリックするだけで、各操作を表すアクティビティテンプレートがツールボックスに自動的に生成されます。 これらのアクティビティは、コントラクトで定義された操作を実装するワークフローを作成するために使用されます。 ワークフロー デザイナーは、ワークフロー サービスを検証し、これらの操作が実装され、ワークフローの署名がコントラクトの署名と一致することを確認します。 また、開発者は、ワークフロー サービスを、実装済みコントラクトのコレクションと関連付けることもできます。 コントラクト優先ワークフローサービスの開発の詳細については、「[方法: 既存のサービスコントラクトを使用するワークフローサービスを作成](how-to-create-a-workflow-service-that-consumes-an-existing-service-contract.md)する」を参照してください。  
  
> [!WARNING]
> コントラクト優先ワークフローの開発は、ワークフロー デザイナーではサポートされていません。
