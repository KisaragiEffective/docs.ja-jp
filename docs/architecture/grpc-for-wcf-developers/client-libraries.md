---
title: gRPC クライアント ライブラリを作成する - gRPC WCF 開発者向け
description: gRPC サービス用の共有クライアントライブラリ/パッケージの説明
ms.date: 09/02/2019
ms.openlocfilehash: bb58cb3cda4b0cbb3a5d34129961349bcb0093e9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79401773"
---
# <a name="create-grpc-client-libraries"></a>gRPC クライアント ライブラリの作成

gRPC アプリケーション用のクライアントライブラリを配布する必要はありません。 組織内のファイルの共有ライブラリを`.proto`作成でき、他のチームは、それらのファイルを使用して、独自のプロジェクトでクライアント コードを生成できます。 ただし、プライベートな NuGet リポジトリがあり、他の多くのチームが .NET Core を使用している場合は、クライアントの NuGet パッケージを作成してサービス プロジェクトの一部として発行できます。 これは、サービスを共有し、促進するための良い方法です。

クライアント ライブラリを配布する利点の 1 つは、便利な "便利な" メソッドとプロパティを使用して、生成された gRPC クラスと Protobuf クラスを強化できることです。 クライアント コードでは、サーバーと同様に、すべてのクラスが として`partial`宣言されるため、生成されたコードを編集せずに拡張できます。 つまり、基本型にコンストラクター、メソッド、および計算プロパティを簡単に追加できます。

> [!CAUTION]
> カスタム コードを使用して、重要な機能を提供しないでください。 共有ライブラリを使用する .NET チームに対してこの重要な機能を制限したり、Python や Java などの他の言語やプラットフォームを使用するチームに提供したりすることは望ましくない。

できるだけ多くのチームが gRPC サービスにアクセスできることを確認します。 これを行う最善の方法は、開発者`.proto`が独自のクライアントを生成できるようにファイルを共有することです。 これは、異なるチームが異なるプログラミング言語やフレームワークを頻繁に使用するマルチプラットフォーム環境や、API が外部からアクセス可能な場合に特に当てはまります。

## <a name="useful-extensions"></a>便利な拡張機能

NET には、オブジェクトのストリームを処理するためによく使用されるインターフェイスが<xref:System.Collections.Generic.IEnumerable%601>2<xref:System.IObservable%601>つあります。 NET Core 3.0 および C# 8.0 以降、<xref:System.Collections.Generic.IAsyncEnumerable%601>ストリームを非同期に処理するためのインターフェイスと、`await foreach`インターフェイスを使用するための構文があります。 このセクションでは、これらのインターフェイスを gRPC ストリームに適用するための再利用可能なコードを示します。

NET Core gRPC クライアント ライブラリには、`ReadAllAsync``IAsyncStreamReader<T>``IAsyncEnumerable<T>`インターフェイスを作成するための拡張メソッドがあります。 リアクティブ プログラミングを使用する開発者の場合、インターフェイスを作成するための`IObservable<T>`同等の拡張メソッドは、次のセクションの例のようになります。

### <a name="iobservable"></a>Iobservable

インターフェイス`IObservable<T>`は の "反応" 逆です`IEnumerable<T>`。 ストリームから項目をプルするのではなく、リアクティブな方法で、ストリームがサブスクライバーに項目をプッシュできます。 これは gRPC ストリームと非常によく似ており、`IObservable<T>`インターフェイスを囲むのは簡単です`IAsyncStreamReader<T>`。

C# には、監視`IAsyncEnumerable<T>`可能な処理に対する組み込みのサポートがないため、このコードはコードよりも長くなります。 実装クラスを手動で作成する必要があります。 しかし、これはジェネリッククラスなので、すべての型で単一の実装が機能します。

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using System.Threading.Tasks;

namespace Grpc.Core
{
    public class GrpcStreamObservable<T> : IObservable<T>
    {
        private readonly IAsyncStreamReader<T> _reader;
        private readonly CancellationToken _token;
        private int _used;

        public Observable(IAsyncStreamReader<T> reader, CancellationToken token = default)
        {
            _reader = reader;
            _token = token;
            _used = 0;
        }

        public IDisposable Subscribe(IObserver<T> observer) =>
            Interlocked.Exchange(ref _used, 1) == 0
                ? new GrpcStreamSubscription(_reader, observer, _token)
                : throw new InvalidOperationException("Subscribe can only be called once.");

    }
}
```

> [!IMPORTANT]
> この観察可能な実装では`Subscribe`、ストリームから読み取ろうとする複数のサブスクライバを持つと混乱が生じるため、メソッドを 1 回だけ呼び出すことができます。 `Replay` [System.Reactive.Linq](https://www.nuget.org/packages/System.Reactive.Linq)など、この実装で使用できる監視可能なバッファリングと反復可能な共有を可能にする演算子があります。

クラス`GrpcStreamSubscription`は、 の列挙体`IAsyncStreamReader`を処理します。

```csharp
public class GrpcStreamSubscription : IDisposable
{
    private readonly Task _task;
    private readonly CancellationTokenSource _tokenSource;
    private bool _completed;

    public Subscription(IAsyncStreamReader<T> reader, IObserver<T> observer, CancellationToken token)
    {
        Debug.Assert(reader != null);
        Debug.Assert(observer != null);
        _tokenSource = new CancellationTokenSource();
        token.Register(_tokenSource.Cancel);
        _task = Run(reader, observer, _tokenSource.Token);
    }

    private async Task Run(IAsyncStreamReader<T> reader, IObserver<T> observer, CancellationToken token)
    {
        while (!token.IsCancellationRequested)
        {
            try
            {
                if (!await reader.MoveNext(token)) break;
            }
            catch (RpcException e) when (e.StatusCode == Grpc.Core.StatusCode.NotFound)
            {
                break;
            }
            catch (OperationCanceledException)
            {
                break;
            }
            catch (Exception e)
            {
                observer.OnError(e);
                _completed = true;
                return;
            }

            observer.OnNext(reader.Current);
        }

        _completed = true;
        observer.OnCompleted();
    }

    public void Dispose()
    {
        if (!_completed && !_tokenSource.IsCancellationRequested)
        {
            _tokenSource.Cancel();
        }

        _tokenSource.Dispose();
        _task.Dispose();
    }

}
```

ここで必要なのは、ストリーム リーダーから監視可能なオブジェクトを作成するための単純な拡張メソッドです。

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using System.Threading.Tasks;

namespace Grpc.Core
{
    public static class AsyncStreamReaderObservableExtensions
    {
        public static IObservable<T> AsObservable<T>(this IAsyncStreamReader<T> reader) =>
            new GrpcStreamObservable<T>(reader);
    }
}
```

## <a name="summary"></a>まとめ

`IAsyncEnumerable`および`IObservable`モデルは、よくサポートされ、よく文書化された方法で、.NET の非同期データ ストリームを処理します。 gRPC ストリームは両方のパラダイムに適切に対応し、.NET Core との密接な統合と、反応型および非同期プログラミング スタイルを提供します。

>[!div class="step-by-step"]
>[前次](streaming-versus-repeated.md)
>[Next](security.md)
