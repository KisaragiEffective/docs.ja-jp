---
title: F# を使用した Azure Blob Storage の概要
description: Azure Blob storage を使用して、非構造化データをクラウドに格納します。
author: sylvanc
ms.date: 09/20/2016
ms.openlocfilehash: 79f6a559ac603b0544916764126a988d3f3f43d7
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77092630"
---
# <a name="get-started-with-azure-blob-storage-using-f"></a>F\# を使用した Azure Blob storage の概要

Azure Blob Storage は、非構造化データをクラウド内にオブジェクト/BLOB として格納するサービスです。 Blob Storage は、ドキュメント、メディア ファイル、アプリケーション インストーラーなど、任意の種類のテキスト データやバイナリ データを格納できます。 Blob Storage は、オブジェクト ストレージとも呼ばれます。

この記事では、Blob storage を使用して一般的なタスクを実行する方法について説明します。 サンプルは、.NET 用F#の Azure Storage クライアントライブラリを使用して記述されています。 説明するタスクには、blob のアップロード、一覧表示、ダウンロード、および削除の方法が含まれます。

Blob storage の概念の概要については、「 [.net ガイド](/azure/storage/blobs/storage-quickstart-blobs-dotnet)」を参照してください。

## <a name="prerequisites"></a>前提条件

このガイドを使用するには、最初に[Azure ストレージアカウントを作成](/azure/storage/common/storage-account-create)する必要があります。 また、このアカウントのストレージアクセスキーも必要です。

## <a name="create-an-f-script-and-start-f-interactive"></a>F#スクリプトを作成してF#対話形式で起動する

この記事のサンプルは、 F#アプリケーションまたはF#スクリプトで使用できます。 F#スクリプトを作成するには、 F#開発環境で `blobs.fsx`などの `.fsx` 拡張機能を使用してファイルを作成します。

次に、[パケット](https://fsprojects.github.io/Paket/)や[NuGet](https://www.nuget.org/)などの[パッケージマネージャー](package-management.md)を使用して `WindowsAzure.Storage` をインストールし `Microsoft.WindowsAzure.ConfigurationManager` パッケージと参照 `WindowsAzure.Storage.dll` を `#r` ディレクティブを使用してスクリプトに `Microsoft.WindowsAzure.Configuration.dll` します。

### <a name="add-namespace-declarations"></a>名前空間宣言の追加

次の `open` ステートメントを `blobs.fsx` ファイルの先頭に追加します。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L1-L5)]

### <a name="get-your-connection-string"></a>接続文字列を取得する

このチュートリアルでは、Azure Storage 接続文字列が必要です。 接続文字列の詳細については、「[ストレージ接続文字列の構成](/azure/storage/storage-configure-connection-string)」を参照してください。

このチュートリアルでは、次のように、スクリプトに接続文字列を入力します。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L11-L11)]

ただし、実際のプロジェクトではこの方法は**お勧めできません**。 ストレージ アカウント キーは、ストレージ アカウントの root パスワードに似ています。 ストレージ アカウント キーは常に慎重に保護してください。 このキーを他のユーザーに配布したり、ハードコーディングしたり、他のユーザーがアクセスできるプレーン テキスト ファイルに保存したりしないでください。 侵害された可能性があると思われる場合は、Azure ポータルを使用してキーを再生成することができます。

実際のアプリケーションでは、ストレージ接続文字列を維持する最善の方法は構成ファイルにあります。 構成ファイルから接続文字列を取得するには、次の手順を実行します。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L13-L15)]

Azure Configuration Manager の使用はオプションです。 .NET Framework の `ConfigurationManager` の種類などの API を使用することもできます。

### <a name="parse-the-connection-string"></a>接続文字列を解析する

接続文字列を解析するには、次のように指定します。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L21-L22)]

`CloudStorageAccount`が返されます。

### <a name="create-some-local-dummy-data"></a>いくつかのローカルダミーデータを作成する

開始する前に、スクリプトのディレクトリにダミーのローカルデータを作成します。 後でこのデータをアップロードします。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L28-L30)]

### <a name="create-the-blob-service-client"></a>Blob service クライアントの作成

`CloudBlobClient` の種類を使用すると、Blob ストレージに格納されているコンテナーと blob を取得できます。 サービス クライアントを作成する方法の 1 つを次に示します。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L36-L36)]

これで、Blob Storage に対してデータの読み取りと書き込みを実行するコードを記述する準備が整いました。

## <a name="create-a-container"></a>コンテナーを作成する

この例は、コンテナーがない場合に、コンテナーを作成する方法を示しています。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L42-L46)]

既定では、新しいコンテナーはプライベートなので、このコンテナーから BLOB をダウンロードするにはストレージ アクセス キーを指定する必要があります。 コンテナー内のファイルをだれでも利用できるようにする場合は、次のコードを使ってコンテナーをパブリックに設定できます。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L48-L49)]

パブリック コンテナー内の BLOB は、インターネットに接続しているすべてのユーザーが表示できますが、変更または削除できるのは、適切なアカウント アクセス キーまたは Shared Access Signature を持っているユーザーだけです。

## <a name="upload-a-blob-into-a-container"></a>コンテナーに BLOB をアップロードする

Azure Blob Storage では、ブロック BLOB とページ BLOB がサポートされています。 ほとんどの場合、ブロック blob を使用することをお勧めします。

ファイルをブロック blob にアップロードするには、コンテナーの参照を取得し、それを使用してブロック blob の参照を取得します。 Blob の参照を取得したら、`UploadFromFile` メソッドを呼び出すことによって、データの任意のストリームをそれにアップロードできます。 この操作では、blob が既に存在していない場合は作成し、存在する場合は上書きします。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L55-L59)]

## <a name="list-the-blobs-in-a-container"></a>コンテナー内の BLOB を一覧表示する

コンテナー内の BLOB を一覧表示するには、まず、コンテナーの参照を取得します。 その後、コンテナーの `ListBlobs` メソッドを使用して、その中の blob やディレクトリを取得できます。 返された `IListBlobItem`の豊富なプロパティとメソッドのセットにアクセスするには、それを `CloudBlockBlob`、`CloudPageBlob`、または `CloudBlobDirectory` オブジェクトにキャストする必要があります。 型がわからない場合は、型チェックを使うとどれにキャストすればよいかがわかります。 次のコードは、 `mydata` コンテナー内の各アイテムの URI を取得して出力する方法を示しています。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L67-L80)]

Blob には、名前にパス情報を指定することもできます。 これで、従来のファイル システムと同じように、整理およびスキャン可能な仮想ディレクトリ構造が作成されます。 ディレクトリ構造は仮想のみであり、BLOB ストレージで使用できるリソースはコンテナーと BLOB のみであることに注意してください。 ただし、ストレージクライアントライブラリは、仮想ディレクトリを参照するための `CloudBlobDirectory` オブジェクトを提供し、この方法で構成された blob を操作するプロセスを簡略化します。

たとえば、 `photos`という名前のコンテナーに次の一連のブロック BLOB があったとします。

*photo1*\
*2015/architecture/description .txt*\
*2015/architecture/photo3*\
*2015/architecture/photo4*\
*2016/architecture/photo5*\
*2016/architecture/photo6*\
*2016/architecture/description .txt*\
*2016/photo7*\

コンテナーで `ListBlobs` を呼び出すと (上記のサンプルのように)、階層化された一覧が返されます。 コンテナー内のディレクトリと blob をそれぞれ表す `CloudBlobDirectory` オブジェクトと `CloudBlockBlob` オブジェクトの両方が含まれている場合、結果の出力は次のようになります。

```console
Directory: https://<accountname>.blob.core.windows.net/photos/2015/
Directory: https://<accountname>.blob.core.windows.net/photos/2016/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

必要に応じて、`ListBlobs` メソッドの `UseFlatBlobListing` パラメーターを `true`に設定できます。 この場合、コンテナー内のすべての blob が `CloudBlockBlob` オブジェクトとして返されます。 フラットリストを返す `ListBlobs` の呼び出しは次のようになります。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L82-L89)]

また、コンテナーの現在の内容によっては、結果は次のようになります。

```console
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2015/architecture/description.txt
Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2015/architecture/photo3.jpg
Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2015/architecture/photo4.jpg
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2016/architecture/description.txt
Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2016/architecture/photo5.jpg
Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2016/architecture/photo6.jpg
Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2016/photo7.jpg
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

## <a name="download-blobs"></a>BLOB をダウンロードする

Blob をダウンロードするには、まず blob の参照を取得し、次に `DownloadToStream` メソッドを呼び出します。 次の例では、`DownloadToStream` メソッドを使用して、ローカルファイルに永続化できるストリームオブジェクトに blob の内容を転送します。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L95-L101)]

`DownloadToStream` メソッドを使用して、blob の内容をテキスト文字列としてダウンロードすることもできます。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L103-L106)]

## <a name="delete-blobs"></a>BLOB を削除する

Blob を削除するには、まず blob の参照を取得してから、その blob の `Delete` メソッドを呼び出します。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L112-L116)]

## <a name="list-blobs-in-pages-asynchronously"></a>BLOB をページで非同期に一覧表示する

多数の BLOB を一覧表示する場合や、1 回の一覧表示操作で返される結果の数を制御する場合には、BLOB の一覧を結果のページで表示できます。 この例は、大きな結果のセットを返すために待機している間に実行がブロックされないように、結果をページで非同期に返す方法を示しています。

この例ではフラット blob の一覧を示していますが、`ListBlobsSegmentedAsync` メソッドの `useFlatBlobListing` パラメーターを `false`に設定して、階層化された一覧を作成することもできます。

このサンプルでは、`async` ブロックを使用して、非同期メソッドを定義します。 ``let!`` キーワードは、リストタスクが完了するまで、サンプルメソッドの実行を中断します。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L122-L160)]

次のように、この非同期ルーチンを使用できるようになりました。 まずダミーデータをアップロードします (このチュートリアルで既に作成したローカルファイルを使用します)。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L162-L166)]

ここで、ルーチンを呼び出します。 非同期操作を強制的に実行するには、`Async.RunSynchronously` を使用します。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L168-L168)]

## <a name="writing-to-an-append-blob"></a>追加 BLOB への書き込み

追加 BLOB は、ログ記録などの追加操作のために最適化されています。 ブロック BLOB のように、追加 BLOB はブロックで構成されますが、追加 BLOB に新しいブロックを追加する場合は常に BLOB の最後に追加されます。 追加 BLOB の既存のブロックは更新したり、削除することはできません。 追加 BLOB のブロック ID はブロック BLOB 用のため、公開されることはありません。

追加 BLOB 内の各ブロックは、最大 4 MB のサイズにすることができます。また追加 BLOB には最大 50,000 のブロックを含めることができます。 よって追加 BLOB の最大サイズは 195 GB (4 MB X 50,000 ブロック) よりも少し大きくなります。

次の例では、新しい追加 blob を作成し、データを追加して、単純なログ記録操作をシミュレートします。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L174-L203)]

3 つの BLOB タイプにおける違いの詳細については、「 [Understanding Block Blobs, Page Blobs, and Append Blobs (ブロック BLOB、ページ BLOB、追加 BLOB を理解する)](https://msdn.microsoft.com/library/azure/ee691964.aspx) 」をご覧ください。

## <a name="concurrent-access"></a>同時アクセス

複数のクライアントまたは複数のプロセス インスタンスからの BLOB への同時アクセスをサポートするには、**ETag** または**占有**を使います。

- **Etag** - BLOB またはコンテナーが別のプロセスによって変更されていることを検出する手段を提供します。

- **占有** - 一定期間、BLOB に対する排他的で更新可能な書き込みアクセスまたは削除アクセスを取得する手段を提供します。

詳細については、「 [Microsoft Azure Storage での同時実行の管理](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/)」を参照してください。

## <a name="naming-containers"></a>コンテナーの名前付け

Azure Storage のどの BLOB もコンテナーに格納する必要があります。 コンテナーは、BLOB 名の一部を形成しています。 たとえば、次の BLOB の URI のサンプルでは、 `mydata` がコンテナーの名前です。

- `https://storagesample.blob.core.windows.net/mydata/blob1.txt`
- `https://storagesample.blob.core.windows.net/mydata/photos/myphoto.jpg`

コンテナー名は有効な DNS 名で、次の名前規則に準拠している必要があります。

1. コンテナー名は英文字または数字で始まり、英文字、数字、ダッシュ (-) 文字のみを含めることができます。
1. すべてのダッシュ (-) 文字は、その直前または直後に文字または数字が使用されている必要があります。連続するダッシュ文字は、コンテナー名では使用できません。
1. コンテナー名の文字はすべて小文字である必要があります。
1. コンテナー名の長さは、3 ～ 63 文字にする必要があります。

コンテナーの名前は、常に小文字にする必要があります。 コンテナー名に大文字が含まれている場合や、コンテナーの名前付け規則の他の違反がある場合、400 エラー (無効な要求) が発生することがあります。

## <a name="managing-security-for-blobs"></a>BLOB のセキュリティの管理

既定では、Azure Storage はアカウント所有者へのアクセスを制限することによってデータのセキュリティを維持します。アカウントの所有者は、アカウント アクセス キーを所持している人です。 ストレージ アカウント内の BLOB データを共有する必要がある場合は、アカウント アクセス キーのセキュリティを損なわずに共有することが重要です。 また、ネットワーク経由の送信と Azure Storage でのセキュリティを確保するために、BLOB データを暗号化することもできます。

### <a name="controlling-access-to-blob-data"></a>BLOB データへのアクセスの制御

既定では、ストレージ アカウント内の BLOB データには、ストレージ アカウント所有者だけがアクセスできます。 Blob Storage に対する認証要求には、既定ではアカウント アクセス キーが必要です。 ただし、特定の blob データを他のユーザーが使用できるようにすることもできます。

### <a name="encrypting-blob-data"></a>BLOB データの暗号化

Azure Storage は、クライアントとサーバーの両方で blob データの暗号化をサポートしています。

## <a name="next-steps"></a>次のステップ

これで、Blob Storage の基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

### <a name="tools"></a>ツール

- [ F# Azurestoragetypeprovider](https://fsprojects.github.io/AzureStorageTypeProvider/)\
Blob F# 、テーブル、およびキュー Azure Storage 資産を探索し、それらに対して CRUD 操作を簡単に適用するために使用できる型プロバイダー。

- [Fsharp.core](https://github.com/fsprojects/FSharp.Azure.Storage)\
Microsoft Azure F# Table Storage サービスを使用するための API

- [Microsoft Azure Storage Explorer (MASE)](/azure/vs-azure-tools-storage-manage-with-storage-explorer)\
Windows、OS X、Linux で Azure Storage データを視覚的に操作できる Microsoft 製の無料のスタンドアロンアプリです。

### <a name="blob-storage-reference"></a>Blob Storage リファレンス

- [.NET 用 Azure Storage API](/dotnet/api/overview/azure/storage)
- [Azure Storage サービスの REST API リファレンス](/rest/api/storageservices/)

### <a name="related-guides"></a>関連ガイド

- [.NET 用の Azure Blob Storage サンプル](https://docs.microsoft.com/samples/azure-samples/storage-blob-dotnet-getting-started/storage-blob-dotnet-getting-started/)
- [AzCopy を使ってみる](/azure/storage/common/storage-use-azcopy-v10)
- [Azure Storage の接続文字列を構成する](/azure/storage/common/storage-configure-connection-string)
- [Azure のストレージ チーム ブログ](https://docs.microsoft.com/archive/blogs/windowsazurestorage/)
- [クイックスタート: .NET を使用してオブジェクトストレージに blob を作成する](/azure/storage/blobs/storage-quickstart-blobs-dotnet)
