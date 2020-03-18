---
title: デザイナーのホスト変更
ms.date: 03/30/2017
ms.assetid: b676ad31-5f64-4d84-9a36-b4d7113a2f4d
ms.openlocfilehash: f98b1823c74471c96f6d4b67ec47637bb0785d8f
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715233"
---
# <a name="designer-rehosting"></a>デザイナーのホスト変更
デザイナーのホスト変更は、カスタム アプリケーション内でワークフロー デザイン キャンバスをホストすることを示す一般的なシナリオです。 多くのユーザーにとって、使い慣れたホスト アプリケーションは Visual Studio ですが、アプリケーションでワークフロー デザイナーを表示すると役立つ場合があるシナリオも数多くあります。  
  
- 監視アプリケーション (エンド ユーザーがプロセス、およびプロセスに関するランタイム データ (現在アクティブな状態、実行時間の集計データ、ワークフローのインスタンスに関するその他の情報など) を表示できるようにします)。  
  
- ユーザーが限られたアクティビティ セットを使用してプロセスをカスタマイズできるようにするアプリケーション。  
  
 このような種類のアプリケーションをサポートするために、ワークフロー デザイナーが .NET Framework に付属しており、WPF アプリケーション内でホストするか、適切な WPF ホスト コードを使用して WinForms アプリケーション内でホストすることができます。 このサンプルでは、次の方法を示します。  
  
- WF デザイナーのホスト変更方法。  
  
- 再ホストされたツールボックスおよびプロパティ グリッドの使用方法。  
  
## <a name="rehosting-the-designer"></a>デザイナーのホスト変更  
 このサンプルでは、次のグリッド レイアウトのような、デザイナーを含む WPF レイアウトを作成する方法を示します (スペースを考慮してツールボックス コードは省略しています)。 デザイナーおよびプロパティ グリッドを含む罫線の名前に注意してください。  
  
```xaml  
<Grid>  
    <Grid.ColumnDefinitions>  
        <ColumnDefinition Width="2*"/>  
        <ColumnDefinition Width="7*"/>  
        <ColumnDefinition Width="3*"/>  
    </Grid.ColumnDefinitions>  
    <Border Grid.Column="0">  
        <sapt:ToolboxControl>...</sapt:ToolboxControl>  
    </Border>  
    <Border Grid.Column="1" Name="DesignerBorder"/>  
    <Border Grid.Column="2" Name="PropertyBorder"/>  
</Grid>  
```  
  
 次に、このサンプルではデザイナーを作成し、そのプライマリ <xref:System.Activities.Presentation.WorkflowDesigner.View%2A> および <xref:System.Activities.Presentation.WorkflowDesigner.PropertyInspectorView%2A> をユーザー インターフェイスの適切なコンテナーに関連付けます。 次の例に示すいくつかのコード行について説明します。 .NET Framework に出荷されたアクティビティの既定のアクティビティデザイナーを関連付けるには、<xref:System.Activities.Core.Presentation.DesignerMetadata.Register%2A> 呼び出しが必要です。 <xref:System.Activities.Presentation.WorkflowDesigner.Load%2A> は、編集する WF 項目を渡すために呼び出されます。 最後に、<xref:System.Activities.Presentation.WorkflowDesigner.View%2A> (プライマリ キャンバス) および <xref:System.Activities.Presentation.WorkflowDesigner.PropertyInspectorView%2A> (プロパティ グリッド) がユーザー インターフェイス画面に配置されます。  
  
```csharp  
protected override void OnInitialized(EventArgs e)  
{  
   base.OnInitialized(e);  
   // register metadata  
   (new DesignerMetadata()).Register();  
  
   // create the workflow designer  
   WorkflowDesigner wd = new WorkflowDesigner();  
   wd.Load(new Sequence());  
   DesignerBorder.Child = wd.View;  
   PropertyBorder.Child = wd.PropertyInspectorView;  
}  
```  
  
## <a name="using-the-rehosted-toolbox"></a>再ホストされたツールボックスの使用  
 このサンプルでは、XAML で宣言によって再ホストされたツールボックス コントロールを使用します。 コードで型を <xref:System.Activities.Presentation.Toolbox.ToolboxItemWrapper> コンストラクターに渡すことができることに注意してください。  
  
```xaml  
<!-- Copyright (c) Microsoft Corporation. All rights reserved-->  
<Window x:Class="Microsoft.Samples.DesignerRehosting.RehostingWfDesigner"  
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"  
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  
        xmlns:sapt="clr-namespace:System.Activities.Presentation.Toolbox;assembly=System.Activities.Presentation"  
        xmlns:sys="clr-namespace:System;assembly=mscorlib"  
        Title="Window1" Height="600" Width="900">  
    <Window.Resources>  
        <sys:String x:Key="AssemblyName">System.Activities, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35</sys:String>  
    </Window.Resources>  
    <Grid>  
        <Grid.ColumnDefinitions>  
            <ColumnDefinition Width="2*"/>  
            <ColumnDefinition Width="7*"/>  
            <ColumnDefinition Width="3*"/>  
        </Grid.ColumnDefinitions>  
        <Border Grid.Column="0">  
            <sapt:ToolboxControl>  
                <sapt:ToolboxCategory CategoryName="Basic">  
                    <sapt:ToolboxItemWrapper AssemblyName="{StaticResource AssemblyName}" >  
                        <sapt:ToolboxItemWrapper.ToolName>  
                            System.Activities.Statements.Sequence  
                        </sapt:ToolboxItemWrapper.ToolName>  
                       </sapt:ToolboxItemWrapper>  
                    <sapt:ToolboxItemWrapper  AssemblyName="{StaticResource AssemblyName}">  
                        <sapt:ToolboxItemWrapper.ToolName>  
                            System.Activities.Statements.WriteLine  
                        </sapt:ToolboxItemWrapper.ToolName>  
  
                    </sapt:ToolboxItemWrapper>  
                    <sapt:ToolboxItemWrapper  AssemblyName="{StaticResource AssemblyName}">  
                        <sapt:ToolboxItemWrapper.ToolName>  
                            System.Activities.Statements.If  
                        </sapt:ToolboxItemWrapper.ToolName>  
  
                    </sapt:ToolboxItemWrapper>  
                    <sapt:ToolboxItemWrapper  AssemblyName="{StaticResource AssemblyName}">  
                        <sapt:ToolboxItemWrapper.ToolName>  
                            System.Activities.Statements.While  
                        </sapt:ToolboxItemWrapper.ToolName>  
  
                    </sapt:ToolboxItemWrapper>  
                </sapt:ToolboxCategory>  
            </sapt:ToolboxControl>  
        </Border>  
        <Border Grid.Column="1" Name="DesignerBorder"/>  
        <Border Grid.Column="2" Name="PropertyBorder"/>  
    </Grid>  
</Window>  
```  
  
#### <a name="using-the-sample"></a>サンプルの使用  
  
1. Visual Studio 2010 で、デザイナーの再ホスト .sln ソリューションを開きます。  
  
2. F5 キーを押してアプリケーションをコンパイルし、実行します。  
  
3. WPF アプリケーションが再ホストされたデザイナーと共に起動します。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\DesignerRehosting\Basic`
