---
title: 'タスク 3: ツールボックス ペインと PropertyGrid ペインの作成'
ms.date: 03/30/2017
ms.assetid: 72c1546a-eed5-4f0f-a616-719a163414f4
ms.openlocfilehash: 29e50b24135cd3d6a02052d846e1781b0d9fa325
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716230"
---
# <a name="task-3-create-the-toolbox-and-propertygrid-panes"></a>タスク 3: ツールボックス ペインと PropertyGrid ペインの作成

このタスクでは、**ツールボックス**ペインと**PropertyGrid**ペインを作成し、再ホストされた Windows ワークフローデザイナーに追加します。

参考として、このトピックの最後に、ワークフローデザイナーシリーズのトピックの再[ホスト](rehosting-the-workflow-designer.md)に関する3つのタスクを完了した後に MainWindow.xaml.cs ファイルに含まれている必要があるコードについて説明します。

## <a name="to-create-the-toolbox-and-add-it-to-the-grid"></a>ツールボックスを作成し、グリッドに追加するには

1. 「[タスク 2: ワークフローデザイナーをホストする](task-2-host-the-workflow-designer.md)」で説明されている手順に従って、取得した HostingApplication プロジェクトを開きます。

2. **ソリューションエクスプローラー**ペインで、 *mainwindow.xaml*ファイルを右クリックし、 **[コードの表示]** を選択します。

3. <xref:System.Activities.Presentation.Toolbox.ToolboxControl>を作成する `MainWindow` クラスに `GetToolboxControl` メソッドを追加し **、ツールボックス**に新しい**ツールボックス**のカテゴリを追加して、<xref:System.Activities.Statements.Assign> および <xref:System.Activities.Statements.Sequence> のアクティビティの種類をそのカテゴリに割り当てます。

    ```csharp
    private ToolboxControl GetToolboxControl()
    {
        // Create the ToolBoxControl.
        var ctrl = new ToolboxControl();

        // Create a category.
        var category = new ToolboxCategory("category1");

        // Create Toolbox items.
        var tool1 =
            new ToolboxItemWrapper("System.Activities.Statements.Assign",
            typeof(Assign).Assembly.FullName, null, "Assign");

        var tool2 = new ToolboxItemWrapper("System.Activities.Statements.Sequence",
            typeof(Sequence).Assembly.FullName, null, "Sequence");

        // Add the Toolbox items to the category.
        category.Add(tool1);
        category.Add(tool2);

        // Add the category to the ToolBox control.
        ctrl.Categories.Add(category);
        return ctrl;
    }
    ```

4. **ツールボックス**をグリッドの左側の列に配置する、`MainWindow` クラスにプライベート `AddToolbox` メソッドを追加します。

    ```csharp
    private void AddToolBox()
    {
        ToolboxControl tc = GetToolboxControl();
        Grid.SetColumn(tc, 0);
        grid1.Children.Add(tc);
    }
    ```

5. 次のコードに示すように、`MainWindow()` クラスコンストラクターに `AddToolBox` メソッドの呼び出しを追加します。

    ```csharp
    public MainWindow()
    {
        InitializeComponent();
        this.RegisterMetadata();
        this.AddDesigner();

        this.AddToolBox();
    }
    ```

6. <kbd>F5</kbd>キーを押して、ソリューションをビルドして実行します。 <xref:System.Activities.Statements.Assign> アクティビティと <xref:System.Activities.Statements.Sequence> アクティビティを含む**ツールボックス**が表示されます。

## <a name="to-create-the-propertygrid"></a>PropertyGrid を作成するには

1. **ソリューションエクスプローラー**ペインで、 *mainwindow.xaml*ファイルを右クリックし、 **[コードの表示]** を選択します。

2. `AddPropertyInspector` メソッドを `MainWindow` クラスに追加して、グリッドの右端の列に **[PropertyGrid]** ペインを配置します。

    ```csharp
    private void AddPropertyInspector()
    {
        Grid.SetColumn(wd.PropertyInspectorView, 2);
        grid1.Children.Add(wd.PropertyInspectorView);
    }
    ```

3. 次のコードに示すように、`MainWindow()` クラスコンストラクターに `AddPropertyInspector` メソッドの呼び出しを追加します。

    ```csharp
    public MainWindow()
    {
        InitializeComponent();
        this.RegisterMetadata();
        this.AddDesigner();
        this.AddToolBox();

        this.AddPropertyInspector();
    }
    ```

4. <kbd>F5</kbd>キーを押して、ソリューションをビルドして実行します。 **[ツールボックス]** 、ワークフローデザインキャンバス、および  **[PropertyGrid]** の各ウィンドウがすべて表示されます。また、デザインキャンバスに <xref:System.Activities.Statements.Assign> アクティビティまたは <xref:System.Activities.Statements.Sequence> アクティビティをドラッグすると、強調表示されているアクティビティに応じてプロパティグリッドが更新されます。

## <a name="example"></a>使用例

*MainWindow.xaml.cs*ファイルには、次のコードが含まれています。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;
// dlls added.
using System.Activities;
using System.Activities.Core.Presentation;
using System.Activities.Presentation;
using System.Activities.Presentation.Metadata;
using System.Activities.Presentation.Toolbox;
using System.Activities.Statements;
using System.ComponentModel;

namespace HostingApplication
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        private WorkflowDesigner wd;

        public MainWindow()
        {
            InitializeComponent();
            RegisterMetadata();
            AddDesigner();
            this.AddToolBox();
            this.AddPropertyInspector();
        }

        private void AddDesigner()
        {
            // Create an instance of WorkflowDesigner class.
            this.wd = new WorkflowDesigner();

            // Place the designer canvas in the middle column of the grid.
            Grid.SetColumn(this.wd.View, 1);

            // Load a new Sequence as default.
            this.wd.Load(new Sequence());

            // Add the designer canvas to the grid.
            grid1.Children.Add(this.wd.View);
        }

        private void RegisterMetadata()
        {
            var dm = new DesignerMetadata();
            dm.Register();
        }

        private ToolboxControl GetToolboxControl()
        {
            // Create the ToolBoxControl.
            var ctrl = new ToolboxControl();

            // Create a category.
            var category = new ToolboxCategory("category1");

            // Create Toolbox items.
            var tool1 =
                new ToolboxItemWrapper("System.Activities.Statements.Assign",
                typeof(Assign).Assembly.FullName, null, "Assign");

            var tool2 = new ToolboxItemWrapper("System.Activities.Statements.Sequence",
                typeof(Sequence).Assembly.FullName, null, "Sequence");

            // Add the Toolbox items to the category.
            category.Add(tool1);
            category.Add(tool2);

            // Add the category to the ToolBox control.
            ctrl.Categories.Add(category);
            return ctrl;
        }

        private void AddToolBox()
        {
            ToolboxControl tc = GetToolboxControl();
            Grid.SetColumn(tc, 0);
            grid1.Children.Add(tc);
        }

        private void AddPropertyInspector()
        {
            Grid.SetColumn(wd.PropertyInspectorView, 2);
            grid1.Children.Add(wd.PropertyInspectorView);
        }

    }
}
```

## <a name="see-also"></a>参照

- [ワークフロー デザイナーのホスト変更](rehosting-the-workflow-designer.md)
- [タスク 1: 新しい Windows Presentation Foundation アプリケーションの作成](task-1-create-a-new-wpf-app.md)
- [タスク 2: ワークフロー デザイナーのホスティング](task-2-host-the-workflow-designer.md)
