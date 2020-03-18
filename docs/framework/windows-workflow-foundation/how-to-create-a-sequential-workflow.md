---
title: '方法: シーケンシャル ワークフローを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 5280e816-ae17-48c4-8de0-a1e6895dd8f0
ms.openlocfilehash: 61e3f01b1259536ff15d71526e91aef42069722e
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/14/2019
ms.locfileid: "70989702"
---
# <a name="how-to-create-a-sequential-workflow"></a>方法: シーケンシャル ワークフローを作成する

ワークフローは、ビルトイン アクティビティおよびカスタム アクティビティから構築できます。 このトピックでは、<xref:System.Activities.Statements.Sequence> アクティビティなどの組み込みアクティビティと、前の [のカスタムアクティビティの両方を使用するワークフローを作成する手順について説明します。アクティビティ](how-to-create-an-activity.md) トピックを作成します。 このワークフローは、数値推測ゲームをモデル化しています。

> [!NOTE]
> チュートリアル入門の各トピックは、前のトピックに応じて異なります。 このトピックを完了するには、最初に次の手順を完了する必要があり [。アクティビティ](how-to-create-an-activity.md)を作成します。

> [!NOTE]
> チュートリアルの完成版をダウンロードするには、「 [Windows Workflow Foundation (WF45) - Getting Started Tutorial (Windows Workflow Foundation (WF45) - チュートリアル入門)](https://go.microsoft.com/fwlink/?LinkID=248976)」を参照してください。

## <a name="to-create-the-workflow"></a>ワークフローを作成するには

1. **ソリューションエクスプローラー**で **[NumberGuessWorkflowActivities]** を右クリックし、 **[追加]** 、 **[新しい項目]** の順に選択します。

2. **[インストール済み]** の **[共通項目]** ノードで、 **[ワークフロー]** を選択します。 **[ワークフロー]** の一覧から **[アクティビティ]** を選択します。

3. **[名前]** ボックスに「`SequentialNumberGuessWorkflow`」と入力し、 **[追加]** をクリックします。

4. **[ツールボックス]** の **[制御フロー]** セクションから**Sequence**アクティビティをドラッグし、ワークフローデザインサーフェイスの **[ここにアクティビティをドロップ]** ラベルの上にドロップします。

## <a name="to-create-the-workflow-variables-and-arguments"></a>ワークフロー変数および引数を作成するには

1. **ソリューションエクスプローラー**で**sequentialnumberguessworkflow.xaml**をダブルクリックして、ワークフローをデザイナーに表示します (まだ表示されていない場合)。

2. ワークフローデザイナーの左下にある **[引数]** をクリックして、 **[引数]** ペインを表示します。

3. **[引数の作成]** をクリックします。

4. **[名前]** ボックスに「`MaxNumber`」と入力し、 **[方向]** ドロップダウンリストから **[In]** を選択します。次に、 **[引数の型]** ドロップダウンリストから **[Int32]** を選択し、enter キーを押して引数を保存します。

5. **[引数の作成]** をクリックします。

6. 新しく追加した `MaxNumber` 引数の下にある **[名前]** ボックスに「`Turns`」と入力し、 **[方向]** ドロップダウンリストから **[Out]** を選択します。次に、 **[引数の型]** ドロップダウンリストから **[Int32]** を選択し、enter キーを押します。

7. アクティビティデザイナーの左下にある **[引数]** をクリックして、 **[引数]** ペインを閉じます。

8. ワークフローデザイナーの左下にある **[変数]** をクリックして、 **[変数]** ペインを表示します。

9. **[変数の作成]** をクリックします。

    > [!TIP]
    > **[変数の作成]** ボックスが表示されない場合は、ワークフローデザイナー画面で**Sequence**アクティビティをクリックして選択します。

10. **[名前]** ボックスに「`Guess`」と入力し、 **[変数の型]** ドロップダウンリストから **[Int32]** を選択します。次に、enter キーを押して変数を保存します。

11. **[変数の作成]** をクリックします。

12. **[名前]** ボックスに「`Target`」と入力し、 **[変数の型]** ドロップダウンリストから **[Int32]** を選択します。次に、enter キーを押して変数を保存します。

13. アクティビティデザイナーの左下にある **[変数]** をクリックして、 **[変数]** ペインを閉じます。

## <a name="to-add-the-workflow-activities"></a>ワークフロー アクティビティを追加するには

1. **ツールボックス**の **[プリミティブ]** セクションから**Assign**アクティビティをドラッグし、 **Sequence**アクティビティにドロップします。 **[To]** ボックスに「`Target`」と入力し、 **[式C#の入力]** または **[VB の式を入力]** してください ボックスに次の式を入力します。

    ```vb
    New System.Random().Next(1, MaxNumber + 1)
    ```

    ```csharp
    new System.Random().Next(1, MaxNumber + 1)
    ```

    > [!TIP]
    > **[ツールボックス]** ウィンドウが表示されていない場合は、 **[表示]** メニューの **[ツールボックス]** を選択します。

2. **ツールボックス**の **[制御フロー]** セクションから**dowhile**アクティビティをドラッグし、 **Assign**アクティビティの下になるようにワークフローにドロップします。

3. **Dowhile**アクティビティの **[Condition]** プロパティ値ボックスに次の式を入力します。

    ```vb
    Guess <> Target
    ```

    ```csharp
    Guess != Target
    ```

     <xref:System.Activities.Statements.DoWhile> アクティビティはその子アクティビティを実行し、その <xref:System.Activities.Statements.DoWhile.Condition%2A> を評価します。 <xref:System.Activities.Statements.DoWhile.Condition%2A> が `True` と評価される場合、<xref:System.Activities.Statements.DoWhile> 内のアクティビティが再度実行されます。 この例では、ユーザーの推定値が評価され、推定値が正しいと判断されるまで <xref:System.Activities.Statements.DoWhile> が続行されます。

4. **[ツールボックス]** の **[NumberGuessWorkflowActivities]** セクションから**Prompt**アクティビティをドラッグし、前の手順の**dowhile**アクティビティにドロップします。

5. [**プロパティ] ウィンドウ**で、 **Prompt**アクティビティの **[BookmarkName]** プロパティ値ボックスに引用符を含む `"EnterGuess"` を入力します。 **[Result]** プロパティ値ボックスに「`Guess`」と入力し、 **[Text]** プロパティボックスに次の式を入力します。

    ```vb
    "Please enter a number between 1 and " & MaxNumber
    ```

    ```csharp
    "Please enter a number between 1 and " + MaxNumber
    ```

    > [!TIP]
    > [**プロパティ] ウィンドウ**が表示されていない場合は、 **[表示]** メニューの **[プロパティウィンドウ]** をクリックします。

6. **ツールボックス**の **[プリミティブ]** セクションから**Assign**アクティビティをドラッグして、 **Prompt**アクティビティに続くように**dowhile**アクティビティにドロップします。

    > [!NOTE]
    > **Assign**アクティビティを削除すると、ワークフローデザイナーが、 **Prompt**アクティビティと新しく追加された**Assign**アクティビティの両方を含む**Sequence**アクティビティを自動的に追加することに注意してください。

7. **[To]** ボックスに「`Turns`」と入力し、 **[式のC#入力]** ボックスまたは **[VB の式を入力]** してください ボックスに `Turns + 1` します。

8. **[ツールボックス]** の **[制御フロー]** セクションから**If**アクティビティをドラッグし、新しく追加した**Assign**アクティビティの後になるように**Sequence**アクティビティにドロップします。

9. **If**アクティビティの **[Condition]** プロパティ値ボックスに次の式を入力します。

    ```vb
    Guess <> Target
    ```

    ```csharp
    Guess != Target
    ```

10. **ツールボックス**の **[制御フロー]** セクションから別の**if**アクティビティをドラッグし、最初の**if**アクティビティの**Then**セクションにドロップします。

11. 新しく追加された**If**アクティビティの **[Condition]** プロパティ値ボックスに次の式を入力します。

    ```text
    Guess < Target
    ```

12. **ツールボックス**の **[プリミティブ]** セクションから2つの**WriteLine**アクティビティをドラッグしてドロップします。1つは、新しく追加した**If**アクティビティの**Then**セクションにあり、もう1つは**Else**セクションにあります。

13. **[Then** ] セクションで **[WriteLine]** アクティビティをクリックして選択し、 **[Text]** プロパティ値ボックスに次の式を入力します。

    ```text
    "Your guess is too low."
    ```

14. **[Else]** セクションの**WriteLine**アクティビティをクリックして選択し、 **[Text]** プロパティ値ボックスに次の式を入力します。

    ```text
    "Your guess is too high."
    ```

     完成したワークフローの例を次に示します。

     ![完成したシーケンシャルワークフローを示すスクリーンショット。](./media/how-to-create-a-sequential-workflow/complete-sequential-workflow.jpg)

## <a name="to-build-the-workflow"></a>ワークフローをビルドするには

1. Ctrl キーと Shift キーを押しながら B キーを押して、ソリューションをビルドします。

     ワークフローを実行する方法については、次のトピック「[方法」を参照してください。ワークフロー](how-to-run-a-workflow.md)を実行します。 [を既に完了している場合は、次の方法を実行します。異なるスタイルのワークフローを使用してワークフロー](how-to-run-a-workflow.md) ステップを実行し、この手順のシーケンシャルワークフローを使用して実行するには、の「[アプリケーションをビルドして実行するに](how-to-run-a-workflow.md#BKMK_ToRunTheApplication)[は」に進んでください: ワークフロー を実行します](how-to-run-a-workflow.md)。

## <a name="see-also"></a>関連項目

- <xref:System.Activities.Statements.Flowchart>
- <xref:System.Activities.Statements.FlowDecision>
- [Windows Workflow Foundation プログラミング](programming.md)
- [ワークフローの設計](designing-workflows.md)
- [チュートリアル入門](getting-started-tutorial.md)
- [方法: アクティビティ を作成する](how-to-create-an-activity.md)
- [方法: ワークフロー を実行する](how-to-run-a-workflow.md)
