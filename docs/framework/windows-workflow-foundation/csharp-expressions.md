---
title: C# の式
ms.date: 03/30/2017
ms.assetid: 29110be7-f4e3-407e-8dbe-78102eb21115
ms.openlocfilehash: d1728758a4f1af76c2d08695a83c0f9acc3dde3e
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74140098"
---
# <a name="c-expressions"></a>C# の式
.NET Framework 4.5 以降ではC# WINDOWS WORKFLOW FOUNDATION (WF) で式がサポートされています。 .NET Framework C# 4.5 C#を対象とする Visual Studio 2012 で作成された新しいワークフロープロジェクトと、Visual Basic ワークフロープロジェクトは Visual Basic 式を使用します。 Visual Basic 式を使用する既存の .NET Framework 4 ワークフロープロジェクトは、プロジェクトの言語に関係なく [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] に移行でき、サポートされています。 ここでは、[!INCLUDE[wf1](../../../includes/wf1-md.md)] での C# 式の概要について説明します。

## <a name="using-c-expressions-in-workflows"></a>ワークフローでの C# 式の使用

- [ワークフローデザイナー C#での式の使用](csharp-expressions.md#WFDesigner)

  - [旧バージョンとの互換性](csharp-expressions.md#BackwardCompat)

- [コードC#ワークフローでの式の使用](csharp-expressions.md#CodeWorkflows)

- [XAML C#ワークフローでの式の使用](csharp-expressions.md#XamlWorkflows)

  - [コンパイルされた Xaml](csharp-expressions.md#CompiledXaml)

  - [ルース Xaml](csharp-expressions.md#LooseXaml)

- [.XAMLX C# workflow services での式の使用](csharp-expressions.md#WFServices)

### <a name="WFDesigner"></a>ワークフローデザイナー C#での式の使用

.NET Framework 4.5 以降ではC# WINDOWS WORKFLOW FOUNDATION (WF) で式がサポートされています。 C#.NET Framework 4.5 を対象とする Visual Studio 2012 で作成さC#れたワークフロープロジェクトは式を使用しますが、Visual Basic ワークフロープロジェクトは Visual Basic 式を使用します。 目的C#の式を指定するには、[  **C#式を入力**してください] というラベルの付いたボックスに入力します。 このラベルは、プロパティ ウィンドウ (デザイナーでアクティビティを選択した場合) またはワークフロー デザイナーのアクティビティに表示されます。 次の例では、2 つの `WriteLine` アクティビティが `Sequence` の中で `NoPersistScope` 内に含まれています。

![自動的に作成された sequence アクティビティを示すスクリーンショット。](./media/csharp-expressions/auto-surround-sequence-activity.png)

> [!NOTE]
> C#式は Visual Studio でのみサポートされており、再ホストされたワークフローデザイナーではサポートされていません。 再ホストされたデザイナーでサポートされている新しい WF45 機能の詳細については、「 [rehosted ワークフローデザイナーでの新しい Workflow Foundation 4.5 機能のサポート](wf-features-in-the-rehosted-workflow-designer.md)」を参照してください。

#### <a name="BackwardCompat"></a>旧バージョンとの互換性

[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] に移行されたC#既存の .NET Framework 4 つのワークフロープロジェクトの Visual Basic 式がサポートされています。 Visual Basic 式がワークフローデザイナーで表示されると、Visual Basic 式が有効なC#構文でない限り、既存の Visual Basic 式のテキストが**XAML で設定された値**に置き換えられます。 Visual Basic 式が有効な C# 構文である場合は、式が表示されます。 Visual Basic 式を C# に更新するには、ワークフロー デザイナーでその式を編集して、対応する C# 式を指定します。 Visual Basic 式を C# に更新する必要はありませんが、ワークフロー デザイナーで式を更新すると、式は C# に変換され、Visual Basic に戻すことができなくなる場合があります。

### <a name="CodeWorkflows"></a>コードC#ワークフローでの式の使用

C# 式は、[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] コードベースのワークフローでサポートされていますが、ワークフローを呼び出す前に、<xref:System.Activities.XamlIntegration.TextExpressionCompiler.Compile%2A?displayProperty=nameWithType> を使用して C# 式をコンパイルする必要があります。 ワークフローの作成者は、`CSharpValue` を使用して式の右辺値を表し、`CSharpReference` を使用して式の左辺値を表すことができます。 次の例では、`Assign` アクティビティに含まれる `WriteLine` アクティビティと `Sequence` アクティビティを使用してワークフローを作成します。 `CSharpReference` は `Assign` の `To` 引数として指定され、式の左辺値を表します。 `CSharpValue` は `Assign` の `Value` 引数、および `WriteLine` の `Text` 引数として指定され、2 つの式の右辺値を表します。

```csharp
Variable<int> n = new Variable<int>
{
    Name = "n"
};

Activity wf = new Sequence
{
    Variables = { n },
    Activities =
    {
        new Assign<int>
        {
            To = new CSharpReference<int>("n"),
            Value = new CSharpValue<int>("new Random().Next(1, 101)")
        },
        new WriteLine
        {
            Text = new CSharpValue<string>("\"The number is \" + n")
        }
    }
};

CompileExpressions(wf);

WorkflowInvoker.Invoke(wf);
```

ワークフローが作成されると、`CompileExpressions` ヘルパー メソッドを呼び出して C# 式がコンパイルされ、ワークフローが呼び出されます。 次の例は、`CompileExpressions` メソッドです。

```csharp
static void CompileExpressions(Activity activity)
{
    // activityName is the Namespace.Type of the activity that contains the
    // C# expressions.
    string activityName = activity.GetType().ToString();

    // Split activityName into Namespace and Type.Append _CompiledExpressionRoot to the type name
    // to represent the new type that represents the compiled expressions.
    // Take everything after the last . for the type name.
    string activityType = activityName.Split('.').Last() + "_CompiledExpressionRoot";
    // Take everything before the last . for the namespace.
    string activityNamespace = string.Join(".", activityName.Split('.').Reverse().Skip(1).Reverse());

    // Create a TextExpressionCompilerSettings.
    TextExpressionCompilerSettings settings = new TextExpressionCompilerSettings
    {
        Activity = activity,
        Language = "C#",
        ActivityName = activityType,
        ActivityNamespace = activityNamespace,
        RootNamespace = null,
        GenerateAsPartialClass = false,
        AlwaysGenerateSource = true,
        ForImplementation = false
    };

    // Compile the C# expression.
    TextExpressionCompilerResults results =
        new TextExpressionCompiler(settings).Compile();

    // Any compilation errors are contained in the CompilerMessages.
    if (results.HasErrors)
    {
        throw new Exception("Compilation failed.");
    }

    // Create an instance of the new compiled expression type.
    ICompiledExpressionRoot compiledExpressionRoot =
        Activator.CreateInstance(results.ResultType,
            new object[] { activity }) as ICompiledExpressionRoot;

    // Attach it to the activity.
    CompiledExpressionInvoker.SetCompiledExpressionRoot(
        activity, compiledExpressionRoot);
}
```

> [!NOTE]
> C#式がコンパイルされていない場合は、次のようなメッセージを使用してワークフローが呼び出されると、<xref:System.NotSupportedException> がスローされます。 `Expression Activity type 'CSharpValue`1 ' を実行するには、コンパイルが必要です。  ワークフローがコンパイルされていることを確認してください。 '

カスタムのコードベースのワークフローで `DynamicActivity` を使用する場合は、次のコード例に示すように、`CompileExpressions` メソッドに変更を加える必要があります。

```csharp
static void CompileExpressions(DynamicActivity dynamicActivity)
{
    // activityName is the Namespace.Type of the activity that contains the
    // C# expressions. For Dynamic Activities this can be retrieved using the
    // name property , which must be in the form Namespace.Type.
    string activityName = dynamicActivity.Name;

    // Split activityName into Namespace and Type.Append _CompiledExpressionRoot to the type name
    // to represent the new type that represents the compiled expressions.
    // Take everything after the last . for the type name.
    string activityType = activityName.Split('.').Last() + "_CompiledExpressionRoot";
    // Take everything before the last . for the namespace.
    string activityNamespace = string.Join(".", activityName.Split('.').Reverse().Skip(1).Reverse());

    // Create a TextExpressionCompilerSettings.
    TextExpressionCompilerSettings settings = new TextExpressionCompilerSettings
    {
        Activity = dynamicActivity,
        Language = "C#",
        ActivityName = activityType,
        ActivityNamespace = activityNamespace,
        RootNamespace = null,
        GenerateAsPartialClass = false,
        AlwaysGenerateSource = true,
        ForImplementation = true
    };

    // Compile the C# expression.
    TextExpressionCompilerResults results =
        new TextExpressionCompiler(settings).Compile();

    // Any compilation errors are contained in the CompilerMessages.
    if (results.HasErrors)
    {
        throw new Exception("Compilation failed.");
    }

    // Create an instance of the new compiled expression type.
    ICompiledExpressionRoot compiledExpressionRoot =
        Activator.CreateInstance(results.ResultType,
            new object[] { dynamicActivity }) as ICompiledExpressionRoot;

    // Attach it to the activity.
    CompiledExpressionInvoker.SetCompiledExpressionRootForImplementation(
        dynamicActivity, compiledExpressionRoot);
}
```

動的アクティビティで C# 式をコンパイルする `CompileExpressions` オーバーロードには、いくつかの相違点があります。

- `CompileExpressions` のパラメーターが `DynamicActivity` です。

- 型名および名前空間の取得に `DynamicActivity.Name` プロパティが使用されます。

- `TextExpressionCompilerSettings.ForImplementation` が `true` に設定されます。

- `CompiledExpressionInvoker.SetCompiledExpressionRootForImplementation` の代わりに `CompiledExpressionInvoker.SetCompiledExpressionRoot` が呼び出されます。

コードで式を使用する方法の詳細については、「[命令型コードを使用したワークフロー、アクティビティ、および式の作成](authoring-workflows-activities-and-expressions-using-imperative-code.md)」を参照してください。

### <a name="XamlWorkflows"></a>XAML C#ワークフローでの式の使用

C# 式は XAML ワークフローでサポートされています。 コンパイルされた XAML ワークフローは型にコンパイルされ、Loose XAML ワークフローはランタイムによって読み込まれ、ワークフローの実行時にアクティビティ ツリーにコンパイルされます。

- [コンパイルされた Xaml](csharp-expressions.md#CompiledXaml)

- [ルース Xaml](csharp-expressions.md#LooseXaml)

#### <a name="CompiledXaml"></a>コンパイルされた Xaml

C# 式は、コンパイルされた XAML ワークフローでサポートされており、[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] を対象とする C# ワークフロー プロジェクトの一部として型にコンパイルされます。 コンパイルされた XAML は、Visual Studio でのワークフロー作成のC#既定の種類であり、visual studio で作成C#され、[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] を対象とするワークフロープロジェクトは、式を使用します。

#### <a name="LooseXaml"></a>ルース Xaml

C# 式は Loose XAML ワークフローでがサポートされています。 Loose XAML ワークフローを読み込んで呼び出すワークフロー ホスト プログラムは、[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] を対象としている必要があります。また、<xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> は `true` (既定値は `false`) に設定する必要がります。 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> を `true` に設定するには、<xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings> プロパティが <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> に設定されている `true` インスタンスを作成し、そのインスタンスを <xref:System.Activities.XamlIntegration.ActivityXamlServices.Load%2A?displayProperty=nameWithType> へパラメーターとして渡します。 `CompileExpressions` が `true`に設定されていない場合は、<xref:System.NotSupportedException> がスローされ、次のようなメッセージが表示されます。 `Expression Activity type 'CSharpValue`1 ' を実行するには、コンパイルが必要です。  ワークフローがコンパイルされていることを確認してください。 '

```csharp
ActivityXamlServicesSettings settings = new ActivityXamlServicesSettings
{
    CompileExpressions = true
};

DynamicActivity<int> wf = ActivityXamlServices.Load(new StringReader(serializedAB), settings) as DynamicActivity<int>;
```

XAML ワークフローの操作の詳細については、「 [xaml との間でのワークフローとアクティビティのシリアル](serializing-workflows-and-activities-to-and-from-xaml.md)化」を参照してください。

### <a name="WFServices"></a>.XAMLX C# workflow services での式の使用

C# 式は XAMLX ワークフロー サービスでがサポートされています。 ワークフロー サービスが IIS または WAS でホストされている場合、追加の手順は必要ありません。ただし、XAML ワークフロー サービスが自己ホスト型サービスの場合は、C# 式をコンパイルする必要があります。 自己ホスト型C#の .xamlx workflow サービスで式をコンパイルするには、最初に .xamlx ファイルを `WorkflowService`に読み込んでから、`WorkflowService` の `Body` を、前の「[コードワークフローでの式C#の使用](csharp-expressions.md#CodeWorkflows)」セクションで説明した `CompileExpressions` メソッドに渡します。 次の例では、XAMLX ワークフロー サービスが読み込まれ、C# 式がコンパイルされた後、ワークフロー サービスが開かれて要求を待機します。

```csharp
// Load the XAMLX workflow service.
WorkflowService workflow1 =
    (WorkflowService)XamlServices.Load(xamlxPath);

// Compile the C# expressions in the workflow by passing the Body to CompileExpressions.
CompileExpressions(workflow1.Body);

// Initialize the WorkflowServiceHost.
var host = new WorkflowServiceHost(workflow1, new Uri("http://localhost:8293/Service1.xamlx"));

// Enable Metadata publishing/
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true;
smb.MetadataExporter.PolicyVersion = PolicyVersion.Policy15;
host.Description.Behaviors.Add(smb);

// Open the WorkflowServiceHost and wait for requests.
host.Open();
Console.WriteLine("Press enter to quit");
Console.ReadLine();
```

C# 式がコンパイルされないと、`Open` の処理は成功しますが、ワークフローは呼び出し時に失敗します。 次の `CompileExpressions` メソッドは、前の「[コードワークフローで式をC#使用する](csharp-expressions.md#CodeWorkflows)」セクションのメソッドと同じです。

```csharp
static void CompileExpressions(Activity activity)
{
    // activityName is the Namespace.Type of the activity that contains the
    // C# expressions.
    string activityName = activity.GetType().ToString();

    // Split activityName into Namespace and Type.Append _CompiledExpressionRoot to the type name
    // to represent the new type that represents the compiled expressions.
    // Take everything after the last . for the type name.
    string activityType = activityName.Split('.').Last() + "_CompiledExpressionRoot";
    // Take everything before the last . for the namespace.
    string activityNamespace = string.Join(".", activityName.Split('.').Reverse().Skip(1).Reverse());

    // Create a TextExpressionCompilerSettings.
    TextExpressionCompilerSettings settings = new TextExpressionCompilerSettings
    {
        Activity = activity,
        Language = "C#",
        ActivityName = activityType,
        ActivityNamespace = activityNamespace,
        RootNamespace = null,
        GenerateAsPartialClass = false,
        AlwaysGenerateSource = true,
        ForImplementation = false
    };

    // Compile the C# expression.
    TextExpressionCompilerResults results =
        new TextExpressionCompiler(settings).Compile();

    // Any compilation errors are contained in the CompilerMessages.
    if (results.HasErrors)
    {
        throw new Exception("Compilation failed.");
    }

    // Create an instance of the new compiled expression type.
    ICompiledExpressionRoot compiledExpressionRoot =
        Activator.CreateInstance(results.ResultType,
            new object[] { activity }) as ICompiledExpressionRoot;

    // Attach it to the activity.
    CompiledExpressionInvoker.SetCompiledExpressionRoot(
        activity, compiledExpressionRoot);
}
```
