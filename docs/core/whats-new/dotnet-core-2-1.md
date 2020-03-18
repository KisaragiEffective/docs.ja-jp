---
title: .NET Core 2.1 の新機能
description: .NET Core 2.1 の新機能について。
dev_langs:
- csharp
- vb
ms.date: 10/10/2018
ms.openlocfilehash: 32784f7d4b9e3a93eb7f81b4829b39c1a06ef949
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76920393"
---
# <a name="whats-new-in-net-core-21"></a>.NET Core 2.1 の新機能

.NET Core 2.1 には、次の分野の拡張機能と新機能が含まれます。

- [ツール](#tooling)
- [ロールフォワード](#roll-forward)
- [配置](#deployment)
- [Windows 互換機能パック](#windows-compatibility-pack)
- [JIT コンパイルの改良](#jit-compiler-improvements)
- [API の変更](#api-changes)

## <a name="tooling"></a>ツール

.NET Core 2.1 に付属のツールである .NET Core 2.1 SDK (v 2.1.300) には次の変更点と拡張機能が含まれています。

### <a name="build-performance-improvements"></a>ビルドのパフォーマンスの向上

.NET Core 2.1 の大きな注目点はビルド時 (特にインクリメンタル ビルド) のパフォーマンスの向上です。 このようなパフォーマンスの向上は、`dotnet build` を使用したコマンドライン ビルドと Visual Studio でのビルドの両方に適用されます。 各分野の向上点の一部を次に示します。

- (パッケージ資産の解決) すべての資産ではなく、ビルドで使用される資産のみ解決する。

- アセンブリ参照のキャッシュ。

- 長時間にわたる SDK ビルド サーバー (`dotnet build` の個々の呼び出しにまたがるプロセス) の使用。 これにより、`dotnet build` が実行されるたびに大きなコード ブロックを JIT コンパイルする必要がなくなります。 ビルド サーバー プロセスは、次のコマンドで自動的に終了することができます。

   ```dotnetcli
   dotnet buildserver shutdown
   ```

### <a name="new-cli-commands"></a>新しい CLI コマンド

[`DotnetCliToolReference`](../tools/extensibility.md) を使用してプロジェクト単位でのみ入手可能であった複数のツールを .NET Core SDK の一部として入手できるようになりました。 それらのツールを以下に示します。

- `dotnet watch` は、ファイルの変更を待機してから、指定された一連のコマンドを実行するファイル システム ウォッチャーを提供します。 たとえば、次のコマンドは現在のプロジェクト内のファイルが変更されるたびにプロジェクトを自動的にリビルドして、詳細な出力を生成します。

   ```dotnetcli
   dotnet watch -- --verbose build
   ```

   `--verbose` オプションの前にある `--` オプションに注目してください。 これは、子プロセスである `dotnet` に渡される引数から `dotnet watch` コマンドに直接渡されるオプションを区切るものです。 このオプションを使用しないと、`--verbose` オプションが `dotnet build` コマンドではなく `dotnet watch` コマンドに適用されます。
  
   詳細については、「[dotnet watch を使用した ASP.NET Core アプリの開発](/aspnet/core/tutorials/dotnet-watch)」を参照してください。

- `dotnet dev-certs` は、ASP.NET Core アプリケーションでの開発時に使用される証明書を生成および管理します。

- `dotnet user-secrets` は、ASP.NET Core アプリケーションでユーザー シークレット ストアのシークレットを管理します。

- `dotnet sql-cache` は、分散キャッシュに使用する Microsoft SQL Server データベースでテーブルとインデックスを作成します。

- `dotnet ef` は、データベース、<xref:Microsoft.EntityFrameworkCore.DbContext> オブジェクト、および Entity Framework Core アプリケーションにおける移行を管理するためのツールです。 詳細については、「[EF Core .NET Command-line Tools](/ef/core/miscellaneous/cli/dotnet)」(EF Core .NET コマンドライン ツール) を参照してください。

### <a name="global-tools"></a>グローバル ツール

.NET Core 2.1 では、*グローバル ツール* (コマンドラインからグローバルに使用できるカスタム ツール) がサポートされます。 以前のバージョンの .NET Core の拡張モデルでは、[`DotnetCliToolReference`](../tools/extensibility.md#consuming-per-project-tools) を使用してカスタム ツールをプロジェクト単位で入手することのみ可能でした。

グローバル ツールをインストールするには、[dotnet tool install](../tools/dotnet-tool-install.md) コマンドを使用します。 次に例を示します。

```dotnetcli
dotnet tool install -g dotnetsay
```

インストールしたツールは、ツール名を指定してコマンドラインから実行できます。 詳細については、[.NET core グローバル ツールの概要](../tools/global-tools.md) を参照してください。

### <a name="tool-management-with-the-dotnet-tool-command"></a>`dotnet tool` コマンドによるツールの管理

.NET Core 2.1 SDK では、すべてのツール操作で `dotnet tool` コマンドが使用されます。 次のオプションを使用できます。

- [`dotnet tool install`](../tools/dotnet-tool-install.md): ツールをインストールします。

- [`dotnet tool update`](../tools/dotnet-tool-update.md): ツールをアンインストールおよび再インストールします。これにより、ツールが効果的に更新されます。

- [`dotnet tool list`](../tools/dotnet-tool-list.md): 現在インストールされているツールの一覧を表示します。

- [`dotnet tool uninstall`](../tools/dotnet-tool-uninstall.md): 現在インストールされているツールをアンインストールします。

## <a name="roll-forward"></a>ロールフォワード

.NET Core 2.0 以降のすべての .NET Core アプリケーションは、システムにインストールされている最新の*マイナー バージョン*に自動的にロールフォワードされます。

.NET Core 2.0 以降では、アプリケーションのビルドに使用した .NET Core のバージョンが実行時に存在しない場合、インストールされている .NET Core の最新の*マイナー バージョン*に対してアプリケーションが自動的に実行されます。 つまり、アプリケーションが .NET Core 2.0 を使用してビルドされ、ホスト システムにインストールされているのが .NET Core 2.0 ではなく .NET Core 2.1 である場合は、.NET Core 2.1 を使用してアプリケーションが実行されます。

> [!IMPORTANT]
> このロールフォワードの動作はプレビュー リリースには適用されません。 既定では、メジャー リリースにも適用されませんが、次の設定で変更できます。

共有フレームワークの候補なしでロール フォワードの設定を変更することで、この動作を変更できます。 使用可能な設定は次のとおりです。

- `0` - マイナー バージョンのロールフォワード動作を無効にします。 この設定では、.NET Core 2.0.0 用にビルドされたアプリケーションが、.NET Core 2.2.0 または .NET Core 3.0.0 ではなく、.NET Core 2.0.1 にロール フォワードされます。
- `1` - マイナー バージョンのロールフォワード動作を有効にします。 これが設定の既定値です。 この設定では、.NET Core 2.0.0 用にビルドされたアプリケーションが、インストールされているバージョンに応じて、.NET Core 2.0.1 または .NET Core 2.2.0 のいずれかにロール フォワードされますが、.NET Core 3.0.0 にはロール フォワードされません。
- `2` - マイナー バージョンとメジャー バージョンのロールフォワード動作を有効にします。 設定すると、異なるメジャー バージョンも考慮されるため、.NET Core 2.0.0 用にビルドされたアプリケーションは、.NET Core 3.0.0 にロール フォワードされます。

この設定は、次の 3 つのいずれかの方法で変更できます。

- `DOTNET_ROLL_FORWARD_ON_NO_CANDIDATE_FX` 環境変数を目的の値に設定します。

- 次の行を、目的の値を指定して *.runtimeconfig.json* ファイルに追加します。

   ```json
   "rollForwardOnNoCandidateFx" : 0
   ```

- [.NET Core CLI](../tools/index.md) を使用している場合、次のオプションに必要な値を指定して .NET Core コマンド (`run` など) に追加します。

   ```dotnetcli
   dotnet run --rollForwardOnNoCandidateFx=0
   ```

修正プログラムのバージョンのロール フォワードは、この設定からは独立しており、任意の潜在的なマイナー バージョンまたはメジャー バージョンのロール フォワードが適用された後に行われます。

## <a name="deployment"></a>配置

### <a name="self-contained-application-servicing"></a>自己完結型アプリケーションのサービス提供

`dotnet publish` は、サービス提供されたランタイム バージョンを含む自己完結型アプリケーションをパブリッシュするようになりました。 .NET Core 2.1 SDK (v 2.1.300) を含む自己完結型アプリケーションをパブリッシュする場合、その SDK で認識済みである最新のサービス提供されたランタイム バージョンがアプリケーションに含まれます。 最新の SDK にアップグレードする場合は、最新の .NET Core ランタイム バージョンでパブリッシュします。 これに該当するのは .NET Core 1.0 以降のランタイムです。

自己完結型のパブリッシュは、NuGet.org のランタイム バージョンに依存します。マシンにはサービス提供されたランタイムは必要ありません。

.NET Core 2.0 SDK を使用すると、`RuntimeFrameworkVersion` プロパティで別のバージョンが指定されている場合を除き、自己完結型アプリケーションが .NET Core 2.0.0 ランタイムでパブリッシュされます。 この新しい動作により、このプロパティを設定して、より新しいバージョンのランタイムを自己完結型アプリケーションに対して選択する必要がなくなりました。 今後は常に .NET Core 2.1 SDK (v 2.1.300) でパブリッシュするのが最も簡単な方法です。

詳細については、「[自己完結型展開ランタイムのロール フォワード](../deploying/runtime-patch-selection.md)」をご覧ください。
## <a name="windows-compatibility-pack"></a>Windows 互換機能パック

既存のコードを .NET Framework から .NET Core に移植する場合は、[Windows 互換機能パック](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)を使用できます。 この Windows 互換機能パックでは、.NET Core よりも 20,000 個も多い API にアクセスできます。 これらの API には、<xref:System.Drawing?displayProperty=nameWithType> 名前空間の型、<xref:System.Diagnostics.EventLog> クラス、WMI、パフォーマンス カウンター、Windows サービス、および Windows レジストリの型とメンバーが含まれています。

## <a name="jit-compiler-improvements"></a>JIT コンパイラの機能強化

.NET Core には*階層型コンパイル* (*適応型最適化*とも呼ばれる) という新しい JIT コンパイラ テクノロジが組み込まれており、パフォーマンスが大幅に向上します。 階層型コンパイルはオプトイン設定です。

JIT コンパイラで実行される重要なタスクの 1 つはコード実行の最適化です。 ただし、使用頻度の低いコード パスについては、最適化されていないコードの実行にランタイムが費やす時間よりも、コードの最適化にコンパイラが費やす時間の方が多くなる場合があります。 階層型コンパイルによって JIT コンパイルに次の 2 つのステージが導入されます。

- **第 1 階層**: コードをできるだけ早く生成します。

- **第 2 階層**: 頻繁に実行されるメソッドに対して、最適化されたコードを生成します。 コンパイルの第 2 階層は、パフォーマンスの向上と並行して実行されます。

次に示す 2 つのいずれかの方法で階層型コンパイルをオプトインできます。

- .NET Core 2.1 SDK を使用するすべてのプロジェクトで階層型コンパイルを使用するには、次の環境変数を設定します。

  ```console
  COMPlus_TieredCompilation="1"
  ```

- プロジェクト単位で階層型コンパイルを使用するには、次の例に示すように、MSBuild プロジェクト ファイルの `<PropertyGroup>` セクションに `<TieredCompilation>` プロパティを追加します。

   ```xml
   <PropertyGroup>
      <!-- other property definitions -->

      <TieredCompilation>true</TieredCompilation>
   </PropertyGroup>
   ```

## <a name="api-changes"></a>API の変更

### <a name="spant-and-memoryt"></a>`Span<T>` および `Memory<T>`

.NET Core 2.1 には、配列とその他の種類のメモリを使用する作業を大幅に効率的に行えるようにするいくつかの新しい型が含まれています。 新しい型は次のとおりです。

- <xref:System.Span%601?displayProperty=nameWithType> および <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>。

- <xref:System.Memory%601?displayProperty=nameWithType> および <xref:System.ReadOnlyMemory%601?displayProperty=nameWithType>。

これらの型を使用しない場合は、アレイの一部やメモリ バッファーのセクションなどの項目を渡すときに、一部のデータのコピーを作成してからメソッドに渡す必要があります。 これらの型はデータを仮想的に表示します。これにより、追加メモリの割り当てとコピーの操作の必要性が排除されます。

次の例では、<xref:System.Span%601> および <xref:System.Memory%601> インスタンスを使用して配列の 10 個の要素を仮想的に表示します。

[!code-csharp[Span\<T>](~/samples/core/whats-new/whats-new-in-21/cs/program.cs)]

[!code-vb[Memory\<T>](~/samples/core/whats-new/whats-new-in-21/vb/program.vb)]

### <a name="brotli-compression"></a>Brotli 圧縮

.NET Core 2.1 には、Brotli 圧縮と展開のサポートが追加されています。 Brotli は、[RFC 7932](https://www.ietf.org/rfc/rfc7932.txt) で定義されており、ほとんどの Web ブラウザーと主要な Web サーバーでサポートされている汎用の無損失圧縮アルゴリズムです。 ストリーム ベースの <xref:System.IO.Compression.BrotliStream?displayProperty=nameWithType> クラスまたは高性能なスパン ベースの <xref:System.IO.Compression.BrotliEncoder?displayProperty=nameWithType> クラスと <xref:System.IO.Compression.BrotliDecoder?displayProperty=nameWithType> クラスを使用できます。 次の例は、<xref:System.IO.Compression.BrotliStream> クラスによる圧縮を示しています。

[!code-csharp[Brotli compression](~/samples/core/whats-new/whats-new-in-21/cs/brotli.cs#1)]

[!code-vb[Brotli compression](~/samples/core/whats-new/whats-new-in-21/vb/brotli.vb#1)]

<xref:System.IO.Compression.BrotliStream> の動作は、<xref:System.IO.Compression.DeflateStream> および <xref:System.IO.Compression.GZipStream> と同じです。そのため、これらの API を呼び出すコードを簡単に <xref:System.IO.Compression.BrotliStream> に変換できます。

### <a name="new-cryptography-apis-and-cryptography-improvements"></a>新しい暗号化 API と暗号化の機能強化

.NET Core 2.1 には、暗号化 API に対する多数の拡張機能が含まれています。

- <xref:System.Security.Cryptography.Pkcs.SignedCms?displayProperty=nameWithType> は System.Security.Cryptography.Pkcs パッケージにあります。 実装は .NET Framework の <xref:System.Security.Cryptography.Pkcs.SignedCms> クラスと同じです。

- <xref:System.Security.Cryptography.X509Certificates.X509Certificate.GetCertHash%2A?displayProperty=nameWithType> メソッドと <xref:System.Security.Cryptography.X509Certificates.X509Certificate.GetCertHashString%2A?displayProperty=nameWithType> メソッドの新しいオーバーロードは、ハッシュ アルゴリズム識別子を受け入れ、SHA-1 以外のアルゴリズムを使用して呼び出し元が証明書の拇印の値を取得できるようにします。

- 新しい <xref:System.Span%601> ベースの暗号化 API をハッシュ、HMAC、暗号乱数の生成、非対称署名の生成、非対称署名の処理、および RSA 暗号化に使用できます。

- <xref:System.Span%601> ベースの実装を使用することにより、<xref:System.Security.Cryptography.Rfc2898DeriveBytes?displayProperty=nameWithType> のパフォーマンスが約 15% 向上しました。

- 新しい <xref:System.Security.Cryptography.CryptographicOperations?displayProperty=nameWithType> クラスには、次に示す 2 つの新しいメソッドが含まれています。

  - <xref:System.Security.Cryptography.CryptographicOperations.FixedTimeEquals%2A> は、同じ長さの任意の 2 つの入力に対して一定の時間を費やして戻ります。これは暗号化の検証での使用に適しており、サイド チャネル情報のタイミング攻撃の一因となることを避けることができます。

  - <xref:System.Security.Cryptography.CryptographicOperations.ZeroMemory%2A> はメモリ クリアのルーチンであり、最適化はできません。

- 静的メソッドである <xref:System.Security.Cryptography.RandomNumberGenerator.Fill%2A?displayProperty=nameWithType> は <xref:System.Span%601> にランダムな値を入力します。

- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms?displayProperty=nameWithType> が Linux と macOS でサポートされるようになりました。

- Elliptic-Curve Diffie-Hellman (ECDH) を <xref:System.Security.Cryptography.ECDiffieHellman?displayProperty=nameWithType> クラス ファミリーで使用できるようになりました。 セキュリティ、外部からのアクセスは .NET Framework の場合と同じです。

- <xref:System.Security.Cryptography.RSA.Create%2A?displayProperty=nameWithType> から返されるインスタンスは、SHA-2 ダイジェストを使用して OAEP で暗号化または暗号化解除できます。また、RSA-PSS を使用して署名を生成または検証できます。

### <a name="sockets-improvements"></a>ソケットの機能強化

.NET Core には、新しい型である <xref:System.Net.Http.SocketsHttpHandler?displayProperty=nameWithType>、および高度なネットワーク API の基盤となる書き換えられた <xref:System.Net.Http.HttpMessageHandler?displayProperty=nameWithType> が含まれています。  たとえば、<xref:System.Net.Http.SocketsHttpHandler?displayProperty=nameWithType> は <xref:System.Net.Http.HttpClient> 実装の基盤です。 以前のバージョンの .NET Core の高度な API は、ネイティブのネットワーク実装を基盤としていました。

.NET Core 2.1 に導入されたソケットの実装には多くの利点があります。

- 以前の実装と比較して、パフォーマンスが大幅に向上しています。

- プラットフォームの依存関係を排除したため、配置やサービス提供が簡略化されます。

- すべての .NET Core プラットフォームで動作が一貫しています。

<xref:System.Net.Http.SocketsHttpHandler> は .NET Core 2.1 における既定の実装です。 ただし、<xref:System.AppContext.SetSwitch%2A?displayProperty=nameWithType> メソッドを呼び出すことで、古い <xref:System.Net.Http.HttpClientHandler> クラスを使用するようにアプリケーションを構成できます。

```csharp
AppContext.SetSwitch("System.Net.Http.UseSocketsHttpHandler", false);
```

```vb
AppContext.SetSwitch("System.Net.Http.UseSocketsHttpHandler", False)
```

環境変数を使用して、<xref:System.Net.Http.SocketsHttpHandler> に基づくソケットの実装の使用をオプトアウトすることもできます。 そのためには、`DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER` を `false` または 0 に設定します。

Windows では、ネイティブ実装に依存する <xref:System.Net.Http.WinHttpHandler?displayProperty=nameWithType> を使用するか、または <xref:System.Net.Http.HttpClient> コンストラクターにクラスのインスタンスを渡すことにより <xref:System.Net.Http.SocketsHttpHandler> クラスを使用することもできます。

Linux と macOS では、<xref:System.Net.Http.HttpClient> をプロセス単位でのみ構成できます。 Linux で古い <xref:System.Net.Http.HttpClient> 実装を使用する場合は、[libcurl](https://curl.haxx.se/libcurl/) を配置する必要があります (これは .NET Core 2.0 と共にインストールされています)。

## <a name="see-also"></a>関連項目

- [.NET Core の新機能](index.md)
- [EF Core 2.1 の新機能](/ef/core/what-is-new/ef-core-2.1)
- [ASP.NET Core 2.1 の新機能](/aspnet/core/aspnetcore-2.1)
