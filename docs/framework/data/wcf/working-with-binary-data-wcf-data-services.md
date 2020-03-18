---
title: バイナリ データの操作 (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, binary data
- WCF Data Services, streams
ms.assetid: aeccc45c-d5c5-4671-ad63-a492ac8043ac
ms.openlocfilehash: aa3e58d559121aaca401e7b851a4b4fd8e7753cd
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75900845"
---
# <a name="working-with-binary-data-wcf-data-services"></a>バイナリ データの操作 (WCF Data Services)

WCF Data Services クライアントライブラリを使用すると、次のいずれかの方法で、Open Data Protocol (OData) フィードからバイナリデータを取得および更新できます。

- エンティティのプリミティブ型のプロパティとして。 メモリに容易に読み込むことができる小さいバイナリ データ オブジェクトを操作する場合は、この方法が適しています。 この場合、バイナリ プロパティは、データ モデルによって公開されるエンティティ プロパティです。データ サービスは、応答メッセージの Base-64 バイナリ エンコード XML としてバイナリ データをシリアル化します。

- 個別のバイナリ リソース ストリームとして。 写真、ビデオ、またはその他の種類のバイナリ エンコード データを表すバイナリ ラージ オブジェクト (BLOB) データにアクセスしたり、変更したりする場合は、この方法が適しています。

WCF Data Services は、OData で定義されている HTTP を使用したバイナリデータのストリーミングを実装します。 バイナリ データはこのメカニズムでは別のメディア リソースとして扱われますが、メディア リンク エントリと呼ばれるエンティティに関連します。 詳細については、[ストリーミング プロバイダー](streaming-provider-wcf-data-services.md)を参照してください。

> [!TIP]
> 写真を格納する OData サービスからバイナリイメージファイルをダウンロードする Windows Presentation Foundation (WPF) クライアントアプリケーションを作成する手順の例については、「 [Data Services ストリーミングプロバイダーシリーズ-パート 2: クライアントからのメディアリソースストリームへのアクセス](https://docs.microsoft.com/archive/blogs/astoriateam/data-services-streaming-provider-series-part-2-accessing-a-media-resource-stream-from-the-client)」を参照してください。 ブログ記事で取り上げているストリーミングフォトデータサービスのサンプルコードをダウンロードするには、GitHub の[ストリーミングフォトデータサービスのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/Streaming%20Photo%20OData%20Service%20Sample)を参照してください。

## <a name="entity-metadata"></a>エンティティ メタデータ

メディア リソース ストリームが関連付けられているエンティティは、データ サービス メタデータで `HasStream` 属性によって示されます。この属性は、メディア リンク エントリであるエンティティ型に適用されます。 次の例では、`PhotoInfo` エンティティは、`HasStream` 属性で示される、関連するメディアリソースを持つメディアリンクエントリです。

[!code-xml[Astoria Photo Streaming Service#HasStream](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_photo_streaming_service/xml/photodata.edmx#hasstream)]

このトピックのその他の例では、メディア リソース ストリームにアクセスし変更する方法を紹介します。 WCF Data Services クライアントライブラリを使用して .NET Framework クライアントアプリケーションでメディアリソースストリームを使用する方法の完全な例については、「[クライアントからメディアリソースストリームにアクセス](https://docs.microsoft.com/archive/blogs/astoriateam/data-services-streaming-provider-series-part-2-accessing-a-media-resource-stream-from-the-client)する」を参照してください。

## <a name="accessing-the-binary-resource-stream"></a>バイナリ リソース ストリームへのアクセス

WCF Data Services クライアントライブラリには、OData ベースのデータサービスからバイナリリソースストリームにアクセスするためのメソッドが用意されています。 メディア リソースをダウンロードするときには、メディア リソースの URI を使用することも、メディア リソース データ自体を含むバイナリ ストリームを取得することもできます。 メディア リソース データをバイナリ ストリームとしてアップロードすることもできます。

> [!TIP]
> 写真を格納する OData サービスからバイナリイメージファイルをダウンロードする Windows Presentation Foundation (WPF) クライアントアプリケーションを作成する手順の例については、「 [Data Services ストリーミングプロバイダーシリーズ-パート 2: クライアントからのメディアリソースストリームへのアクセス](https://docs.microsoft.com/archive/blogs/astoriateam/data-services-streaming-provider-series-part-2-accessing-a-media-resource-stream-from-the-client)」を参照してください。 ブログ記事で取り上げているストリーミングフォトデータサービスのサンプルコードをダウンロードするには、GitHub の[ストリーミングフォトデータサービスのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/Streaming%20Photo%20OData%20Service%20Sample)を参照してください。

### <a name="getting-the-uri-of-the-binary-stream"></a>バイナリ ストリームの URI の取得

画像やその他のメディア ファイルなど、取得するメディア リソースの種類によっては、アプリケーションでメディア リソースの URI を使用する方がバイナリ データ ストリーム自体を処理するよりも簡単です。 特定のメディア リンク エントリに関連付けられているリソース ストリームの URI を取得するには、そのエンティティを追跡している <xref:System.Data.Services.Client.DataServiceContext.GetReadStreamUri%2A> インスタンスの <xref:System.Data.Services.Client.DataServiceContext> メソッドを呼び出す必要があります。 次の例は、クライアントで新しい画像を作成するために使用するメディア リソース ストリームの URI を取得するために <xref:System.Data.Services.Client.DataServiceContext.GetReadStreamUri%2A> メソッドを呼び出す方法を示しています。

[!code-csharp[Astoria Photo Streaming Client#GetReadStreamUri](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_photo_streaming_client/cs/photowindow.xaml.cs#getreadstreamuri)]
[!code-vb[Astoria Photo Streaming Client#GetReadStreamUri](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_photo_streaming_client/vb/photowindow.xaml.vb#getreadstreamuri)]

### <a name="downloading-the-binary-resource-stream"></a>バイナリ リソース ストリームのダウンロード

バイナリ リソース ストリームを取得する場合、メディア リンク エントリを追跡している <xref:System.Data.Services.Client.DataServiceContext.GetReadStream%2A> インスタンスの <xref:System.Data.Services.Client.DataServiceContext> メソッドを呼び出す必要があります。 このメソッドは、<xref:System.Data.Services.Client.DataServiceStreamResponse> オブジェクトを返すデータ サービスに要求を送信します。このオブジェクトは、リソースが含まれるストリームを参照します。 アプリケーションでバイナリ リソースを <xref:System.IO.Stream> として取得する必要がある場合にこのメソッドを使用します。 次の例は、クライアントで新しい画像を作成するために使用するストリームを取得するために <xref:System.Data.Services.Client.DataServiceContext.GetReadStream%2A> メソッドを呼び出す方法を示しています。

[!code-csharp[Astoria Streaming Client#GetReadStreamClient](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_streaming_client/cs/customerphotowindow.xaml.cs#getreadstreamclient)]
[!code-vb[Astoria Streaming Client#GetReadStreamClient](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_streaming_client/vb/customerphotowindow.xaml.vb#getreadstreamclient)]

> [!NOTE]
> バイナリ ストリームを含む応答メッセージの Content-Length ヘッダーは、このデータ サービスによって設定されません。 この値は、バイナリ データ ストリームの実際の長さを反映していない場合があります。

### <a name="uploading-a-media-resource-as-a-stream"></a>ストリームとしてのメディア リソースのアップロード

メディア リソースを挿入または更新するには、エンティティを追跡している <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> インスタンスの <xref:System.Data.Services.Client.DataServiceContext> メソッドを呼び出します。 このメソッドは、指定ストリームから読み込まれたメディア リソースを含むデータ サービスに要求を送信します。 次の例は、<xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> メソッドを呼び出してデータ サービスに画像を送信する方法を示しています。

[!code-csharp[Astoria Photo Streaming Client#SetSaveStream](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_photo_streaming_client/cs/photodetailswindow.xaml.cs#setsavestream)]
[!code-vb[Astoria Photo Streaming Client#SetSaveStream](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_photo_streaming_client/vb/photodetailswindow.xaml.vb#setsavestream)]

この例では、<xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> パラメーターに値 `true` を指定して `closeStream` メソッドを呼び出しています。 したがって、必ず、バイナリ データがデータ サービスにアップロードされた後で <xref:System.Data.Services.Client.DataServiceContext> がストリームを閉じます。

> [!NOTE]
> <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> を呼び出すときには、<xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> が呼び出されるまでストリームはデータ サービスに送信されないことに注意してください。

## <a name="see-also"></a>関連項目

- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
- [コントロールへのデータのバインド](binding-data-to-controls-wcf-data-services.md)
