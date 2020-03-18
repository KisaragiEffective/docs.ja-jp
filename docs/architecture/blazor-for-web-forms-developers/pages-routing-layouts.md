---
title: ページ、ルーティング、レイアウト
description: Blazor でページを作成する方法、クライアント側のルーティングを使用する方法、およびページレイアウトを管理する方法について説明します。
author: danroth27
ms.author: daroth
ms.date: 09/19/2019
ms.openlocfilehash: 693eee270a46ccb56ed5fef8fced1d4a1cf1974f
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "73841235"
---
# <a name="pages-routing-and-layouts"></a>ページ、ルーティング、レイアウト

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

ASP.NET Web フォームアプリは、 *.aspx*ファイルで定義されたページで構成されます。 各ページのアドレスは、プロジェクトの物理ファイルパスに基づいています。 ブラウザーがページに要求を行うと、ページの内容がサーバーに動的に表示されます。 ページの HTML マークアップとそのサーバーコントロールの両方に対する表示アカウント。

Blazor では、アプリの各ページは、通常、1つまたは複数の指定されたルートを使用して、razor ファイルで定義されているコンポーネントです *。* ルーティングは、ほとんどの場合、特定のサーバー要求を介さずにクライアント側で行われます。 ブラウザーはまず、アプリのルートアドレスに要求を行います。 次に、Blazor アプリ内のルート `Router` コンポーネントが、受信ナビゲーション要求を処理し、正しいコンポーネントに処理します。

Blazor は*ディープリンク*もサポートしています。 ディープリンクは、ブラウザーがアプリのルート以外の特定のルートに対して要求を行うときに発生します。 サーバーに送信されるディープリンクの要求は Blazor アプリにルーティングされ、その後、要求クライアント側が正しいコンポーネントにルーティングされます。

ASP.NET Web フォームの単純なページには、次のマークアップが含まれる場合があります。

*名前 .aspx*

```aspx-csharp
<%@ Page Title="Name" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Name.aspx.cs" Inherits="WebApplication1.Name" %>

<asp:Content ID="BodyContent" ContentPlaceHolderID="MainContent" runat="server">
    <div>
        What is your name?<br />
        <asp:TextBox ID="TextBox1" runat="server"></asp:TextBox>
        <asp:Button ID="Button1" runat="server" Text="Submit" OnClick="Button1_Click" />
    </div>
    <div>
        <asp:Literal ID="Literal1" runat="server" />
    </div>
</asp:Content>
```

*Name.aspx.cs*

```csharp
public partial class Name : System.Web.UI.Page
{
    protected void Button1_Click1(object sender, EventArgs e)
    {
        Literal1.Text = "Hello " + TextBox1.Text;
    }
}
```

Blazor アプリの同等のページは次のようになります。

*名前。 razor*

```razor
@page "/Name"
@layout MainLayout

<div>
    What is your name?<br />
    <input @bind="text" />
    <button @onclick="OnClick">Submit</button>
</div>
<div>
    @if (name != null)
    {
        @:Hello @name
    }
</div>

@code {
    string text;
    string name;

    void OnClick() {
        name = text;
    }
}
```

## <a name="create-pages"></a>ページの作成

Blazor でページを作成するには、コンポーネントを作成し、コンポーネントのルートを指定するために `@page` Razor ディレクティブを追加します。 `@page` ディレクティブは、そのコンポーネントに追加するルートテンプレートである1つのパラメーターを受け取ります。

```razor
@page "/counter"
```

ルートテンプレートパラメーターが必要です。 ASP.NET Web フォームとは異なり、Blazor コンポーネントへのルートはファイルの場所からは推論され*ません*(ただし、将来追加される機能である可能性があります)。

ルートテンプレートの構文は、ASP.NET Web フォームでのルーティングに使用される基本構文と同じです。 ルートパラメーターは、テンプレート内で中かっこを使用して指定されます。 Blazor は、ルート値を同じ名前のコンポーネントパラメーターにバインドします (大文字と小文字は区別されません)。

```razor
@page "/product/{id}"

<h1>Product @Id</h1>

@code {
    [Parameter]
    public string Id { get; set; }
}
```

Route パラメーターの値に対して制約を指定することもできます。 たとえば、製品 ID を `int`に制限するには、次のようにします。

```razor
@page "/product/{id:int}"

<h1>Product @Id</h1>

@code {
    [Parameter]
    public int Id { get; set; }
}
```

Blazor でサポートされているルート制約の完全な一覧については、「[ルート制約](/aspnet/core/blazor/routing#route-constraints)」を参照してください。

## <a name="router-component"></a>ルーターコンポーネント

Blazor でのルーティングは、`Router` コンポーネントによって処理されます。 `Router` コンポーネントは、通常、アプリのルートコンポーネント (*app.xaml*) で使用されます。

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

`Router` コンポーネントは、指定された `AppAssembly` とオプションで指定された `AdditionalAssemblies`内のルーティング可能なコンポーネントを検出します。 ブラウザーが移動すると、`Router` はナビゲーションをインターセプトし、展開された `RouteData` の `Found` パラメーターの内容を表示します。ルートがアドレスと一致する場合、`Router` はその `NotFound` パラメーターをレンダリングします。

`RouteView` コンポーネントは、`RouteData` によって指定された一致するコンポーネントがある場合、そのレイアウトを表示します。 一致したコンポーネントにレイアウトがない場合は、必要に応じて `DefaultLayout` を使用します。

`LayoutView` コンポーネントは、指定されたレイアウト内で子コンテンツをレンダリングします。 レイアウトについては、この章で後ほど詳しく説明します。

## <a name="navigation"></a>ナビゲーション

ASP.NET Web フォームでは、ブラウザーにリダイレクト応答を返すことによって、別のページへのナビゲーションをトリガーします。 例 :

```csharp
protected void NavigateButton_Click(object sender, EventArgs e)
{
    Response.Redirect("Counter");
}
```

Blazor では、通常、リダイレクト応答を返すことはできません。 Blazor は、要求/応答モデルを使用しません。 ただし、JavaScript の場合と同様に、ブラウザーのナビゲーションを直接トリガーできます。

Blazor には、次のために使用できる `NavigationManager` サービスが用意されています。

- 現在のブラウザーアドレスを取得します
- ベースアドレスを取得する
- ナビゲーションのトリガー
- アドレスが変更されたときに通知を受け取る

別のアドレスに移動するには、`NavigateTo` メソッドを使用します。

```razor
@page "/"
@inject NavigationManager NavigationManager

<button @onclick="Navigate">Navigate</button>

@code {
    void Navigate() {
        NavigationManager.NavigateTo("counter");
    }
}
```

すべての `NavigationManager` メンバーの説明については、「 [URI およびナビゲーション状態ヘルパー](/aspnet/core/blazor/routing#uri-and-navigation-state-helpers)」を参照してください。

## <a name="base-urls"></a>ベース Url

Blazor アプリが基本パスの下にデプロイされている場合は、[作業にルーティング] プロパティで `<base>` タグを使用して、ページメタデータにベース URL を指定する必要があります。 アプリケーションのホストページが Razor を使用してサーバーでレンダリングされる場合は、`~/` 構文を使用して、アプリのベースアドレスを指定できます。 ホストページが静的な HTML の場合は、ベース URL を明示的に指定する必要があります。

```html
<base href="~/" />
```

## <a name="page-layout"></a>ページレイアウト

ASP.NET Web フォームのページレイアウトは、マスターページによって処理されます。 マスターページは、1つまたは複数のコンテンツのプレースホルダーを持つテンプレートを定義します。このプレースホルダーは、個々のページで提供できます。 マスターページは、 *.master*ファイルで定義され、`<%@ Master %>` ディレクティブで始まります。 *.Master*ファイルの内容は *.aspx*ページと同じようにコード化されますが、`<asp:ContentPlaceHolder>` コントロールを追加して、ページがコンテンツを提供できる場所をマークします。

*サイト. master*

```aspx-csharp
<%@ Master Language="C#" AutoEventWireup="true" CodeBehind="Site.master.cs" Inherits="WebApplication1.SiteMaster" %>

<!DOCTYPE html>
<html lang="en">
<head runat="server">
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title><%: Page.Title %> - My ASP.NET Application</title>
    <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
</head>
<body>
    <form runat="server">
        <div class="container body-content">
            <asp:ContentPlaceHolder ID="MainContent" runat="server">
            </asp:ContentPlaceHolder>
            <hr />
            <footer>
                <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
            </footer>
        </div>
    </form>
</body>
</html>
```

Blazor では、レイアウトコンポーネントを使用してページレイアウトを処理します。 レイアウトコンポーネントは `LayoutComponentBase`から継承します。これにより、ページの内容を表示するために使用できる、`RenderFragment`型の単一の `Body` プロパティが定義されます。

*MainLayout。 razor*

```razor
@inherits LayoutComponentBase
<h1>Main layout</h1>
<div>
    @Body
</div>
```

レイアウトが指定されたページが表示されると、そのページは、レイアウトによって `Body` プロパティがレンダリングされる場所で、指定されたレイアウトのコンテンツ内に表示されます。

ページにレイアウトを適用するには、`@layout` ディレクティブを使用します。

```razor
@layout MainLayout
```

*_Imports の razor*ファイルを使用して、フォルダーおよびサブフォルダー内のすべてのコンポーネントのレイアウトを指定できます。 [ルーターコンポーネント](#router-component)を使用して、すべてのページの既定のレイアウトを指定することもできます。

マスターページでは複数のコンテンツプレースホルダーを定義できますが、Blazor のレイアウトには、1つの `Body` プロパティしかありません。 Blazor レイアウトコンポーネントのこの制限は、今後のリリースで対処される予定です。

ASP.NET Web フォームのマスターページは入れ子にすることができます。 つまり、マスターページでマスターページを使用することもできます。 Blazor のレイアウトコンポーネントも入れ子にすることができます。 レイアウトコンポーネントをレイアウトコンポーネントに適用できます。 内部レイアウトの内容は、外側のレイアウト内にレンダリングされます。

*ChildLayout。 razor*

```razor
@layout MainLayout
<h2>Child layout</h2>
<div>
    @Body
</div>
```

*インデックス。 razor*

```razor
@page "/"
@layout ChildLayout
<p>I'm in a nested layout!</p>
```

ページのレンダリングされた出力は次のようになります。

```html
<h1>Main layout</h1>
<div>
    <h2>Child layout</h2>
    <div>
        <p>I'm in a nested layout!</p>
    </div>
</div>
```

Blazor のレイアウトでは、通常、ページのルート HTML 要素 (`<html>`、`<body>`、`<head>`など) を定義しません。 ルート HTML 要素は、代わりに Blazor アプリのホストページで定義されます。このページは、アプリの初期 HTML コンテンツを表示するために使用されます (「[ブートストラップ Blazor](project-structure.md#bootstrap-blazor)」を参照してください)。 ホストページでは、周囲のマークアップを含むアプリの複数のルートコンポーネントをレンダリングできます。

Blazor のコンポーネント (ページを含む) は `<script>` タグをレンダリングできません。 `<script>` タグが一度読み込まれてから変更できないため、この表示制限が存在します。 Razor 構文を使用してタグを動的に表示しようとすると、予期しない動作が発生することがあります。 代わりに、すべての `<script>` タグをアプリのホストページに追加する必要があります。

>[!div class="step-by-step"]
>[前へ](components.md)
>[次へ](state-management.md)
