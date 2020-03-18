---
title: .NET 4.5 での Windows Workflow Foundation の新機能
ms.date: 03/30/2017
ms.assetid: 195c43a8-e0a8-43d9-aead-d65a9e6751ec
ms.openlocfilehash: b14f18dce64bc5738f3d3c6af11d6d6224764486
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937883"
---
# <a name="whats-new-in-windows-workflow-foundation-in-net-45"></a>.NET 4.5 での Windows Workflow Foundation の新機能

.NET Framework 4.5 の Windows Workflow Foundation (WF) では、新しいアクティビティ、デザイナー機能、ワークフロー開発モデルなど、多くの新機能が導入されています。 .NET Framework 4.5 で導入された新しいワークフロー機能の多くは、再ホストされたワークフローデザイナーでサポートされています。 サポートされる新機能の詳細については、「再ホストされた[ワークフローデザイナーでの新しい Workflow Foundation 4.5 機能のサポート](wf-features-in-the-rehosted-workflow-designer.md)」を参照してください。 .NET 3.0 と .NET 3.5 ワークフローアプリケーションを移行して最新バージョンを使用する方法の詳細については、「[移行のガイダンス](migration-guidance.md)」を参照してください。 このトピックでは、.NET Framework 4.5 で導入された新しいワークフロー機能の概要について説明します。

> [!WARNING]
> .NET Framework 4.5 で導入された新しい Windows Workflow Foundation 機能は、以前のバージョンのフレームワークを対象とするプロジェクトでは使用できません。 .NET Framework 4.5 を対象とするプロジェクトが以前のバージョンのフレームワークを再ターゲットにすると、いくつかの問題が発生する可能性があります。
>
> - C#デザイナーでは、式が**XAML で設定されたメッセージ値**に置き換えられます。
> - 次のエラーを含む、多くのビルド エラーが発生します。
>
> **ファイル形式は、現在のターゲットフレームワークと互換性がありません。ファイル形式を変換するには、明示的にファイルを保存してください。このエラーメッセージは、ファイルを保存してデザイナーを再度開いた後に表示されます。**

## <a name="BKMK_Versioning"></a>ワークフローのバージョン管理

.NET Framework 4.5 では、新しい <xref:System.Activities.WorkflowIdentity> クラスに基づいて、いくつかの新しいバージョン管理機能が導入されました。 <xref:System.Activities.WorkflowIdentity> には、ワークフロー アプリケーションの作成者向けに、永続化されたワークフロー インスタンスをその定義でマップするメカニズムが備わっています。

- <xref:System.Activities.WorkflowApplication> ホスティングを使用する開発者は、<xref:System.Activities.WorkflowIdentity> を使用して、ワークフローの複数のバージョンを同時にホストできます。 永続化されたワークフロー インスタンスは新しい <xref:System.Activities.WorkflowApplicationInstance> クラスを使用して読み込むことができ、ホストは <xref:System.Activities.WorkflowApplicationInstance.DefinitionIdentity%2A> を使用して、<xref:System.Activities.WorkflowApplication> のインスタンス化時に適切なバージョンのワークフロー定義を提供できます。 詳細については、「 [WorkflowIdentity とバージョン管理の使用](using-workflowidentity-and-versioning.md)」および「[方法: 複数のバージョンのワークフローをサイドバイサイドでホスト](how-to-host-multiple-versions-of-a-workflow-side-by-side.md)する」を参照してください。

- 現在、<xref:System.ServiceModel.WorkflowServiceHost> は複数のバージョンのホストです。 ワークフロー サービスの新しいバージョンを配置すると、新しいインスタンスが新しいサービスを使用して作成されますが、既存のインスタンスは以前のバージョンを使用して完了します。 詳細については、「 [WorkflowServiceHost でのサイドバイサイドバージョン管理](../wcf/feature-details/side-by-side-versioning-in-workflowservicehost.md)」を参照してください。

- 永続化されたワークフロー インスタンスの定義を更新するためのメカニズムを提供する動的更新が導入されました。 詳細については、「[動的更新](dynamic-update.md)」および「[方法: 実行中のワークフローインスタンスの定義を更新する](how-to-update-the-definition-of-a-running-workflow-instance.md)」を参照してください。

- .NET Framework 4 つのデータベーススクリプトを使用して作成された永続性データベースをアップグレードするために、Sqlworkflowinstancestoreschemaupgrade.sql データベーススクリプトが用意されています。 このスクリプトは、.NET Framework 4.5 で導入された新しいバージョン管理機能をサポートするために .NET Framework 4 つの永続性データベースを更新します。 データベースで永続化されたワークフロー インスタンスは、既定のバージョン番号が付与されるため、side-by-side 実行および動的更新に参加できるようになります。 詳細については、「 [.NET Framework 4 の永続性データベースのアップグレード」を](using-workflowidentity-and-versioning.md#UpdatingWF4PersistenceDatabases)参照してください。

## <a name="BKMK_NewActivities"></a>実習

組み込みのアクティビティ ライブラリには、既存のアクティビティ用の新しいアクティビティと新しい機能が含まれています。

### <a name="BKMK_NoPersistScope"></a>NoPersist スコープ

<xref:System.Activities.Statements.NoPersistScope> は、NoPersistScope の子アクティビティの実行時にワークフローが永続化されないようにする新しいコンテナー アクティビティです。 これは、ワークフローがファイル ハンドルなどのコンピューター固有のリソースを使用している場合、データベース トランザクション中など、ワークフローの永続化が適切でないシナリオで役立ちます。 以前のバージョンでは、アクティビティ実行中に永続化されないようにするために、<xref:System.Activities.NativeActivity> を使用したカスタム <xref:System.Activities.NoPersistHandle> が必要でした。

### <a name="BKMK_NewFlowchartCapabilities"></a>新しいフローチャート機能

.NET Framework 4.5 のフローチャートが更新され、次の新機能が追加されました。

- `DisplayName` アクティビティまたは <xref:System.Activities.Statements.FlowSwitch%601> アクティビティの <xref:System.Activities.Statements.FlowDecision> プロパティは編集できます。 これにより、アクティビティ デザイナーではアクティビティの目的に関する詳細な情報を表示できます。

- フローチャートには <xref:System.Activities.Statements.Flowchart.ValidateUnconnectedNodes%2A> という新しいプロパティがあります。このプロパティの既定値は `False` です。 このプロパティを `True` に設定すると、接続されていないフローチャート ノードでは検証エラーが発生します。

## <a name="support-for-partial-trust"></a>部分信頼のサポート

.NET Framework 4 のワークフローには、完全に信頼されたアプリケーションドメインが必要でした。 .NET Framework 4.5 では、ワークフローは部分信頼環境で動作できます。 部分信頼環境では、サード パーティのコンポーネントは、ホストのリソースに対するフル アクセスを許可しなくても使用できます。 部分信頼でのワークフローの実行に関する問題は次のとおりです。

1. 部分信頼環境では、<xref:System.Activities.Statements.Interop> アクティビティに含まれるレガシ コンポーネント (規則など) を使用することはできません。

2. <xref:System.ServiceModel.WorkflowServiceHost> において部分信頼でワークフローを実行することはできません。

3. 部分信頼シナリオで例外を永続化すると、セキュリティ上の脅威になる可能性があります。 例外の永続化を無効にするには、例外が永続化されないように <xref:System.Activities.ExceptionPersistenceExtension> 型の拡張機能をプロジェクトに追加する必要があります。 この型を実装する方法を示したコード例を次に示します。

    ```csharp
    public class ExceptionPersistenceExtension
    {
        public ExceptionPersistenceExtension()
        {
            this.PersistExceptions = false;
        }
        public bool PersistExceptions { get; set; }
    }
    ```

     例外がシリアル化されない場合は、例外が <xref:System.Activities.Statements.NoPersistScope> 内で使用されていることを確認してください。

4. アクティビティの作成者は、<xref:System.Activities.Activity.CacheMetadata%2A> をオーバーライドして、ワークフロー ランタイムが型に対してリフレクションを自動的に実行しないようにする必要があります。 引数と子アクティビティには null 以外を指定する必要があり、<xref:System.Activities.ActivityMetadata.Bind%2A> は明示的に呼び出す必要があります。 <xref:System.Activities.Activity.CacheMetadata%2A>のオーバーライドの詳細については、「 [CacheMetadata を使用したデータの公開](exposing-data-with-cachemetadata.md)」を参照してください。 また、リフレクションによって作成されないようにするには、`internal` または**private**である型の引数のインスタンスを <xref:System.Activities.Activity.CacheMetadata%2A> で明示的に作成する必要があります。

5. 型は、シリアル化に <xref:System.Runtime.Serialization.ISerializable> または <xref:System.SerializableAttribute> を使用しません。シリアル化する型では、<xref:System.Runtime.Serialization.DataContractSerializer> をサポートする必要があります。

6. <xref:System.Activities.Expressions.LambdaValue%601> を使用する式は <xref:System.Security.Permissions.ReflectionPermissionAttribute.RestrictedMemberAccess%2A> を必要とするため、部分信頼では動作しません。 <xref:System.Activities.Expressions.LambdaValue%601> を使用するワークフローでは、これらの式を、<xref:System.Activities.CodeActivity%601> から派生するアクティビティと置き換える必要があります。 を確認しています。

7. 部分信頼では、<xref:System.Activities.XamlIntegration.TextExpressionCompiler> または Visual Basic でホストされているコンパイラを使用して式をコンパイルすることはできませんが、以前コンパイルされた式を実行することはできます。

8. [レベル2の透過性](https://aka.ms/Level2Transparency)を使用する1つのアセンブリを .NET Framework 4 で使用したり、完全信頼で [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] したり、部分信頼で [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] したりすることはできません。

## <a name="BKMK_NewDesignerCapabilites"></a>新しいデザイナーの機能

### <a name="BKMK_DesignerSearch"></a>デザイナー検索

より大規模なワークフローを管理しやすくするために、キーワードでワークフローを検索できるようになりました。 この機能は、Visual Studio でのみ使用できます。この機能は、再ホストされたデザイナーでは使用できません。 利用できる検索機能には 2 種類あります。

- [クイック検索]。 **Ctrl + F**または **[編集**]、 **[検索と置換]** 、 **[クイック検索]** のいずれかを使用して開始されます。

- **Ctrl + Shift + F キー**または **[編集]** 、 **[検索と置換]** 、 **[フォルダー]** を使用して検索 のいずれかで開始されたフォルダーを検索します。

置換はサポートされていません。

#### <a name="BKMK_QuickFind"></a>クイック検索

ワークフロー内で検索されるキーワードは、次のデザイナー項目に一致します。

- <xref:System.Activities.Activity> オブジェクト、<xref:System.Activities.Statements.FlowNode> オブジェクト、<xref:System.Activities.Statements.State> オブジェクト、<xref:System.Activities.Statements.Transition> オブジェクト、およびその他のカスタム フロー制御項目のプロパティ。

- ［変数］

- 引数

- 式

クイック検索はデザイナーの <xref:System.Activities.Presentation.Model.ModelItem> ツリーで実行されます。 クイック検索は、ワークフロー定義にインポートされた名前空間を検索しません。

#### <a name="BKMK_FindInFiles"></a>フォルダーを検索する

ワークフロー内で検索されるキーワードは、ワークフロー ファイルの実際のコンテンツに一致します。 検索結果は Visual Studio の検索結果ビュー ペインに表示されます。 結果の項目をダブルクリックすると、ワークフロー デザイナーで一致を含むアクティビティに移動します。

### <a name="BKMK_VariableDeleteContextMenu"></a>変数および引数デザイナーのコンテキストメニュー項目の削除

.NET Framework 4 では、変数と引数は、キーボードを使用してデザイナーでのみ削除できました。 .NET Framework 4.5 以降では、コンテキストメニューを使用して変数と引数を削除できます。

変数デザイナーと引数デザイナーのコンテキスト メニューを次のスクリーンショットに示しています。

![変数/引数デザイナーのコンテキスト メニュー](./media/whats-new-in-wf-in-dotnet/designer-context-menu.png)

### <a name="BKMK_AutoSurround"></a>シーケンスを使用した自動ブロック

ワークフローまたは特定のコンテナー アクティビティ (<xref:System.Activities.Statements.NoPersistScope> など) には Body アクティビティを 1 つしか含めることができないため、2 つ目のアクティビティを追加するには、開発者が最初のアクティビティを削除し、<xref:System.Activities.Statements.Sequence> アクティビティを追加してから、シーケンス アクティビティに両方のアクティビティを追加する必要がありました。 .NET Framework 4.5 以降では、デザイナー画面に2つ目のアクティビティを追加すると、両方のアクティビティをラップする `Sequence` アクティビティが自動的に作成されます。

次のスクリーンショットは、`WriteLine` の `Body` 内の `NoPersistScope` アクティビティを示しています。

![NoPersistScope アクティビティの本体の WriteLine アクティビティ。](./media/whats-new-in-wf-in-dotnet/auto-surround-write-line-activity.png)

次のスクリーンショットは、2 つ目の `WriteLine` を 1 つ目の下にドロップしたときに `Body` 内に自動的に作成された `Sequence` アクティビティを示しています。

![NoPersistScope の本体に自動的に作成されたシーケンス。](./media/whats-new-in-wf-in-dotnet/auto-surround-sequence-activity.png)

### <a name="BKMK_PanMode"></a>パンモード

デザイナーで大規模なワークフロー内をより簡単に移動するには、パン モードを有効にすると、開発者は、スクロール バーを使用する必要なく、ワークフローの表示される部分をクリックおよびドラッグして移動できるようになります。 パン モードをアクティブ化するボタンは、デザイナーの右下隅にあります。

次のスクリーンショットは、ワークフロー デザイナーの右下隅にあるパン ボタンを示しています。

![ワークフローデザイナーで [パン] ボタンが強調表示されています。](./media/whats-new-in-wf-in-dotnet/pan-button-workflow-designer.png)

マウスの中央ボタンまたは Space キーを使用して、ワークフロー デザイナーをパンすることもできます。

### <a name="BKMK_MultiSelect"></a>複数選択

複数のアクティビティを同時に選択できます。これを行うには、複数のアクティビティを囲むようにドラッグするか (パン モードが無効な場合)、Ctrl キーを押したまま目的のアクティビティを 1 つずつクリックします。

選択した複数のアクティビティは、デザイナー内でドラッグ アンド ドロップすることも、コンテキスト メニューを使用して操作することもできます。

### <a name="BKMK_DocumentOutline"></a>ワークフロー項目のアウトラインビュー

階層ワークフローを移動しやすくするため、ワークフローのコンポーネントはツリー スタイルのアウトライン表示で示されます。 アウトライン ビューは、**ドキュメントアウトライン** ビューに表示されます。 このビューを開くには、上部のメニューで **[表示]** 、 **[その他のウィンドウ]** 、 **[ドキュメントアウトライン]** の順に選択するか、Ctrl W キーを押しながら U キーを押します。 アウトライン表示でノードをクリックすると、ワークフロー デザイナーの対応するアクティビティに移動し、アウトライン表示が更新されて、デザイナーで選択されているアクティビティが表示されます。

[はじめにチュートリアル](getting-started-tutorial.md)の完成したワークフローの次のスクリーンショットは、シーケンシャルワークフローを含むアウトラインビューを示しています。

![Visual Studio のシーケンシャルワークフローを含むアウトラインビューのスクリーンショット。](./media/whats-new-in-wf-in-dotnet/outline-view-in-workflow-designer.jpg)

### <a name="BKMK_CSharpExpressions"></a>C#式

.NET Framework 4.5 より前では、ワークフロー内のすべての式は Visual Basic でのみ記述できました。 .NET Framework 4.5 では、Visual Basic 式は Visual Basic を使用して作成されたプロジェクトにのみ使用されます。 Visual C# プロジェクトでは、式に C# が使用されるようになりました。 文法強調表示や Intellisense などの機能を備えた、フル機能の C# 式エディターが用意されています。 以前のバージョンで作成された、Visual Basic の式を使用する C# ワークフロー プロジェクトは引き続き動作します。

C# の式はデザイン時に検証されます。 C# 式のエラーは赤い波下線でマークされます。

式の詳細については、「式」を参照してください。 [ C# ](csharp-expressions.md) C#

### <a name="BKMK_Visibility"></a>シェルバーとヘッダー項目の表示をより詳細に制御する

再ホストされたデザイナーでは、標準 UI コントロールの中に、特定のワークフローにとって意味がないものもあれば、無効になっているものもあります。 .NET Framework 4 では、このカスタマイズはデザイナーの下部にあるシェルバーでのみサポートされています。 .NET Framework 4.5 では、デザイナーの上部にあるシェルヘッダー項目の表示を調整するには、適切な <xref:System.Activities.Presentation.View.ShellHeaderItemsVisibility> 値を使用して <xref:System.Activities.Presentation.View.DesignerView.WorkflowShellHeaderItemsVisibility%2A> を設定します。

### <a name="BKMK_AutoConnect"></a>フローチャートおよびステートマシンワークフローでの自動接続と自動挿入

.NET Framework 4 では、フローチャートワークフロー内のノード間の接続を手動で追加する必要がありました。 .NET Framework 4.5 では、フローチャートおよびステートマシンノードには、アクティビティをツールボックスからデザイナー画面にドラッグしたときに表示される自動接続ポイントがあります。 アクティビティをこれらのポイントのうち 1 つにドロップすると、アクティビティが必要な接続と共に自動的に追加されます。

次のスクリーンショットは、アクティビティがツールボックスからドラッグされるときに表示されるアタッチ ポイントを示します。

![自動接続ポイントを示すフローチャートの開始ノード](./media/whats-new-in-wf-in-dotnet/auto-connect-points-start-node.png)

アクティビティは、フローチャート ノードと状態の間の接続にドラッグすることで、その他 2 つのノード間にノードを自動挿入することもできます。 次のスクリーンショットは、アクティビティをツールボックスからドラッグ アンド ドロップできる、強調表示された接続線を示しています。

![アクティビティをドロップするための自動挿入ハンドル](./media/whats-new-in-wf-in-dotnet/auto-insert-connecting-line.png)

### <a name="BKMK_Annotations"></a>デザイナー注釈

より大規模なワークフローの開発を容易にするため、デザイン プロセスを追跡できるよう注釈の追加がサポートされるようになりました。 注釈は、アクティビティ、状態、フローチャート ノード、変数、および引数に追加できます。 次のスクリーンショットは、デザイナーに注釈を追加するためのコンテキスト メニューを示しています。

![注釈を追加するためのメニューを示すスクリーンショット。](./media/whats-new-in-wf-in-dotnet/designer-annotations-context-menu.png)

### <a name="debugging-states"></a>デバッグ状態

.NET Framework 4 では、アクティビティ以外の要素は実行単位ではないため、デバッグブレークポイントをサポートできませんでした。 このリリースでは、<xref:System.Activities.Statements.State> オブジェクトにブレークポイントを追加する機能が用意されています。 ブレークポイントを <xref:System.Activities.Statements.State> に設定した場合、そのエントリ アクティビティまたはトリガーがスケジュールされる前に、状態が遷移すると実行が停止します。

### <a name="BKMK_ActivityDelegates"></a>デザイナーでの ActivityDelegate オブジェクトの定義と使用

.NET Framework 4 のアクティビティは、ワークフローの他の部分がワークフローの実行と対話できる実行ポイントを公開するためにオブジェクト <xref:System.Activities.ActivityDelegate> 使用されていますが、通常、これらの実行ポイントを使用するにはかなりの量のコードが必要です。 このリリースでは、開発者はワークフロー デザイナーを使用してアクティビティ デリゲートを定義および使用できます。 詳細については、「[方法: ワークフローデザイナーでアクティビティデリゲートを定義および使用する](/visualstudio/workflow-designer/how-to-define-and-consume-activity-delegates-in-the-workflow-designer)」を参照してください。

### <a name="BKMK_BuildTimeValidation"></a>ビルド時の検証

.NET Framework 4 では、ワークフロープロジェクトのビルド中に、ワークフローの検証エラーがビルドエラーとしてカウントされませんでした。 つまり、ワークフローの検証エラーが発生した場合でも、ワークフロー プロジェクトのビルドは成功している可能性があります。 .NET Framework 4.5 では、ワークフローの検証エラーによってビルドが失敗します。

### <a name="BKMK_DesignTimeValidation"></a>デザイン時のバックグラウンド検証

.NET Framework 4 では、ワークフローがフォアグラウンドプロセスとして検証されました。これにより、複雑な、または時間のかかる検証プロセスで UI がブロックされる可能性があります。 現在、ワークフローの検証はバックグラウンド スレッドで実行されるため、UI がブロックされることはありません。

### <a name="BKMK_ViewState"></a>XAML ファイル内の別の場所にあるビューステート

.NET Framework 4 では、ワークフローのビューステート情報は、さまざまな場所にある XAML ファイルに格納されます。 これは、XAML を直接読み取ったり、ビューステート情報を削除するコードを記述したりする開発者にとっては不便です。 .NET Framework 4.5 では、XAML ファイル内のビューステート情報が XAML ファイル内の個別の要素としてシリアル化されます。 開発者は、アクティビティのビューステート情報を簡単に見つけて編集したり、ビューステートを完全に削除したりできます。

### <a name="BKMK_ExpressionExtensibility"></a>式の拡張

.NET Framework 4.5 では、開発者がワークフローデザイナーにプラグインできる独自の式および式作成操作を作成する方法を提供しています。

### <a name="BKMK_BackwardCompatRehostedDesigner"></a>再ホストされたデザイナーのワークフロー4.5 機能のオプトイン

旧バージョンとの互換性を維持するために、再ホストされたデザイナーでは、.NET Framework 4.5 に含まれるいくつかの新機能が既定で有効になっていません。 これは、再ホストされたデザイナーを使用する既存のアプリケーションが、最新バージョンに更新することで壊れないようにするためです。 再ホストされたデザイナーで新機能を有効にするには、<xref:System.Activities.Presentation.DesignerConfigurationService.TargetFrameworkName%2A> を ".NET Framework 4.5" に設定するか、<xref:System.Activities.Presentation.DesignerConfigurationService> の各メンバーを設定して各機能を有効にします。

## <a name="BKMK_NewWFModels"></a>新しいワークフロー開発モデル

このリリースには、フローチャートおよびシーケンシャル ワークフロー開発モデルに加えて、ステート マシンのワークフロー、およびコントラクト優先ワークフロー サービスが含まれています。

### <a name="BKMK_StateMachine"></a>ステートマシンのワークフロー

ステートマシンのワークフローは、 [Microsoft .NET Framework 4 Platform Update 1](https://docs.microsoft.com/archive/blogs/endpoint/microsoft-net-framework-4-platform-update-1)で .NET Framework 4 バージョン4.0.1 の一部として導入されました。 この更新プログラムには、開発者がステート マシンのワークフローを作成できるようにする、いくつかの新しいクラスとアクティビティが含まれていました。 これらのクラスとアクティビティは .NET Framework 4.5 に対して更新されました。 更新プログラムは、下記のものなどです。

1. 状態にブレークポイントを設定する機能。

2. ワークフロー デザイナーで遷移をコピーして貼り付ける機能。

3. トリガーを共有する遷移の作成に対するデザイナーのサポート。

4. ステート マシンのワークフロー作成に使用するアクティビティ (<xref:System.Activities.Statements.StateMachine><xref:System.Activities.Statements.State>、<xref:System.Activities.Statements.Transition> など)。

次のスクリーンショットは、[はじめにチュートリアル](getting-started-tutorial.md)の手順[「ステートマシンワークフローを作成する方法](how-to-create-a-state-machine-workflow.md)」の完成したステートマシンワークフローを示しています。

![完成したステートマシンのワークフローを示す図。](./media/whats-new-in-wf-in-dotnet/complete-state-machine-workflow.jpg)

ステートマシンワークフローの作成の詳細については、「[ステートマシンワークフロー](state-machine-workflows.md)」を参照してください。

### <a name="BKMK_ContractFirst"></a>コントラクト優先ワークフローの開発

コントラクト優先ワークフロー開発ツールを使用すると、開発者は code first でコントラクトをデザインできます。 Visual Studio で数回クリックするだけで、各操作を表すアクティビティテンプレートがツールボックスに自動的に生成されます。 これらのアクティビティは、コントラクトで定義された操作を実装するワークフローを作成するために使用されます。 ワークフロー デザイナーは、ワークフロー サービスを検証し、これらの操作が実装され、ワークフローの署名がコントラクトの署名と一致することを確認します。 また、開発者は、ワークフロー サービスを、実装済みコントラクトのコレクションと関連付けることもできます。 コントラクト優先ワークフローサービスの開発の詳細については、「[方法: 既存のサービスコントラクトを使用するワークフローサービスを作成](how-to-create-a-workflow-service-that-consumes-an-existing-service-contract.md)する」を参照してください。
