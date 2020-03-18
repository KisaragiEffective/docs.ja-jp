---
title: ドキュメントのシリアル化および保存
ms.date: 03/30/2017
helpviewer_keywords:
- 'serialization of documents [WPF], , '
- documents [WPF], storage
- documents [WPF], serialization
ms.assetid: 4839cd87-e206-4571-803f-0200098ad37b
ms.openlocfilehash: 6703825d4453b2e0e65036fa2d9c856139ee473c
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094567"
---
# <a name="document-serialization-and-storage"></a>ドキュメントのシリアル化および保存

Microsoft .NET Framework には、高品質のドキュメントを作成および表示するための強力な環境が用意されています。  固定ドキュメントとフロードキュメント、高度な表示コントロールをサポートする強化された機能は、強力な2D および3D グラフィック機能と組み合わせて、アプリケーションを新しいレベルの品質とユーザーエクスペリエンスに .NET Framework します。  ドキュメントのメモリ内表現を柔軟に管理できることは .NET Framework の重要な機能であり、データストアから効率的にドキュメントを保存して読み込むには、ほぼすべてのアプリケーションが必要です。  内部のメモリ内表現から外部のデータ ストアにドキュメントを変換するプロセスは、シリアル化と呼ばれます。  データ ストアを読み取って元のメモリ内インスタンスを再作成する逆のプロセスは、逆シリアル化と呼ばれます。

<a name="AboutSerialization"></a>

## <a name="about-document-serialization"></a>ドキュメントのシリアル化について

ドキュメントをメモリからシリアル化し、後で逆シリアル化してメモリに戻すプロセスは、アプリケーションに対して透過的に行われるのが理想的です。  アプリケーションは、シリアライザーの "書き込み" メソッドを呼び出してドキュメントを保存します。デシリアライザーの "読み取り" メソッドは、データ ストアにアクセスし、メモリ内の元のインスタンスを再作成します。  通常、シリアル化と逆シリアル化のプロセスで元の形式のドキュメントが再作成される限り、データが格納される特定の形式はアプリケーションにとって問題ではありません。

多くの場合、アプリケーションでは複数のシリアル化オプションが提供され、ユーザーは異なるメディアまたは異なる形式にドキュメントを保存できます。  たとえば、[名前を付けて保存] オプションでは、ドキュメントをディスク ファイル、データベース、Web サービスなどに保存できる場合があります。  同様に、別のシリアライザーでは、HTML、RTF、XML、XPS、サード パーティ形式などのさまざまな形式でドキュメントを格納できます。  アプリケーションに対して、シリアル化により、実装内のストレージ メディアの詳細を分離するインターフェイスが定義されます。  ストレージの詳細をカプセル化する利点に加えて、.NET Framework <xref:System.Windows.Documents.Serialization> Api には他にもいくつかの重要な機能が用意されています。

### <a name="features-of-net-framework-30-document-serializers"></a>.NET Framework 3.0 ドキュメント シリアライザーの機能

- 上位レベルのドキュメント オブジェクト (論理ツリーとビジュアル) に直接アクセスすることにより、ページ分割されたコンテンツ、2D/3D 要素、イメージ、メディア、ハイパーリンク、注釈、およびその他のサポート コンテンツの効率的な保存が可能になります。

- 同期操作と非同期操作。

- 拡張機能でのプラグイン シリアライザーのサポート:

  - すべての .NET Framework アプリケーションで使用するためのシステム全体のアクセス。

  - 簡単なアプリケーション プラグインの検出。

  - カスタム サードパーティ プラグインの簡単な展開、インストール、更新。

  - カスタム実行時設定とオプションのユーザー インターフェイス サポート。

### <a name="xps-print-path"></a>XPS 印刷パス

Microsoft .NET Framework XPS 印刷パスは、印刷出力を使用してドキュメントを書き込むための拡張可能な機構も提供します。  XPS は、ドキュメントファイル形式として機能し、Windows Vista のネイティブな印刷スプール形式として機能します。  XPS ドキュメントは、中間形式に変換しなくても、XPS 互換プリンターに直接送信できます。  印刷パス出力オプションと機能について詳しくは、「[印刷の概要](printing-overview.md)」をご覧ください。

<a name="PluginSerializers"></a>

## <a name="plug-in-serializers"></a>プラグイン シリアライザー

<xref:System.Windows.Documents.Serialization> Api は、アプリケーションとは別にインストールされ、実行時にバインドされ、<xref:System.Windows.Documents.Serialization.SerializerProvider> 検出メカニズムを使用してアクセスされる、プラグインシリアライザーとリンクされたシリアライザーの両方をサポートします。  プラグイン シリアライザーには、簡単に展開でき、システム全体で使用できるという大きな利点があります。  リンクされたシリアライザーは、プラグインシリアライザーにアクセスできない、XAML ブラウザーアプリケーション (Xbap) などの部分信頼環境に実装することもできます。  <xref:System.Windows.Documents.Serialization.SerializerWriter> クラスの派生実装に基づくリンクされたシリアライザーは、コンパイルされ、アプリケーションに直接リンクされます。  プラグイン シリアライザーとリンクされたシリアライザーはどちらも同じパブリック メソッドとイベントを通じて動作するので、同じアプリケーションでどちらか一方でも両方のシリアライザーでも簡単に使うことができます。

アプリケーション開発者にとっての利点として、プラグイン シリアライザーは新しいストレージ設計およびファイル形式に対する拡張性を備え、ビルド時に可能性のあるすべての形式を直接コーディングする必要はありません。  また、サードパーティの開発者にとっても、プラグイン シリアライザーには、カスタムまたは独自のファイル形式のためのシステムでアクセス可能なプラグインを展開、インストール、更新する標準化された手段が提供されるというメリットがあります。

### <a name="using-a-plug-in-serializer"></a>プラグイン シリアライザーの使用

プラグイン シリアライザーは簡単に使うことができます。  <xref:System.Windows.Documents.Serialization.SerializerProvider> クラスは、システムにインストールされている各プラグインの <xref:System.Windows.Documents.Serialization.SerializerDescriptor> オブジェクトを列挙します。  <xref:System.Windows.Documents.Serialization.SerializerDescriptor.IsLoadable%2A> プロパティは、現在の構成に基づいてインストールされているプラグインをフィルター処理し、シリアライザーがアプリケーションによって読み込まれて使用されることを確認します。  <xref:System.Windows.Documents.Serialization.SerializerDescriptor> には、<xref:System.Windows.Documents.Serialization.SerializerDescriptor.DisplayName%2A> や <xref:System.Windows.Documents.Serialization.SerializerDescriptor.DefaultFileExtension%2A>などの他のプロパティも用意されています。このプロパティを使用して、アプリケーションは、使用可能な出力形式のシリアライザーを選択するようにユーザーに要求できます。  XPS 用の既定のプラグインシリアライザーは .NET Framework と共に提供され、常に列挙されます。  ユーザーが出力形式を選択した後、<xref:System.Windows.Documents.Serialization.SerializerProvider.CreateSerializerWriter%2A> メソッドを使用して、特定の形式の <xref:System.Windows.Documents.Serialization.SerializerWriter> を作成します。  その後、<xref:System.Windows.Documents.Serialization.SerializerWriter.Write%2A?displayProperty=nameWithType> メソッドを呼び出して、ドキュメントストリームをデータストアに出力できます。

次の例は、"PlugInFileFilter" プロパティで <xref:System.Windows.Documents.Serialization.SerializerProvider> メソッドを使用するアプリケーションを示しています。  PlugInFileFilter は、インストールされているプラグインを列挙し、<xref:Microsoft.Win32.SaveFileDialog>の使用可能なファイルオプションを含むフィルター文字列を構築します。

[!code-csharp[DocumentSerialize#DocSerializeFileFilter](~/samples/snippets/csharp/VS_Snippets_Wpf/DocumentSerialize/CSharp/ThumbViewer.cs#docserializefilefilter)]

ユーザーが出力ファイル名を選択した後、次の例は、<xref:System.Windows.Documents.Serialization.SerializerProvider.CreateSerializerWriter%2A> メソッドを使用して指定された形式で特定のドキュメントを格納する方法を示しています。

[!code-csharp[DocumentSerialize#DocSerializePlugIn](~/samples/snippets/csharp/VS_Snippets_Wpf/DocumentSerialize/CSharp/ThumbViewer.cs#docserializeplugin)]

<a name="InstallingPluginSerializers"></a>

### <a name="installing-plug-in-serializers"></a>プラグイン シリアライザーのインストール

<xref:System.Windows.Documents.Serialization.SerializerProvider> クラスは、プラグインシリアライザーの検出とアクセスのための上位レベルのアプリケーションインターフェイスを提供します。  <xref:System.Windows.Documents.Serialization.SerializerProvider> は、システムにインストールされていてアクセスできるシリアライザーの一覧を、アプリケーションに検索して提供します。  インストールされているシリアライザーの詳細は、レジストリ設定によって定義されます。  プラグインシリアライザーは、<xref:System.Windows.Documents.Serialization.SerializerProvider.RegisterSerializer%2A> メソッドを使用してレジストリに追加できます。または .NET Framework がまだインストールされていない場合は、プラグインのインストールスクリプトによって、レジストリ値自体を直接設定できます。  <xref:System.Windows.Documents.Serialization.SerializerProvider.UnregisterSerializer%2A> メソッドを使用して、以前にインストールされたプラグインを削除することも、アンインストールスクリプトによって同様にレジストリ設定をリセットすることもできます。

### <a name="creating-a-plug-in-serializer"></a>プラグイン シリアライザーの作成

プラグイン シリアライザーとリンクされたシリアライザーはどちらも、同じ公開されたパブリック メソッドとイベントを使い、同期または非同期で動作するように同じように設計できます。  通常、プラグイン シリアライザーの作成には 3 つの基本的な手順があります。

1. 最初に、シリアライザーをリンクされたシリアライザーとして実装してデバッグします。  コンパイルしてテスト アプリケーションに直接リンクするシリアライザーを最初に作成すると、テストに役立つブレークポイントや他のデバッグ サービスに完全にアクセスできます。

2. シリアライザーが完全にテストされた後、プラグインを作成するための <xref:System.Windows.Documents.Serialization.ISerializerFactory> インターフェイスが追加されます。  <xref:System.Windows.Documents.Serialization.ISerializerFactory> インターフェイスは、論理ツリー、<xref:System.Windows.UIElement> オブジェクト、<xref:System.Windows.Documents.IDocumentPaginatorSource>、および <xref:System.Windows.Media.Visual> 要素を含むすべての .NET Framework オブジェクトへのフルアクセスを許可します。  さらに <xref:System.Windows.Documents.Serialization.ISerializerFactory> には、リンクされたシリアライザーで使用される同期および非同期のメソッドとイベントが用意されています。  サイズの大きいドキュメントの出力には時間がかかる場合があるため、ユーザーの操作に応答できる状態を維持し、データ ストアで問題が発生した場合の "キャンセル" オプションを提供できるよう、非同期操作を使うことをお勧めします。

3. プラグイン シリアライザーを作成した後、プラグインを配布してインストール (およびアンインストール) するためのインストール スクリプトを実装します (前の「[プラグイン シリアライザーのインストール](#InstallingPluginSerializers)」を参照)。

## <a name="see-also"></a>参照

- <xref:System.Windows.Documents.Serialization>
- <xref:System.Windows.Xps.XpsDocumentWriter>
- <xref:System.Windows.Xps.Packaging.XpsDocument>
- [WPF のドキュメント](documents-in-wpf.md)
- [印刷の概要](printing-overview.md)
- [XML Paper Specification](https://www.ecma-international.org/activities/XML%20Paper%20Specification/XPS%20Standard%20WD%201.6.pdf)
