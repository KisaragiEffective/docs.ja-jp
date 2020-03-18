---
title: カスタム アクティビティ デザイナーでの ExpressionTextBox の使用
ms.date: 03/30/2017
ms.assetid: f82e73e7-a256-4a4d-82b7-c0d62f4ab5e7
ms.openlocfilehash: 6b581b42c882c12425a17b9a518f8957ca10898a
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715535"
---
# <a name="using-the-expressiontextbox-in-a-custom-activity-designer"></a>カスタム アクティビティ デザイナーでの ExpressionTextBox の使用
このサンプルでは、カスタム アクティビティ デザイナーで <xref:System.Activities.Presentation.View.ExpressionTextBox> を使用する方法を示します。 カスタム アクティビティ `MultiAssign` は、2 つの文字列値を 2 つの文字列変数に割り当てます。 <xref:System.Activities.Presentation.View.ExpressionTextBox> コントロールには、<xref:System.Activities.InArgument> にバインドされるものと <xref:System.Activities.OutArgument> にバインドされるものがあります。

## <a name="sample-details"></a>サンプルの詳細
 `ArgumentToExpressionConverter` は、式を引数にバインドするときに使用される型コンバーターです。 `ConverterParameter` は、必要に応じて、`In` または `Out` に設定する必要があります。 `InOut` がサポートされていません。

 `OutArgument`s で `UseLocationExpression` 属性を使用して、式が左辺値 ("左辺値" または "location value") 式であることを指定します。 ほとんど場合、L 値式は、返される `OutArgument` が変数または引数の名前であることを示すために使用される有効な Visual Basic 識別子です。

 この例では、`MaxLines` 属性は 1 に設定され、`MinLines` は設定されていません。 つまり、ユーザーによって入力されるテキストの量に関係なく、<xref:System.Activities.Presentation.View.ExpressionTextBox> のサイズが 1 行に固定されることを示しています。 <xref:System.Activities.Presentation.View.ExpressionTextBox> がユーザーの入力に合わせて拡大されるようにするには、`MaxLines` に `MinLines` より大きい値を設定します。

 ExpressionTextBox は、引数にのみバインドできます。CLR プロパティにはバインドできません。

#### <a name="to-use-this-sample"></a>このサンプルを使用するには

1. Visual Studio 2010 を使用して、ExpressionTextBoxSample ファイルを開きます。

2. ソリューションをビルドするには、Ctrl キーと Shift キーを押しながら B キーを押します。

#### <a name="to-run-this-sample"></a>このサンプルを実行するには

1. 新しいワークフロー コンソール アプリケーションをソリューションに追加します。

2. 新しいワークフローコンソールアプリケーションプロジェクトから、 **ExpressionTextBoxSample**プロジェクトへの参照を追加します。

3. ソリューションをビルドします。

4. ツールボックス から  **Multiassign** アクティビティをドラッグし、ワークフローにドロップします。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\CustomActivities\CustomActivityDesigners\ExpressionTextBox`  
  
## <a name="see-also"></a>参照

- <xref:System.Activities.Presentation.View.ExpressionTextBox>
- [ワークフロー デザイナーを使用したアプリケーションの開発](/visualstudio/workflow-designer/developing-applications-with-the-workflow-designer)
