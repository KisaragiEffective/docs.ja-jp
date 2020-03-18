---
title: F# を使用した Azure Queue Storage の概要
description: Azure Queue は、アプリケーション コンポーネント間の信頼性の高い非同期メッセージングを提供します。 クラウド メッセージングにより、アプリケーション コンポーネントのスケールを個別に変更できます。
author: sylvanc
ms.date: 09/20/2016
ms.openlocfilehash: 841068ac91aecc53811359e27d984907569a2c6d
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75935491"
---
# <a name="get-started-with-azure-queue-storage-using-f"></a>F\# を使用した Azure Queue storage の概要

Azure Queue Storage は、アプリケーション コンポーネント間のクラウド メッセージングを提供します。 拡張性を重視してアプリケーションを設計する場合、通常、アプリケーション コンポーネントを個別に拡張できるように分離します。 Queue Storage は、アプリケーション コンポーネントがクラウド、デスクトップ、オンプレミスのサーバー、モバイル デバイスのいずれで実行されている場合でも、アプリケーション コンポーネント間の通信に非同期メッセージングを提供します。 Queue Storage ではまた、非同期タスクの管理とプロセス ワークフローの構築もサポートします。

### <a name="about-this-tutorial"></a>このチュートリアルについて

このチュートリアルでは、Azure F# Queue storage を使用していくつかの一般的なタスクのコードを記述する方法について説明します。 キューの作成と削除、キューメッセージの追加、読み取り、削除などのタスクについて説明します。

Queue storage の概念の概要については、「 [.net のキューストレージに関するガイド](/azure/storage/storage-dotnet-how-to-use-queues)」を参照してください。

## <a name="prerequisites"></a>[前提条件]

このガイドを使用するには、最初に[Azure ストレージアカウントを作成](/azure/storage/storage-create-storage-account)する必要があります。
また、このアカウントのストレージアクセスキーも必要になります。

## <a name="create-an-f-script-and-start-f-interactive"></a>F#スクリプトを作成してF#対話形式で起動する

この記事のサンプルは、 F#アプリケーションまたはF#スクリプトで使用できます。 F#スクリプトを作成するには、 F#開発環境で `queues.fsx`などの `.fsx` 拡張機能を使用してファイルを作成します。

次に、[パケット](https://fsprojects.github.io/Paket/)や[NuGet](https://www.nuget.org/)などの[パッケージマネージャー](package-management.md)を使用して、`#r` ディレクティブを使用してスクリプトに `WindowsAzure.Storage` パッケージと参照 `WindowsAzure.Storage.dll` をインストールします。

### <a name="add-namespace-declarations"></a>名前空間宣言を追加する

次の `open` ステートメントを `queues.fsx` ファイルの先頭に追加します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L1-L3)]

### <a name="get-your-connection-string"></a>接続文字列を取得する

このチュートリアルでは、Azure Storage 接続文字列が必要です。 接続文字列の詳細については、「[ストレージ接続文字列の構成](/azure/storage/storage-configure-connection-string)」を参照してください。

このチュートリアルでは、次のように、スクリプトに接続文字列を入力します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L9-L9)]

ただし、実際のプロジェクトではこの方法は**お勧めできません**。 ストレージ アカウント キーは、ストレージ アカウントの root パスワードに似ています。 ストレージ アカウント キーは常に慎重に保護してください。 このキーを他のユーザーに配布したり、ハードコーディングしたり、他のユーザーがアクセスできるプレーン テキスト ファイルに保存したりしないでください。 侵害された可能性があると思われる場合は、Azure ポータルを使用してキーを再生成することができます。

実際のアプリケーションでは、ストレージ接続文字列を維持する最善の方法は構成ファイルにあります。 構成ファイルから接続文字列を取得するには、次の手順を実行します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L11-L13)]

Azure Configuration Manager の使用はオプションです。 .NET Framework の `ConfigurationManager` の種類などの API を使用することもできます。

### <a name="parse-the-connection-string"></a>接続文字列を解析する

接続文字列を解析するには、次のように指定します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L19-L20)]

これにより、`CloudStorageAccount`が返されます。

### <a name="create-the-queue-service-client"></a>Queue サービス クライアントを作成する

`CloudQueueClient` クラスを使用すると、Queue storage に格納されているキューを取得できます。 サービス クライアントを作成する方法の 1 つを次に示します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L26-L26)]

これで、Queue Storage に対してデータの読み取りと書き込みを実行するコードを記述する準備が整いました。

## <a name="create-a-queue"></a>キューを作成する

この例では、キューが存在しない場合に作成する方法を示します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L32-L36)]

## <a name="insert-a-message-into-a-queue"></a>メッセージをキューに挿入する

既存のキューにメッセージを挿入するには、最初に新しい `CloudQueueMessage`を作成します。 次に、`AddMessage` メソッドを呼び出します。 `CloudQueueMessage` は、次のように、文字列 (UTF-8 形式) または `byte` 配列から作成できます。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L42-L44)]

## <a name="peek-at-the-next-message"></a>次のメッセージをピークする

`PeekMessage` メソッドを呼び出すことにより、キューの先頭にあるメッセージをキューから削除せずにピークできます。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L50-L52)]

## <a name="get-the-next-message-for-processing"></a>処理する次のメッセージを取得します。

`GetMessage` メソッドを呼び出すことにより、キューの先頭にあるメッセージを処理のために取得できます。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L58-L59)]

後で `DeleteMessage`を使用して、メッセージの処理が成功したことを示します。

## <a name="change-the-contents-of-a-queued-message"></a>キューに配置されたメッセージの内容を変更する

取得したメッセージの内容をキュー内のインプレースで変更できます。 メッセージが作業タスクを表している場合は、この機能を使用して、作業タスクの状態を更新できます。 次のコードでは、キュー メッセージを新しい内容に更新し、表示タイムアウトを設定して、60 秒延長します。 これにより、メッセージに関連付けられている作業の状態が保存され、クライアントにメッセージの操作を続行する時間が 1 分与えられます。 この方法を使用すると、キュー メッセージに対する複数の手順から成るワークフローを追跡でき、ハードウェアまたはソフトウェアの問題が原因で処理手順が失敗した場合に最初からやり直す必要がなくなります。 通常は、再試行回数も保持します。また、メッセージが何回も再試行される場合は、削除します。 こうすることで、処理するたびにアプリケーション エラーをトリガーするメッセージから保護されます。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L65-L69)]

## <a name="de-queue-the-next-message"></a>次のメッセージをデキューする

コードでは、2 つの手順でキューからメッセージをデキューします。 `GetMessage`を呼び出すと、キュー内の次のメッセージが取得されます。 `GetMessage` から返されたメッセージは、このキューからメッセージを読み取る他のコードから参照できなくなります。 既定では、このメッセージを参照できない状態は 30 秒間続きます。 キューからのメッセージの削除を完了するには、`DeleteMessage`も呼び出す必要があります。 このようにメッセージを 2 つの手順で削除することで、ハードウェアまたはソフトウェアの問題が原因でコードによるメッセージの処理が失敗した場合に、コードの別のインスタンスで同じメッセージを取得し、もう一度処理することができます。 ご自分のコードで、メッセージが処理された直後に `DeleteMessage` を呼び出します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L75-L76)]

## <a name="use-async-workflows-with-common-queue-storage-apis"></a>一般的な Queue storage Api での非同期ワークフローの使用

この例では、一般的な Queue storage Api で非同期ワークフローを使用する方法を示します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L82-L91)]

## <a name="additional-options-for-de-queuing-messages"></a>メッセージのデキュー用の追加オプション

キューからのメッセージの取得をカスタマイズする方法は 2 つあります。
1 つ目の方法では、(最大 32 個の) メッセージのバッチを取得できます。 2 つ目の方法では、コードで各メッセージを完全に処理できるように、非表示タイムアウトの設定を長くまたは短くすることができます。 次のコード例では、`GetMessages` を使用して1回の呼び出しで20個のメッセージを取得し、各メッセージを処理します。 また、各メッセージの非表示タイムアウトを 5 分に設定します。 5分がすべてのメッセージに対して同時に開始されるため、`GetMessages`の呼び出しから5分が経過すると、削除されていないすべてのメッセージが再び表示されることに注意してください。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L97-L99)]

## <a name="get-the-queue-length"></a>キューの長さを取得する

キュー内のメッセージの概数を取得できます。 `FetchAttributes` メソッドは、メッセージ数を含むキューの属性を取得するように Queue サービスに要求します。 `ApproximateMessageCount` プロパティは、Queue サービスを呼び出さずに、`FetchAttributes` メソッドによって取得された最後の値を返します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L105-L106)]

## <a name="delete-a-queue"></a>キューの削除

キューおよびキューに格納されているすべてのメッセージを削除するには、キューオブジェクトの `Delete` メソッドを呼び出します。

[!code-fsharp[QueueStorage](~/samples/snippets/fsharp/azure/queue-storage.fsx#L112-L113)]

## <a name="next-steps"></a>次のステップ:

これで、Queue ストレージの基本を学習できました。さらに複雑なストレージ タスクを実行するには、次のリンク先を参照してください。

- [.NET 用 Azure Storage API](/dotnet/api/overview/azure/storage)
- [Azure Storage 型プロバイダー](https://github.com/fsprojects/AzureStorageTypeProvider)
- [Azure のストレージ チーム ブログ](https://docs.microsoft.com/archive/blogs/windowsazurestorage/)
- [Azure Storage の接続文字列を構成する](/azure/storage/common/storage-configure-connection-string)
- [Azure Storage サービスの REST API リファレンス](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference)
