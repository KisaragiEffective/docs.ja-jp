---
title: デザイナーによって XAML ファイルに追加されるビューステートを削除する-WF
ms.date: 03/30/2017
ms.assetid: a801ce22-8699-483c-a392-7bb3834aae4f
ms.openlocfilehash: f431275140e821aa5ec4d2235322f06be87d5ee2
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715621"
---
# <a name="removing-the-view-state-the-designer-adds-to-an-xaml-file"></a>デザイナーによって XAML ファイルに追加されるビューステートを削除する

このサンプルでは、<xref:System.Xaml.XamlWriter> から派生するクラスを作成する方法を示し、XAML ファイルからビュー ステートを削除します。 Windows ワークフローデザイナーは、ビューステートと呼ばれる情報を XAML ドキュメントに書き込みます。 ビュー ステートは、ランタイムでは不必要なレイアウト配置などの、デザイン時に必要な情報を参照します。 ワークフローデザイナーは、編集時にこの情報を XAML ドキュメントに挿入します。 ワークフローデザイナーは、`mc:Ignorable` 属性を使用してビューステートを XAML ファイルに書き込みます。このため、ランタイムが XAML ファイルを読み込むときに、この情報は読み込まれません。 このサンプルでは、XAML ノードの処理中にそのビューステート情報を削除するクラスを作成する方法を示します。

## <a name="discussion"></a>説明

このサンプルでは、カスタム ライターを作成する方法を示します。

カスタム XAML ライターをビルドするには、<xref:System.Xaml.XamlWriter> を継承するクラスを作成します。 多くの場合、XAML ライターは入れ子になっているため、"内部" の XAML ライターを追跡します。 これらの "内部" ライターは、XAML ライターの残りのスタックへの参照と考えることができます。これにより、複数のエントリポイントを使用して処理を行い、処理をスタックの残りの部分に委任することができます。

このサンプルには、重要な項目がいくつかあります。 その 1 つとして、書き込まれる項目がデザイナー名前空間からのものであるかどうかの確認が挙げられます。 これにより、ワークフローでデザイナー名前空間の他の型の使用が排除されることに注意してください。

```csharp
static Boolean IsDesignerAttachedProperty(XamlMember xamlMember)
{
    return xamlMember.IsAttachable &&
        xamlMember.PreferredXamlNamespace.Equals(c_sapNamespaceURI, StringComparison.OrdinalIgnoreCase);
}

const String c_sapNamespaceURI = "http://schemas.microsoft.com/netfx/2009/xaml/activities/presentation";

// The next item of interest is the constructor, where the utilization of the inner XAML writer is seen.
public ViewStateCleaningWriter(XamlWriter innerWriter)
{
    this.InnerWriter = innerWriter;
    this.MemberStack = new Stack<XamlMember>();
}

XamlWriter InnerWriter {get; set; }
Stack<XamlMember> MemberStack {get; set; }
```

これにより、ノード ストリームの走査中に使用される XAML メンバーのスタックも作成されます。 このサンプルの残りの作業は、主に `WriteStartMember` メソッドに含まれています。

```csharp
public override void WriteStartMember(XamlMember xamlMember)
{
    MemberStack.Push(xamlMember);

    if (IsDesignerAttachedProperty(xamlMember))
    {
        m_attachedPropertyDepth++;
    }

    if (m_attachedPropertyDepth > 0)
    {
        return;
    }

    InnerWriter.WriteStartMember(xamlMember);
}
```

後続のメソッドで、まだビューステート コンテナーに含まれているかどうかがチェックされ、含まれている場合は制御が戻り、ノードはライター スタックに渡されません。

```csharp
public override void WriteValue(Object value)
{
    if (m_attachedPropertyDepth > 0)
    {
        return;
    }

    InnerWriter.WriteValue(value);
}
```

カスタム XAML ライターを使用するには、XAML ライターのスタックでまとめて連結する必要があります。 この使用方法を次のコードに示します。

```csharp
XmlWriterSettings writerSettings = new XmlWriterSettings {  Indent = true };
XmlWriter xmlWriter = XmlWriter.Create(File.OpenWrite(args[1]), writerSettings);
XamlXmlWriter xamlWriter = new XamlXmlWriter(xmlWriter, new XamlSchemaContext());
XamlServices.Save(new ViewStateCleaningWriter(ActivityXamlServices.CreateBuilderWriter(xamlWriter)), ab);
```

## <a name="to-use-this-sample"></a>このサンプルを使用するには

1. Visual Studio 2010 を使用して、ViewStateCleaningWriter ソリューションファイルを開きます。

2. コマンド プロンプトを開き、ViewStageCleaningWriter.exe がビルドされているディレクトリに移動します。

3. Workflow1.xaml ファイルに対して ViewStateCleaningWriter.exe を実行します。

   実行可能ファイルの構文の例を次に示します。

   ```console
   ViewStateCleaningWriter.exe [input file] [output file]
   ```

   これにより、すべてのビューステート情報が削除された \の [ファイル名] に XAML ファイルが出力されます。

> [!NOTE]
> <xref:System.Activities.Statements.Sequence> ワークフローでは、多数の仮想化のヒントが削除されます。 その結果、デザイナーは次回の読み込み時にレイアウトを再計算します。 このサンプルを <xref:System.Activities.Statements.Flowchart> に対して使用すると、すべての配置情報および線のルーティング情報が削除され、後続のデザイナーへの読み込み時にすべてのアクティビティが画面の左側に積み上げられます。

## <a name="to-create-a-sample-xaml-file-for-use-with-this-sample"></a>このサンプルで使用するサンプル XAML ファイルを作成するには

1. Visual Studio 2010 を開きます。

2. 新しいワークフロー コンソール アプリケーションを作成します。

3. いくつかのアクティビティをキャンバスにドラッグ アンド ドロップします。

4. ワークフロー XAML ファイルを保存します。

5. XAML ファイルを調べて、ビューステートの添付プロパティを確認します。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Designer\ViewStateCleaningWriter`
