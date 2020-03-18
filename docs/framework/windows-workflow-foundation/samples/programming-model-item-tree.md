---
title: モデル アイテム ツリーのプログラミング
ms.date: 03/30/2017
ms.assetid: 0229efde-19ac-4bdc-a187-c6227a7bd1a5
ms.openlocfilehash: efda69ac568b0ad9c5fdcf4d42722c5b7dadd3f3
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715667"
---
# <a name="programming-model-item-tree"></a>モデル アイテム ツリーのプログラミング
このサンプルでは、Windows Presentation Foundation (WPF) ツリービューの宣言型データバインディングを使用して <xref:System.Activities.Presentation.Model.ModelItem> ツリー内を移動する方法を示します。

## <a name="sample-details"></a>サンプルの詳細
 <xref:System.Activities.Presentation.Model.ModelItem> ツリーは、編集されている基になるインスタンスに関するデータを公開するために Windows ワークフローデザイナーインフラストラクチャによって使用される抽象化です。 次の図は、ワークフローデザイナー内のインフラストラクチャのさまざまな層を示しています。

 ![ワークフローデザイナーアーキテクチャを示す図。](./media/programming-model-item-tree/workflow-designer-architecture.jpg)

 <xref:System.Activities.Presentation.Model.ModelItem> は、基になる値へのポインターと、<xref:System.Activities.Presentation.Model.ModelProperty> オブジェクトのコレクションで構成されています。 <xref:System.Activities.Presentation.Model.ModelProperty> オブジェクトはさらに、プロパティの名前や型などのデータと、また別の <xref:System.Activities.Presentation.Model.ModelItem> である値へのポインターで構成されています。 <xref:System.Activities.Presentation.Model.ModelItem> から返される一部の <xref:System.Activities.Presentation.Model.ModelProperty> に対しては、ツリー ビューに正しく表示されるように操作するために値コンバーターが使用されます。 このサンプルでは、次の例のような命令構文を使用して <xref:System.Activities.Presentation.Model.ModelItem> ツリーを命令型プログラミングで操作する方法を示します。

```csharp
ModelItem mi = wd.Context.Services.GetService<ModelService>().Root;
ModelProperty mp = mi.Properties["Activities"];
mp.Collection.Add(new Persist());
ModelItem justAdded = mp.Collection.Last();
justAdded.Properties["DisplayName"].SetValue("new name");
```

#### <a name="to-use-this-sample"></a>このサンプルを使用するには

1. Visual Studio 2010 で ProgrammingModelItemTree ソリューションを開きます。

2. **[ビルド]** メニューの **[ソリューションのビルド]** を選択して、ソリューションをビルドします。

3. F5 キーを押してアプリケーションを実行します。 WPF フォームが表示されます。

4. **[WF の読み込み]** ボタンをクリックして <xref:System.Activities.Presentation.Model.ModelItem> を読み込み、ツリービューにバインドします。

5. **[モデル項目ツリーの変更]** ボタンをクリックすると、前のコードが実行され、ツリーに項目が追加され、プロパティが設定されます。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Designer\ProgrammingModelItemTree`  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Data.IValueConverter>
