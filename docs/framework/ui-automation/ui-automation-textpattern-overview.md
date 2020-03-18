---
title: UI オートメーション TextPattern の概要
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, TextPattern class
- TextPattern class
- classes, TextPattern
ms.assetid: 41787927-df1f-4f4a-aba3-641662854fc4
ms.openlocfilehash: 22966c8ed80be99497e7d05b56455c3057fdd81a
ms.sourcegitcommit: 9a97c76e141333394676bc5d264c6624b6f45bcf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75741418"
---
# <a name="ui-automation-textpattern-overview"></a>UI オートメーション TextPattern の概要

> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。

この概要では、 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] を使用して、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]でサポートされているプラットフォームのテキスト コントロールのテキストの内容 (書式とスタイルの属性など) を公開する方法について説明します。 これらのコントロールには、Microsoft .NET Framework <xref:System.Windows.Controls.TextBox> と <xref:System.Windows.Controls.RichTextBox> だけでなく、Win32 に相当するものが含まれます。

コントロールのテキストの内容は、 <xref:System.Windows.Automation.TextPattern> コントロール パターンを使用することで公開できます。コントロール パターンは、テキスト コンテナーの内容をテキスト ストリームとして表したものです。 さらに、 <xref:System.Windows.Automation.TextPattern> は、書式とスタイルの属性を公開する <xref:System.Windows.Automation.Text.TextPatternRange> クラスをサポートする必要があります。 <xref:System.Windows.Automation.Text.TextPatternRange> は、 <xref:System.Windows.Automation.TextPattern> および <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.Start> エンドポイントのコレクションがあるテキスト コンテナーに、連続する、または複数の非結合テキスト範囲を表すことで <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.End> をサポートします。 <xref:System.Windows.Automation.Text.TextPatternRange> は、選択、比較、取得、トラバースなどの機能をサポートしています。

> [!NOTE]
> <xref:System.Windows.Automation.TextPattern> クラスには、テキストを挿入または変更する手段がありません。 ただし、コントロールによっては、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] <xref:System.Windows.Automation.ValuePattern> または直接キーボード入力によって行われることがあります。 例については、「 [TextPattern Insert Text Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InsertText) 」をご覧ください。

この概要で説明されている機能は、支援技術ベンダーとそのエンドユーザーにとって重要です。 支援技術は、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] を使用してユーザー向けに完全なテキストの書式設定情報を収集するとともに、プログラムによるナビゲーションと、 <xref:System.Windows.Automation.Text.TextUnit> によるテキストの選択 (文字、単語、行、または段落) を行います。

<a name="UI_Automation_TextPattern_vs__Cicero"></a>

## <a name="ui-automation-textpattern-vs-text-services-framework"></a>UI オートメーション TextPattern とText Services Framework (TSF)

テキストサービスフレームワーク (TSF) は、デスクトップおよびアプリケーション内で自然言語サービスと高度なテキスト入力を可能にする、シンプルでスケーラブルなシステムフレームワークです。 アプリケーションのテキスト ストアを公開するインターフェイスをアプリケーションに提供するだけでなく、そのテキスト ストアのメタデータもサポートします。

ただし、TSF は、コンテキストに対応したシナリオに入力を挿入する必要があるアプリケーション向けに設計されていましたが、<xref:System.Windows.Automation.TextPattern> は読み取り専用のソリューションです (前述の制限付きの回避策を使用します)。これは、スクリーンリーダーと点字デバイスのテキストストアへのアクセスを最適化することを目的としています。

つまり、テキストストアへの読み取り専用アクセスを必要とするアクセス可能なテクノロジは <xref:System.Windows.Automation.TextPattern>を使用できますが、コンテキストに対応した入力については、TSF のより複雑な機能が必要になります。

<a name="Control_Types"></a>

## <a name="control-types"></a>コントロールの種類

### <a name="text"></a>テキスト

テキスト コントロールは、画面上のテキストの一部を表す基本要素です。

スタンドアロンのテキスト コントロールは、フォーム上のラベルまたは静的テキストとして使用できます。 また、テキスト コントロールを、 <xref:System.Windows.Automation.ControlType.ListItem>、 <xref:System.Windows.Automation.ControlType.TreeItem> 、または <xref:System.Windows.Automation.ControlType.DataItem>の構造に含めることもできます。

> [!NOTE]
> テキスト コントロールは、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコンテンツ ビューに表示されないことがあります (「 [UI Automation Tree Overview](ui-automation-tree-overview.md)」をご覧ください)。 これは、テキスト コントロールが別のコントロールの Name プロパティを通じて表示されることがよくあるためです。 たとえば、エディット コントロールのラベルに使用するテキストは、エディット コントロールの Name プロパティを通じて公開されます。 エディット コントロールが [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコンテンツ ビューに存在するので、テキスト要素自体は [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーの対象のビューに存在する必要はありません。 コンテンツ ビューに表示されるテキストは、冗長な情報ではないテキストのみです。 これにより、支援テクノロジは、ユーザーが必要とする情報のみをすばやくフィルター処理できるようになります。

### <a name="edit"></a>[編集]

エディット コントロールを使用すると、ユーザーは 1 行のテキストを表示および編集できます。

> [!NOTE]
> 1 行のテキストは、特定のレイアウトのシナリオでは折り返すことができます。

### <a name="document"></a>ドキュメント

ドキュメント コントロールを使用すると、ユーザーは複数のページのテキストに移動して、そこから情報を取得できます。

<a name="TextPattern_Client_API_s"></a>

## <a name="textpattern-client-apis"></a>TextPattern クライアント Api

|||
|-|-|
|`System.Windows.Automation.TextPattern Class`|[!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] テキスト モデルのエントリ ポイントです。<br /><br /> このクラスには、 <xref:System.Windows.Automation.TextPattern> と <xref:System.Windows.Automation.TextPattern.TextSelectionChangedEvent> という 2 つの <xref:System.Windows.Automation.TextPattern.TextChangedEvent>イベント リスナーが含まれています。|
|`System.Windows.Automation.Text.TextPatternRange Class`|<xref:System.Windows.Automation.TextPattern>をサポートする、テキスト コンテナー内のテキストの範囲の表記です。<br /><br /> UI オートメーション クライアントは、 <xref:System.Windows.Automation.Text.TextPatternRange>を使用して作成されるテキスト範囲の現在の有効性に関して注意する必要があります。 テキスト コントロールにある元のテキストが完全に新しいテキストに置き換えられた場合、現在のテキスト範囲は無効になります。 ただし、元のテキストの一部のみが変更され、基になるテキスト コントロールが、文字の絶対位置ではなくアンカー (またはエンドポイント) が付いたテキストの「ポインター」で管理している場合、テキスト範囲にはまだいくらかの有効性があります。<br /><br /> クライアントは、 <xref:System.Windows.Automation.TextPattern.TextChangedEvent> をリッスンして、操作するテキスト コンテンツに変更があった場合に通知を受けることができます。|
|`System.Windows.Automation.AutomationTextAttribute Class`|テキスト範囲の書式設定の属性を識別するために使用されます。|

<a name="TextPattern_Provider_API_s"></a>

## <a name="textpattern-provider-apis"></a>TextPattern プロバイダー Api

ネイティブに、または <xref:System.Windows.Automation.TextPattern> プロキシを介して <xref:System.Windows.Automation.Provider.ITextProvider> および <xref:System.Windows.Automation.Provider.ITextRangeProvider> の各インターフェイスを実装することで [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] をサポートする UI 要素またはコントロールは、堅牢なナビゲーション機能を提供するだけでなく、含まれている任意のテキストの詳細な属性情報を公開することができます。

<xref:System.Windows.Automation.TextPattern> プロバイダーは、コントロールで特定の属性のサポートが不足している場合、すべてのテキスト属性をサポートする必要はありません。

<xref:System.Windows.Automation.TextPattern> プロバイダーは、コントロールがテキスト領域内でテキストの選択またはテキストのカーソル (またはシステム キャレット) の配置をサポートする場合に <xref:System.Windows.Automation.TextPattern.GetSelection%2A> 機能と <xref:System.Windows.Automation.Text.TextPatternRange.Select%2A> 機能をサポートする必要があります。 コントロールがこの機能をサポートしない場合、これらのメソッドのいずれもサポートする必要はありません。 ただし、 <xref:System.Windows.Automation.Provider.ITextProvider.SupportedTextSelection%2A> プロパティを実装することで、コントロールはサポートするテキスト選択項目の種類を公開する必要があります。

<xref:System.Windows.Automation.TextPattern> プロバイダーは、 <xref:System.Windows.Automation.Text.TextUnit> の定数 <xref:System.Windows.Automation.Text.TextUnit.Character> と <xref:System.Windows.Automation.Text.TextUnit.Document> 、およびサポートが可能なその他すべての <xref:System.Windows.Automation.Text.TextUnit> 定数を常にサポートする必要があります。

> [!NOTE]
> プロバイダーは、次に大きい <xref:System.Windows.Automation.Text.TextUnit> ( <xref:System.Windows.Automation.Text.TextUnit> 、 <xref:System.Windows.Automation.Text.TextUnit.Character>、 <xref:System.Windows.Automation.Text.TextUnit.Format>、 <xref:System.Windows.Automation.Text.TextUnit.Word>、 <xref:System.Windows.Automation.Text.TextUnit.Line>、 <xref:System.Windows.Automation.Text.TextUnit.Paragraph>、 <xref:System.Windows.Automation.Text.TextUnit.Page>の順序でサポートされています) を遅らせることで、特定の <xref:System.Windows.Automation.Text.TextUnit.Document>のサポートを省略できます。

|||
|-|-|
|`ITextProvider Interface`|クライアント アプリケーションで <xref:System.Windows.Automation.TextPattern> をサポートするメソッド、プロパティ、および属性を公開します ( <xref:System.Windows.Automation.Provider.ITextProvider>を参照してください)。|
|`ITextRangeProvider Interface`|テキスト プロバイダー内のテキストの範囲を表します ( <xref:System.Windows.Automation.Provider.ITextRangeProvider>を参照してください)。|
|`System.Windows.Automation.TextPatternIdentifiers Class`|テキスト プロバイダーの識別子として使用される値が含まれています ( <xref:System.Windows.Automation.TextPatternIdentifiers>を参照してください)。|

<a name="Security"></a>

## <a name="security"></a>セキュリティ

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] アーキテクチャは、セキュリティを考慮して設計されています (「 [UI オートメーションのセキュリティの概要](ui-automation-security-overview.md)」を参照してください)。 ただし、この概要で説明する TextPattern クラスにはいくつか特定のセキュリティの考慮事項が必要です。

- [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] テキスト プロバイダーは、読み取り専用のインターフェイスを提供しますが、コントロールにある既存のテキストを変更する機能はありません。

- UI オートメーション クライアントは、それらが完全に「信頼できる」場合にのみ [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] を使用します。 この一例となるものが、信頼できる既知のアプリケーションのみが実行できる、保護されたログオン デスクトップです。

- UI オートメーション プロバイダーの開発者は、 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] を介してコントロールに公開することにしたすべての情報が、基本的にパブリックであり、他のコードで完全にアクセスできることを認識する必要があります。 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] は、UI オートメーション クライアントの信頼性を判断しようとしません。そのため、保護されたコンテンツまたは機密性の高いテキストの情報 (パスワード フィールドなど) をUI オートメーション プロバイダーで公開しないことをお勧めします。

- Windows Vista のセキュリティの最も重要な変更点の1つは、"セキュリティで保護された入力" と呼ばれます。これには、最小特権 (または制限付き) ユーザーアカウント (LUA) と UI 特権レベルの分離 (UIPI) などのテクノロジが含まれます。

  - UIPI は、プログラムが他のより「特権が高い」プログラムを制御または監視できないようにすることで、ユーザー入力になりすましたプロセス間のウィンドウ メッセージ攻撃を防ぎます。

  - LUA は、管理者グループのユーザーが実行しているアプリケーションの特権に制限を設定します。 アプリケーションには必ずしも管理者特権は必要なく、代わりに必要最小限の特権で実行します。 その結果、LUA シナリオに制限が適用される可能性があります。 最も顕著な文字列の切り捨て (TextPattern 文字列など) では、管理者レベルのアプリケーションから取得される文字列のサイズを制限する必要があることがあります。そうすることで、これらのアプリケーションは、アプリケーションを無効にするポイントまで強制的にメモリを割り当てられずに済みます。

<a name="Performance"></a>

## <a name="performance"></a>パフォーマンス

TextPattern はその機能のほとんどでプロセス間の呼び出しに依存しているので、コンテンツを処理するときにパフォーマンスを向上させるキャッシュのメカニズムは提供しません。 これは、 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] メソッドまたは <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A> メソッドを使用してアクセスできる <xref:System.Windows.Automation.AutomationElement.TryGetCachedPattern%2A> の他のコントロール パターンとは異なります。

パフォーマンスを向上させるための 1 つの方法は、 <xref:System.Windows.Automation.Text.TextPatternRange.GetText%2A>を使用して、UI オートメーション クライアントに中型のテキスト ブロックの取得を試みさせることです。 たとえば、GetText(1) の呼び出しでは、文字ごとにプロセス間の影響を受けます。一方、GetText(-1) の呼び出しでは、1 つのプロセス間の影響を受けますが、テキストのプロバイダーのサイズによっては待機時間が長くなることがあります。

<a name="Glossary"></a>

## <a name="textpattern-terminology"></a>TextPattern 用語集

**属性**\
テキスト範囲の書式設定特性 ( <xref:System.Windows.Automation.TextPattern.IsItalicAttribute> または <xref:System.Windows.Automation.TextPattern.FontNameAttribute>など)。

**低次元テキスト範囲**の\
低次元テキスト範囲は、空または 0 文字のテキスト範囲です。 TextPattern コントロール パターンの目的から、テキスト挿入ポイント (またはシステム キャレット) は低次元テキスト範囲と見なされます。 テキストが選択されていない場合、 <xref:System.Windows.Automation.TextPattern.GetSelection%2A> は、テキスト挿入ポイントで低次元テキスト範囲を返し、 <xref:System.Windows.Automation.TextPattern.RangeFromPoint%2A> は、開始エンドポイントとして低次元テキスト範囲を返します。 テキスト プロバイダーで指定された条件に一致するテキスト範囲が見つからない場合、<xref:System.Windows.Automation.TextPattern.RangeFromChild%2A> および <xref:System.Windows.Automation.TextPattern.GetVisibleRanges%2A> は低次元テキスト範囲を返す可能性があります。 この低次元テキスト範囲は、テキスト プロバイダー内の開始エンドポイントとして使用できます。 <xref:System.Windows.Automation.Text.TextPatternRange.FindText%2A> と <xref:System.Windows.Automation.Text.TextPatternRange.FindAttribute%2A> は、検出された範囲と低次元テキスト範囲との混同を避けるために、null 参照 (Microsoft Visual Basic .NET では`Nothing`) を返します。

**埋め込みオブジェクト**\
[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] テキスト モデルには、2 種類の埋め込みオブジェクトがあります。 これらは、ハイパーリンクやテーブルなどのテキスト ベースのコンテンツ要素、およびイメージとボタンなどのコントロール要素で構成しています。 詳しくは、「 [Access Embedded Objects Using UI Automation](access-embedded-objects-using-ui-automation.md)」をご覧ください。

**エンドポイント**\
テキスト コンテナー内のテキスト範囲の <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.Start> または <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.End> の絶対ポイント。

![Textpattern Rangeendpoints &#40;の開始と&#41;終了。](./media/uia-textpattern-endpoints.PNG "UIA_TextPattern_Endpoints")
始点と終点のセットを次に示します。

**TextRange**\
関連するすべての属性と機能を含むテキスト コンテナーの始点と終点で、テキストの範囲を表したもの。

<xref:System.Windows.Automation.Text.TextUnit>\
テキストの範囲の論理セグメントを介して移動するために使用するテキストの定義済みの単位 (文字、単語、行、または段落) 。

## <a name="see-also"></a>関連項目

- [クライアントの UI オートメーション コントロール パターン](ui-automation-control-patterns-for-clients.md)
- [UI Automation コントロール パターンの概要](ui-automation-control-patterns-overview.md)
- [UI Automation ツリーの概要](ui-automation-tree-overview.md)
- [UI オートメーションにおけるキャッシュの使用](use-caching-in-ui-automation.md)
- [UI オートメーション プロバイダーでのコントロール パターンのサポート](support-control-patterns-in-a-ui-automation-provider.md)
- [UI オートメーション クライアントのコントロール パターン マッピング](control-pattern-mapping-for-ui-automation-clients.md)
- [Text Services Framework (TSF)](/windows/desktop/api/_tsf/)
