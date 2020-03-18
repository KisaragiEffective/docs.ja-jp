---
title: イメージングの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- metadata [WPF], images
- displaying images [WPF]
- Imaging API [WPF]
- image metadata [WPF]
- converting images [WPF]
- encoding image formats [WPF]
- format decoding for images [WPF]
- painting with images [WPF]
- stretching images [WPF]
- images [WPF], about imaging
- format encoding for images [WPF]
- cropping images [WPF]
- decoding image formats [WPF]
- rotating images [WPF]
ms.assetid: 72aad87a-e6f3-4937-94cd-a18b7766e990
ms.openlocfilehash: a4151ff610c67ac762f0096c6a136f4475317782
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636641"
---
# <a name="imaging-overview"></a>イメージングの概要
このトピックでは、Microsoft Windows Presentation Foundation Imaging コンポーネントの概要について説明します。 WPF イメージングを使用すると、開発者はイメージの表示、変換、および書式設定を行うことができます。  

## <a name="wpf-imaging-component"></a>WPF Imaging Component  
 WPF イメージングでは、Microsoft Windows 内のイメージング機能が大幅に強化されています。 ビットマップの表示や一般的なコントロールでのイメージの使用などのイメージング機能は、以前は Microsoft Windows グラフィックスデバイスインターフェイス (GDI) または Microsoft Windows GDI + ライブラリに依存していました。 これらの API は、ベースラインイメージング機能を提供しますが、コーデック拡張機能や忠実度の高いイメージのサポートなどの機能を備えていません。 WPF イメージングは、GDI と GDI + の欠点を克服し、アプリケーション内でイメージを表示して使用するための新しい API のセットを提供するように設計されています。  
  
 WPF イメージング API にアクセスするには、マネージコンポーネントとアンマネージコンポーネントの2つの方法があります。 アンマネージ コンポーネントは、次の機能を提供します。  
  
- 新規または独自のイメージ形式の機能拡張モデル。  
  
- ビットマップ (BMP)、ジョイント Photographics エキスパートグループ (JPEG)、ポータブルネットワークグラフィックス (PNG)、Tagged Image File Format (TIFF)、Microsoft Windows Media Photo、グラフィックスインターチェンジ形式 (GIF) など、ネイティブイメージ形式のパフォーマンスとセキュリティが向上しました。とアイコン (.ico)。  
  
- チャネルあたり最大 8 ビットのビット深度の高いイメージ データの保存 (ピクセルあたり 32 ビット)。  
  
- 非破壊的なイメージのスケーリング、クロップ、および回転。  
  
- 単純化されたカラー管理。  
  
- ファイル内での専用メタデータのサポート。  
  
- マネージコンポーネントは、アンマネージインフラストラクチャを使用して、[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)]、アニメーション、グラフィックスなどの他の WPF 機能とイメージをシームレスに統合します。 マネージコンポーネントは、WPF アプリケーションで新しいイメージ形式を自動的に認識できるようにする Windows Presentation Foundation (WPF) イメージングコーデック拡張モデルの利点もあります。  
  
 マネージ WPF Imaging API の大部分は <xref:System.Windows.Media.Imaging?displayProperty=nameWithType> 名前空間に存在しますが、<xref:System.Windows.Media.ImageBrush> や <xref:System.Windows.Media.ImageDrawing> などのいくつかの重要な型は <xref:System.Windows.Media?displayProperty=nameWithType> 名前空間に存在し、<xref:System.Windows.Controls.Image> は <xref:System.Windows.Controls?displayProperty=nameWithType> 名前空間に存在します。  
  
 このトピックでは、マネージド コンポーネントに関する追加情報について説明します。 アンマネージ API の詳細については、[アンマネージ WPF イメージングコンポーネント](/windows/desktop/wic/-wic-lh)のドキュメントを参照してください。  
  
<a name="_imageformats"></a>   
## <a name="wpf-image-formats"></a>WPF イメージ形式  

 コーデックは、特定のメディア形式をデコードまたはエンコードするために使用されます。 WPF イメージングには、BMP、JPEG、PNG、TIFF、Windows Media Photo、GIF、および ICON image 形式のコーデックが含まれています。 アプリケーションは、これらのコーデックを使用して、対応するイメージ形式をデコードでき、ICON を除くイメージ形式をエンコードできます。  
  
 <xref:System.Windows.Media.Imaging.BitmapSource> は、イメージのデコードとエンコードで使用される重要なクラスです。 これは、WPF イメージングパイプラインの基本的な構成要素であり、特定のサイズと解像度で1つの定数のピクセルセットを表します。 <xref:System.Windows.Media.Imaging.BitmapSource> には、複数のフレームイメージの個々のフレームを指定することも、<xref:System.Windows.Media.Imaging.BitmapSource>に対して実行される変換の結果にすることもできます。 これは、<xref:System.Windows.Media.Imaging.BitmapFrame>など、WPF イメージングで使用される主なクラスの多くの親です。  
  
 <xref:System.Windows.Media.Imaging.BitmapFrame> は、イメージ形式の実際のビットマップデータを格納するために使用されます。 多くのイメージ形式でサポートされるのは1つの <xref:System.Windows.Media.Imaging.BitmapFrame>のみですが、GIF や TIFF などの形式では、イメージごとに複数のフレームがサポートされます。 フレームは、デコーダーによって入力データとして使用され、イメージ ファイルを作成するためにエンコーダーに渡されます。  
  
 次の例では、<xref:System.Windows.Media.Imaging.BitmapSource> から <xref:System.Windows.Media.Imaging.BitmapFrame> を作成し、TIFF イメージに追加する方法を示します。  
  
 [!code-csharp[BitmapFrameExample#10](~/samples/snippets/csharp/VS_Snippets_Wpf/BitmapFrameExample/CSharp/BitmapFrame.cs#10)]
 [!code-vb[BitmapFrameExample#10](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BitmapFrameExample/VB/BitmapFrame.vb#10)]  
  
### <a name="image-format-decoding"></a>イメージ形式のデコード  
 イメージのデコードは、イメージ形式をシステムで使用できるイメージ データに変換する操作です。 変換したイメージ データを使用して、表示、処理、または別の形式へのエンコードを実行できます。 デコーダーの選択は、イメージ形式に基づきます。 コーデックの選択は、特定のデコーダーを指定しない限り、自動的に行われます。 自動デコード操作については、「[WPF でのイメージの表示](#_displayingimages)」セクションの例を参照してください。 アンマネージ WPF イメージングインターフェイスを使用して開発され、システムに登録されたカスタム書式指定デコーダーは、自動的にデコーダーの選択に参加します。 これにより、カスタム書式を WPF アプリケーションに自動的に表示できます。  
  
 次の例は、ビットマップデコーダーを使用して、BMP 形式のイメージをデコードする方法を示しています。  
  
 [!code-cpp[BmpBitmapDecoderEncoder#5](~/samples/snippets/cpp/VS_Snippets_Wpf/BmpBitmapDecoderEncoder/CPP/anotherfile.cpp#5)]
 [!code-csharp[BmpBitmapDecoderEncoder#5](~/samples/snippets/csharp/VS_Snippets_Wpf/BmpBitmapDecoderEncoder/CSharp/BitmapFrame.cs#5)]
 [!code-vb[BmpBitmapDecoderEncoder#5](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BmpBitmapDecoderEncoder/VB/BitmapFrame.vb#5)]  
  
### <a name="image-format-encoding"></a>イメージ形式のエンコード  
 イメージのエンコードは、イメージ データを特定のイメージ形式に変換する操作です。 エンコードされたイメージ データを使用して、新しいイメージ ファイルを作成できます。 WPF イメージングでは、上記で説明したイメージ形式ごとにエンコーダーが提供されます。  
  
 次の例は、エンコーダーを使用して新しく作成されたビットマップ イメージを保存する操作を示しています。  
  
 [!code-cpp[BmpBitmapDecoderEncoder#3](~/samples/snippets/cpp/VS_Snippets_Wpf/BmpBitmapDecoderEncoder/CPP/anotherfile.cpp#3)]
 [!code-csharp[BmpBitmapDecoderEncoder#3](~/samples/snippets/csharp/VS_Snippets_Wpf/BmpBitmapDecoderEncoder/CSharp/BitmapFrame.cs#3)]
 [!code-vb[BmpBitmapDecoderEncoder#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BmpBitmapDecoderEncoder/VB/BitmapFrame.vb#3)]  
  
<a name="_displayingimages"></a>   
## <a name="displaying-images-in-wpf"></a>WPF でのイメージの表示  
 Windows Presentation Foundation (WPF) アプリケーションでイメージを表示するには、いくつかの方法があります。 画像を表示するには、<xref:System.Windows.Controls.Image> コントロールを使用するか、<xref:System.Windows.Media.ImageBrush>を使用してビジュアルに描画するか、<xref:System.Windows.Media.ImageDrawing>を使用して描画します。  
  
### <a name="using-the-image-control"></a>イメージ コントロールの使用  
 <xref:System.Windows.Controls.Image> はフレームワーク要素であり、アプリケーションにイメージを表示する主な方法です。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]では、次の2つの方法で <xref:System.Windows.Controls.Image> を使用できます。属性構文またはプロパティ構文。 次の例は、属性構文とプロパティ タグ構文の両方を使用して、幅 200 ピクセルのイメージをレンダリングする方法を示しています。 属性の構文とプロパティの構文の詳細については、[Dependency Properties Overview](../advanced/dependency-properties-overview.md)を参照してください。  
  
 [!code-xaml[ImageElementExample_snip#ImageSimpleExampleInlineMarkup](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageElementExample_snip/CSharp/ImageSimpleExample.xaml#imagesimpleexampleinlinemarkup)]  
  
 多くの例では、<xref:System.Windows.Media.Imaging.BitmapImage> オブジェクトを使用してイメージファイルを参照しています。 <xref:System.Windows.Media.Imaging.BitmapImage> は、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] の読み込み用に最適化された特殊な <xref:System.Windows.Media.Imaging.BitmapSource> であり、<xref:System.Windows.Controls.Image> コントロールの <xref:System.Windows.Controls.Image.Source%2A> としてイメージを表示する簡単な方法です。  
  
 次の例では、コードを使用して幅 200 ピクセルのイメージをレンダリングする方法を示します。  
  
> [!NOTE]
> <xref:System.Windows.Media.Imaging.BitmapImage> は、<xref:System.ComponentModel.ISupportInitialize> インターフェイスを実装して、複数のプロパティの初期化を最適化します。 プロパティの変更は、オブジェクトの初期化中にのみ実行できます。 <xref:System.Windows.Media.Imaging.BitmapImage.BeginInit%2A> を呼び出して、初期化が開始されたことを通知し、初期化が完了したことを通知する <xref:System.Windows.Media.Imaging.BitmapImage.EndInit%2A> ます。 初期化の完了後は、プロパティの変更は無視されます。  
  
 [!code-csharp[ImageElementExample_snip#ImageSimpleExampleInlineCode1](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageElementExample_snip/CSharp/ImageSimpleExample.xaml.cs#imagesimpleexampleinlinecode1)]
 [!code-vb[ImageElementExample_snip#ImageSimpleExampleInlineCode1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImageElementExample_snip/VB/ImageSimpleExample.xaml.vb#imagesimpleexampleinlinecode1)]  
  
#### <a name="rotating-converting-and-cropping-images"></a>イメージの回転、変換、およびクロップ  
 WPF を使用すると、ユーザーは <xref:System.Windows.Media.Imaging.BitmapImage> のプロパティを使用して、または <xref:System.Windows.Media.Imaging.CroppedBitmap> や <xref:System.Windows.Media.Imaging.FormatConvertedBitmap>などの追加の <xref:System.Windows.Media.Imaging.BitmapSource> オブジェクトを使用してイメージを変換できます。 これらのイメージの変換では、イメージの拡大縮小、回転、ピクセル形式の変更、またはクロップを行うことができます。  
  
 イメージの回転は、<xref:System.Windows.Media.Imaging.BitmapImage>の <xref:System.Windows.Media.Imaging.BitmapImage.Rotation%2A> プロパティを使用して実行されます。 回転は、90 ° 単位でのみ実行できます。 次の例では、イメージが 90 ° 回転します。  
  
 [!code-xaml[ImageElementExample#TransformedXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageElementExample/CSharp/TransformedImageExample.xaml#transformedxaml2)]  
  
 [!code-csharp[ImageElementExample#TransformedCSharp1](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageElementExample/CSharp/TransformedImageExample.xaml.cs#transformedcsharp1)]
 [!code-vb[ImageElementExample#TransformedCSharp1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImageElementExample/VB/TransformedImageExample.xaml.vb#transformedcsharp1)]  
  
 画像をグレースケールなどの別のピクセル形式に変換するには、<xref:System.Windows.Media.Imaging.FormatConvertedBitmap>を使用します。 次の例では、イメージが <xref:System.Windows.Media.PixelFormats.Gray4%2A>に変換されます。  
  
 [!code-xaml[ImageElementExample_snip#ConvertedXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageElementExample_snip/CSharp/FormatConvertedExample.xaml#convertedxaml2)]  
  
 [!code-csharp[ImageElementExample_snip#ConvertedCSharp1](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageElementExample_snip/CSharp/FormatConvertedExample.xaml.cs#convertedcsharp1)]
 [!code-vb[ImageElementExample_snip#ConvertedCSharp1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImageElementExample_snip/VB/FormatConvertedExample.xaml.vb#convertedcsharp1)]  
  
 イメージをトリミングするには、<xref:System.Windows.Controls.Image> または <xref:System.Windows.Media.Imaging.CroppedBitmap> の <xref:System.Windows.UIElement.Clip%2A> プロパティを使用できます。 通常、イメージの一部を表示するだけの場合は、<xref:System.Windows.UIElement.Clip%2A> を使用する必要があります。 トリミングされたイメージをエンコードして保存する必要がある場合は、<xref:System.Windows.Media.Imaging.CroppedBitmap> を使用する必要があります。 次の例では、<xref:System.Windows.Media.EllipseGeometry>を使用して、クリッププロパティを使用してイメージをトリミングします。  
  
 [!code-xaml[ImageElementExample_snip#CroppedXAMLUsingClip1](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageElementExample_snip/CSharp/CroppedImageExample.xaml#croppedxamlusingclip1)]  
  
 [!code-csharp[ImageElementExample_snip#CroppedCSharpUsingClip1](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageElementExample_snip/CSharp/CroppedImageExample.xaml.cs#croppedcsharpusingclip1)]
 [!code-vb[ImageElementExample_snip#CroppedCSharpUsingClip1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImageElementExample_snip/VB/CroppedImageExample.xaml.vb#croppedcsharpusingclip1)]  
  
#### <a name="stretching-images"></a>イメージの引き伸ばし  
 <xref:System.Windows.Controls.Image.Stretch%2A> プロパティは、コンテナーを埋めるためにイメージをどのように拡大するかを制御します。 <xref:System.Windows.Controls.Image.Stretch%2A> プロパティは、<xref:System.Windows.Media.Stretch> 列挙体によって定義される次の値を受け入れます。  
  
- <xref:System.Windows.Media.Stretch.None>: イメージは、出力領域に合わせて拡大されません。 イメージが出力領域よりも大きい場合は、出力領域に収まらない部分がクリップされたイメージが出力領域に描画されます。  
  
- <xref:System.Windows.Media.Stretch.Fill>: イメージは、出力領域に合わせて拡大縮小されます。 イメージの高さと幅は個別に拡大縮小されるため、イメージの元の縦横比は保持されないことがあります。 つまり、イメージは、出力コンテナーを完全に埋めるためにゆがんで表示される可能性があります。  
  
- <xref:System.Windows.Media.Stretch.Uniform>: イメージは、出力領域内に完全に収まるようにスケーリングされます。 イメージの縦横比は保持されます。  
  
- <xref:System.Windows.Media.Stretch.UniformToFill>: イメージは、イメージの元の縦横比を維持したまま、出力領域を完全に塗りつぶすようにスケーリングされます。  
  
 次の例では、使用可能な <xref:System.Windows.Media.Stretch> 列挙をそれぞれ <xref:System.Windows.Controls.Image>に適用します。  
  
 次の図は、この例の出力を示しています。また、イメージに適用した場合のさまざまな <xref:System.Windows.Controls.Image.Stretch%2A> 設定の影響を示しています。  
  
 ![異なる TileBrush Stretch 設定](./media/img-mmgraphics-stretchenum.jpg "img_mmgraphics_stretchenum")  
異なる Stretch 設定  
  
 [!code-xaml[ImageElementExample_snip#ImageStretchExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageElementExample_snip/CSharp/ImageStretchExample.xaml#imagestretchexamplewholepage)]  
  
### <a name="painting-with-images"></a>イメージによる塗りつぶし  
 画像は、<xref:System.Windows.Media.Brush>で描画することによってアプリケーションに表示することもできます。 ブラシを使用すると、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] オブジェクトを単色で塗りつぶすことも、パターンとイメージの複雑な組み合わせで塗りつぶすこともできます。 イメージで描画するには、<xref:System.Windows.Media.ImageBrush>を使用します。 <xref:System.Windows.Media.ImageBrush> は、そのコンテンツをビットマップイメージとして定義する <xref:System.Windows.Media.TileBrush> の一種です。 <xref:System.Windows.Media.ImageBrush> には、<xref:System.Windows.Media.ImageBrush.ImageSource%2A> プロパティによって指定された1つのイメージが表示されます。 イメージがゆがまないようしたり、パターンやその他の効果を作成したりするために、イメージの伸縮、配置、およびタイル表示方法を制御できます。 次の図は、<xref:System.Windows.Media.ImageBrush>で実現できるいくつかの効果を示しています。  
  
 ![ImageBrush の出力例](./media/wcpsdk-mmgraphics-imagebrushexamples.gif "wcpsdk_mmgraphics_imagebrushexamples")  
イメージ ブラシは、図形、コントロール、テキストなどを塗りつぶすことができる  
  
 次の例では、<xref:System.Windows.Media.ImageBrush>を使用して、イメージでボタンの背景を描画する方法を示します。  
  
 [!code-xaml[UsingImageBrush#4](~/samples/snippets/csharp/VS_Snippets_Wpf/UsingImageBrush/CS/PaintingWithImages.xaml#4)]  
  
 イメージの <xref:System.Windows.Media.ImageBrush> と描画の詳細については[、「イメージ、描画、およびビジュアルによる塗りつぶし](painting-with-images-drawings-and-visuals.md)」を参照してください。  
  
<a name="_metadata"></a>   
## <a name="image-metadata"></a>イメージのメタデータ  
 一部のイメージ ファイルには、ファイルの内容または特性を記述するメタデータが含まれています。 たとえば、ほとんどのデジタル カメラは、イメージをキャプチャするために使用されるカメラの型番に関するメタデータを含むイメージを作成します。 各イメージ形式では、メタデータの処理方法が異なりますが、WPF イメージングは、サポートされているイメージ形式ごとにメタデータを格納および取得するための統一された方法を提供  
  
 メタデータへのアクセスは、<xref:System.Windows.Media.Imaging.BitmapSource> オブジェクトの <xref:System.Windows.Media.Imaging.BitmapSource.Metadata%2A> プロパティを通じて提供されます。 <xref:System.Windows.Media.Imaging.BitmapSource.Metadata%2A> は、イメージに含まれるすべてのメタデータを含む <xref:System.Windows.Media.Imaging.BitmapMetadata> オブジェクトを返します。 このデータは、1 つのメタデータ スキーマでも、異なるスキーマの組み合わせでもかまいません。 WPF イメージングでは、次のイメージメタデータスキーマがサポートされています。 Exchangeable 可能イメージファイル (Exif)、テキスト (PNG テキストデータ)、イメージファイルディレクトリ (IFD)、国際通話通信委員会 (IPTC)、および拡張可能メタデータプラットフォーム (XMP)。  
  
 メタデータの読み取りプロセスを簡略化するために、<xref:System.Windows.Media.Imaging.BitmapMetadata> には、<xref:System.Windows.Media.Imaging.BitmapMetadata.Author%2A>、<xref:System.Windows.Media.Imaging.BitmapMetadata.Title%2A>、<xref:System.Windows.Media.Imaging.BitmapMetadata.CameraModel%2A>などの簡単にアクセスできる名前付きプロパティがいくつか用意されています。 これらの名前付きプロパティの多くは、メタデータを書き込むためにも使用できます。 メタデータを読み取るための追加サポートは、メタデータ クエリ リーダーによって提供されます。 <xref:System.Windows.Media.Imaging.BitmapMetadata.GetQuery%2A> メソッドは、 *"/app1/exif/"* などの文字列クエリを指定することによって、メタデータクエリリーダーを取得するために使用されます。 次の例では、 *"/Text/Description"* の場所に格納されているテキストを取得するために <xref:System.Windows.Media.Imaging.BitmapMetadata.GetQuery%2A> が使用されています。  
  
 [!code-cpp[BitmapMetadata#GetQuery](~/samples/snippets/cpp/VS_Snippets_Wpf/BitMapMetadata/CPP/BitmapMetadata.cpp#getquery)]
 [!code-csharp[BitmapMetadata#GetQuery](~/samples/snippets/csharp/VS_Snippets_Wpf/BitMapMetadata/CSharp/BitmapMetadata.cs#getquery)]
 [!code-vb[BitmapMetadata#GetQuery](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BitMapMetadata/VB/BitmapMetadata.vb#getquery)]  
  
 メタデータを書き込むには、メタデータ クエリ ライターが使用されます。 <xref:System.Windows.Media.Imaging.BitmapMetadata.SetQuery%2A> はクエリライターを取得し、目的の値を設定します。 次の例では、 *"/Text/Description"* の場所に格納されているテキストを書き込むために <xref:System.Windows.Media.Imaging.BitmapMetadata.SetQuery%2A> が使用されています。  
  
 [!code-cpp[BitmapMetadata#SetQuery](~/samples/snippets/cpp/VS_Snippets_Wpf/BitMapMetadata/CPP/BitmapMetadata.cpp#setquery)]
 [!code-csharp[BitmapMetadata#SetQuery](~/samples/snippets/csharp/VS_Snippets_Wpf/BitMapMetadata/CSharp/BitmapMetadata.cs#setquery)]
 [!code-vb[BitmapMetadata#SetQuery](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BitMapMetadata/VB/BitmapMetadata.vb#setquery)]  
  
<a name="_extensibility"></a>   
## <a name="codec-extensibility"></a>コーデックの機能拡張  
 WPF イメージングのコア機能は、新しいイメージコーデックの拡張モデルです。 これらのアンマネージインターフェイスにより、コーデック開発者はコーデックを WPF と統合できるため、新しいイメージ形式を WPF アプリケーションで自動的に使用できるようになります。  
  
 機能拡張 API のサンプルについては、 [Win32 サンプルコーデック](https://go.microsoft.com/fwlink/?LinkID=160052)を参照してください。 このサンプルは、カスタム イメージ形式用のデコーダーとエンコーダーの作成方法を示しています。  
  
> [!NOTE]
> システムで認識するは、コーデックをデジタル署名する必要があります。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Media.Imaging.BitmapSource>
- <xref:System.Windows.Media.Imaging.BitmapImage>
- <xref:System.Windows.Controls.Image>
- <xref:System.Windows.Media.Imaging.BitmapMetadata>
- [2D グラフィックスとイメージング](../advanced/optimizing-performance-2d-graphics-and-imaging.md)
- [Win32 サンプル コーデック](https://go.microsoft.com/fwlink/?LinkID=160052)
