---
title: .NET Framework の新機能
ms.custom: updateeachrelease
ms.date: 04/18/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- what's new [.NET Framework]
ms.assetid: 1d971dd7-10fc-4692-8dac-30ca308fc0fa
ms.openlocfilehash: fa7138127379b069b646c4b2488d1973a3ddd628
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79143318"
---
# <a name="whats-new-in-net-framework"></a>.NET Framework の新機能

この記事は、.NET Framework の次のバージョンにおける主な新機能と機能強化の概要を示します。

- [.NET Framework 4.8](#v48)
- [.NET Framework 4.7.2](#v472)
- [.NET Framework 4.7.1](#v471)
- [.NET Framework 4.7](#v47)
- [.NET Framework 4.6.2](#v462)
- [.NET Framework 4.6.1](#v461)
- [.NET 2015 と .NET Framework 4.6](#v46)
- [.NET Framework 4.5.2](#v452)
- [.NET Framework 4.5.1](#v451)
- [.NET Framework 4.5](#v45)

この記事は、各新機能の包括的な情報を説明するものではありません。また、この内容は変更される可能性があります。 .NET Framework の概要については、「[.NET Framework の概要](../get-started/index.md)」をご覧ください。 サポートされているプラットフォームについては、「[.NET Framework システム要件](../get-started/system-requirements.md)」を参照してください。 ダウンロード リンクとインストール手順については、「[.NET Framework のインストール](../install/guide-for-developers.md)」を参照してください。

> [!NOTE]
> また .NET Framework チームは、NuGet により OOB 機能をリリースすることによって、プラットフォーム サポートを拡張し、新しい機能 (変更できないコレクションや SIMD 対応ベクター型など) を提供しています。 詳細については、「[その他のクラス ライブラリと API](../additional-apis/index.md)」および[.NET Framework と OOB リリース](../get-started/the-net-framework-and-out-of-band-releases.md)に関するページを参照してください。
> .NET Framework 用の [NuGet パッケージの完全な一覧](https://www.nuget.org/profiles/dotnetframework)を参照してください。

<a name="v48" />

## <a name="introducing-net-framework-48"></a>.NET Framework 4.8 の概要

.NET Framework 4.8 は、.NET Framework 4.x の以前のバージョンを基に、多数の新しい修正といくつかの新機能を追加したものであり、これまでと同様に非常に安定した製品です。

### <a name="downloading-and-installing-net-framework-48"></a>.NET Framework 4.8 のダウンロードとインストール

.NET Framework 4.8 は、次の場所からダウンロードできます。

- [.NET Framework 4.8 の Web インストーラー](https://dotnet.microsoft.com/download/dotnet-framework/net48)

- [NET Framework 4.8 のオフライン インストーラー](https://dotnet.microsoft.com/download/dotnet-framework/net48)

.NET Framework 4.8 は、Windows 10、Windows 8.1、Windows 7 SP1、および Windows Server 2008 R2 SP1 以降に相当するサーバー プラットフォームにインストールできます。 .NET Framework 4.8 は、Web インストーラーまたはオフライン インストーラーを使用してインストールできます。 ほとんどのユーザーにお勧めする方法は、Web インストーラーの使用です。

[.NET Framework 4.8 Developer Pack](https://go.microsoft.com/fwlink/?LinkId=2085167) をインストールすれば、Visual Studio 2012 以降で、.NET Framework 4.8 をインストール対象に設定できます。

### <a name="whats-new-in-net-framework-48"></a>.NET Framework 4.8 の新機能

.NET Framework 4.8 には、次の領域の新機能が導入されています。

- [基底クラス](#core48)
- [Windows Communication Foundation (WCF)](#wcf48)
- [Windows Presentation Foundation (WPF)](#wpf48)
- [共通言語ランタイム](#clr48)

アプリケーションで支援技術のユーザーに適切なエクスペリエンスを提供できるようにするアクセシビリティ機能の向上は、.NET Framework 4.8 でも引き続き大きな注目点です。 .NET Framework 4.8 でのアクセシビリティ機能の改善について詳しくは、「[.NET Framework のアクセシビリティの新機能](whats-new-in-accessibility.md)」をご覧ください。

<a name="core48" />

#### <a name="base-classes"></a>基底クラス

**暗号での FIPS への影響の低下**。 以前のバージョンの .NET Framework では、システムの暗号化ライブラリが "FIPS モード" で構成されていると、<xref:System.Security.Cryptography.SHA256Managed> などのマネージド暗号化プロバイダー クラスで <xref:System.Security.Cryptography.CryptographicException> がスローされます。 これらの例外がスローされるのは、暗号化プロバイダー クラスのマネージド バージョンは、システムの暗号化ライブラリとは異なり、FIPS (Federal Information Processing Standards) 140-2 の認定を受けていないためです。 開発用コンピューターを FIPS モードにしている開発者はほとんどいないため、例外は一般に運用システムでスローされます。

.NET Framework 4.8 を対象とするアプリケーションの既定の設定では、この場合に次のマネージド暗号化クラスで <xref:System.Security.Cryptography.CryptographicException> がスローされることはなくなりました。

- <xref:System.Security.Cryptography.MD5Cng>
- <xref:System.Security.Cryptography.MD5CryptoServiceProvider>
- <xref:System.Security.Cryptography.RC2CryptoServiceProvider>
- <xref:System.Security.Cryptography.RijndaelManaged>
- <xref:System.Security.Cryptography.RIPEMD160Managed>
- <xref:System.Security.Cryptography.SHA256Managed>

代わりに、これらのクラスでは、暗号化操作はシステムの暗号化ライブラリにリダイレクトされます。 この変更により、開発者環境と運用環境での混乱を招く可能性のある違いは実質的になくなり、ネイティブ コンポーネントとマネージド コンポーネントは同じ暗号化ポリシーの下で動作するようになります。 これらの例外に依存するアプリケーションでは、AppContext スイッチ `Switch.System.Security.Cryptography.UseLegacyFipsThrow` を `true` に設定することで、以前の動作に戻すことができます。 詳しくは、「[Managed cryptography classes do not throw a CryptographyException in FIPS mode (FIPS モードのマネージド暗号クラスで CryptographyException がスローされない)](../migration-guide/retargeting/4.7.2-4.8.md#managed-cryptography-classes-do-not-throw-a-cryptographyexception-in-fips-mode)」をご覧ください。

**ZLib の更新バージョンの使用**

.NET Framework 4.5 以降の clrcompression.dll アセンブリでは、deflate アルゴリズムの実装を提供するため、データ圧縮用のネイティブ外部ライブラリである [ZLib](https://www.zlib.net) が使われています。 .NET Framework 4.8 の clrcompression.dll は、いくつかの重要な機能強化と修正を含む ZLib バージョン 1.2.11 を使用するように更新されています。

<a name="wcf48" />

#### <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

**ServiceHealthBehavior の導入**

正常性エンドポイントは、正常性の状態に基づいてサービスを管理するため、オーケストレーション ツールで広く使用されています。 正常性チェックは、サービスの可用性とパフォーマンスを追跡して通知を提供するため、監視ツールでも使用できます。

**ServiceHealthBehavior** は、<xref:System.ServiceModel.Description.IServiceBehavior> を拡張する WCF サービスの動作です。  <xref:System.ServiceModel.Description.ServiceDescription.Behaviors?displayProperty=nameWithType> コレクションに追加すると、サービスの動作は次のようになります。

- HTTP 応答コードでサービスの正常性状態が返されます。 HTTP/GET 正常性プローブ要求に対する HTTP 状態コードを、クエリ文字列で指定できます。

- サービスの正常性に関する情報が公開されます。 サービスの状態、スロットルの数、容量などのサービス固有の詳細を、`?health` クエリ文字列を含む HTTP/GET 要求を使用して表示できます。 動作がおかしい WCF サービスのトラブルシューティングを行うときは、このような情報に簡単にアクセスできることが重要です。

正常性エンドポイントを公開し、WCF サービスの正常性情報を公開するには、2 つの方法があります。

- コードの使用。 次に例を示します。

  ```csharp
  ServiceHost host = new ServiceHost(typeof(Service1),
                     new Uri("http://contoso:81/Service1"));
  ServiceHealthBehavior healthBehavior =
      host.Description.Behaviors.Find<ServiceHealthBehavior>();
  healthBehavior ??= new ServiceHealthBehavior();
  host.Description.Behaviors.Add(healthBehavior);
  ```

  ```vb
  Dim host As New ServiceHost(GetType(Service1),
              New Uri("http://contoso:81/Service1"))
  Dim healthBehavior As ServiceHealthBehavior =
     host.Description.Behaviors.Find(Of ServiceHealthBehavior)()
  If healthBehavior Is Nothing Then
     healthBehavior = New ServiceHealthBehavior()
  End If
  host.Description.Behaviors.Add(healthBehavior)
  ```

- 構成ファイルの使用。 次に例を示します。

  ```xml
  <behaviors>
    <serviceBehaviors>
      <behavior name="DefaultBehavior">
        <serviceHealth httpsGetEnabled="true"/>
      </behavior>
    </serviceBehaviors>
  </behaviors>
  ```

`OnServiceFailure`、`OnDispatcherFailure`、`OnListenerFailure`、`OnThrottlePercentExceeded` などのクエリ パラメーターを使用して、サービスの正常性状態のクエリを実行することができ、各クエリ パラメーターに対して HTTP 応答コードを指定できます。 クエリ パラメーターで HTTP 応答コードを省略すると、503 HTTP 応答コードが既定で使われます。 次に例を示します。

- OnServiceFailure: `https://contoso:81/Service1?health&OnServiceFailure=450`

  [ServiceHost.State](xref:System.ServiceModel.Channels.CommunicationObject.State) が <xref:System.ServiceModel.CommunicationState.Opened?displayProperty=nameWithType> より大きい場合、450 HTTP 応答状態コードが返されます。
クエリ パラメーターと例:

- OnDispatcherFailure: `https://contoso:81/Service1?health&OnDispatcherFailure=455`

  いずれかのチャネル ディスパッチャーの状態が <xref:System.ServiceModel.CommunicationState.Opened?displayProperty=nameWithType> より大きい場合、455 HTTP 応答状態コードが返されます。

- OnListenerFailure: `https://contoso:81/Service1?health&OnListenerFailure=465`

  いずれかのチャネル リスナーの状態が <xref:System.ServiceModel.CommunicationState.Opened?displayProperty=nameWithType> より大きい場合、465 HTTP 応答状態コードが返されます。

- OnThrottlePercentExceeded: `https://contoso:81/Service1?health&OnThrottlePercentExceeded= 70:350,95:500`

  応答をトリガーする割合 {1 – 100} とその HTTP 応答コード {200 – 599} を指定します。 この例では、次のように記述されています。

  - 割合が 95 より大きい場合、500 HTTP 応答コードが返されます。

  - 割合が 70 – 95 の場合は、350 が返されます。

  - それ以外の場合は、200 が返されます。

サービスの正常性状態は、HTML (`https://contoso:81/Service1?health` のようなクエリ文字列を指定) または XML (`https://contoso:81/Service1?health&Xml` のようなクエリ文字列を指定) で表示できます。 `https://contoso:81/Service1?health&NoContent` のようなクエリ文字列では、空の HTML ページが返されます。

<a name="wpf48" />

#### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

**高 DPI の機能強化**

.NET Framework 4.8 では、WPF でモニターごとの V2 DPI の認識および混合モードの DPI スケーリングのサポートが追加されています。 高 DPI の開発に関する追加情報については、「[High DPI Desktop Application Development on Windows (Windows での高 DPI デスクトップ アプリケーション開発)](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows)」をご覧ください。

.NET framework 4.8 では、混合モードの DPI スケーリングをサポートするプラットフォーム上の高 DPI WPF アプリケーションでのホステッド HWND と Windows フォームの相互運用のためのサポートが向上しています (Windows 10 April 2018 Update 以降)。 [SetThreadDpiHostingBehavior](/windows/desktop/api/winuser/nf-winuser-setthreaddpihostingbehavior) および [SetThreadDpiAwarenessContext](/windows/desktop/api/winuser/nf-winuser-setthreaddpiawarenesscontext) を呼び出すことによって、ホステッド HWND または Windows フォームのコントロールを混合モードの DPI スケーリングされたウィンドウとして作成すると、それらはモニターごとの V2 WPF アプリケーション内でホストでき、適切にサイズ設定およびスケーリングされます。 そのようなホステッド コンテンツは、ネイティブ DPI ではレンダリングされず、代わりにオペレーティング システムによって適切なサイズにスケーリングされます。 モニターごとの v2 DPI 認識モードのサポートにより、高 DPI アプリケーションのネイティブ ウィンドウで WPF コントロールをホストする (つまり、子にする) こともできます。

混合モードの高 DPI スケーリングのサポートを有効にするには、アプリケーション構成ファイルで次の [AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) スイッチを設定します。

```xml
<runtime>
   <AppContextSwitchOverrides value = "Switch.System.Windows.DoNotScaleForDpiChanges=false; Switch.System.Windows.DoNotUsePresentationDpiCapabilityTier2OrGreater=false"/>
</runtime>
```

<a name="clr48" />

#### <a name="common-language-runtime"></a>共通言語ランタイム

NET Framework 4.8 のランタイムには、次の変更と強化が含まれます。

**JIT コンパイラの機能強化**。 .NET Framework 4.8 の Just-In-Time (JIT) コンパイラは、.NET Core 2.1 の JIT コンパイラが基になっていいます。 .NET Core 2.1 の JIT コンパイラに対して行われた最適化の多くとすべてのバグ修正が、.NET Framework 4.8 の JIT コンパイラに含まれます。

**NGEN の機能強化**。 ランタイムでは、[ネイティブ イメージ ジェネレーター](../tools/ngen-exe-native-image-generator.md) (NGEN) のイメージに対するメモリ管理が改善され、NGEN イメージからマップされているデータがメモリに常駐しないようになっています。 これにより、実行されるメモリを変更することによって任意のコードを実行しようとする攻撃に利用可能な領域が減ります。

**すべてのアセンブリに対するマルウェア対策スキャン**。 以前のバージョンの .NET Framework のランタイムでは、Windows Defender またはサード パーティのマルウェア対策ソフトウェアを使用して、ディスクから読み込まれたすべてのアセンブリがスキャンされます。 しかし、<xref:System.Reflection.Assembly.Load(System.Byte[])?displayProperty=nameWithType> メソッドなどの他のソースから読み込まれたアセンブリはスキャンされず、検出されていないマルウェアが含まれる可能性があります。 Windows 10 で実行される .NET Framework 4.8 以降のランタイムでは、[Antimalware Scan Interface (AMSI)](/windows/desktop/AMSI/antimalware-scan-interface-portal) を実装するマルウェア対策ソリューションによるスキャンがトリガーされます。

<a name="v472" />

## <a name="whats-new-in-net-framework-472"></a>.NET Framework 4.7.2 の新機能

.NET Framework 4.7.2 には、次の領域における新機能が含まれています。

- [基底クラス](#core-472)
- [ASP.NET](#asp-net472)
- [ネットワーク](#net472)
- [SQL](#sql472)
- [WPF](#wpf472)
- [ClickOnce](#clickonce)

.NET Framework 4.7.2 では引き続きアクセシビリティ機能の向上が重視されており、それによりアプリケーションでは支援技術のユーザーに適切なエクスペリエンスを提供できます。 .NET Framework 4.7.2 でのアクセシビリティ機能の改善について詳しくは、「[.NET Framework のアクセシビリティの新機能](whats-new-in-accessibility.md)」をご覧ください。

<a name="core-472" />

#### <a name="base-classes"></a>基底クラス

.NET Framework 4.7.2 は、大量の暗号化の機能強化、ZIP アーカイブの圧縮解除サポートの向上、および追加のコレクションの API を備えています。

**RSA.Create および DSA.Create の新しいオーバーロード**

<xref:System.Security.Cryptography.DSA.Create(System.Security.Cryptography.DSAParameters)?displayProperty=nameWithType> および <xref:System.Security.Cryptography.RSA.Create(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType> メソッドを使うと、新しい <xref:System.Security.Cryptography.DSA> または <xref:System.Security.Cryptography.RSA> キーをインスタンス化するときにキーのパラメーターを指定できます。 次のようなコードを置き換えることができます。

```csharp
// Before .NET Framework 4.7.2
using (RSA rsa = RSA.Create())
{
   rsa.ImportParameters(rsaParameters);
   // Other code to execute using the RSA instance.
}
```

```vb
' Before .NET Framework 4.7.2
Using rsa = RSA.Create()
   rsa.ImportParameters(rsaParameters)
   ' Other code to execute using the rsa instance.
End Using
```

次のようなコードに置き換えます。

```csharp
// Starting with .NET Framework 4.7.2
using (RSA rsa = RSA.Create(rsaParameters))
{
   // Other code to execute using the rsa instance.
}
```

```vb
' Starting with .NET Framework 4.7.2
Using rsa = RSA.Create(rsaParameters)
   ' Other code to execute using the rsa instance.
End Using
```

<xref:System.Security.Cryptography.DSA.Create(System.Int32)?displayProperty=nameWithType> および <xref:System.Security.Cryptography.RSA.Create(System.Int32)?displayProperty=nameWithType> メソッドでは、特定のキー サイズを指定して、新しい <xref:System.Security.Cryptography.DSA> または <xref:System.Security.Cryptography.RSA> キーを生成することができます。 次に例を示します。

```csharp
using (DSA dsa = DSA.Create(2048))
{
   // Other code to execute using the dsa instance.
}
```

```vb
Using dsa = DSA.Create(2048)
   ' Other code to execute using the dsa instance.
End Using
```

**Rfc2898DeriveBytes コンストラクターは、ハッシュ アルゴリズムの名前を受け入れる**

<xref:System.Security.Cryptography.Rfc2898DeriveBytes> クラスには、3 つの新しいコンストラクターがあり、キーを派生するときに使用する HMAC アルゴリズムを識別する <xref:System.Security.Cryptography.HashAlgorithmName> パラメーターを使用します。 SHA-1 を使用する代わりに、開発者は、次の例に示すように SHA-256 などの SHA-2 ベースの HMAC を使用する必要があります。

```csharp
private static byte[] DeriveKey(string password, out int iterations, out byte[] salt,
                                out HashAlgorithmName algorithm)
{
   iterations = 100000;
   algorithm = HashAlgorithmName.SHA256;

   const int SaltSize = 32;
   const int DerivedValueSize = 32;

   using (Rfc2898DeriveBytes pbkdf2 = new Rfc2898DeriveBytes(password, SaltSize,
                                                             iterations, algorithm))
   {
      salt = pbkdf2.Salt;
      return pbkdf2.GetBytes(DerivedValueSize);
   }
}
```

```vb
Private Shared Function DeriveKey(password As String, ByRef iterations As Integer,
                                  ByRef salt AS Byte(), ByRef algorithm As HashAlgorithmName) As Byte()
   iterations = 100000
   algorithm = HashAlgorithmName.SHA256

   Const SaltSize As Integer = 32
   Const  DerivedValueSize As Integer = 32

   Using pbkdf2 = New Rfc2898DeriveBytes(password, SaltSize, iterations, algorithm)
      salt = pbkdf2.Salt
      Return pbkdf2.GetBytes(DerivedValueSize)
   End Using
End Function
```

**一時的なキーのサポート**

PFX のインポートでは、必要に応じてハード ドライブをバイパスして、メモリから直接秘密キーを読み込むことができます。 新しい <xref:System.Security.Cryptography.X509Certificates.X509KeyStorageFlags.EphemeralKeySet?displayProperty=nameWithType> フラグが、<xref:System.Security.Cryptography.X509Certificates.X509Certificate2> コンストラクターまたは <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.Import%2A?displayProperty=nameWithType> メソッドのいずれかのオーバーロードで指定されているときには、一時的なキーとして秘密キーが読み込まれます。 これにより、キーがディスク上に表示されることを防ぎます。 ただし、

- キーは、ディスク上に持続しないので、このフラグで読み込まれた証明書は、X509Store に追加する適切な候補ではありません。

- この方法で読み込まれたキーは、ほとんどの場合、Windows CNG を使用して読み込まれます。 そのため呼び出し元は、[cert.GetRSAPrivateKey()](xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey%2A) などの拡張メソッドを呼び出すことによって秘密キーにアクセスする必要があります。 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.PrivateKey?displayProperty=nameWithType> プロパティは機能しません。

- 従来の <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.PrivateKey?displayProperty=nameWithType> プロパティは、証明書では機能しないので、開発者は、一時的なキーに切り替える前に厳格なテストを実行する必要があります。

**PKCS #10 証明書署名要求と X.509 公開キー証明書のプログラムによる作成**

.NET Framework 4.7.2 以降、ワークロードは、証明書署名要求 (CSR) を生成できるようになりました。これにより、証明書要求の生成を既存のツールにステージングすることができます。 これは、テストのシナリオでよく役に立ちます。

詳細およびコードの例については、[.NET ブログ](https://devblogs.microsoft.com/dotnet/net-framework-4-7-2-developer-pack-early-access-build-3056-is-available/)の「Programmatic creation of PKCS#10 certification signing requests and X.509 public key certificates」(PKCS #10 証明書署名要求と X.509 公開キー証明書のプログラムによる作成) を参照してください。

**新しい SignerInfo メンバー**

.NET Framework 4.7.2 以降、<xref:System.Security.Cryptography.Pkcs.SignerInfo> クラスでは署名についてさらに多くの詳細が公開されます。 <xref:System.Security.Cryptography.Pkcs.SignerInfo.SignatureAlgorithm?displayProperty=fullName> プロパティの値を取得して、署名者によって使用される署名アルゴリズムを判断することができます。 <xref:System.Security.Cryptography.Pkcs.SignerInfo.GetSignature%2A?displayProperty=nameWithType> を呼び出して、この署名者の暗号化署名のコピーを取得することができます。

**CryptoStream が破棄された後にラップされたストリームを開いたままにする**

.NET Framework 4.7.2 以降、<xref:System.Security.Cryptography.CryptoStream> クラスの追加のコンストラクターを使用して、<xref:System.Security.Cryptography.CryptoStream.Dispose%2A> でラップされたストリームを開いたままにすることができます。 <xref:System.Security.Cryptography.CryptoStream> のインスタンスが破棄された後でラップされたストリームを開いたままにするには、次のように新しい <xref:System.Security.Cryptography.CryptoStream> コンストラクターを作成します。

```csharp
var cStream = new CryptoStream(stream, transform, mode, leaveOpen: true);
```

```vb
Dim cStream = New CryptoStream(stream, transform, mode, leaveOpen:=true)
```

**DeflateStream の圧縮解除の変更**

.NET Framework 4.7.2 以降では、<xref:System.IO.Compression.DeflateStream> クラスの圧縮解除操作の実装の際に、既定でネイティブの Windows API を使用するように変更されました。 通常は、これによりパフォーマンスが大幅に改善されます。

Windows API を使用した圧縮解除のサポートは .NET Framework 4.7.2 を対象とするアプリケーションで既定で有効です。 以前のバージョンの .NET Framework を対象とするアプリケーションが .NET Framework 4.7.2 未満で実行される場合は、次の [AppContext スイッチ](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)をアプリケーション構成ファイルに追加することで、この動作を選択できます。

```xml
<AppContextSwitchOverrides value="Switch.System.IO.Compression.DoNotUseNativeZipLibraryForDecompression=false" />
```

**追加のコレクション API**

.NET Framework 4.7.2 でいくつかの新しい API が <xref:System.Collections.Generic.SortedSet%601> および <xref:System.Collections.Generic.HashSet%601> 型に追加されました。 次の設定があります。

- `TryGetValue` メソッドは、他のコレクション型で使用する try パターンを次の 2 つの型に拡張します。 これらのメソッドを以下に示します。

  - [public bool HashSet\<T>.TryGetValue(T equalValue, out T actualValue)](xref:System.Collections.Generic.SortedSet%601.TryGetValue%2A)
  - [public bool SortedSet\<T>.TryGetValue(T equalValue, out T actualValue)](xref:System.Collections.Generic.SortedSet%601.TryGetValue%2A)

- <xref:System.Collections.Generic.HashSet%601> 拡張メソッドは、コレクションを `Enumerable.To*` に変換します。

  - [public static HashSet\<TSource> ToHashSet\<TSource>(this IEnumerable\<TSource> source)](xref:System.Linq.Enumerable.ToHashSet%2A)
  - [public static HashSet\<TSource> ToHashSet\<TSource>(this IEnumerable\<TSource> source, IEqualityComparer\<TSource> comparer)](xref:System.Linq.Enumerable.ToHashSet%2A)

- 新しい <xref:System.Collections.Generic.HashSet%601> コンストラクターを使用して、<xref:System.Collections.Generic.HashSet%601> のサイズが事前にわかっている場合に、コレクションの容量を設定できます。

  - [public HashSet(int capacity)](xref:System.Collections.Generic.HashSet%601.%23ctor(System.Int32))
  - [public HashSet(int capacity, IEqualityComparer\<T> comparer)](xref:System.Collections.Generic.HashSet%601.%23ctor(System.Int32,System.Collections.Generic.IEqualityComparer%7B%600%7D))

<xref:System.Collections.Concurrent.ConcurrentDictionary%602> クラスには、<xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A> および <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> メソッドの新しいオーバーロードが含まれています、これにより、ディクショナリから値を取得したり、またはそれが見つからない場合にそれを追加したり、値が既に存在する場合に、値をディクショナリに追加するか更新したりすることができます。

```csharp
public TValue AddOrUpdate<TArg>(TKey key, Func<TKey, TArg, TValue> addValueFactory, Func<TKey, TValue, TArg, TValue> updateValueFactory, TArg factoryArgument)

public TValue GetOrAdd<TArg>(TKey key, Func<TKey, TArg, TValue> valueFactory, TArg factoryArgument)
```

```vb
Public AddOrUpdate(Of TArg)(key As TKey, addValueFactory As Func(Of TKey, TArg, TValue), updateValueFactory As Func(Of TKey, TValue, TArg, TValue), factoryArgument As TArg) As TValue

Public GetOrAdd(Of TArg)(key As TKey, valueFactory As Func(Of TKey, TArg, TValue), factoryArgument As TArg) As TValue
```

<a name="asp-net472" />

#### <a name="aspnet"></a>ASP.NET

**Web フォームでの依存関係挿入のサポート**

[依存性挿入 (DI)](/aspnet/core/fundamentals/dependency-injection#overview-of-dependency-injection) は、オブジェクトとその依存関係を分離し、依存関係の変更だけを理由としてオブジェクトのコードを変更する必要がないようにします。 .NET Framework 4.7.2 を対象とする ASP.NET アプリケーションを開発するときに次のことができます。

- ASP.NET Web アプリケーション プロジェクトの[ハンドラーとモジュール](https://docs.microsoft.com/previous-versions/aspnet/bb398986(v=vs.100))、[ページ インスタンス](xref:System.Web.UI.Page)、および[ユーザー コントロール](https://docs.microsoft.com/previous-versions/aspnet/y6wb1a0e(v=vs.100))でアクセス操作子ベース、インターフェイス ベース、およびコンストラクター ベースの挿入を使用します。

- ASP.NET Web サイト プロジェクトの[ハンドラーとモジュール](https://docs.microsoft.com/previous-versions/aspnet/bb398986(v=vs.100))、[ページ インスタンス](xref:System.Web.UI.Page)、および[ユーザー コントロール](https://docs.microsoft.com/previous-versions/aspnet/y6wb1a0e(v=vs.100))でアクセス操作子ベースおよびインターフェイス ベースの挿入を使用します。

- 別の依存関係の挿入のフレームワークにプラグインします。

**同じサイトの cookie のサポート**

[SameSite](https://tools.ietf.org/html/draft-west-first-party-cookies-07) は、ブラウザーがサイト間の要求と共に cookie を送信するのを防ぎます。 .NET Framework 4.7.2 では、<xref:System.Web.SameSiteMode?displayProperty=nameWithType> 列挙型のメンバーの値を持つ <xref:System.Web.HttpCookie.SameSite?displayProperty=nameWithType> プロパティが追加されました。 値が <xref:System.Web.SameSiteMode.Strict?displayProperty=nameWithType> または <xref:System.Web.SameSiteMode.Lax?displayProperty=nameWithType> の場合、ASP.NET は `SameSite` 属性を set-cookie ヘッダーに追加します。 SameSite のサポートは、<xref:System.Web.HttpCookie> オブジェクト、および <xref:System.Web.Security.FormsAuthentication> と <xref:System.Web.SessionState> cookie に適用されます。

<xref:System.Web.HttpCookie> オブジェクトの SameSite を次のように設定できます。

```csharp
var c = new HttpCookie("secureCookie", "same origin");
c.SameSite = SameSiteMode.Lax;
```

```vb
Dim c As New HttpCookie("secureCookie", "same origin")
c.SameSite = SameSiteMode.Lax
```

Web.config ファイルを変更して、アプリケーション レベルで SameSite cookie を構成することもできます。

```xml
<system.web>
   <httpCookies sameSite="Strict" />
</system.web>
```

web config ファイルを変更することによって、<xref:System.Web.Security.FormsAuthentication> および <xref:System.Web.SessionState> cookie の SameSite を追加することができます。

```xml
<system.web>
   <authentication mode="Forms">
      <forms cookieSameSite="Lax">
         <!-- ...   -->
      </forms>
   <authentication />
   <sessionState cookieSameSite="Lax"></sessionState>
</system.web>
```

<a name="net472" />

#### <a name="networking"></a>ネットワーキング

**HttpClientHandler プロパティの実装**

.NET Framework 4.7.1 で 8 個のプロパティが、<xref:System.Net.Http.HttpClientHandler?displayProperty=nameWithType> クラスに追加されました。 ただし、2 つは <xref:System.PlatformNotSupportedException> をスローしていました。 .NET Framework 4.7.2 では、それらのプロパティの実装が提供されるようになりました。 次のプロパティです。

- <xref:System.Net.Http.HttpClientHandler.CheckCertificateRevocationList>
- <xref:System.Net.Http.HttpClientHandler.SslProtocols>

<a name="sql472" />

#### <a name="sqlclient"></a>SQLClient

**Azure Active Directory のユニバーサル認証と多要素認証のサポート**

コンプライアンスとセキュリティのニーズが高まったために、多くのお客様が多要素認証 (MFA) を使用しています。 さらに、現在のベスト プラクティスでは、接続文字列内に直接ユーザーのパスワードを含めることは推奨されません。 これらの変更をサポートするため、.NET Framework 4.7.2 では [SQLClient 接続文字列](xref:System.Data.SqlClient.SqlConnection.ConnectionString)が拡張され、MFA および [Azure AD Authentication](/azure/sql-database/sql-database-aad-authentication-configure) をサポートするための既存の "Authentication" キーワードに新しい値 "Active Directory Interactive" が追加されています。 新しい対話型メソッドでは、ネイティブおよびフェデレーションの Azure AD ユーザーだけでなく Azure AD ゲスト ユーザーをサポートします。 このメソッドを使用する場合は、Azure AD によって課される MFA 認証が SQL データベースに対してサポートされます。 さらに、認証プロセスは、セキュリティのベスト プラクティスに準拠するためにユーザーのパスワードを要求します。

以前のバージョンの .NET Framework では、SQL 接続は、<xref:System.Data.SqlClient.SqlAuthenticationMethod.ActiveDirectoryPassword?displayProperty=nameWithType> と <xref:System.Data.SqlClient.SqlAuthenticationMethod.ActiveDirectoryIntegrated?displayProperty=nameWithType> のオプションのみをサポートしていました。 これらはどちらも非対話型の [ADAL プロトコル](/azure/active-directory/develop/active-directory-authentication-libraries) の一部で、MFA はサポートされていません。 新しい <xref:System.Data.SqlClient.SqlAuthenticationMethod.ActiveDirectoryInteractive?displayProperty=nameWithType> オプションを使用して、SQL 接続は MFA および既存の認証方法 (パスワードや統合認証) をサポートします。これにより、ユーザーが接続文字列でパスワードを永続化せずに、対話形式でパスワードを入力することができます。

詳細および例については、[.NET ブログ](https://devblogs.microsoft.com/dotnet/net-framework-4-7-2-developer-pack-early-access-build-3056-is-available/)の「SQL -- Azure AD Universal and Multi-factor Authentication Support」(SQL--Azure AD ユニバーサルおよび多要素認証のサポート) を参照してください。

**Always Encrypted バージョン 2 のサポート**

NET Framework 4.7.2 では、エンクレーブ ベースの Always Encrypted のサポートが追加されました。 Always Encrypted の元のバージョンは、暗号化キーがクライアントから離れないクライアント側暗号化テクノロジです。 エンクレーブ ベースの Always Encrypted では、クライアントが、オプションで暗号化キーをセキュアなエンクレーブに送信することができます。エンクレーブは、SQL Server の一部と見なされても SQL Server のコードを改ざんできないセキュアなコンピューティング エンティティです。 エンクレーブ ベースの Always Encrypted をサポートするために、.NET Framework 4.7.2 では、次の型とメンバーが <xref:System.Data.SqlClient> 名前空間に追加されています。

- <xref:System.Data.SqlClient.SqlConnectionStringBuilder.EnclaveAttestationUrl?displayProperty=nameWithType>。エンクレーブ ベースの Always Encrypted の URI を指定します。

- <xref:System.Data.SqlClient.SqlColumnEncryptionEnclaveProvider>。すべてのエンクレーブ プロバイダーの派生元の抽象クラスです。

- <xref:System.Data.SqlClient.SqlEnclaveSession>。指定されたエンクレーブ セッションの状態をカプセル化します。

- <xref:System.Data.SqlClient.SqlEnclaveAttestationParameters>。SQL Server によって、特定の構成証明プロトコルを実行するために必要な情報を取得するために使用される構成証明パラメーター。

アプリケーション構成ファイルで、エンクレーブ プロバイダーの機能を提供する抽象 <xref:System.Data.SqlClient.SqlColumnEncryptionEnclaveProvider?displayProperty=nameWithType> クラスの完全な実装を指定します。 次に例を示します。

```xml
<configuration>
  <configSections>
    <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection,System.Data,Version=4.0.0.0,Culture=neutral,PublicKeyToken=b77a5c561934e089"/>
  </configSections>
  <SqlColumnEncryptionEnclaveProviders>
    <providers>
      <add name="Azure" type="Microsoft.SqlServer.Management.AlwaysEncrypted.AzureEnclaveProvider,MyApp"/>
      <add name="HGS" type="Microsoft.SqlServer.Management.AlwaysEncrypted.HGSEnclaveProvider,MyApp" />
    </providers>
  </SqlColumnEncryptionEnclaveProviders >
</configuration>
```

エンクレーブ ベースの Always Encrypted の基本的な流れを示します。

1. ユーザーが、エンクレーブ ベースの Always Encrypted をサポートしていた SQL Server への AlwaysEncrypted 接続を作成します。 ドライバーが、構成証明サービスにアクセスし、正しいエンクレーブに接続されていることを確認します。

1. エンクレーブが証明されると、ドライバーが、SQL Server でホストされているセキュリティで保護されたエンクレーブとのセキュリティで保護されたチャネルを確立します。

1. ドライバーは、SQL 接続の間にセキュリティで保護されたエンクレーブとクライアントによって承認された暗号化キーを共有します。

<a name="wpf472" />

#### <a name="windows-presentation-foundation"></a>Windows Presentation Foundation

**ソースによる ResourceDictionaries の検索**

.NET Framework 4.7.2 以降、診断アシスタントでは、特定のソース URI から作成された  <xref:System.Windows.Xps.Packaging.IXpsFixedPageReader.ResourceDictionaries> を見つけることができます。 (この機能は、実稼働アプリケーションではなく診断アシスタントによって使用されます)。Visual Studio の "エディット コンティニュ" 機能などの診断アシスタントでは、ユーザーが、実行中のアプリケーションに変更を適用する目的で ResourceDictionary を編集できます。 これを実現するための 1 つの手順は、実行中のアプリケーションが編集されているディクショナリから作成したすべての ResourceDictionaries を見つけることです。 たとえば、アプリケーションは、コンテンツが特定のソース URI からコピーされる ResourceDictionary を宣言できます。

```xml
<ResourceDictionary Source="MyRD.xaml">
```

*MyRD.xaml*  の元のマークアップを編集する診断アシスタントでは、ディクショナリを検索する新しい機能を使用できます。 この機能は、新しい静的メソッド <xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetResourceDictionariesForSource%2A?displayProperty=nameWithType> によって実装されます。 診断のアシスタントは、次のコードに示すように、元のマークアップを識別する絶対 URI を使用して新しいメソッドを呼び出します。

```csharp
IEnumerable<ResourceDictionary> dictionaries = ResourceDictionaryDiagnostics.GetResourceDictionariesForSource(new Uri("pack://application:,,,/MyApp;component/MyRD.xaml"));
```

```vb
Dim dictionaries As IEnumerable(Of ResourceDictionary) = ResourceDictionaryDiagnostics.GetResourceDictionariesForSource(New Uri("pack://application:,,,/MyApp;component/MyRD.xaml"))
```

このメソッドは、 <xref:System.Windows.Diagnostics.VisualDiagnostics> が有効で、かつ [`ENABLE_XAML_DIAGNOSTICS_SOURCE_INFO`](xref:System.Windows.Diagnostics.VisualDiagnostics.GetXamlSourceInfo%2A)  環境変数が設定されている場合を除き、空白の列挙を返します。

**ResourceDictionary 所有者の検索**

.NET Framework 4.7.2 以降、診断アシスタントでは、特定の <xref:Windows.UI.Xaml.ResourceDictionary> の所有者を見つけることができます。 (この機能は、実稼働アプリケーションではなく診断アシスタントによって使用されます。)<xref:Windows.UI.Xaml.ResourceDictionary> の変更が行われたときに、WPF が、変更の影響を受ける可能性があるすべての [DynamicResource](../wpf/advanced/dynamicresource-markup-extension.md) の参照を自動的に見つけます。

Visual Studio の "エディット コンティニュ" 機能などの診断アシスタントでは、場合によっては [StaticResource](../wpf/advanced/staticresource-markup-extension.md) 参照を処理するためにこれを拡張する必要があります。 このプロセスの最初の手順は、ディクショナリの所有者を検索することです。つまり、`Resources` プロパティがディクショナリを (直接または <xref:System.Windows.ResourceDictionary.MergedDictionaries?displayProperty=nameWithType> プロパティを使用して間接的に) 参照しているすべてのオブジェクトを見つけます。 <xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics?displayProperty=nameWithType> クラスに実装された 3 つの新しい静的メソッドは、`Resources` プロパティを持つ各基本データ型ごとに 1 つずつあり、次の手順をサポートします。

- [`public static IEnumerable<FrameworkElement> GetFrameworkElementOwners(ResourceDictionary dictionary);`](xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetFrameworkElementOwners%2A)

- [`public static IEnumerable<FrameworkContentElement> GetFrameworkContentElementOwners(ResourceDictionary dictionary);`](xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetFrameworkContentElementOwners%2A)

- [`public static IEnumerable<Application> GetApplicationOwners(ResourceDictionary dictionary);`](xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetApplicationOwners%2A)

これらのメソッドは、 <xref:System.Windows.Diagnostics.VisualDiagnostics> が有効で、かつ [`ENABLE_XAML_DIAGNOSTICS_SOURCE_INFO`](xref:System.Windows.Diagnostics.VisualDiagnostics.GetXamlSourceInfo%2A)  環境変数が設定されている場合を除き、空白の列挙を返します。

**StaticResource 参照の検索**

診断アシスタントで、[StaticResource](../wpf/advanced/staticresource-markup-extension.md) 参照が解決されるたびに通知を受け取ることができるようになりました。 (この機能は、実稼働アプリケーションではなく診断アシスタントによって使用されます。)Visual Studio の "エディット コンティニュ" 機能などの診断アシスタントは、場合によっては <xref:Windows.UI.Xaml.ResourceDictionary> の値が変更されたときにリソースのすべての使用を更新する必要があります。 WPF は、[DynamicResource](../wpf/advanced/dynamicresource-markup-extension.md) 参照に関してこれを自動的に実行しますが、[StaticResource](../wpf/advanced/staticresource-markup-extension.md) 参照に関しては意図的に実行しません。 .NET Framework 4.7.2 以降、診断アシスタントは、静的リソースのそれらの使用を検索するのにこれらの通知を使用できます。

通知は、新しい <xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.StaticResourceResolved?displayProperty=nameWithType> イベントによって実装されます。

```csharp
public static event EventHandler<StaticResourceResolvedEventArgs> StaticResourceResolved;
```

```vb
Public Shared Event StaticResourceResolved As EventHandler(Of StaticResourceResolvedEventArgs)
```

このイベントは、ランタイムが [StaticResource](../wpf/advanced/staticresource-markup-extension.md) 参照を解決するたびに発生します。 <xref:System.Windows.Diagnostics.StaticResourceResolvedEventArgs> 引数によって解決が記述され、[StaticResource](../wpf/advanced/staticresource-markup-extension.md) 参照をホストするオブジェクトとプロパティ、および解決に使用される  <xref:Windows.UI.Xaml.ResourceDictionary> とキーが示されます。

```csharp
public class StaticResourceResolvedEventArgs : EventArgs
{
   public Object TargetObject { get; }

   public Object TargetProperty { get; }

   public ResourceDictionary ResourceDictionary { get; }

   public object ResourceKey { get; }
}
```

```vb
Public Class StaticResourceResolvedEventArgs : Inherits EventArgs
   Public ReadOnly Property TargetObject As Object
   Public ReadOnly Property TargetProperty As Object
   Public ReadOnly Property ResourceDictionary As ResourceDictionary
   Public ReadOnly Property ResourceKey As Object
End Class
```

 <xref:System.Windows.Diagnostics.VisualDiagnostics> が有効で、かつ [`ENABLE_XAML_DIAGNOSTICS_SOURCE_INFO`](xref:System.Windows.Diagnostics.VisualDiagnostics.GetXamlSourceInfo%2A)  環境変数が設定されていない限り、イベントを発生しません (その `add` アクセサーは無視されます)。

#### <a name="clickonce"></a>ClickOnce

ClickOnce を使用して、Windows フォームの HDPI 対応アプリケーション、Windows Presentation Foundation (WPF)、および Visual Studio Tools for Office (VSTO) のすべてを配置できます。 次のエントリがアプリケーション マニフェストに見つかった場合、.NET Framework 4.7.2 で展開が成功します。

```xml
<windowsSettings>
   <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
</windowsSettings>
```

Windows フォーム アプリケーションの場合、DPI 認識をアプリケーション マニフェストではなくアプリケーション構成ファイルで設定するという以前の回避策は、ClickOnce 展開を成功させるために必要ではなくなりました。

<a name="v471" />

## <a name="whats-new-in-net-framework-471"></a>.NET Framework 4.7.1 の新機能

.NET Framework 4.7.1 には、次の領域における新機能が含まれています。

- [基底クラス](#core471)
- [共通言語ランタイム (CLR)](#clr)
- [ネットワーク](#net471)
- [ASP.NET](#asp-net471)

さらに、.NET Framework 4.7.1 ではアクセシビリティ機能の向上が重視されており、それによりアプリケーションでは支援技術のユーザーに適切なエクスペリエンスを提供できます。 .NET Framework 4.7.1 でのアクセシビリティ機能の改善について詳しくは、「[.NET Framework のアクセシビリティの新機能](whats-new-in-accessibility.md)」をご覧ください。

<a name="core471" />

#### <a name="base-classes"></a>基底クラス

**.NET Standard 2.0 のサポート**

[.NET Standard](../../standard/net-standard.md) は、そのバージョンの標準をサポートする各 .NET 実装で使用する必要がある API のセットを定義します。 .NET Framework 4.7.1 では、.NET Standard 2.0 が完全にサポートされており、.NET Standard 2.0 で定義されていて .NET Framework 4.6.1、4.6.2、および 4.7 にはなかった[約 200 の API](https://github.com/dotnet/standard/blob/master/src/netstandard/src/ApiCompatBaseline.net461.txt) が追加されています。 (これらのバージョンの .NET Framework は、追加の .NET Standard サポート ファイルもターゲット システムにも展開されている場合にのみ .NET Standard 2.0 をサポートします)。詳細については、「[.NET Framework 4.7.1 Runtime and Compiler Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/)」(.NET Framework 4.7.1 ランタイムとコンパイラの機能) ブログ投稿の「BCL - .NET Standard 2.0 Support」(BCL - .NET Standard 2.0 のサポート) を参照してください。

**構成ビルダーのサポート**

構成ビルダーを使用して、開発者が、実行時にアプリケーションの構成設定を動的に注入およびビルドすることができます。 カスタム構成ビルダーを使用して、構成セクションの既存のデータを変更したり、最初から完全に構成セクション全体をビルドしたりすることができます。 構成ビルダーを使用しない場合は、構成ファイルは静的であり、それらの設定はアプリケーションが起動されるしばらく前に定義されます。

カスタム構成ビルダーを作成するには、抽象 <xref:System.Configuration.ConfigurationBuilder> クラスからビルダーを派生させ、その <xref:System.Configuration.ConfigurationBuilder.ProcessConfigurationSection%2A?displayProperty=nameWithType> と <xref:System.Configuration.ConfigurationBuilder.ProcessRawXml%2A?displayProperty=nameWithType> をオーバーライドします。 また、.config ファイルでもビルダーを定義します。 詳細については、「[.NET Framework 4.7.1 ASP.NET and Configuration Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/) 」(.NET Framework 4.7.1 ASP.NET と構成機能) ブログ投稿の「Configuration Builders」(構成ビルダー) セクションを参照してください。

**実行時の機能の検出**

<xref:System.Runtime.CompilerServices.RuntimeFeature?displayProperty=nameWithType> クラスは、コンパイル時または実行時に特定の .NET の実装上で事前に定義された機能がサポートされているかどうかを判断するためのメカニズムを提供します。 コンパイラは、コンパイル時に、指定されたフィールドが存在するかどうかを確認し、その機能がサポートされているかどうかを判断します。サポートされている場合は、その機能を利用するコードを生成できます。 実行時に、アプリケーションは、コードを生成する前に <xref:System.Runtime.CompilerServices.RuntimeFeature.IsSupported%2A?displayProperty=nameWithType> メソッドを呼び出すことができます。 詳細については、「[Add helper method to describe features supported by the runtime](https://github.com/dotnet/corefx/issues/17116)」(ランタイムでサポートされる機能を記述するヘルパー メソッドを追加する) を参照してください。

**値のタプル型はシリアル化できる**

.NET Framework 4.7.1 以降、<xref:System.ValueTuple?displayProperty=nameWithType> および関連するジェネリック型は、[Serializable](xref:System.SerializableAttribute) としてマークされ、バイナリのシリアル化ができるようになりました。 これにより、<xref:System.Tuple%603> や <xref:System.Tuple%604> などのタプル型を簡単に値のタプル型に移行できます。 詳細については、「[.NET Framework 4.7.1 Runtime and Compiler Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/)」(.NET Framework 4.7.1 ランタイムとコンパイラの機能) ブログ投稿の「Compiler -- ValueTuple is Serializable」(コンパイラ -- 値タプルはシリアル化できる) を参照してください。

**読み取り専用の参照のサポート**

.NET Framework 4.7.1 では、<xref:System.Runtime.CompilerServices.IsReadOnlyAttribute?displayProperty=nameWithType> が追加されました。 この属性は、読み取り専用の ref 戻り値型またはパラメーターを持つメンバーをマークする言語コンパイラで使用します。 詳細については、「[.NET Framework 4.7.1 Runtime and Compiler Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/)」(.NET Framework 4.7.1 ランタイムとコンパイラの機能) ブログ投稿の「Compiler - Support for ReadOnlyReferences」(コンパイラ - ReadOnlyReferences のサポート) を参照してください。 参照戻り値の詳細については、[参照戻り値と参照ローカル変数 (C# Guide)](../../csharp/programming-guide/classes-and-structs/ref-returns.md)および[参照戻り値 (Visual Basic)](../../visual-basic/programming-guide/language-features/procedures/ref-return-values.md)に関するページを参照してください。

<a name="clr" />

#### <a name="common-language-runtime-clr"></a>共通言語ランタイム (CLR)

**ガベージ コレクションのパフォーマンス改善**

.NET Framework 4.7.1 でのガベージ コレクション (GC) に対する変更により、特に大きなオブジェクト ヒープ (LOH) の割り当ての場合に全体的なパフォーマンスが向上します。 .NET Framework 4.7.1 では、小さなオブジェクト ヒープ (SOH) と LOH の割り当てに個別のロックが使用されます。これにより、バックグラウンド GC で SOH をスイープするときに、LOH を割り当てることができます。 その結果、多数の LOH の割り当てを行うアプリケーションでは、割り当てのロックの競合が減少し、パフォーマンスが向上します。 詳細については、「[.NET Framework 4.7.1 Runtime and Compiler Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/)」(.NET Framework 4.7.1 ランタイムとコンパイラの機能) ブログ投稿の「Runtime -- GC Performance Improvements」(ランタイム - GC のパフォーマンスの向上) セクションを参照してください。

<a name="net471"/>

#### <a name="networking"></a>ネットワーキング

**Message.HashAlgorithm 用の SHA-2 のサポート**

.NET Framework 4.7 およびそれ以前のバージョンの <xref:System.Messaging.Message.HashAlgorithm%2A?displayProperty=nameWithType> プロパティでは、値 <xref:System.Messaging.HashAlgorithm.Md5?displayProperty=nameWithType> および <xref:System.Messaging.HashAlgorithm.Sha?displayProperty=nameWithType> のみがサポートされていました。 .NET Framework 4.7.1 以降では、<xref:System.Messaging.HashAlgorithm.Sha256?displayProperty=nameWithType>、<xref:System.Messaging.HashAlgorithm.Sha384?displayProperty=nameWithType>、<xref:System.Messaging.HashAlgorithm.Sha512?displayProperty=nameWithType> もサポートされます。 <xref:System.Messaging.Message> インスタンス自体は、ハッシュは行わず、MSMQ に値を渡すだけですなので、この値が実際に使用されるかどうかは MSMQ によって異なります。 詳細については、「[.NET Framework 4.7.1 ASP.NET and Configuration Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/) 」(.NET Framework 4.7.1 ASP.NET と構成機能) ブログ投稿の「SHA-2 support for Message.HashAlgorithm」(Message.HashAlgorithm 用の SHA-2 のサポート) セクションを参照してください。

<a name="asp-net471" />

#### <a name="aspnet"></a>ASP.NET

**ASP.NET アプリケーションの実行ステップ**

ASP.NET は、23 のイベントを含む定義済みのパイプラインで要求を処理します。 ASP.NET は、実行ステップとして、各イベント ハンドラーを実行します。 .NET Framework 4.7 までの ASP.NET のバージョンでは、ASP.NET は、ネイティブ スレッドとマネージド スレッドの切り替えのために、実行コンテキストをフローすることはできません。 代わりに、ASP.NET は、<xref:System.Web.HttpContext> のみを選択的にフローします。 .NET Framework 4.7.1 以降、<xref:System.Web.HttpApplication.OnExecuteRequestStep(System.Action{System.Web.HttpContextBase,System.Action})?displayProperty=nameWithType> メソッドでは、モジュールがアンビエント データを復元することもできます。 この機能は、アプリケーションの実行フローを考慮する、トレース、プロファイリング、診断、トランザクションなどに関連するライブラリを対象とします。 詳細については、「[.NET Framework 4.7.1 ASP.NET and Configuration Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/) 」(.NET Framework 4.7.1 ASP.NET と構成機能) ブログ投稿の「ASP.NET Execution Step Feature」(ASP.NET 実行ステップの機能) を参照してください。

**ASP.NET HttpCookie 解析**

.NET Framework 4.7.1 には、新しいメソッド <xref:System.Web.HttpCookie.TryParse%2A?displayProperty=nameWithType> が含まれています。これは、文字列から <xref:System.Web.HttpCookie> オブジェクトを作成し、有効期限日やパスなどの cookie の値を正確に割り当てるための標準化された方法を提供します。 詳細については、「[.NET Framework 4.7.1 ASP.NET and Configuration Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/) 」(.NET Framework 4.7.1 ASP.NET と構成機能) ブログ投稿の「ASP.NET HttpCookie parsing」(ASP.NET HttpCookie の解析) を参照してください。

**ASP.NET フォーム認証資格情報の SHA-2 ハッシュ オプション**

.NET Framework 4.7 およびそれ以前のバージョンの ASP.NET では、開発者は、MD5 または SHA1 を使用して、ユーザーの資格情報とハッシュされたパスワードを構成ファイルに格納できました。 .NET Framework 4.7.1 以降の ASP.NET では、SHA256、SHA384、SHA512 などの新しい安全な SHA-2 ハッシュ オプションもサポートされるようになっています。 SHA1 は既定のまま残り、Web 構成ファイルで既定以外のハッシュ アルゴリズムを定義することができます。 次に例を示します。

```xml
<system.web>
    <authentication mode="Forms">
        <forms loginUrl="~/login.aspx">
          <credentials passwordFormat="SHA512">
            <user name="jdoe" password="6D003E98EA1C7F04ABF8FCB375388907B7F3EE06F278DB966BE960E7CBBD103DF30CA6D61F7E7FD981B2E4E3A64D43C836A4BEDCA165C33B163E6BCDC538A664" />
          </credentials>
        </forms>
    </authentication>
</system.web>
```

<a name="v47" />

## <a name="whats-new-in-net-framework-47"></a>.NET Framework 4.7 の新機能

.NET Framework 4.7 には、次の領域における新機能が含まれています。

- [基底クラス](#Core47)
- [ネットワーク](#net47)
- [ASP.NET](#ASP-NET47)
- [Windows Communication Foundation (WCF)](#wcf47)
- [Windows フォーム](#wf47)
- [Windows Presentation Foundation (WPF)](#WPF47)

.NET Framework 4.7 に追加された新しい API の一覧については、GitHub で [.NET Framework 4.7 API の変更点](https://github.com/Microsoft/dotnet/blob/master/releases/net47/dotnet47-api-changes.md)に関するページをご覧ください。 .NET Framework 4.7 における機能の改善とバグ修正の一覧については、GitHub で「[.NET Framework 4.7 List of Changes (.NET Framework 4.7 の変更点の一覧)](https://github.com/Microsoft/dotnet/blob/master/releases/net47/dotnet47-changes.md)」を参照してください。 詳細については、.NET ブログの「[.NET Framework 4.7 の発表](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-7/)」を参照してください。

<a name="Core47" />

#### <a name="base-classes"></a>基底クラス

.NET Framework 4.7 では、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> によるシリアル化が改善されています。

**楕円曲線暗号 (ECC) による機能強化***

.NET Framework 4.7 では、`ImportParameters(ECParameters)` メソッドが <xref:System.Security.Cryptography.ECDsa> クラスと <xref:System.Security.Cryptography.ECDiffieHellman> クラスに追加され、既に確立されたキーをオブジェクトで表すことができるようになりました。 明示的な曲線パラメーターを使ってキーをエクスポートするための `ExportParameters(Boolean)` メソッドも追加されました。

また、.NET Framework 4.7 では、その他の曲線 (Brainpool 曲線スイートなど) のサポートと、新しい <xref:System.Security.Cryptography.ECDsa.Create%2A> および <xref:System.Security.Cryptography.ECDiffieHellman.Create%2A> ファクトリ メソッドによる作成しやすい事前定義も追加されました。

GitHub で [.NET Framework 4.7 の暗号化の向上の例](https://gist.github.com/richlander/5a182899895a87a296c21ada97f7a54e)を見ることができます。

**DataContractJsonSerializer による制御文字のサポートの向上**

.NET Framework 4.7 の <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> では、ECMAScript 6 標準に準拠して制御文字がシリアル化されます。 この動作は、.NET Framework 4.7 を対象とするアプリケーションでは既定で有効になり、.NET Framework 4.7 で実行していても対象が以前のバージョンの .NET Framework であるアプリケーションの場合はオプトイン機能です。 詳細については、「[.NET Framework 4.7 における再ターゲットの変更点](../migration-guide/retargeting-changes-in-the-net-framework-4-7.md)」をご覧ください。

<a name="net47" />

#### <a name="networking"></a>ネットワーキング

.NET Framework 4.7 では、次のネットワーク関連機能が追加されています。

**TLS プロトコルの既定のオペレーティング システム サポート***

<xref:System.Net.Security.SslStream?displayProperty=nameWithType> および HTTP、FTP、SMTP などの上位スタック コンポーネントによって使われる TLS スタックにより、開発者はオペレーティング システムによってサポートされる既定の TLS プロトコルを使うことができます。 TLS バージョンをハード コーディングする必要はなくなりました。

<a name="ASP-NET47" />

#### <a name="aspnet"></a>ASP.NET

.NET Framework 4.7 の ASP.NET には、次の新機能が含まれます。

**オブジェクト キャッシュの拡張性**

.NET Framework 4.7 以降では、ASP.NET で追加された新しい API セットを使うことで、開発者は、メモリ内オブジェクト キャッシュとメモリ監視に関する既定の ASP.NET の実装を置き換えることができます。 ASP.NET の実装が適切ではない場合、次の 3 つのコンポーネントを置き換えることができるようになりました。

- **オブジェクト キャッシュ ストア**。 新しいキャッシュ プロバイダー構成セクションを使うことで、開発者は、新しい **ICacheStoreProvider** インターフェイスを使って、ASP.NET アプリケーション用のオブジェクト キャッシュの新しい実装をプラグインできます。

- **メモリ監視**。 ASP.NET の既定のメモリ モニターは、アプリケーションがプロセスに構成されているプライベート バイト制限に近づくと、またはコンピューターの使用可能な合計物理 RAM が低下すると、アプリケーションに通知します。 これらの制限に近づくと、通知が発生します。 一部のアプリケーションでは、通知の発生が構成されている制限に近すぎるため、有効な対応を取れません。 開発者は、<xref:System.Web.Hosting.ApplicationMonitors.MemoryMonitor%2A?displayProperty=nameWithType> プロパティを使用して既定値を置き換える独自のメモリ モニターを作成できるようになりました。

- **メモリ制限への対応**。 既定では、プライベート バイト プロセス制限が近づくと、ASP.NET はオブジェクト キャッシュの削減を試み、<xref:System.GC.Collect%2A?displayProperty=nameWithType> を定期的に呼び出します。 アプリケーションによっては、<xref:System.GC.Collect%2A?displayProperty=nameWithType> の呼び出し頻度またはトリミングされるキャッシュ量が非効率になります。 開発者は、アプリケーションのメモリ モニターに **IObserver** の実装をサブスクライブすることにより、既定の動作を置換または補完できます。

<a name="wcf47" />

#### <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

Windows Communication Foundation (WCF) では以下の機能が追加または変更されました。

**既定のメッセージ セキュリティ設定を TLS1.1 または TLS1.2 に構成する機能。**

.NET Framework 4.7 以降の WCF では、既定のメッセージ セキュリティ プロトコルとして、SSL 3.0 と TSL 1.0 に加えて、TSL 1.1 または TLS 1.2 を構成できます。 これはオプトイン設定です。有効にするには、アプリケーション構成ファイルに次のエントリを追加する必要があります。

```xml
<runtime>
   <AppContextSwitchOverrides value="Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols=false;Switch.System.Net.DontEnableSchUseStrongCrypto=false" />
</runtime>
```

**WCF アプリケーションと WCF シリアル化の信頼性の向上**

WCF に競合状態を除去する複数のコード変更が追加され、シリアル化オプションの信頼性とパフォーマンスが向上しています。 次の設定があります。

- **SocketConnection.BeginRead** および **SocketConnection.Read** の呼び出しでの非同期コードと同期コードの併用のサポートの向上。
- **SharedConnectionListener** および **DuplexChannelBinder** で接続を中止するときの信頼性の向上。
- <xref:System.Runtime.Serialization.FormatterServices.GetSerializableMembers%28System.Type%29?displayProperty=nameWithType> メソッドの呼び出し時のシリアル化操作の信頼性を向上しました。
- **ChannelSynchronizer.RemoveWaiter** メソッドを呼び出して待機を削除するときの信頼性の向上。

<a name="wf47" />

#### <a name="windows-forms"></a>Windows フォーム

.NET Framework 4.7 では、Windows フォームによる高 DPI モニターのサポートが向上しました。

**高 DPI のサポート**

.NET Framework 4.7 を対象とするアプリケーションでは、Windows フォーム アプリケーションに対して高 DPI および動的 DPI がサポートされるようになっています。 高 DPI のサポートにより、フォームのレイアウトと外観および高 DPI モニターでのコントロールが向上します。 動的 DPI は、ユーザーが実行中のアプリケーションの DPI または表示倍率を変更すると、フォームのレイアウトと外観およびコントロールを変更します。

高 DPI のサポートはオプトイン機能であり、アプリケーション構成ファイルの [\<System.Windows.Forms.ConfigurationSection>](../configure-apps/file-schema/winforms/index.md) セクションを定義することで構成します。 Windows フォーム アプリケーションへの高 DPI サポートおよび動的 DPI サポート追加の詳細については、「[Windows フォームの高 DPI サポート](../winforms/high-dpi-support-in-windows-forms.md)」をご覧ください。

<a name="WPF47" />

#### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

.NET Framework 4.7 では、WPF の機能が次のように強化されています。

**Windows WM_POINTER メッセージに基づくタッチ/スタイラス スタックのサポート**

Windows Ink Services Platform (WISP) の代わりに [WM_POINTER メッセージ](https://docs.microsoft.com/previous-versions/windows/desktop/InputMsg/messages)に基づいてタッチ/スタイラス スタックを使うオプションが追加されました。 これは、.NET Framework のオプトイン機能です。 詳細については、「[.NET Framework 4.7 における再ターゲットの変更点](../migration-guide/retargeting-changes-in-the-net-framework-4-7.md)」をご覧ください。

**WPF 印刷 API の新しい実装**

<xref:System.Printing.PrintQueue?displayProperty=nameWithType> クラスの WPF 印刷 API は、非推奨になった [XPS 印刷 API](/windows/desktop/printdocs/xps-printing) ではなく Windows [ドキュメント印刷パッケージ API](/windows/desktop/printdocs/tailored-app-printing-api) を呼び出します。 アプリケーションの互換性に対するこの変更の影響については、「[.NET Framework 4.7 における再ターゲットの変更点](../migration-guide/retargeting-changes-in-the-net-framework-4-7.md)」をご覧ください。

<a name="v462" />

## <a name="whats-new-in-net-framework-462"></a>.NET Framework 4.6.2 の新機能

.NET Framework 4.6.2 には、次の領域における新機能が含まれています。

- [ASP.NET](#ASPNET462)

- [文字カテゴリ](#Strings)

- [暗号](#Crypto462)

- [SQLClient](#SQLClient)

- [Windows Communication Foundation](#WCF)

- [Windows Presentation Foundation (WPF)](#WPF462)

- [Windows Workflow Foundation (WF)](#WF462)

- [ClickOnce](#clickonce-1)

- [Windows フォームおよび WPF アプリの UWP アプリへの変換](#UWPConvert)

- [デバッグの機能強化](#Debug462)

.NET Framework 4.6.2 に追加された新しい API の一覧については、GitHub で「[.NET Framework 4.6.2 API Changes (.NET Framework 4.6.2 API の変更点)](https://github.com/Microsoft/dotnet/blob/master/releases/net462/dotnet462-api-changes.md)」をご覧ください。 .NET Framework 4.6.2 における機能の改善とバグ修正の一覧については、GitHub で「[.NET Framework 4.6.2 List of Changes (.NET Framework 4.6.2 の変更点の一覧)](https://github.com/Microsoft/dotnet/blob/master/releases/net462/dotnet462-changes.md)」を参照してください。 詳細については、.NET ブログの「[.NET Framework 4.6.2 の発表](https://devblogs.microsoft.com/dotnet/announcing-net-framework-4-6-2/)」を参照してください。

<a name="ASPNET462" />

### <a name="aspnet"></a>ASP.NET

.NET Framework 4.6.2 では、ASP.NET の機能が次のように強化されています。

**データ注釈検証コントロールのローカライズされたエラー メッセージのサポート強化**

データ注釈検証コントロールを使用して、1 つ以上の属性をクラス プロパティに追加して検証を行うことができます。 属性の <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.ErrorMessage%2A?displayProperty=nameWithType> 要素は、検証が失敗した場合にエラー メッセージのテキストを定義します。 .NET Framework 4.6.2 以降では、ASP.NET でエラー メッセージを簡単にローカライズできます。 エラー メッセージは次のような場合にローカライズされます。

1. <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.ErrorMessage%2A?displayProperty=nameWithType> が検証属性で指定されている。

2. リソース ファイルが App_LocalResources フォルダーに格納されている。

3. ローカライズされたリソース ファイル名の形式が `DataAnnotation.Localization.{`*名前*`}.resx` (この*名前*は、*languageCode*`-`*country/regionCode* または *languageCode* 形式のカルチャ名) である。

4. リソースのキー名が <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.ErrorMessage%2A?displayProperty=nameWithType> 属性に割り当てられている文字列で、その値がローカライズされたエラー メッセージである。

たとえば、次のデータ注釈属性では、無効な評価の場合に表示する、既定のカルチャのエラー メッセージを定義します。

```csharp
public class RatingInfo
{
   [Required(ErrorMessage = "The rating must be between 1 and 10.")]
   [Display(Name = "Your Rating")]
   public int Rating { get; set; }
}
```

```vb
Public Class RatingInfo
   <Required(ErrorMessage = "The rating must be between 1 and 10.")>
   <Display(Name = "Your Rating")>
   Public Property Rating As Integer = 1
End Class
```

キーがエラー メッセージ文字列であり、その値がローカライズされたエラー メッセージであるようにリソース ファイル DataAnnotation.Localization.fr.resx を作成します。 ファイルは `App.LocalResources` フォルダーになければいけません。 たとえば下記は、キーとその値で、値はローカライズされたフランス語 (fr) のエラー メッセージです。

| 名前                                 | [値]                                     |
| ------------------------------------ | ----------------------------------------- |
| 評価は、1 から 10 の範囲である必要があります。 | La note doit être comprise entre 1 et 10. |

 また、データ注釈ローカリゼーションを拡張することができます。 開発者は、<xref:System.Web.Globalization.IStringLocalizerProvider> インターフェイスを実装してリソース ファイル以外の場所にローカリゼーション文字列を格納することで、独自の文字列ローカライザー プロバイダーにプラグインできます。

 **セッション状態ストア プロバイダーでの非同期サポート**

 ASP.NET では、セッション状態ストア プロバイダーでタスクを返すメソッドを使用できるようになりました。これにより、ASP.NET アプリで非同期のスケーラビリティのメリットが得られます。 セッション状態ストア プロバイダーでの非同期操作をサポートするために、ASP.NET には新しいインターフェイスである <xref:System.Web.SessionState.ISessionStateModule?displayProperty=nameWithType> が含まれています。これは <xref:System.Web.IHttpModule> から継承され、開発者はこれを使用して独自のセッション状態モジュールと非同期セッション ストア プロバイダーを実装することができます。 このインターフェイスは次のように定義されます。

```csharp
public interface ISessionStateModule : IHttpModule {
    void ReleaseSessionState(HttpContext context);
    Task ReleaseSessionStateAsync(HttpContext context);
}
```

```vb
Public Interface ISessionStateModule : Inherits IHttpModule
   Sub ReleaseSessionState(context As HttpContext)
   Function ReleaseSessionStateAsync(context As HttpContext) As Task
End Interface
```

 また、<xref:System.Web.SessionState.SessionStateUtility> クラスには <xref:System.Web.SessionState.SessionStateUtility.IsSessionStateReadOnly%2A> および <xref:System.Web.SessionState.SessionStateUtility.IsSessionStateRequired%2A> という 2 つの新しいメソッドが含まれています。これらを使用して、非同期操作をサポートできます。

 **出力キャッシュ プロバイダーの非同期サポート**

 .NET Framework 4.6.2 以降では、タスクを返すメソッドを出力キャッシュ プロバイダーで使って、非同期のスケーラビリティのメリットを得ることができます。  これらのメソッドを実装するプロバイダーは、Web サーバー上のスレッド ブロックを減らし、ASP.NET サービスのスケーラビリティを向上させます。

 非同期出力キャッシュ プロバイダーをサポートするために、次の API が追加されました。

- <xref:System.Web.Caching.OutputCacheProvider?displayProperty=nameWithType> から継承される <xref:System.Web.Caching.OutputCacheProviderAsync?displayProperty=nameWithType> クラス。これにより、開発者は非同期出力キャッシュ プロバイダーを実装できます。

- <xref:System.Web.Caching.OutputCacheUtility> クラス。出力キャッシュを構成するためのヘルパー メソッドを提供します。

- <xref:System.Web.HttpCachePolicy?displayProperty=nameWithType> クラスの新しい 18 個のメソッド。 これらのメソッドには、<xref:System.Web.HttpCachePolicy.GetCacheability%2A>、<xref:System.Web.HttpCachePolicy.GetCacheExtensions%2A>、<xref:System.Web.HttpCachePolicy.GetETag%2A>、<xref:System.Web.HttpCachePolicy.GetETagFromFileDependencies%2A>、<xref:System.Web.HttpCachePolicy.GetMaxAge%2A>、<xref:System.Web.HttpCachePolicy.GetMaxAge%2A>、<xref:System.Web.HttpCachePolicy.GetNoStore%2A>、<xref:System.Web.HttpCachePolicy.GetNoTransforms%2A>、<xref:System.Web.HttpCachePolicy.GetOmitVaryStar%2A>、<xref:System.Web.HttpCachePolicy.GetProxyMaxAge%2A>、<xref:System.Web.HttpCachePolicy.GetRevalidation%2A>、<xref:System.Web.HttpCachePolicy.GetUtcLastModified%2A>、<xref:System.Web.HttpCachePolicy.GetVaryByCustom%2A>、<xref:System.Web.HttpCachePolicy.HasSlidingExpiration%2A>、および <xref:System.Web.HttpCachePolicy.IsValidUntilExpires%2A> が含まれます。

- <xref:System.Web.HttpCacheVaryByContentEncodings?displayProperty=nameWithType> クラスの新しい 2 つのメソッド (<xref:System.Web.HttpCacheVaryByContentEncodings.GetContentEncodings%2A> と <xref:System.Web.HttpCacheVaryByContentEncodings.SetContentEncodings%2A>)。

- <xref:System.Web.HttpCacheVaryByHeaders?displayProperty=nameWithType> クラスの新しい 2 つのメソッド (<xref:System.Web.HttpCacheVaryByHeaders.GetHeaders%2A> と <xref:System.Web.HttpCacheVaryByHeaders.SetHeaders%2A>)。

- <xref:System.Web.HttpCacheVaryByParams?displayProperty=nameWithType> クラスの新しい 2 つのメソッド (<xref:System.Web.HttpCacheVaryByParams.GetParams%2A> と <xref:System.Web.HttpCacheVaryByParams.SetParams%2A>)。

- <xref:System.Web.Caching.AggregateCacheDependency?displayProperty=nameWithType> クラスの <xref:System.Web.Caching.AggregateCacheDependency.GetFileDependencies%2A> メソッド。

- <xref:System.Web.Caching.CacheDependency> の <xref:System.Web.Caching.CacheDependency.GetFileDependencies%2A> メソッド。

<a name="Strings" />

### <a name="character-categories"></a>文字カテゴリ

.NET Framework 4.6.2 の文字は [Unicode Standard バージョン 8.0.0](https://www.unicode.org/versions/Unicode8.0.0/) に基づいて分類されます。 .NET Framework 4.6 と .NET Framework 4.6.1 では、文字は Unicode 6.3 文字のカテゴリに基づいて分類されました。

Unicode 8.0 のサポートは、<xref:System.Globalization.CharUnicodeInfo> クラスによる文字列の分類およびそれに依存する型とメソッドに限定されます。 これらには、<xref:System.Globalization.StringInfo> クラス、オーバーロードされた <xref:System.Char.GetUnicodeCategory%2A?displayProperty=nameWithType> メソッド、および .NET Framework の正規表現エンジンによって認識される[文字クラス](../../standard/base-types/character-classes-in-regular-expressions.md)が含まれます。  文字や文字列の比較と並べ替えは、この変更の影響を受けることなく、引き続き基となるオペレーティング システムに依存します (Windows 7 システムの場合は、.NET Framework で提供される文字データに依存)。

Unicode 6.0 から Unicode 7.0 への文字カテゴリの変更については、Unicode Consortium Web サイトの [Unicode Standard バージョン 7.0.0](https://www.unicode.org/versions/Unicode7.0.0/) に関する記述を参照してください。 Unicode 7.0 から Unicode 8.0 への変更については、Unicode Consortium の Web サイトの [Unicode Standard バージョン 8.0.0](https://www.unicode.org/versions/Unicode8.0.0/) に関する記述を参照してください。

<a name="Crypto462" />

### <a name="cryptography"></a>暗号

**FIPS 186-3 DSA を含む X509 証明書のサポート**

.NET Framework 4.6.2 には、FIPS 186-2 の 1024 ビットという制限を超えるキーを持つ、DSA (Digital Signature Algorithm) X509 証明書のサポートが追加されています。

より大きな FIPS 186-3 のキー サイズのサポートに加えて、.NET Framework 4.6.2 では、SHA-2 ファミリのハッシュ アルゴリズム (SHA256、SHA384、SHA512) によるシグネチャ計算も可能です。 FIPS 186-3 のサポートは新しい <xref:System.Security.Cryptography.DSACng?displayProperty=nameWithType> クラスで提供されます。

.NET Framework 4.6 の <xref:System.Security.Cryptography.RSA> クラスと .NET Framework 4.6.1 の <xref:System.Security.Cryptography.ECDsa> クラスの最近の変更に合わせて、.NET Framework 4.6.2 の <xref:System.Security.Cryptography.DSA> 抽象基本クラスには追加のメソッドがあり、これによって呼び出し元はこの機能をキャストせずに使用することができます。 データに署名する場合は、以下の例のように、<xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPrivateKey%2A?displayProperty=nameWithType> 拡張メソッドを呼び出すことができます。

```csharp
public static byte[] SignDataDsaSha384(byte[] data, X509Certificate2 cert)
{
    using (DSA dsa = cert.GetDSAPrivateKey())
    {
        return dsa.SignData(data, HashAlgorithmName.SHA384);
    }
}
```

```vb
Public Shared Function SignDataDsaSha384(data As Byte(), cert As X509Certificate2) As Byte()
    Using DSA As DSA = cert.GetDSAPrivateKey()
        Return DSA.SignData(data, HashAlgorithmName.SHA384)
    End Using
End Function
```

署名データを検証する場合は、以下の例のように、<xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPublicKey%2A?displayProperty=nameWithType> 拡張メソッドを呼び出すことができます。

```csharp
public static bool VerifyDataDsaSha384(byte[] data, byte[] signature, X509Certificate2 cert)
{
    using (DSA dsa = cert.GetDSAPublicKey())
    {
        return dsa.VerifyData(data, signature, HashAlgorithmName.SHA384);
    }
}
```

```vb
 Public Shared Function VerifyDataDsaSha384(data As Byte(), signature As Byte(), cert As X509Certificate2) As Boolean
    Using dsa As DSA = cert.GetDSAPublicKey()
        Return dsa.VerifyData(data, signature, HashAlgorithmName.SHA384)
    End Using
End Function
```

**ECDiffieHellman キー派生ルーチンへの入力をよりわかりやすく**

.NET Framework 3.5 では、3 種類の KDF (キー派生関数) ルーチンによる Elliptic Curve Diffie-Hellman キー承諾のサポートが追加されました。 ルーチンへの入力、およびルーチン自体は <xref:System.Security.Cryptography.ECDiffieHellmanCng> オブジェクトのプロパティで構成されました。 しかし、すべてのルーチンですべての入力プロパティが読み取られるわけではないため、これまで開発者がよく混乱することがありました。

.NET Framework 4.6.2 ではこれに対処するために、次の 3 つのメソッドが <xref:System.Security.Cryptography.ECDiffieHellman> 基底クラスに追加され、これらの KDF ルーチンとその入力がよりわかりやくなりました。

|ECDiffieHellman メソッド|説明|
|----------------------------|-----------------|
|<xref:System.Security.Cryptography.ECDiffieHellman.DeriveKeyFromHash%28System.Security.Cryptography.ECDiffieHellmanPublicKey%2CSystem.Security.Cryptography.HashAlgorithmName%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%29>|次の式を使用してキー マテリアルを派生させます。<br /><br /> HASH(secretPrepend &#124;&#124; *x* &#124;&#124; secretAppend)<br /><br /> HASH(secretPrepend OrElse *x* OrElse secretAppend)<br /><br /> ここで *x* は、EC Diffie-Hellman アルゴリズムの計算結果を表します。|
|<xref:System.Security.Cryptography.ECDiffieHellman.DeriveKeyFromHmac%28System.Security.Cryptography.ECDiffieHellmanPublicKey%2CSystem.Security.Cryptography.HashAlgorithmName%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%29>|次の式を使用してキー マテリアルを派生させます。<br /><br /> HMAC(hmacKey, secretPrepend &#124;&#124; *x* &#124;&#124; secretAppend)<br /><br /> HMAC(hmacKey, secretPrepend OrElse *x* OrElse secretAppend)<br /><br /> ここで *x* は、EC Diffie-Hellman アルゴリズムの計算結果を表します。|
|<xref:System.Security.Cryptography.ECDiffieHellman.DeriveKeyTls%28System.Security.Cryptography.ECDiffieHellmanPublicKey%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%29>|TLS 擬似乱数関数 (PRF) 派生アルゴリズムを使用して、キー マテリアルを派生させます。|

**永続化されたキーによる対称暗号化のサポート**

Windows の暗号化ライブラリ (CNG) では、永続化された対称キーの格納とハードウェアに格納された対称キーの使用のサポートが追加され、.NET Framework 4.6.2 で開発者はこの機能を利用できるようになりました。  キー名とキー プロバイダーの概念が実装に固有であるため、この機能を使用するには、推奨されるファクトリ手法 (`Aes.Create` の呼び出しなど) ではなく、具象実装型のコンストラクターを利用する必要があります。

永続化されたキーによる対称暗号化は、AES (<xref:System.Security.Cryptography.AesCng>) と 3DES (<xref:System.Security.Cryptography.TripleDESCng>) アルゴリズムでサポートされます。 次に例を示します。

```csharp
public static byte[] EncryptDataWithPersistedKey(byte[] data, byte[] iv)
{
    using (Aes aes = new AesCng("AesDemoKey", CngProvider.MicrosoftSoftwareKeyStorageProvider))
    {
        aes.IV = iv;

        // Using the zero-argument overload is required to make use of the persisted key
        using (ICryptoTransform encryptor = aes.CreateEncryptor())
        {
            if (!encryptor.CanTransformMultipleBlocks)
            {
                throw new InvalidOperationException("This is a sample, this case wasn’t handled...");
            }

            return encryptor.TransformFinalBlock(data, 0, data.Length);
        }
    }
}
```

```vb
Public Shared Function EncryptDataWithPersistedKey(data As Byte(), iv As Byte()) As Byte()
    Using Aes As Aes = New AesCng("AesDemoKey", CngProvider.MicrosoftSoftwareKeyStorageProvider)
        Aes.IV = iv

        ' Using the zero-argument overload Is required to make use of the persisted key
        Using encryptor As ICryptoTransform = Aes.CreateEncryptor()
            If Not encryptor.CanTransformMultipleBlocks Then
                Throw New InvalidOperationException("This is a sample, this case wasn’t handled...")
            End If
            Return encryptor.TransformFinalBlock(data, 0, data.Length)
        End Using
    End Using
End Function
```

**SHA-2 ハッシュの SignedXml サポート**

.NET Framework 4.6.2 では <xref:System.Security.Cryptography.Xml.SignedXml> クラスに対するサポートが追加され、RSA-SHA256、RSA-SHA384、RSA-SHA512 の各 PKCS#1 署名メソッド、および SHA256、SHA384、SHA512 の各参照ダイジェスト アルゴリズムが使うことができるようになりました。

以下のように、URI 定数はすべて <xref:System.Security.Cryptography.Xml.SignedXml> で示されます。

|SignedXml フィールド|定数|
|---------------------|--------------|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigSHA256Url>|"http://www.w3.org/2001/04/xmlenc#sha256"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigRSASHA256Url>|"http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigSHA384Url>|"http://www.w3.org/2001/04/xmldsig-more#sha384"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigRSASHA384Url>|"http://www.w3.org/2001/04/xmldsig-more#rsa-sha384"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigSHA512Url>|"http://www.w3.org/2001/04/xmlenc#sha512"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigRSASHA512Url>|"http://www.w3.org/2001/04/xmldsig-more#rsa-sha512"|

 これらのアルゴリズムのサポートを追加するためにカスタムの <xref:System.Security.Cryptography.SignatureDescription> ハンドラーを <xref:System.Security.Cryptography.CryptoConfig> に登録していたプログラムはすべて従来どおり引き続き機能しますが、既定でプラットフォームでサポートされるようになったため、<xref:System.Security.Cryptography.CryptoConfig> への登録は不要になりました。

<a name="SQLClient" />

### <a name="sqlclient"></a>SqlClient

.NET Framework 4.6.2 では、.NET Framework Data Provider for SQL Server (<xref:System.Data.SqlClient?displayProperty=nameWithType>) に次の新機能が含まれています。

**Azure SQL Database への接続プールとタイムアウト**

接続プールが有効な状態で、タイムアウトまたは他のログイン エラーが発生した場合は、例外がキャッシュされ、キャッシュされた例外は次の 5 秒から 1 分の間の後続の接続試行時にすべてスローされます。 詳しくは、「[SQL Server の接続プール (ADO.NET)](../data/adonet/sql-server-connection-pooling.md)」をご覧ください。

通常は迅速に復旧される一時的なエラーで接続試行が失敗する可能性があるため、Azure SQL Database への接続時のこの動作は望ましくありません。 接続試行操作をより最適化するため、Azure SQL Database への接続が失敗した場合は、接続プールのブロック期間の動作は削除されます。

新しい `PoolBlockingPeriod` キーワードを追加することで、ご利用のアプリに最適なブロック期間を選択できます。 次の値が含まれます。

<xref:System.Data.SqlClient.PoolBlockingPeriod.Auto>

Azure SQL Database に接続しているアプリケーションの接続プールのブロック期間は無効になり、他のすべての SQL Server インスタンスに接続しているアプリケーションの接続プールのブロック期間が有効になります。 これが既定値です。 サーバー エンドポイント名の末尾が以下のいずれかである場合は、Azure SQL Database と見なされます。

- .database.windows.net

- .database.chinacloudapi.cn

- .database.usgovcloudapi.net

- .database.cloudapi.de

<xref:System.Data.SqlClient.PoolBlockingPeriod.AlwaysBlock>

接続プールのブロック期間は常に有効になります。

<xref:System.Data.SqlClient.PoolBlockingPeriod.NeverBlock>

接続プールのブロック期間は常に無効になります。

**Always Encrypted の強化**

SQLClient では、Always Encrypted の以下の 2 つの機能が強化されました。

- 暗号化されたデータベースの列に対するパラメーター クエリのパフォーマンスを向上させるために、クエリ パラメーターの暗号化メタデータがキャッシュされるようになりました。 <xref:System.Data.SqlClient.SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled%2A?displayProperty=nameWithType> プロパティが `true` (既定値) に設定されている状態で、同じクエリが複数回呼び出された場合、クライアントはサーバーからパラメーターのメタデータを 1 回だけ取得します。

- キー キャッシュ内の列暗号化キー エントリは、<xref:System.Data.SqlClient.SqlConnection.ColumnEncryptionKeyCacheTtl%2A?displayProperty=nameWithType> プロパティを使用して設定された構成可能な時間間隔後に削除されるようになりました。

<a name="WCF" />

### <a name="windows-communication-foundation"></a>Windows Communication Foundation

.NET Framework 4.6.2 では、Windows Communication Foundation の次の領域の機能が強化されています。

**CNG を使用して格納される証明書の WCF トランスポート セキュリティ サポート**

WCF トランスポート セキュリティでは、Windows 暗号化ライブラリ (CNG) を使用して格納される証明書がサポートされます。 .NET Framework 4.6.2 では、このサポートは、指数の長さが 32 ビット以下の公開キーを持つ証明書を使う場合に限定されます。 .NET Framework 4.6.2 を対象とするアプリケーションでは、この機能は既定で有効になります。

.NET Framework 4.6.1 以前を対象とするアプリケーションが .NET Framework 4.6.2 で実行されている場合、app.config または web.config ファイルの [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) セクションに次の行を追加することで、この機能を有効にできます。

```xml
<AppContextSwitchOverrides
    value="Switch.System.ServiceModel.DisableCngCertificates=false"
/>
```

以下のようなコードを使用してプログラムで行うこともできます。

```csharp
private const string DisableCngCertificates = @"Switch.System.ServiceModel.DisableCngCertificates";
AppContext.SetSwitch(disableCngCertificates, false);
```

```vb
Const DisableCngCertificates As String = "Switch.System.ServiceModel.DisableCngCertificates"
AppContext.SetSwitch(disableCngCertificates, False)
```

**DataContractJsonSerializer クラスによる複数の夏時間調整規則のサポート強化**

お客様はアプリケーションの構成設定を使用して、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> クラスで 1 つのタイム ゾーンに対して複数の調整規則がサポートされているかどうかを判別することができます。 これはオプトイン機能です。 これを有効にするには、app.config ファイルに次の設定を追加します。

```xml
<runtime>
     <AppContextSwitchOverrides value="Switch.System.Runtime.Serialization.DoNotUseTimeZoneInfo=false" />
</runtime>
```

この機能が有効な場合、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> オブジェクトは <xref:System.TimeZone> 型ではなく <xref:System.TimeZoneInfo> 型を使用して、日時データを逆シリアル化します。 <xref:System.TimeZoneInfo> では、<xref:System.TimeZone> でサポートされない複数の調整規則がサポートされるため、過去のタイム ゾーン データを操作できます。

<xref:System.TimeZoneInfo> 構造体とタイム ゾーン調整の詳細については、「[タイム ゾーンの概要](../../standard/datetime/time-zone-overview.md)」を参照してください。

**NetNamedPipeBinding の最適な一致**

WCF には、クライアント アプリケーションで設定できる新しいアプリ設定があります。これにより、クライアント アプリケーションは常に、要求したものと最も一致する URI でリッスンしているサービスに接続できます。 このアプリ設定が `false` (既定値) に設定されている場合、クライアントは <xref:System.ServiceModel.NetNamedPipeBinding> を使用して、要求した URI の部分文字列である URI でリッスンしているサービスへの接続を試行できます。

たとえば、クライアントが `net.pipe://localhost/Service1` でリッスンしているサービスに接続しようとしているときに、管理者特権で実行しているコンピューター上の別のサービスが `net.pipe://localhost` でリッスンしているとします。 このアプリ設定が `false` に設定されている場合、クライアントは間違ったサービスに接続しようとします。 アプリ設定を `true` に設定すれば、クライアントは常に最も一致するサービスに接続するようになります。

> [!NOTE]
> <xref:System.ServiceModel.NetNamedPipeBinding> を使用するクライアントは、完全なエンドポイント アドレスではなく、サービスのベース アドレス (存在する場合) に基づいてサービスを検索します。 この設定が常に機能するようにするには、サービスは一意のベース アドレスを使用する必要があります。

この変更を有効にするには、以下のアプリ設定をクライアント アプリケーションの App.config ファイルまたは Web.config ファイルに追加します。

```xml
<configuration>
    <appSettings>
        <add key="wcf:useBestMatchNamedPipeUri" value="true" />
    </appSettings>
</configuration>
```

**SSL 3.0 が既定のプロトコルではなくなった**

トランスポート セキュリティで NetTcp を使用し、証明書の資格情報の種類を使用する場合、SSL 3.0 は、安全な接続のネゴシエーションに使用される既定のプロトコルではなくなりました。 TLS 1.0 が NetTcp のプロトコル一覧に含まれているため、ほとんどの場合、既存のアプリには影響はないと考えられます。 既存のすべてのクライアントは TLS 1.0 以降を使用して接続をネゴシエートできるようになりました。 Ssl3 が必要な場合は、以下の構成メカニズムのいずれかを使用して、ネゴシエートされたプロトコルの一覧に追加します。

- <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols%2A?displayProperty=nameWithType> プロパティ

- <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols%2A?displayProperty=nameWithType> プロパティ

- [\<netTcpBinding>](../configure-apps/file-schema/wcf/nettcpbinding.md) セクションの [\<transport>](../configure-apps/file-schema/wcf/transport-of-nettcpbinding.md) セクション

- [\<customBinding>](../configure-apps/file-schema/wcf/custombinding.md) セクションの [\<sslStreamSecurity>](../configure-apps/file-schema/wcf/sslstreamsecurity.md) セクション

<a name="WPF462" />

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

.NET Framework 4.6.2 では、Windows Presentation Foundation の次の領域の機能が強化されています。

**グループの並べ替え**

<xref:System.Windows.Data.CollectionView> オブジェクトを使用してデータをグループ化するアプリケーションは、グループを並べ替える方法を明示的に宣言できるようになりました。 明示的な並べ替えにより、アプリがグループを動的に追加または削除する場合や、グループ化に関連する項目のプロパティの値を変更する場合に発生する非直感的な順序付けの問題が解決されます。 また、グループ化プロパティの比較がコレクション全体の並べ替えからグループの並べ替えに変更されるため、グループ作成プロセスのパフォーマンスを向上させることができます。

グループの並べ替えをサポートするために、新しい <xref:System.ComponentModel.GroupDescription.SortDescriptions%2A?displayProperty=nameWithType> および <xref:System.ComponentModel.GroupDescription.CustomSort%2A?displayProperty=nameWithType> プロパティで、<xref:System.ComponentModel.GroupDescription> オブジェクトによって生成されるグループのコレクションを並べ替える方法が示されます。 これは、同じ名前の <xref:System.Windows.Data.ListCollectionView> プロパティでデータ項目を並べ替える方法を示すのと同様です。

<xref:System.Windows.Data.PropertyGroupDescription> クラスの新しい 2 つの静的プロパティである <xref:System.Windows.Data.PropertyGroupDescription.CompareNameAscending%2A> と <xref:System.Windows.Data.PropertyGroupDescription.CompareNameDescending%2A> は、最も一般的なケースで使用できます。

たとえば、次の XAML ではデータを年齢でグループ化し、年齢グループを昇順に並べ替え、各年齢グループ内の項目を姓でグループ化します。

```xaml
<GroupDescriptions>
     <PropertyGroupDescription
         PropertyName="Age"
         CustomSort=
              "{x:Static PropertyGroupDescription.CompareNamesAscending}"/>
     </PropertyGroupDescription>
</GroupDescriptions>

<SortDescriptions>
     <SortDescription PropertyName="LastName"/>
</SortDescriptions>
```

**タッチ キーボードのサポート**

タッチ キーボードのサポートにより、WPF アプリケーションでのフォーカス追跡が可能になります。テキスト入力が可能なコントロールによってタッチ入力が受信されると、Windows 10 の新しいタッチ キーボードが自動的に起動および終了します。

.NET Framework の以前のバージョンでは、WPF アプリケーションで、WPF のペンまたはタッチ ジェスチャ サポートを無効にしないとフォーカス追跡を選択できません。 そのため、WPF アプリケーションは完全な WPF タッチのフル サポートを選ぶか、Windows のマウス プロモーションに依存する必要があります。

**モニターごとの DPI**

WPF アプリ用の高 DPI とハイブリッド DPI 環境の最近の急激な増加に対応するために、.NET Framework 4.6.2 の WPF でモニターごとに対応できるようになりました。 ご使用の WPF アプリでモニターごとの DPI 対応を有効にする方法の詳細については、GitHub の[サンプルと開発者向けガイド](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI)に関するページを参照してください。

.NET Framework の以前のバージョンでは、WPF アプリはシステム DPI 対応です。 つまり、アプリケーションの UI は、アプリがレンダリングされるモニターの DPI に基づき、必要に応じて OS でスケーリングされます。

.NET Framework 4.6.2 で実行されるアプリの場合、次のように、アプリケーションの構成ファイルの [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) セクションに構成ステートメントを追加して、WPF アプリでモニターごとの DPI 変更を無効にすることができます。

```xml
<runtime>
   <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
</runtime>
```

<a name="WF462" />

### <a name="windows-workflow-foundation-wf"></a>Windows Workflow Foundation (WF)

.NET Framework 4.6.2 では、Windows Workflow Foundation の次領域の機能が強化されています。

**再ホストされた WF デザイナーにおける C# 式と IntelliSense のサポート**

.NET Framework 4.5 以降では、WF によって、Visual Studio デザイナーとコード ワークフローの両方で C# 式がサポートされます。 再ホストされたワークフロー デザイナーは WF の主な機能です。これにより、ワークフロー デザイナーを Visual Studio の外部のアプリケーション (WPF など) で使用できるようになります。  Windows Workflow Foundation は、再ホストされたワークフロー デザイナーで C# 式と IntelliSense をサポートできるようにします。 詳細については、[Windows Workflow Foundation のブログ](https://docs.microsoft.com/archive/blogs/workflowteam/building-c-expressions-support-and-intellisense-in-the-rehosted-workflow-designer)を参照してください。

`Availability of IntelliSense when a customer rebuilds a workflow project from Visual Studio` 4.6.2 より前のバージョンの .NET Framework で、お客様が Visual Studio からワークフロー プロジェクトを再ビルドした場合、WF Designer IntelliSense は破損してしまいます。 プロジェクトのビルドに成功しても、デザイナーでワークフローの種類が見つからず、 **[エラー一覧]** ウィンドウにワークフローの種類が欠落していることを示す IntelliSense からの警告が表示されます。 .NET Framework 4.6.2 ではこのイシューに対処し、IntelliSense を使用できるようにします。

**ワークフロー追跡を有効にしたワークフロー V1 アプリケーションを FIPS モードで実行**

FIPS コンプライアンス モードが有効なコンピューターで、ワークフロー追跡が有効なワークフロー バージョン 1 スタイルのアプリケーションを正常に実行できるようになりました。 このシナリオを有効にするには、app.config ファイルを以下のように変更する必要があります。

```xml
<add key="microsoft:WorkflowRuntime:FIPSRequired" value="true" />
```

このシナリオが有効でない場合、アプリケーションを実行すると、引き続き例外が生成され、"この実装は Windows プラットフォーム FIPS 検証暗号化アルゴリズムの一部ではありません" というメッセージが表示されます。

**Visual Studio ワークフロー デザイナーで動的更新を使用する場合のワークフローの改善**

ワークフロー デザイナー、フローチャート アクティビティ デザイナー、およびその他のワークフロー アクティビティ デザイナーで、<xref:System.Activities.DynamicUpdate.DynamicUpdateServices.PrepareForUpdate%2A?displayProperty=nameWithType> メソッドを呼び出した後に保存されたワークフローが正常に読み込まれ、表示されるようになりました。 .NET Framework 4.6.2 より前のバージョンの .NET Framework では、<xref:System.Activities.DynamicUpdate.DynamicUpdateServices.PrepareForUpdate%2A?displayProperty=nameWithType> を呼び出した後に保存されたワークフローの XAML ファイルを Visual Studio で読み込むと、以下の問題が発生する場合がありました。

- ワークフロー デザイナーで XAML ファイルが正しく読み込めない (行の末尾に <xref:System.Activities.Presentation.ViewState.ViewStateData.Id%2A?displayProperty=nameWithType> がある場合)。

- フローチャート アクティビティ デザイナーまたは他のワークフロー アクティビティ デザイナーで、アタッチされるプロパティの値ではなく既定の場所にすべてのオブジェクトが表示される場合がある。

<a name="clickonce-1" />

### <a name="clickonce"></a>ClickOnce

ClickOnce が更新され、既にサポートされている 1.0 プロトコルに加え、TLS 1.1 と TLS 1.2 がサポートがサポートされるようになりました。 ClickOnce が必要なプロトコルを自動的に検出するため、TLS 1.1 と 1.2 のサポートを有効にするために、ClickOnce アプリケーション内での追加の手順は不要です。

<a name="UWPConvert" />

### <a name="converting-windows-forms-and-wpf-apps-to--uwp-apps"></a>Windows フォームおよび WPF アプリの UWP アプリへの変換

Windows では、WPF および Windows フォーム アプリを含む、既存の Windows デスクトップ アプリを UWP (ユニバーサル Windows プラットフォーム) で使用できるようになりました。 このテクノロジはブリッジとして機能し、既存のコード ベースを段階的に UWP に移行し、アプリをすべての Windows 10 デバイスで使用できるようにします。

変換後のデスクトップ アプリは UWP アプリのアプリ ID のようなアプリ ID を取得し、UWP API をアクセス可能にして、ライブ タイルや通知などの機能を有効にすることができます。 アプリは引き続き従来どおり動作し、完全信頼アプリとして実行されます。 アプリが変換されたら、アプリ コンテナー プロセスを既存の完全信頼プロセスに追加し、適応ユーザー インターフェイスを追加できます。 すべての機能がアプリ コンテナー プロセスに移行されたら、完全信頼プロセスを削除し、新しい UWP アプリをすべての Windows 10 デバイスで有効にすることができます。

<a name="Debug462" />

### <a name="debugging-improvements"></a>デバッグの機能強化

.NET Framework 4.6.2 で *アンマネージ デバッグ API* が強化され、<xref:System.NullReferenceException> がスローされたときに、ソース コードの単一行でどの変数が `null` になっているかを判別できるように、さらに分析されるようになりました。   このシナリオをサポートするために、次の API がアンマネージ デバッグ API に追加されました。

- [ICorDebugCode4](../unmanaged-api/debugging/icordebugcode4-interface.md)、[ICorDebugVariableHome](../unmanaged-api/debugging/icordebugvariablehome-interface.md)、および [ICorDebugVariableHomeEnum](../unmanaged-api/debugging/icordebugvariablehomeenum-interface.md) インターフェイス。これらは管理対象変数の元のホームを公開します。 これにより、デバッガーは、<xref:System.NullReferenceException> の発生時になんらかのコード フロー分析を行い、さかのぼって、`null` だった元の場所に対応する管理対象変数を判別できます。

- [ICorDebugType2::GetTypeID](../unmanaged-api/debugging/icordebugtype2-gettypeid-method.md) メソッドでは ICorDebugType を [COR_TYPEID](../unmanaged-api/debugging/cor-typeid-structure.md) にマッピングできます。これにより、デバッガーは、ICorDebugType のインスタンスがなくても [COR_TYPEID](../unmanaged-api/debugging/cor-typeid-structure.md) を取得できます。 その後、[COR_TYPEID](../unmanaged-api/debugging/cor-typeid-structure.md) で既存の API を使用して、型のクラス レイアウトを判別できます。

<a name="v461" />

## <a name="whats-new-in-net-framework-461"></a>.NET Framework 4.6.1 の新機能

.NET Framework 4.6.1 には、次の領域における新機能が含まれています。

- [暗号](#Crypto)

- [ADO.NET](#ADO.NET461)

- [Windows Presentation Foundation (WPF)](#WPF461)

- [Windows Workflow Foundation](#WWF461)

- [プロファイル](#Profile461)

- [NGen](#NGEN461)

.NET Framework 4.6.1 の詳細については、次のトピックを参照してください。

- [.NET Framework 4.6.1 の変更の一覧](https://github.com/Microsoft/dotnet/blob/master/releases/net461/dotnet461-changes.md)

- [4.6.1 でのアプリケーションの互換性](../migration-guide/application-compatibility.md)

- [.NET Framework API の diff (差分)](https://github.com/Microsoft/dotnet/blob/master/releases/net461/dotnet461-api-changes.md) (GitHub 上)

<a name="Crypto" />

### <a name="cryptography-support-for-x509-certificates-containing-ecdsa"></a>暗号: ECDSA を含む X509 証明書のサポート

.NET Framework 4.6 では、X509 証明書の RSACng サポートが追加されました。 .NET Framework 4.6.1 では、ECDSA (Elliptic Curve Digital Signature Algorithm: 楕円曲線デジタル署名アルゴリズム) X509 証明書のサポートが追加されました。

ECDSA は、RSA よりも安全な暗号化アルゴリズムで、パフォーマンスも向上するため、トランスポート層セキュリティ (TLS) のパフォーマンスとスケーラビリティが問題になる場合に最適な選択肢です。 .NET Framework の実装では、既存の Windows 機能の呼び出しがラップされます。

次のサンプル コードでは、.NET Framework 4.6.1 での新しい ECDSA X509 証明書のサポートを使ってバイト ストリームの署名が簡単に生成できることを示しています。

[!code-csharp[whatsnew.461.crypto#1](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.461.crypto/cs/Code46.cs#1)]
[!code-vb[whatsnew.461.crypto#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.461.crypto/vb/Code461.vb#1)]

このコードは、.NET Framework 4.6 での署名の生成に必要な次のコードとは大きく異なっています。

[!code-csharp[whatsnew.461.crypto#2](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.461.crypto/cs/Code46.cs#2)]
[!code-vb[whatsnew.461.crypto#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.461.crypto/vb/Code46.vb#2)]

<a name="ADO.NET461" />

### <a name="adonet"></a>ADO.NET

次のものが ADO.NET に追加されました。

**ハードウェア保護キーに対する Always Encrypted のサポート**

ADO.NET では、Always Encrypted 列 (常に暗号化される列) のマスター キーをハードウェア セキュリティ モジュール (HSM) に格納することが、ネイティブでサポートされるようになりました。 このサポートによって、ユーザーは、HSM に格納された非対称キーを利用できます。カスタムの列マスター キー ストア プロバイダーを作成してからアプリケーションに登録する必要はありません。

HSM に格納された列マスター キーで保護された Always Encrypted データにアクセスするには、HSM ベンダー提供の CSP プロバイダーまたは CNG キー ストア プロバイダーをアプリ サーバーまたはクライアント コンピューターにインストールする必要があります。

**AlwaysOn に対する <xref:System.Data.SqlClient.SqlConnectionStringBuilder.MultiSubnetFailover%2A> の接続動作の向上**

SqlClient で、AlwaysOn 可用性グループ (AG) へのより高速な接続が自動的に提供されるようになりました。 SqlClient では、アプリケーションが別のサブネット上の AlwaysOn 可用性グループ (AG) に接続しているかどうかを自動的に検出し、現在のアクティブなサーバーを迅速に探索し、そのサーバーへの接続を提供します。 このリリースよりも前は、アプリケーションで接続文字列を設定し、`"MultisubnetFailover=true"` を含め、AlwaysOn 可用性グループに接続されていることを示す必要がありました。 `true` への接続キーワードを設定しないと、アプリケーションで AlwaysOn 可用性グループに接続中にタイムアウトが発生する可能性がありました。 このリリースを使用すれば、アプリケーションで <xref:System.Data.SqlClient.SqlConnectionStringBuilder.MultiSubnetFailover%2A> を `true` に設定する必要が*なくなり*ます。 Always On 可用性グループの SqlClient サポートの詳細については、「[高可用性障害復旧のための SqlClient サポート](../data/adonet/sql/sqlclient-support-for-high-availability-disaster-recovery.md)」をご覧ください。

<a name="WPF461" />

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

Windows Presentation Foundation には、さまざまな改善と変更が含まれています。

**パフォーマンスの向上**

.NET Framework 4.6.1 で、タッチ イベントの開始の遅延が修正されました。 また、<xref:System.Windows.Controls.RichTextBox> コントロールの入力が高速入力時のレンダーのスレッドを停止させなくなりました。

**スペル チェック機能の改善**

WPF のスペル チェック機能が、追加の言語のスペル チェックのためにオペレーティング システムのサポートを利用するように、Windows 8.1 以降のバージョンで更新されました。  Windows 8.1 よりも前の Windows バージョンの機能の変更はありません。

以前のバージョンの .NET Framework と同様に、<xref:System.Windows.Controls.TextBox> コントロールまたは <xref:System.Windows.Controls.RichTextBox> ブロック用の言語が、次の順序で情報を探すことによって検出されます。

- `xml:lang` (存在する場合)。

- 現在の入力言語。

- 現在のスレッド カルチャ。

WPF での言語サポートの詳細については、[.NET Framework 4.6.1 機能に関する WPF ブログの記事](https://devblogs.microsoft.com/wpf/wpf-in-net-4-6-1/)を参照してください。

**ユーザーごとのユーザー辞書の追加サポート**

.NET Framework 4.6.1 では、WPF で、グローバルに登録されているユーザー辞書が認識されます。 この機能は、ユーザー辞書をコントロールごとに登録する機能に加えて使用できます。

以前のバージョンの WPF では、ユーザー辞書で除外された単語とオートコレクト リストが認識されませんでした。 `%AppData%\Microsoft\Spelling\<language tag>` ディレクトリに配置できるファイルの使用によって、これらが Windows 8.1 と Windows 10 でサポートされます。  これらのファイルには、次の規則が適用されます。

- これらのファイルには、拡張子の .dic (追加された単語の場合)、.exc (除外された単語の場合)、または .acl (オートコレクトの場合) が付いている必要があります。

- これらのファイルは、バイト オーダー マーク (BOM) で始まる UTF-16 LE プレーン テキストである必要があります。

- それぞれの行は、1 つの単語 (追加および除外された単語の一覧内)、または単語が縦棒 ("&#124;") で区切られたオートコレクトのペア (オートコレクトの単語の一覧内) で構成される必要があります。

- これらのファイルは読み取り専用と見なされ、システムによって変更されることはありません。

> [!NOTE]
> これらの新しいファイル形式は、WPF スペル チェック API で直接サポートされるわけではありません。また、アプリケーションで WPF に提供されるユーザー辞書では以前と同様に .lex ファイルを使用する必要があります。

**サンプル**

WPF のサンプルは、[Microsoft/WPF サンプル](https://github.com/Microsoft/WPF-Samples) GitHub リポジトリにいくつかあります。 プル要求を送信するか、または[GitHub issue (GitHub の問題)](https://github.com/Microsoft/WPF-Samples/issues) を開いて、これらのサンプルの改善にご協力ください。

**DirectX の拡張機能**

WPF には、DX10 および Dx11 のコンテンツとの相互運用を容易にする、<xref:System.Windows.Interop.D3DImage> の新しい実装を提供する [NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Wpf.Interop.DirectX-x86/)が含まれています。 このパッケージのコードはオープン ソース化されており、[GitHub で](https://github.com/Microsoft/WPFDXInterop)入手できます。

<a name="WWF461" />

### <a name="windows-workflow-foundation-transactions"></a>Windows Workflow Foundation: トランザクション

<xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A?displayProperty=nameWithType> メソッドで、MSDTC 以外の分散トランザクション マネージャーを使用して、トランザクションをプロモート (昇格) できるようになりました。 このプロモートは、新しい <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%28System.Transactions.IPromotableSinglePhaseNotification%2CSystem.Guid%29?displayProperty=nameWithType> オーバーロードに GUID のトランザクション プロモーター識別子を指定することによって行います。 この操作が成功すると、そのトランザクションの機能に制限が加えられます。 MSDTC 以外のトランザクション プロモーターが登録される (参加する) と、次の各メソッドで MSDTC へのプロモートが必要になるため、これらのメソッドで <xref:System.Transactions.TransactionPromotionException> がスローされます。

- <xref:System.Transactions.Transaction.EnlistDurable%2A?displayProperty=nameWithType>

- <xref:System.Transactions.TransactionInterop.GetDtcTransaction%2A?displayProperty=nameWithType>

- <xref:System.Transactions.TransactionInterop.GetExportCookie%2A?displayProperty=nameWithType>

- <xref:System.Transactions.TransactionInterop.GetTransmitterPropagationToken%2A?displayProperty=nameWithType>

MSDTC 以外のトランザクション プロモーターが登録されると、そのプロモーターで定義するプロトコルを使用することで、そのプロモーターをその後の永続参加リストに使用する必要があります。 トランザクション プロモーターの <xref:System.Guid> は、<xref:System.Transactions.Transaction.PromoterType%2A> プロパティを使用して取得できます。 トランザクションがプロモートする際、トランザクション プロモーターによって、プロモート済みトークンを表す <xref:System.Byte> 配列が提供されます。 アプリケーションで、MSDTC 以外のプロモート済みトランザクションのプロモート済みトークンを <xref:System.Transactions.Transaction.GetPromotedToken%2A> メソッドを使用して取得できます。

新しい <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%28System.Transactions.IPromotableSinglePhaseNotification%2CSystem.Guid%29?displayProperty=nameWithType> オーバーロードのユーザーは、プロモーション操作が正常に完了するように、特定の呼び出しシーケンスに従う必要があります。 これらの規則は、メソッドのドキュメントに記載されています。

<a name="Profile461" />

### <a name="profiling"></a>プロファイル

アンマネージド プロファイリング API が、次のように拡張されました。

- [ICorProfilerInfo7](../unmanaged-api/profiling/icorprofilerinfo7-interface.md) インターフェイス内の PDB へのアクセスのサポートが向上。

  ASP.NET Core では、アセンブリがメモリ内で Roslyn によってコンパイルされることがよりいっそう一般的になっています。 プロファイル ツールを作成する開発者にとって、これは従来ディスクにシリアル化されていた PDB が存在しなくなる可能性があることを意味しています。 プロファイル ツールでは、多くの場合 PDB を使用して、コード カバレッジや 1 行単位のパフォーマンス分析などのタスクのソース行に、コードをマップし戻します。 [ICorProfilerInfo7](../unmanaged-api/profiling/icorprofilerinfo7-interface.md) インターフェイスは、メモリ内の PDB データにアクセスして、これらのプロファイル ツールを提供する 2 つの新しいメソッド、[ICorProfilerInfo7::GetInMemorySymbolsLength](../unmanaged-api/profiling/icorprofilerinfo7-getinmemorysymbolslength-method.md) と [ICorProfilerInfo7::ReadInMemorySymbols](../unmanaged-api/profiling/icorprofilerinfo7-readinmemorysymbols.md) を含むようになりました。これらの新しい API を使用すれば、プロファイラーでメモリ内の PDB の内容をバイト配列として取得してから、それを処理したり、ディスクにシリアル化したりできます。

- ICorProfiler インターフェイスを使用したインストルメンテーションの改善。

  動的インストルメンテーションに `ICorProfiler` API ReJit 機能を使用しているプロファイラーで、いくつかのメタデータを変更できるようになりました。 従来、このようなツールでは、いつでも IL をインストルメント化できましたが、メタデータを変更できるのはモジュールを読み込む時だけでした。 IL はメタデータを参照するため、実行できるインストルメンテーションの種類が限られています。 モジュールの読み込み後のメタデータの編集のサブセットをサポートするために、[ICorProfilerInfo7::ApplyMetaData](../unmanaged-api/profiling/icorprofilerinfo7-applymetadata-method.md) メソッドを追加することで (特に新しい `AssemblyRef`、`TypeRef`、`TypeSpec`、`MemberRef`、`MemberSpec`、および `UserString` の各レコードを追加することで)、これらの制限の一部を解消しました。 この変更により、はるかに広範な実行時インストルメンテーションが可能になります。

<a name="NGEN461" />

### <a name="native-image-generator-ngen-pdbs"></a>ネイティブ イメージ ジェネレーター (NGEN) PDB

マシン間のイベント トレースを使用すれば、ユーザーはマシン A 上でプログラムのプロファイリングを行い、マシン B 上でソース行のマッピングを使用して、プロファイリング データを確認できます。以前のバージョンの.NET Framework を使用する場合、ソースからネイティブへのマッピングを作成するには、ユーザーは、プロファイルされたコンピューターにあるすべてのモジュールとネイティブ イメージを、IL PDB が含まれた分析用コンピューターにコピーする必要がありました。 この処理は、Phone アプリケーションなどファイル サイズが比較的小さい場合には高速に実行されますが、これらのファイルのサイズがデスクトップ システム上で非常に大きくなる可能性があり、その場合コピーにかなりの時間を要する可能性があります。

NGen PDB を使用すれば、IL PDB に依存することなく、NGen で IL からネイティブへのマッピングが含まれた PDB を作成できます。 このマシン間のイベント トレース シナリオで必要なことは、コンピューター A で生成されたネイティブ イメージの PDB をコンピューター B にコピーすることと、[Debug Interface Access API](/visualstudio/debugger/debug-interface-access/debug-interface-access-sdk-reference) を使用して IL PDB のソースから IL へのマッピングとネイティブ イメージの PDB の IL からネイティブへのマッピングを読み取ることだけです。 両方のマッピングを組み合わせることで、ソースからネイティブへのマッピングが実現します。 ネイティブ イメージの PDB は、どのモジュールとネイティブ イメージよりもはるかにサイズが小さいため、コンピューター A からコンピューター B へのコピーの処理は非常に高速になります。

<a name="v46" />

## <a name="whats-new-in-net-2015"></a>.NET 2015 の新機能

.NET 2015 では、.NET Framework 4.6 と .NET Core が導入されています。 一部の新機能はその両方に適用され、その他の機能は .NET Framework 4.6 または .NET Core に固有です。

- **ASP.NET Core**

  .NET 2015 には、ASP.NET Core が含まれています。これは、最新のクラウド ベースのアプリを構築するのに効率的な .NET 実装です。 ASP.NET Core はモジュール形式であるため、ご利用のアプリケーションで必要な機能のみを含めることができます。 IIS でホストすることも、カスタムのプロセスでセルフホストすることもできます。さらに同じサーバー上のさまざまなバージョンの .NET Framework でアプリを実行することも可能です。 クラウド展開用に設計されている新たな環境構成システムが含まれています。

  MVC、Web API、および Web ページは、MVC 6 と呼ばれる 1 つのフレームワークに統合されています。 Visual Studio 2015 以降のツールで ASP.NET Core アプリをビルドします。 既存のアプリケーションは、この新しい .NET Framework で動作しますが、MVC 6 または SignalR 3 を使用するアプリをビルドするには Visual Studio 2015 以降のプロジェクト システムを使用する必要があります。

  詳細については、[ASP.NET Core](/aspnet/core/) に関するページを参照してください。

- **ASP.NET の更新プログラム**

  - **非同期応答フラッシュ用のタスク ベース API**

    ASP.NET で、非同期応答フラッシュ用の単純なタスク ベース API である <xref:System.Web.HttpResponse.FlushAsync%2A?displayProperty=nameWithType> が提供されるようになりました。これにより、言語の `async/await` サポートを使用して非同期的に応答をフラッシュできます。

  - **モデル バインドによるタスクを返すメソッドのサポート**

    .NET Framework 4.5 では、Web フォーム ページやユーザー コントロールでの CRUD ベースのデータ操作に対して拡張可能なコード中心のアプローチを可能にするモデル バインド機能が ASP.NET に追加されました。 このモデル バインド システムで、<xref:System.Threading.Tasks.Task> を返すモデル バインド メソッドがサポートされるようになりました。 この機能により、Web フォーム開発者は Entity Framework などの ORM の新しいバージョンを使用するときに、データ バインド システムのように簡単に非同期のスケーラビリティ上のメリットを得ることができます。

    非同期モデル バインドは、`aspnet:EnableAsyncModelBinding` 構成設定によって制御されます。

    ```xml
    <appSettings>
        <add key=" aspnet:EnableAsyncModelBinding" value="true|false" />
    </appSettings>
    ```

    .NET Framework 4.6 を対象とするアプリでは、既定で `true` になります。 .NET Framework 4.6 で実行され、.NET Framework の以前のバージョンを対象とするアプリでは、既定で `false` になります。 有効にするには、この構成設定を `true` に設定します。

  - **HTTP/2 のサポート (Windows 10)**

    [HTTP/2](https://www.wikipedia.org/wiki/HTTP/2) は HTTP プロトコルの新しいバージョンで、接続の使用状況が (クライアントとサーバー間のラウンド トリップ数の減少により) 大幅に改善されて、ユーザーが Web ページを読み込むときの遅延が軽減されています。  このプロトコルは単一のエクスペリエンスの一部として要求さる複数の成果物のために最適化されているので、HTTP/2 が最も役立つのは (サービスではなく) web ページです。 .NET Framework 4.6 では、HTTP/2 のサポートが ASP.NET に追加されました。 ネットワーク機能は複数のレイヤーで存在するので、HTTP/2 を有効にするために、Windows、IIS、および ASP.NET で新機能が必要とされていました。 ASP.NET で HTTP/2 を使用するには、Windows 10 を実行している必要があります。

    HTTP/2 は、<xref:System.Net.Http.HttpClient?displayProperty=nameWithType> API を使用する Windows 10 ユニバーサル Windows プラットフォーム (UWP) アプリでもサポートされ、既定で有効になります。

    ASP.NET アプリケーションで [PUSH_PROMISE](https://http2.github.io/http2-spec/#PUSH_PROMISE) 機能を使用する手段を提供するために、<xref:System.Web.HttpResponse.PushPromise%28System.String%29> と <xref:System.Web.HttpResponse.PushPromise%28System.String%2CSystem.String%2CSystem.Collections.Specialized.NameValueCollection%29> の 2 つのオーバーロードを持つ新しいメソッドが <xref:System.Web.HttpResponse> クラスに追加されました。

    > [!NOTE]
    > ASP.NET Core では HTTP/2 がサポートされますが、PUSH PROMISE 機能のサポートはまだ追加されていません。

    ブラウザーと web サーバー (Windows 上の IIS) がすべての処理を実行します。 ユーザーの側で複雑な操作を実行する必要はありません。

    [主要なブラウザーのほとんどは HTTP/2 をサポートしている](https://www.wikipedia.org/wiki/HTTP/2)ため、多くの場合、サーバーが HTTP/2 をサポートしていれば、ユーザーもそのメリットを得ることができます。

  - **トークン バインディング プロトコルのサポート**

    Microsoft と Google は、[トークン バインディング プロトコル](https://github.com/TokenBinding/Internet-Drafts)と呼ばれる、認証のための新しいアプローチに共同で取り組んできました。 この前提となっているのは、犯罪者が (ブラウザー キャッシュ内の) 認証トークンを盗用することで、本来ならパスワードや他の特別な情報でセキュリティ保護されているリソース (銀行口座など) にアクセスできる可能性がある、ということです。 新しいプロトコルは、この問題を軽減することを目指しています。

    トークン バインディング プロトコルは、Windows 10 でブラウザーの機能として実装されます。 ASP.NET アプリは、認証トークンの正当性が検証されるように、このプロトコルに参加します。 クライアントとサーバーの実装は、プロトコルによって指定されるエンド ツー エンドの保護を確立します。

  - **ランダムな文字列のハッシュ アルゴリズム**

    .NET Framework 4.5 で、[ランダム化された文字列のハッシュ アルゴリズム](../configure-apps/file-schema/runtime/userandomizedstringhashalgorithm-element.md)が導入されました。 しかし、ASP.NET の一部の機能は安定したハッシュ コードに依存していたため、この機能は ASP.NET ではサポートされませんでした。 .NET Framework 4.6 では、ランダムな文字列のハッシュ アルゴリズムがサポートされるようになりました。 この機能を有効にするには、`aspnet:UseRandomizedStringHashAlgorithm` 構成設定を使用します。

    ```xml
    <appSettings>
        <add key="aspnet:UseRandomizedStringHashAlgorithm" value="true|false" />
    </appSettings>
    ```

- **ADO.NET**

  ADO .NET では、SQL Server 2016 Community Technology Preview 2 (CTP2) で使用できる Always Encrypted 機能がサポートされました。 SQL Server は、Always Encrypted 機能を使用して、暗号化されたデータに対して操作を実行できます。特に、サーバー上ではなく、ユーザーが信頼している環境内にアプリケーションと共に暗号化キーを保存できるようになりました。 Always Encrypted でユーザー データが保護されるので、DBA はプレーン テキスト データにアクセスできません。 データの暗号化と復号化は、ドライバー レベルで透過的に行われるので、既存のアプリケーションに対する変更が最小限に抑えられます。 詳細については、「[Always Encrypted (データベース エンジン)](/sql/relational-databases/security/encryption/always-encrypted-database-engine)」および「[Always Encrypted (クライアント開発)](/sql/relational-databases/security/encryption/always-encrypted-client-development)」を参照してください。

- **マネージド コードの JIT コンパイラ (64 ビット)**

  .NET Framework 4.6 の特徴の 1 つとして、新しいバージョンの 64 ビット JIT コンパイラ (RyuJIT というコードネームで呼ばれていたもの) があります。 この新しい 64 ビット コンパイラは、これまでの 64 ビット JIT コンパイラよりもパフォーマンスが大幅に向上しています。 新しい 64 ビット コンパイラは、.NET Framework 4.6 上で実行される 64 ビット プロセスで有効になります。 64 ビットまたは AnyCPU としてコンパイルされ、64 ビット オペレーティング システム上で実行されるアプリは、64 ビットで動作します。 新しいコンパイラへの移行をできる限り透過的に行うように注意を払いましたが、動作の変更が発生する可能性があります。

  新しい 64 ビット JIT コンパイラには、ハードウェア SIMD アクセラレータ機能も含まれています。これを <xref:System.Numerics> 名前空間の SIMD 対応の型と組み合わせると、パフォーマンスが大幅に向上する可能性があります。

- **アセンブリ ローダーの改善**

  アセンブリ ローダーで、対応する NGEN イメージの読み込み後に IL アセンブリをアンロードすることにより、メモリがより効率的に使用されるようになりました。 この変更は、仮想メモリが減少するため、特に大規模な 32 ビット アプリ (Visual Studio など) で有益であり、物理メモリの節約にもなります。

- **基本クラス ライブラリの変更点**

  主なシナリオを利用できるようにするため、多くの新しい API が .NET Framework 4.6 に関連して追加されました。 変更点と追加点を以下に示します。

  - **IReadOnlyCollection\<T> implementations**

    追加のコレクション <xref:System.Collections.Generic.IReadOnlyCollection%601> (<xref:System.Collections.Generic.Queue%601>、<xref:System.Collections.Generic.Stack%601> など) が実装されています。

  - **CultureInfo.CurrentCulture と CultureInfo.CurrentUICulture**

    <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> プロパティと <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> プロパティが読み取り専用ではなく、読み取り/書き込み可能になりました。 新しい <xref:System.Globalization.CultureInfo> オブジェクトをこれらのプロパティに割り当てると、`Thread.CurrentThread.CurrentCulture` プロパティで現在定義されているスレッド カルチャと、`Thread.CurrentThread.CurrentUICulture` プロパティで現在定義されている UI スレッド カルチャも変更されます。

  - **ガベージ コレクション (GC) の機能強化**

    <xref:System.GC> クラスに <xref:System.GC.TryStartNoGCRegion%2A> メソッドと <xref:System.GC.EndNoGCRegion%2A> メソッドが含まれるようになりました。これらのメソッドを使用するとクリティカル パスの実行中にガベージ コレクションを不許可にできます。

    <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%2CSystem.Boolean%29?displayProperty=nameWithType> メソッドの新しいオーバーロードを使用して、小さなオブジェクト ヒープと大きなオブジェクト ヒープの両方に関して、スイープして圧縮するのか、スイープのみを行うのかを制御できます。

  - **SIMD が有効な型**

    <xref:System.Numerics> 名前空間には、<xref:System.Numerics.Matrix3x2>、<xref:System.Numerics.Matrix4x4>、<xref:System.Numerics.Plane>、<xref:System.Numerics.Quaternion>、<xref:System.Numerics.Vector2>、<xref:System.Numerics.Vector3>、<xref:System.Numerics.Vector4> など、いくつかの SIMD 対応の型が含まれるようになりました。

    新しい 64 ビット JIT コンパイラにはハードウェア SIMD アクセラレータ機能も含まれているため、新しい 64 ビット JIT コンパイラで SIMD 対応の型を使用すると、パフォーマンスが大幅に向上します。

  - **暗号の更新**

    <xref:System.Security.Cryptography?displayProperty=nameWithType> API は、[Windows CNG 暗号化 API](/windows/desktop/SecCNG/cng-reference) をサポートするために更新中です。 以前のバージョンの .NET Framework は、<xref:System.Security.Cryptography?displayProperty=nameWithType> の実装の基礎として、[それより前のバージョンの Windows 暗号化 API](/windows/desktop/SecCrypto/cryptography-portal) に完全に依存していました。 CNG API は特定のカテゴリのアプリにとって重要な[最新の暗号アルゴリズム](/windows/desktop/SecCNG/cng-features#suite-b-support)をサポートするものであるため、CNG API をサポートしてほしいというリクエストが寄せられていました。

    .NET Framework 4.6 には、Windows CNG 暗号化 API をサポートするために、次の新しい機能強化が含まれています。

    - 可能な場合に CAPI ベースの実装ではなく CNG ベースの実装を返す X509 証明書用の一連の拡張メソッド `System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey(System.Security.Cryptography.X509Certificates.X509Certificate2)` および `System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey(System.Security.Cryptography.X509Certificates.X509Certificate2)` (一部のスマート カードなどでは現在も CAPI が必要であり、API がフォールバックを処理します)。

    - RSA アルゴリズムの CNG 実装を提供する <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType> クラス。

    - 一般的な操作でキャストを不要にする RSA API の機能強化。 たとえば、以前のバージョンの .NET Framework で <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> オブジェクトを使用してデータを暗号化するには、次のようなコードが必要です。

      [!code-csharp[WhatsNew.Casting#1](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.casting/cs/program.cs#1)]
      [!code-vb[WhatsNew.Casting#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.casting/vb/module1.vb#1)]

      .NET Framework 4.6 で新しい暗号化を使用するコードは、次のように書き換えてキャストを回避することができます。

      [!code-csharp[WhatsNew.Casting#2](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.casting/cs/program.cs#2)]
      [!code-vb[WhatsNew.Casting#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.casting/vb/module1.vb#2)]

  - **日付および時間と UNIX 時間の変換のサポート**

    日付値および時間値と UNIX 時間の変換をサポートするため、次の新しいメソッドが <xref:System.DateTimeOffset> 構造体に追加されました。

    - <xref:System.DateTimeOffset.FromUnixTimeSeconds%2A?displayProperty=nameWithType>

    - <xref:System.DateTimeOffset.FromUnixTimeMilliseconds%2A?displayProperty=nameWithType>

    - <xref:System.DateTimeOffset.ToUnixTimeSeconds%2A?displayProperty=nameWithType>

    - <xref:System.DateTimeOffset.ToUnixTimeMilliseconds%2A?displayProperty=nameWithType>

  - **互換性スイッチ**

    <xref:System.AppContext> クラスでは、ライブラリの作成者が統一された新機能のオプトアウト メカニズムをユーザーに提供できるようにする、新しい互換性機能を追加します。 これにより、オプトアウト要求を伝達するために、コンポーネント間に疎結合のコントラクトが確立されます。 通常、この機能は既存の機能が変更されるときに重要となります。 それに対して、新しい機能には暗黙のオプトインが既に存在しています。

    <xref:System.AppContext> によって、ライブラリは互換性スイッチを定義して公開します。また、それらに依存するコードは、それらのスイッチを設定してライブラリの動作に影響を与えることができます。 ライブラリは、既定では新しい機能を提供し、スイッチが設定されている場合のみそれを変更する (つまり以前の機能を提供する) ことができます。

    アプリケーション (またはライブラリ) は、依存するライブラリが定義したスイッチの値 (常に <xref:System.Boolean> 値) を宣言できます。 スイッチは常に暗黙的に `false` です。 スイッチを `true` に設定すると、それが有効になります。 スイッチを明示的に `false` に設定すると、新しい動作が提供されます。

    ```csharp
    AppContext.SetSwitch("Switch.AmazingLib.ThrowOnException", true);
    ```

    ```vb
    AppContext.SetSwitch("Switch.AmazingLib.ThrowOnException", True)
    ```

    ライブラリは、コンシューマーがスイッチの値を宣言したことを確認してから、それを適切に実行する必要があります。

    ```csharp
    if (!AppContext.TryGetSwitch("Switch.AmazingLib.ThrowOnException", out shouldThrow))
    {
        // This is the case where the switch value was not set by the application.
        // The library can choose to get the value of shouldThrow by other means.
        // If no overrides nor default values are specified, the value should be 'false'.
        // A false value implies the latest behavior.
    }

    // The library can use the value of shouldThrow to throw exceptions or not.
    if (shouldThrow)
    {
        // old code
    }
    else
    {
        // new code
    }
    ```

    ```vb
    If Not AppContext.TryGetSwitch("Switch.AmazingLib.ThrowOnException", shouldThrow) Then
        ' This is the case where the switch value was not set by the application.
        ' The library can choose to get the value of shouldThrow by other means.
        ' If no overrides nor default values are specified, the value should be 'false'.
        ' A false value implies the latest behavior.
    End If

    ' The library can use the value of shouldThrow to throw exceptions or not.
    If shouldThrow Then
        ' old code
    Else
        ' new code
    End If
    ```

    スイッチは、ライブラリによって公開される正式なコントラクトであるため、一貫性のある形式を使用することをお勧めします。 2 つの明確な形式を次に示します。

    - *Switch*.*namespace*.*switchname*

    - *Switch*.*library*.*switchname*

  - **タスク ベースの非同期パターン (TAP) の変更点**

    .NET Framework 4.6 をターゲットとするアプリの場合、<xref:System.Threading.Tasks.Task> および <xref:System.Threading.Tasks.Task%601> オブジェクトは、呼び出し元のスレッドのカルチャと UI カルチャを継承します。 以前のバージョンの .NET Framework をターゲットするとアプリまたは特定のバージョンの .NET Framework をターゲットとしないアプリの動作には影響を及ぼしません。 詳細については、<xref:System.Globalization.CultureInfo> クラスのトピックの「カルチャとタスク ベースの非同期の操作」セクションをご覧ください。

    <xref:System.Threading.AsyncLocal%601?displayProperty=nameWithType> クラスを使用すると、`async` メソッドなど、特定の非同期制御フローに対してローカルなアンビエント データを表すことができます。 これは、スレッド間でデータを保持するために使用できます。 <xref:System.Threading.AsyncLocal%601.Value%2A?displayProperty=nameWithType> プロパティの明示的な変更やスレッドでのコンテキスト変換の発生などによってアンビエント データが変更されるたびに通知されるコールバック メソッドを定義することもできます。

    タスク ベースの非同期パターン (TAP) に、特定の状態で完了したタスクを返す 3 つの便利なメソッド <xref:System.Threading.Tasks.Task.CompletedTask%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Task.FromCanceled%2A?displayProperty=nameWithType>、および <xref:System.Threading.Tasks.Task.FromException%2A?displayProperty=nameWithType> が追加されました。

    <xref:System.IO.Pipes.NamedPipeClientStream> クラスで、新しい <xref:System.IO.Pipes.NamedPipeClientStream.ConnectAsync%2A> メソッドによる非同期通信がサポートされるようになりました。 メソッドをオーバーライドします。

  - **EventSource がイベント ログへの書き込みをサポート**

    コンピューター上で作成された既存の ETW セッションに加えて、<xref:System.Diagnostics.Tracing.EventSource> クラスを使用して管理または操作メッセージをイベント ログに記録できるようになりました。 以前は、この機能のために Microsoft.Diagnostics.Tracing.EventSource NuGet パッケージを使用する必要がありました。 この機能は .NET Framework 4.6 に組み込まれました。

    NuGet パッケージと .NET Framework 4.6 は、どちらも次の機能によって更新されています。

    - **ダイナミック イベント**

      イベント メソッドを作成せずに、実行時にイベントを定義できます。

    - **リッチ ペイロード**

      特別な属性が指定されたクラスや配列に加えて、プリミティブ型もペイロードとして渡すことができます。

    - **アクティビティの追跡**

      Start イベントと Stop イベントの間のイベントに、現在アクティブなすべてのアクティビティを表す ID が付けられます。

    これらの機能をサポートするため、<xref:System.Diagnostics.Tracing.EventSource> クラスにオーバーロードされた <xref:System.Diagnostics.Tracing.EventSource.Write%2A> メソッドが追加されました。

- **Windows Presentation Foundation (WPF)**

  - **HDPI の強化**

    .NET Framework 4.6 では、WPF での HDPI サポートが強化されました。 境界があるコントロールでのクリッピングの発生を減らすため、レイアウトの丸め処理が変更されました。 既定では、<xref:System.Runtime.Versioning.TargetFrameworkAttribute> が .NET 4.6 に設定されている場合にのみ、この機能が有効になります。  以前のバージョンの Framework を対象とするアプリケーションが .NET Framework 4.6 で実行される場合は、app.config ファイルの [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) セクションに次の行を追加することで、新しい動作を選択できます。

    ```xml
    <AppContextSwitchOverrides
    value="Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=false"
    />
    ```

    異なる DPI 設定を持つ (マルチ DPI セットアップの) 複数のモニターにまたがる WPF ウィンドウは、ブラック アウト領域なしで完全にレンダリングされるようになりました。 この動作の選択を解除するには、app.config ファイルの `<appSettings>` セクションに次の行を追加して、この新しい動作を無効にします。

    ```xml
    <add key="EnableMultiMonitorDisplayClipping" value="true"/>
    ```

    DPI 設定に基づいて自動的に正しいカーソルを読み込む機能のサポートが <xref:System.Windows.Input.Cursor?displayProperty=nameWithType> に追加されました。

  - **タッチの改善**

    タッチで予期しない動作が発生するという、お客様から報告された [Connect](https://connect.microsoft.com/VisualStudio/feedback/details/903760/) の問題が .NET Framework 4.6 で対処されました。 Windows 8.1 以降では、Windows ストア アプリケーションと WPF アプリケーションのダブルタップのしきい値が同じになりました。

  - **透過的な子ウィンドウのサポート**

    .NET Framework 4.6 の WPF では、Windows 8.1 以降の透過的な子ウィンドウがサポートされます。 これにより、最上位レベルのウィンドウ内に四角形でない透過的な子ウィンドウを作成できます。 この機能を有効にするには、<xref:System.Windows.Interop.HwndSourceParameters.UsesPerPixelTransparency%2A?displayProperty=nameWithType> プロパティを `true` に設定します。

- **Windows Communication Foundation (WCF)**

  - **SSL のサポート**

    WCF で、NetTcp をトランスポート セキュリティおよびクライアント認証と共に使用する場合に、SSL のバージョンとして SSL 3.0 および TLS 1.0 に加えて TLS 1.1 および TLS 1.2 がサポートされるようになりました。 使用するプロトコルを選択することも、安全性の低い古いプロトコルを無効化することもできるようになりました。 これを行うには、<xref:System.ServiceModel.TcpTransportSecurity.SslProtocols%2A> プロパティを設定するか、または構成ファイルに以下を追加します。

    ```xml
    <netTcpBinding>
        <binding>
          <security mode= "None|Transport|Message|TransportWithMessageCredential" >
              <transport clientCredentialType="None|Windows|Certificate"
                        protectionLevel="None|Sign|EncryptAndSign"
                        sslProtocols="Ssl3|Tls1|Tls11|Tls12">
                </transport>
          </security>
        </binding>
    </netTcpBinding>
    ```

  - **異なる HTTP 接続によるメッセージの送信**

    WCF で、ユーザーが別の基になる HTTP 接続を使用して特定のメッセージを送信できるようになりました。 これには、2 つの方法があります。

    - **接続グループ名プレフィックスの使用**

      ユーザーは、WCF で接続グループ名のプレフィックスとして使用される文字列を指定できます。 異なるプレフィックスを持つ 2 つのメッセージは、別の基になる HTTP 接続を使用して送信されます。 プレフィックスを設定するには、メッセージの <xref:System.ServiceModel.Channels.Message.Properties%2A?displayProperty=nameWithType> プロパティにキー/値のペアを追加します。 キーは "HttpTransportConnectionGroupNamePrefix" で、値は使用するプレフィックスです。

    - **異なるチャネル ファクトリの使用**

      ユーザーは、異なるチャネル ファクトリによって作成されたチャネルを使用して送信されるメッセージで別の基になる HTTP 接続を使用する機能を有効にすることもできます。 この機能を有効にするには、ユーザーが次の `appSetting` を `true` に設定する必要があります。

      ```xml
      <appSettings>
          <add key="wcf:httpTransportBinding:useUniqueConnectionPoolPerFactory" value="true" />
      </appSettings>
      ```

- **Windows Workflow Foundation (WWF)**

  順序不定の操作要求がタイムアウトする前に未処理の "非プロトコル" ブックマークが存在する場合に、ワークフロー サービスが要求を保持する秒数を指定できるようになりました。 "非プロトコル" ブックマークとは、未処理の Receive アクティビティに関連付けられていないブックマークです。 一部のアクティビティはその実装内で非プロトコル ブックマークを作成するため、必ずしも非プロトコル ブックマークの存在が明らかにわかるとは限りません。 例として State や Pick などがあります。 ステート マシンを使用して実装されたワークフロー サービスや、Pick アクティビティを含むワークフロー サービスがある場合は、非プロトコル ブックマークが存在する可能性が高くなります。 この間隔を指定するには、app.config ファイルの `appSettings` セクションに次のような行を追加します。

  ```xml
  <add key="microsoft:WorkflowServices:FilterResumeTimeoutInSeconds" value="60"/>
  ```

  既定値は 60 秒です。 `value` を 0 に設定すると、順序不定の要求はただちに拒否され、次のようなテキストを含むエラーが返されます。

  ```console
  Operation 'Request3|{http://tempuri.org/}IService' on service instance with identifier '2b0667b6-09c8-4093-9d02-f6c67d534292' cannot be performed at this time. Please ensure that the operations are performed in the correct order and that the binding in use provides ordered delivery guarantees.
  ```

  これは、順序不定の操作メッセージを受信し、非プロトコル ブックマークが存在しない場合に受信するメッセージと同じです。

  `FilterResumeTimeoutInSeconds` 要素の値がゼロ以外で、非プロトコル ブックマークが存在し、タイムアウト間隔が終了した場合、その操作は失敗し、タイムアウト メッセージが発生します。

- **トランザクション**

  <xref:System.Transactions.TransactionException> から派生した例外がスローされる原因となったトランザクションに、分散トランザクション識別子を含めることができるようになりました。 これを行うには、次のキーを app.config ファイルの `appSettings` セクションに追加します。

  ```xml
  <add key="Transactions:IncludeDistributedTransactionIdInExceptionMessage" value="true"/>
  ```

  既定値は `false` です。

- **ネットワーク**

  - **ソケットの再利用**

    Windows 10 には、送信 TCP 接続のローカル ポートを再利用してコンピューターのリソースを効率的に使用する新しいスケーラビリティの高いネットワーク アルゴリズムが含まれています。 .NET Framework 4.6 では、この新しいアルゴリズムがサポートされ、.NET アプリで新しい動作を利用できます。 以前のバージョンの Windows では、人工的なコンカレント接続の制限 (通常は動的なポート範囲の既定のサイズである 16,384) があったため、負荷がかかったときにポートが使い尽くされ、サービスのスケーラビリティが制限されることがありました。

    .NET Framework 4.6 では、ポートの再利用を有効にする次の 2 つの API が追加され、コンカレント接続に対する 64 KB の制限が実質的になくなりました。

    - <xref:System.Net.Sockets.SocketOptionName?displayProperty=nameWithType> 列挙型値。

    - <xref:System.Net.ServicePointManager.ReusePort%2A?displayProperty=nameWithType> プロパティ。

    既定では、`HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319` レジストリ キーの `HWRPortReuseOnSocketBind` の値が 0x1 に設定されないかぎり、<xref:System.Net.ServicePointManager.ReusePort%2A?displayProperty=nameWithType> プロパティは `false` になります。 HTTP 接続でのローカル ポートの再利用を有効にするには、<xref:System.Net.ServicePointManager.ReusePort%2A?displayProperty=nameWithType> プロパティを `true` に設定します。 これにより、<xref:System.Net.Http.HttpClient> および <xref:System.Net.HttpWebRequest> からの外向きのすべての TCP ソケット接続で Windows 10 の新しいソケット オプション [SO_REUSE_UNICASTPORT](/windows/desktop/WinSock/sol-socket-socket-options) が使用されるようになるため、ローカル ポートの再利用が可能になります。

    ソケット専用のアプリケーションを作成する開発者は、<xref:System.Net.Sockets.Socket.SetSocketOption%2A?displayProperty=nameWithType> などのメソッドを呼び出すときに <xref:System.Net.Sockets.SocketOptionName?displayProperty=nameWithType> オプションを指定することにより、送信ソケットでバインド中にローカル ポートを再利用できるようになります。

  - **国際ドメイン名と PunyCode のサポート**

    <xref:System.Uri> クラスに新しいプロパティ <xref:System.Uri.IdnHost%2A> が追加され、国際ドメイン名と PunyCode のサポートが強化されました。

- **Windows フォーム コントロールでのサイズ変更**

  この機能が .NET Framework 4.6 にも拡張されました。<xref:System.Windows.Forms.DomainUpDown>、<xref:System.Windows.Forms.NumericUpDown>、<xref:System.Windows.Forms.DataGridViewComboBoxColumn>、<xref:System.Windows.Forms.DataGridViewColumn>、<xref:System.Windows.Forms.ToolStripSplitButton> 型、および <xref:System.Drawing.Design.UITypeEditor> を描画するときに使用する <xref:System.Drawing.Design.PaintValueEventArgs.Bounds%2A> プロパティで指定される四角形も含まれるようになりました。

  これはオプトイン機能です。 この機能を有効にするには、アプリケーション構成 (app.config) ファイルで `EnableWindowsFormsHighDpiAutoResizing` 要素を `true` に設定します。

  ```xml
  <appSettings>
      <add key="EnableWindowsFormsHighDpiAutoResizing" value="true" />
  </appSettings>
  ```

- **コード ページのエンコーディングのサポート**

  .NET Core は、本来 Unicode エンコードをサポートし、既定ではコード ページ エンコーディングに関するサポートは限定的です。 <xref:System.Text.Encoding.RegisterProvider%2A?displayProperty=nameWithType> メソッドを使用してコード ページ エンコードを登録することで、.NET Framework では利用できるものの .NET Core ではサポートされていないコード ページ エンコードのサポートを追加できます。 詳細については、「<xref:System.Text.CodePagesEncodingProvider?displayProperty=nameWithType>」を参照してください。

- **.NET ネイティブ**

  .NET Core をターゲットとし、C# または Visual Basic で作成されている Windows 10 用の Windows アプリは、IL ではなくネイティブ コードにアプリをコンパイルする新しい技術を活用できます。 これで、起動時間と実行時間がより速いアプリを生成できます。 詳しくは、「[.NET ネイティブによるアプリのコンパイル](../net-native/index.md)」をご覧ください。 JIT コンパイルと NGEN による結果の違い、およびコードにおけるその影響の概要については、「[.NET ネイティブとコンパイル](../net-native/net-native-and-compilation.md)」をご覧ください。

  ご利用のアプリは、Visual Studio 2015 以降でコンパイルするときに、既定でネイティブ コードにコンパイルされます。 詳しくは、「[.NET ネイティブの概要](../net-native/getting-started-with-net-native.md)」をご覧ください。

  .NET ネイティブ アプリのデバッグをサポートするため、アンマネージ デバッグ APIに対して数多くの新しいインターフェイスと列挙型が追加されました。 詳しくは、「[デバッグ (アンマネージ API リファレンス)](../unmanaged-api/debugging/index.md)」をご覧ください。

- **オープン ソースの .NET Framework パッケージ**

  .NET Core のパッケージ (変更できないコレクションなど)、[SIMD API](https://www.nuget.org/packages/Microsoft.Bcl.Simd)、およびネットワーク API (<xref:System.Net.Http> 名前空間に含まれるものなど) は、[GitHub](https://github.com/) でオープンソース パッケージとして入手できるようになりました。 このコードにアクセスするには、[GitHub 上の .NET](https://github.com/dotnet/runtime) をご覧ください。 これらのパッケージの詳細、および投稿方法については、「[.NET Core とオープン ソース](../get-started/net-core-and-open-source.md)」および [GitHub の .NET ホーム ページ](https://github.com/dotnet/home)を参照してください。

<a name="v452" />

## <a name="whats-new-in-net-framework-452"></a>.NET Framework 4.5.2 の新機能

- **ASP.NET アプリ用の新しい API。** 新しい <xref:System.Web.HttpResponse.AddOnSendingHeaders%2A?displayProperty=nameWithType> メソッドと <xref:System.Web.HttpResponseBase.AddOnSendingHeaders%2A?displayProperty=nameWithType> メソッドにより、応答がクライアント アプリにフラッシュされる際の、応答ヘッダーと状態コードを確認および変更できます。 <xref:System.Web.HttpApplication.PreSendRequestHeaders> イベントや <xref:System.Web.HttpApplication.PreSendRequestContent> イベントの代わりに、より効率的で信頼性の高いこれらのメソッドの使用を検討してください。

  <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem%2A?displayProperty=nameWithType> メソッドにより、小さなバックグラウンド作業項目をスケジュールできます。 ASP.NET はこれらの項目を追跡し、すべてのバックグラウンド作業項目が完了するまで IIS が突然ワーカー プロセスを終了しないようにします。 このメソッドは、ASP.NET マネージド アプリ ドメインの外部で呼び出すことはできません。

  新しい <xref:System.Web.HttpResponse.HeadersWritten?displayProperty=nameWithType> プロパティと <xref:System.Web.HttpResponseBase.HeadersWritten?displayProperty=nameWithType> プロパティは、応答ヘッダーが書き込まれたかどうかを示すブール値を返します。 これらのプロパティを使用して、<xref:System.Web.HttpResponse.StatusCode%2A?displayProperty=nameWithType> (ヘッダーが書き込まれた場合に例外をスローする) などの API への呼び出しが成功することを確認できます。

- **Windows フォーム コントロールでのサイズ変更** この機能が拡張されました。 これにより、システム DPI 設定を使用して、次の追加コントロールのコンポーネント (例: コンボ ボックスのドロップダウン矢印) をサイズ変更できます。

  - <xref:System.Windows.Forms.ComboBox>
  - <xref:System.Windows.Forms.ToolStripComboBox>
  - <xref:System.Windows.Forms.ToolStripMenuItem>
  - <xref:System.Windows.Forms.Cursor>
  - <xref:System.Windows.Forms.DataGridView>
  - <xref:System.Windows.Forms.DataGridViewComboBoxColumn>

  これはオプトイン機能です。 この機能を有効にするには、アプリケーション構成 (app.config) ファイルで `EnableWindowsFormsHighDpiAutoResizing` 要素を `true` に設定します。

  ```xml
  <appSettings>
      <add key="EnableWindowsFormsHighDpiAutoResizing" value="true" />
  </appSettings>
  ```

- **新しいワークフロー機能。** <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> メソッドを使用している (および、結果として <xref:System.Transactions.IPromotableSinglePhaseNotification> インターフェイスを実装している) リソース マネージャーは、新しい <xref:System.Transactions.Transaction.PromoteAndEnlistDurable%2A?displayProperty=nameWithType> メソッドを使用して、次の事柄を要求できます。

  - トランザクションを Microsoft 分散トランザクション コーディネーター (MSDTC) トランザクションに昇格する。

  - <xref:System.Transactions.IPromotableSinglePhaseNotification> を <xref:System.Transactions.ISinglePhaseNotification> (単一フェーズのコミットをサポートする永続的登録リスト) に置き換えます。

  これは同じアプリ ドメイン内で実行でき、MSDTC とやり取りして昇格を実行するためにアンマネージ コードを追加する必要はありません。 <xref:System.Transactions?displayProperty=nameWithType> から昇格可能な登録リストにより実装された <xref:System.Transactions.IPromotableSinglePhaseNotification>`Promote` メソッドへの未処理の呼び出しがある場合のみ、この新しいメソッドを呼び出すことができます。

- **プロファイリングの機能強化。** 次の新しいアンマネージ プロファイリング API により、さらに信頼性の高いプロファイリングを提供します。

  - [COR_PRF_ASSEMBLY_REFERENCE_INFO 構造体](../unmanaged-api/profiling/cor-prf-assembly-reference-info-structure.md)
  - [COR_PRF_HIGH_MONITOR 列挙型](../unmanaged-api/profiling/cor-prf-high-monitor-enumeration.md)
  - [GetAssemblyReferences メソッド](../unmanaged-api/profiling/icorprofilercallback6-getassemblyreferences-method.md)
  - [GetEventMask2 メソッド](../unmanaged-api/profiling/icorprofilerinfo5-geteventmask2-method.md)
  - [SetEventMask2 メソッド](../unmanaged-api/profiling/icorprofilerinfo5-seteventmask2-method.md)
  - [AddAssemblyReference メソッド](../unmanaged-api/profiling/icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)

  以前の `ICorProfiler` の実装は、依存アセンブリの遅延読み込みをサポートしていました。 新しいプロファイリング API では、プロファイラーにより挿入される依存アセンブリを、アプリの完全な初期化後に読み込むのではなく、すぐに読み込む必要があります。 この変更は、既存の `ICorProfiler` API のユーザーには影響しません。

- **デバッグの機能強化。** 次の新しいアンマネージド デバッグ API により、プロファイラーとの統合性が向上しました。 これにより、ダンプのデバッグ時にコンパイラ ReJIT 要求により作成されたローカル変数やコードだけでなく、プロファイラーにより挿入されたメタデータにアクセスできます。

  - [SetWriteableMetadataUpdateMode メソッド](../unmanaged-api/debugging/icordebugprocess7-setwriteablemetadataupdatemode-method.md)
  - [EnumerateLocalVariablesEx メソッド](../unmanaged-api/debugging/icordebugilframe4-enumeratelocalvariablesex-method.md)
  - [GetLocalVariableEx メソッド](../unmanaged-api/debugging/icordebugilframe4-getlocalvariableex-method.md)
  - [GetCodeEx メソッド](../unmanaged-api/debugging/icordebugilframe4-getcodeex-method.md)
  - [GetActiveReJitRequestILCode メソッド](../unmanaged-api/debugging/icordebugfunction3-getactiverejitrequestilcode-method.md)
  - [GetInstrumentedILMap メソッド](../unmanaged-api/debugging/icordebugilcode2-getinstrumentedilmap-method.md)

- **イベント トレーシングの変更。** .NET Framework 4.5.2 では、より大きなサーフェイス領域において、アウトプロセスの Windows イベント トレーシング (ETW) に基づくアクティビティ トレーシングができるようになりました。 これにより、アドバンスト パワー マネージメント (APM) ベンダーは、スレッドを越えた個々の要求とアクティビティのコストを正確に追跡する軽量ツールを提供できます。  これらのイベントは、ETW コントローラーで有効にされた場合にのみ発生します。したがって、変更は以前に記述された ETW コードや ETW が無効な状態で実行されるコードには影響しません。

- **トランザクションの昇格と永続参加リストへの変換**

  <xref:System.Transactions.Transaction.PromoteAndEnlistDurable%2A?displayProperty=nameWithType> は、.NET Framework 4.5.2 および 4.6 に追加された新しい API です。

  ```csharp
  [System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name = "FullTrust")]
  public Enlistment PromoteAndEnlistDurable(Guid resourceManagerIdentifier,
                                            IPromotableSinglePhaseNotification promotableNotification,
                                            ISinglePhaseNotification enlistmentNotification,
                                            EnlistmentOptions enlistmentOptions)
  ```

  ```vb
  <System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust")>
  public Function PromoteAndEnlistDurable(resourceManagerIdentifier As Guid,
                                          promotableNotification As IPromotableSinglePhaseNotification,
                                          enlistmentNotification As ISinglePhaseNotification,
                                          enlistmentOptions As EnlistmentOptions) As Enlistment
  ```

  このメソッドは、以前に <xref:System.Transactions.ITransactionPromoter.Promote%2A?displayProperty=nameWithType> メソッドへの応答として <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A?displayProperty=nameWithType> によって作成された参加リストで使用できます。 これは、`System.Transactions` に対して、トランザクションを MSDTC トランザクションに昇格させ、昇格可能参加リストを永続参加リストに "変換" するように要求します。 このメソッドが正常に完了すると、<xref:System.Transactions.IPromotableSinglePhaseNotification> インターフェイスが `System.Transactions` から参照されなくなり、その後の通知は指定された <xref:System.Transactions.ISinglePhaseNotification> インターフェイスに到着します。 問題の参加リストは、永続参加リストとして機能し、トランザクションのログ記録と復旧をサポートする必要があります。 詳細については、<xref:System.Transactions.Transaction.EnlistDurable%2A?displayProperty=nameWithType> を参照してください。 さらに、この参加リストは <xref:System.Transactions.ISinglePhaseNotification> もサポートする必要があります。  このメソッドは、<xref:System.Transactions.ITransactionPromoter.Promote%2A?displayProperty=nameWithType> の呼び出しの処理中に*のみ*呼び出すことができます。 そうでない場合は、<xref:System.Transactions.TransactionException> 例外がスローされます。

<a name="v451" />

## <a name="whats-new-in-net-framework-451"></a>.NET Framework 4.5.1 の新機能

**2014 年 4 月の更新**:

- [Visual Studio 2013 更新プログラム 2](https://go.microsoft.com/fwlink/p/?LinkId=393658) には、次のシナリオをサポートするポータブル クラス ライブラリ テンプレートの更新が含まれています。

  - Windows 8.1、Windows Phone 8.1、および Windows Phone Silverlight 8.1 を対象とするポータブル ライブラリで Windows ランタイム API を使用できます。

  - Windows 8.1 または Windows Phone 8.1 を対象とする場合、ポータブル ライブラリに XAML (Windows.UI.XAML 型) を含めることができます。 次の XAML テンプレートがサポートされています: 空白のページ、リソース ディクショナリ、テンプレート コントロール、およびユーザー コントロール。

  - Windows 8.1 および Windows Phone 8.1 を対象とするストア アプリで使用するためにポータブル Windows ラインタイム コンポーネント (.winmd ファイル) を作成できます。

  - ポータブル クラス ライブラリのような Windows ストアまたは Windows Phone ストア クラス ライブラリを再ターゲットできます。

  これらの変更の詳細については、[ポータブル クラス ライブラリ](../../standard/cross-platform/cross-platform-development-with-the-portable-class-library.md)に関するページを参照してください。

- .NET Framework のコンテンツ セットには、Windows アプリをビルドして配置するためのプリコンパイル テクノロジである .NET Native のドキュメントが含まれます。 .NET Native は、中間言語 (IL) ではなくネイティブ コードへアプリを直接コンパイルすることにより、パフォーマンスを向上させます。 詳しくは、「[.NET ネイティブによるアプリのコンパイル](../net-native/index.md)」をご覧ください。

- [.NET Framework Reference Source](https://referencesource.microsoft.com/) で、新しい参照エクスペリエンスと強化された機能が提供されます。 これにより、.NET Frameworkのソース コードをオンラインで参照したり、[リファレンスをダウンロード](https://referencesource.microsoft.com/download.html)してオフラインで表示したりできます。さらに、デバッグ中にソース (パッチや更新を含む) をステップ実行できます。 詳細については、ブログ記事「[A new look for .NET Reference Source (.NET Reference Source の新しい外観)](https://devblogs.microsoft.com/dotnet/a-new-look-for-net-reference-source/)」を参照してください。

.NET Framework 4.5.1 の基底クラスの新機能と機能強化には次が含まれます。

- アセンブリの自動バインディング リダイレクト。 Visual Studio 2013 以降では、アプリまたはそのコンポーネントが同じアセンブリの複数バージョンを参照している場合、.NET Framework 4.5.1 を対象とするアプリのコンパイル時に、バインディング リダイレクトをアプリ構成ファイルに追加できます。 また、.NET Framework の以前のバージョンを対象とするプロジェクトで、この機能を有効にすることもできます。 詳細については、[自動バインディング リダイレクトを有効/無効にする](../configure-apps/how-to-enable-and-disable-automatic-binding-redirection.md)」をご覧ください。

- 開発者がサーバーおよびクラウド アプリケーションのパフォーマンスを向上するために役立つ診断情報を収集する機能。 詳細については、<xref:System.Diagnostics.Tracing.EventSource.WriteEventWithRelatedActivityId%2A> クラスの <xref:System.Diagnostics.Tracing.EventSource.WriteEventWithRelatedActivityIdCore%2A> メソッドと <xref:System.Diagnostics.Tracing.EventSource> メソッドを参照してください。

- ガベージ コレクション中に大きなオブジェクト ヒープ (LOH) を圧縮する機能。 詳細については、<xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode%2A?displayProperty=nameWithType> プロパティを参照してください。

- その他のパフォーマンス向上 (ASP.NET アプリの中断、マルチコア JIT の改良、.NET Framework 更新後のアプリ起動時間の短縮など)。 詳細については、ブログ記事「[.NET Framework 4.5.1 announcement](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/)」(.NET Framework 4.5.1 についてのお知らせ) および「[ASP.NET app suspend](https://devblogs.microsoft.com/dotnet/asp-net-app-suspend-responsive-shared-net-web-hosting/)」 (ASP.NET アプリケーションの中断) を参照してください。

Windows フォームの機能強化には次が含まれます。

- Windows フォーム コントロールでのサイズ変更。 アプリのアプリケーション構成ファイル (app.config) 内のエントリで有効にすることにより、システム DIP 設定を使用してコントロールのコンポーネント (例: プロパティ グリッドに表示されるアイコン) をサイズ変更できます。 現在、この機能は次の Windows フォーム コントロールでサポートされています。

  - <xref:System.Windows.Forms.PropertyGrid>
  - <xref:System.Windows.Forms.TreeView>
  - <xref:System.Windows.Forms.DataGridView> の一部 (サポートされている追加コントロールについては、「[4.5.2 の新機能](#v452)」を参照してください)

  この機能を有効にするには、構成ファイル (app.config) に新しい \<appSettings> 要素を追加し、`EnableWindowsFormsHighDpiAutoResizing` 要素を `true` に設定します。

  ```xml
  <appSettings>
      <add key="EnableWindowsFormsHighDpiAutoResizing" value="true" />
  </appSettings>
  ```

Visual Studio 2013 で .NET Framework アプリをデバッグするときの改善点は次のとおりです。

- Visual Studio デバッガーの戻り値。 Visual Studio 2013 でマネージド アプリをデバッグする場合、メソッドの戻り値の型と値が [自動変数] ウィンドウに表示されます。 デスクトップ アプリ、Windows ストア アプリ、Windows Phone アプリについて、この情報を使用できます。 詳細については、「[メソッド呼び出しの戻り値の調査](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/dn323257(v=vs.120))」をご覧ください。

- 64 ビット アプリのエディット コンティニュ。 Visual Studio 2013 では、デスクトップ、Windows ストア、Windows Phone 用の 64 ビット マネージド アプリについて、エディット コンティニュ機能がサポートされています。 既存の制限は、32 ビット アプリと 64 ビット アプリの両方でまだ有効です (「[Supported Code Changes (C#)](/visualstudio/debugger/supported-code-changes-csharp)」(サポートされているコードの変更 (C#)) の記事の最後のセクションを参照してください)。

- 非同期対応のデバッグ。 Visual Studio 2013 で非同期アプリを簡単にデバッグするために、呼び出し履歴には、非同期プログラミングをサポートするためにコンパイラに用意されているインフラストラクチャ コード、および論理上の親フレーム内のチェーンが表示されません。これにより、論理的なプログラムの実行をよりわかりやすく行うことができます。 [タスク] ウィンドウが [並列タスク] ウィンドウに代わって使用され、特定のブレークポイントに関連するタスクが表示されます。また、現在アクティブなタスクやアプリでスケジュールされているタスクなども表示されます。 この機能については、「[.NET Framework 4.5.1 announcement](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/)」(.NET Framework 4.5.1 についてのお知らせ) の「Async-aware debugging」(非同期対応のデバッグ) セクションを参照してください。

- Windows ランタイム コンポーネント向けのより適切な例外処理のサポート。 Windows 8.1 では、言語の種類に関係なく、Windows ストア アプリから発生する例外には、例外を発生させたエラーに関する情報が保持されます。 この機能については、「[.NET Framework 4.5.1 announcement](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/)」(.NET Framework 4.5.1 についてのお知らせ) の「Windows Store app development」(Windows ストア アプリ開発) のセクションを参照してください。

Visual Studio 2013 以降では、[Mpgo.exe (マネージド プロファイル ガイド付き最適化ツール)](../tools/mpgo-exe-managed-profile-guided-optimization-tool.md) を使って、Windows 8.x ストア アプリとデスクトップ アプリを最適化することができます。

ASP.NET 4.5.1 の新機能については、「[ASP.NET and Web Tools for Visual Studio 2013 のリリース ノート](/aspnet/visual-studio/overview/2013/release-notes)」を参照してください。

<a name="v45" />

## <a name="whats-new-in-net-framework-45"></a>.NET Framework 4.5 の新機能

### <a name="base-classes"></a>基底クラス

- 配置時に .NET Framework 4 アプリケーションを検出して終了することにより、システムの再起動を減らす機能。 「[.NET Framework 4.5 のインストール中のシステム再起動の削減](../deployment/reducing-system-restarts.md)」をご覧ください。

- 64 ビット プラットフォームでの 2 ギガバイト (GB) を超える配列のサポート。 この機能は、アプリケーション構成ファイルで有効にすることができます。 「[\<gcAllowVeryLargeObjects> 要素](../configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md)」も参照してください。オブジェクト サイズと配列サイズに関する他の制限も記載されています。

- サーバーのガベージ コレクションをバックグラウンドで行うことにより、パフォーマンスを改善します。 .NET Framework 4.5 でサーバーのガベージ コレクションを使うと、バックグラウンド ガベージ コレクションが自動的に有効になります。 「[ガベージ コレクションの基礎](../../standard/garbage-collection/fundamentals.md)」(ガベージ コレクションの基礎) トピックの「バックグラウンド サーバー ガベージ コレクション」というセクションをご覧ください。

- マルチコア プロセッサでオプションで使用できるバックグラウンドの Just-in-time (JIT) コンパイルを使用すると、アプリケーションのパフォーマンスが向上します。 「<xref:System.Runtime.ProfileOptimization>」を参照してください。

- 正規表現エンジンがタイムアウトする前に、正規表現の解決を試みる時間に制限を設ける機能。<xref:System.Text.RegularExpressions.Regex.MatchTimeout%2A?displayProperty=nameWithType> プロパティを参照してください。

- アプリケーション ドメインの既定のカルチャを定義する機能。 詳細については、<xref:System.Globalization.CultureInfo> クラスのトピックを参照してください。

- Unicode UTF-16 エンコードのコンソール サポート。 詳細については、<xref:System.Console> クラスのトピックを参照してください。

- カルチャに従った文字列の順序のバージョン指定と比較データのサポート。 詳細については、<xref:System.Globalization.SortVersion> クラスのトピックを参照してください。

- リソースを取得するときのパフォーマンスを改善します。 「[リソースのパッケージ化と配置](../resources/packaging-and-deploying-resources-in-desktop-apps.md)」を参照してください。

- Zip 圧縮の機能強化により、圧縮ファイルのサイズが小さくなりました。 <xref:System.IO.Compression?displayProperty=nameWithType> 名前空間を参照してください。

- <xref:System.Reflection.Context.CustomReflectionContext> クラスを使用して、既定のリフレクションの動作をオーバーライドするリフレクション コンテキストをカスタマイズできる機能。

- <xref:System.Globalization.IdnMapping?displayProperty=nameWithType> クラスを Windows 8 で使用した場合の IDNA (Internationalized Domain Names in Applications) 規格の 2008 バージョンのサポート。

- オペレーティング システムへの文字列比較の処理代行。 .NET Framework を Windows 8 で使ったときに、Unicode 6.0 が実装されます。 他のプラットフォームで実行されている場合、.NET Framework には、Unicode 5.x. を実装する独自の文字列比較データが含まれています。 <xref:System.String> クラスおよび <xref:System.Globalization.SortVersion> クラスの「コメント」セクションを参照してください。

- アプリケーション ドメインごとに文字列のハッシュ コードを計算する機能。 「[\<UseRandomizedStringHashAlgorithm> 要素](../configure-apps/file-schema/runtime/userandomizedstringhashalgorithm-element.md)」をご覧ください。

- <xref:System.Type> クラスと <xref:System.Reflection.TypeInfo> クラスで、タイプ リフレクションのサポートが分割されました。 「[Windows ストア アプリのための .NET Framework のリフレクション](../reflection-and-codedom/reflection-for-windows-store-apps.md)」をご覧ください。

### <a name="managed-extensibility-framework-mef"></a>MEF (Managed Extensibility Framework)

.NET Framework 4.5 では、MEF (Managed Extensibility Framework) に次の新機能が用意されています。

- ジェネリック型のサポート。

- 属性ではなく命名規則に基づいてパーツを作成できるようにする、規則ベースのプログラミング モデル。

- 複数のスコープ。

- Windows 8.x ストア アプリを作成するときに使うことができる MEF のサブセット。 このサブセットは、[ダウンロード可能パッケージ](https://www.nuget.org/packages/Microsoft.Composition)として NuGet ギャラリーから入手できます。 パッケージをインストールするには、Visual Studio でプロジェクトを開き、 **[プロジェクト]** メニューの **[NuGet パッケージの管理]** をクリックし、`Microsoft.Composition` パッケージをオンラインで検索します。

詳しくは、「[Managed Extensibility Framework (MEF)](../mef/index.md)」を参照してください。

### <a name="asynchronous-file-operations"></a>非同期のファイル操作

.NET Framework 4.5 では、新しい非同期機能が C# および Visual Basic 言語に追加されました。 これらの機能は、非同期操作を実行するためのタスク ベースのモデルを追加します。 この新しいモデルを使用するには、I/O クラスで非同期メソッドを使用します。 「[非同期ファイル I/O](../../standard/io/asynchronous-file-i-o.md)」を参照してください。

<a name="tools" />

### <a name="tools"></a>ツール

.NET Framework 4.5 では、リソース ファイル ジェネレーター (Resgen.exe) を使うと、.NET Framework アセンブリに埋め込まれた .resources ファイルから Windows 8.x ストア アプリ用の .resw ファイルを作成することができます。 詳しくは、「[Resgen.exe (リソース ファイル ジェネレーター)](../tools/resgen-exe-resource-file-generator.md)」をご覧ください。

マネージド プロファイル ガイド付き最適化ツール (Mpgo.exe) を使用すると、ネイティブ イメージ アセンブリを最適化することによって、アプリケーションの起動時間、メモリの使用率 (ワーキング セットのサイズ)、およびスループットを向上させることができます。 このコマンド ライン ツールは、ネイティブ イメージ アプリケーション アセンブリ用のプロファイル データを生成します。 「[Mpgo.exe (マネージド プロファイル ガイド付き最適化ツール)](../tools/mpgo-exe-managed-profile-guided-optimization-tool.md)」をご覧ください。 Visual Studio 2013 以降では、Windows 8.x ストア アプリおよびデスクトップ アプリを最適化するために、Mpgo.exe を使うことができます。

<a name="parallel" />

### <a name="parallel-computing"></a>並列コンピューティング

.NET Framework 4.5 では、並列計算用にいくつかの新機能と機能強化が提供されています。 パフォーマンスの向上、コントロールの強化、非同期プログラミングのサポートの改善、新しいデータフロー ライブラリ、並列デバッグとパフォーマンス分析のためのサポートの強化などが挙げられます。 ブログ「Parallel Programming with .NET」(.NET での並列プログラミング) の記事「[What’s New for Parallelism in .NET 4.5](https://devblogs.microsoft.com/pfxteam/whats-new-for-parallelism-in-net-4-5/)」(.NET 4.5 での並列処理の新機能) を参照してください。

<a name="web" />

### <a name="web"></a>Web

ASP.NET 4.5 および 4.5.1 では、Web フォーム モデルのバインディング、WebSocket のサポート、非同期ハンドラー、パフォーマンスの向上などの多くの機能が追加されています。 詳細については、次のリソースを参照してください。

- [ASP.NET 4.5 と Visual Studio 2012](https://docs.microsoft.com/previous-versions/aspnet/hh420390(v=vs.110))

- [Visual Studio 2013 の ASP.NET と Web ツールのリリース ノート](/aspnet/visual-studio/overview/2013/release-notes)

### <a name="networking-a-namenetworking-"></a>ネットワーク <a name="networking" />

.NET Framework 4.5 は、HTTP アプリケーションに新しいプログラミング インターフェイスを提供します。 詳細については、新しい <xref:System.Net.Http?displayProperty=nameWithType> 名前空間と <xref:System.Net.Http.Headers?displayProperty=nameWithType> 名前空間を参照してください。

既存の <xref:System.Net.HttpListener> と関連クラスを使用して WebSocket 接続を受け入れ、やり取りするための新しいプログラミング インターフェイスのサポートも含まれています。 詳細については、新しい <xref:System.Net.WebSockets> 名前空間と <xref:System.Net.HttpListener> クラスを参照してください。

また .NET Framework 4.5 には、次のネットワークの機能強化が含まれています。

- RFC 準拠の URI サポート。 詳細については、<xref:System.Uri> および関連クラスを参照してください。

- 国際化ドメイン名 (IDN: Internationalized Domain Name) による解析のサポート。 詳細については、<xref:System.Uri> および関連クラスを参照してください。

- 電子メール アドレスの国際化 (EAI) のサポート。 詳細については、「<xref:System.Net.Mail>」を参照してください。

- 強化された IPv6 サポート。 詳細については、「<xref:System.Net.NetworkInformation>」を参照してください。

- デュアル モードのソケットのサポート。 詳細については、<xref:System.Net.Sockets.Socket> クラスおよび <xref:System.Net.Sockets.TcpListener> クラスを参照してください。

<a name="client" />

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

.NET Framework 4.5 では、WPF (Windows Presentation Foundation) の次の領域の機能が変更および強化されています。

- クイック アクセス ツール バー、アプリケーション メニューおよびタブをホストするリボン ユーザー インターフェイスを実装できる新しい <xref:System.Windows.Controls.Ribbon.Ribbon> コントロール。

- 同期および非同期のデータ検証をサポートする新しい <xref:System.ComponentModel.INotifyDataErrorInfo> インターフェイス。

- <xref:System.Windows.Controls.VirtualizingPanel> および <xref:System.Windows.Threading.Dispatcher> クラスの新しい機能。

- グループ化された大量のデータを表示するときに、非 UI スレッドのコレクションにアクセスすることによってパフォーマンスを向上。

- 静的プロパティへのデータ バインディング、<xref:System.Reflection.ICustomTypeProvider> インターフェイスを実装するカスタム型へのデータ バインディング、およびバインディング式からのデータ バインディング情報の取得。

- 値の変更に伴うデータの再配置 (ライブ形成)。

- 項目コンテナーのデータ コンテキストを切断するかどうかをチェックする機能。

- プロパティの変更とデータ ソースの更新までの経過時間を設定する機能。

- 弱いイベント パターン実装時のサポートの強化。 また、イベントでマークアップ拡張機能を使用することもできるようになりました。

<a name="windows_communication_foundation" />

### <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

.NET Framework 4.5 では、Windows Communication Foundation (WCF) アプリケーションの記述と保守を簡単にするため、次の機能が追加されました。

- 生成された構成ファイルの簡略化。

- コントラクト優先開発のサポート。

- ASP.NET 互換モードをより簡単に構成できる機能。

- 既定のトランスポート プロパティ値に変更を加え、設定しなければならない可能性を低減しました。

- <xref:System.Xml.XmlDictionaryReaderQuotas> クラスを更新して、XML ディクショナリ リーダーのクォータを手動で構成する手間を省けるようにしました。

- ビルド処理の一部として Visual Studio による WCF 構成ファイルを検証することで、アプリケーションを実行する前に構成エラーを検出できます。

- 新しい非同期ストリーミングのサポート。

- Internet Information Services (IIS) での HTTPS 上のエンドポイントを公開しやすくする新しい HTTPS プロトコル マッピング。

- `?singleWSDL` をサービス URL に追加して 1 つの WSDL ドキュメントのメタデータを生成する機能。

- TCP トランスポートと同様のパフォーマンス特性を持つポート 80 と 443 で真の双方向通信を可能にする Websockets のサポート。

- コード内の構成サービスのサポート。

- XML エディターのツール ヒント。

- <xref:System.ServiceModel.ChannelFactory> キャッシュのサポート。

- バイナリ エンコーダーの圧縮サポート。

- 開発者が「ファイア アンド フォーゲット (撃ち放し)」メッセージングを使用するサービスを作成できるようにする UDP トランスポートのサポート。 クライアントは、サービスにメッセージを送信しますが、サービスからの応答を期待しません。

- HTTP トランスポートとトランスポート セキュリティを使用したときに、単一の WCF エンドポイントで複数の認証モードをサポートする機能。

- 国際化ドメイン名 (IDN: Internationalized Domain Name) を使用する WCF サービスのサポート。

詳細については、「[Windows Communication Foundation 4.5 の新機能](../wcf/whats-new.md)」を参照してください。

<a name="windows_workflow_foundation" />

### <a name="windows-workflow-foundation-wf"></a>Windows Workflow Foundation (WF)

.NET Framework 4.5 では、次に示すようないくつかの新しい機能が Windows Workflow Foundation (WF) に追加されました。

- 最初に .NET Framework 4.0.1 ([.NET Framework 4 Platform Update 1](https://docs.microsoft.com/archive/blogs/endpoint/microsoft-net-framework-4-platform-update-1)) の一部として導入された、ステート マシンのワークフロー。 この更新プログラムには、開発者がステート マシン ワークフローを作成できるようにする新しいクラスとアクティビティが複数含まれていました。 .NET Framework 4.5 では、これらのクラスとアクティビティが、次を含むように更新されました。

  - 状態でブレークポイントを設定する機能。

  - ワークフロー デザイナーで遷移をコピーして貼り付ける機能。

  - 共有されるトリガーの遷移作成のデザイナー サポート。

  - <xref:System.Activities.Statements.StateMachine>、 <xref:System.Activities.Statements.State>、および <xref:System.Activities.Statements.Transition> などのようにステート マシン ワークフローを作成する機能。

- 次のようなワークフロー デザイナーの機能が強化されました。

  - **[クイック検索]** や **[フォルダーを指定して検索]** など、Visual Studio で強化されたワークフロー検索機能。

  - 2 番目の子アクティビティがコンテナー アクティビティに追加されたときに、自動的にシーケンス アクティビティを作成し、両方のアクティビティをシーケンス アクティビティに含める機能。

  - スクロール バーを使用せずに変更されるワークフローの表示部分を変更できるようにするパン サポート。

  - ツリー スタイルのアウトライン ビューでワークフローのコンポーネントを表示し、 **[ドキュメント アウトライン]** ビューでコンポーネントを選択できるようにする新しい **[ドキュメント アウトライン]** ビュー。

  - アクティビティに注釈を追加できる機能。

  - ワークフロー デザイナーでアクティビティ デリゲートを定義および使用する機能。

  - ステート マシンのアクティビティと遷移、およびフローチャート ワークフローの自動接続と自動挿入。

- ワークフローのビュー状態情報を XAML ファイルの 1 つの要素に保管して、ビュー状態情報を簡単に見つけ、編集できるようにします。

- 子アクティビティの永続化を防ぐ NoPersistScope コンテナー アクティビティ。

- C# 式のサポート:

  - Visual Basic を使用するワークフロー プロジェクトは、Visual Basic 式を使用し、C# ワークフロー プロジェクトは C# 式を使用します。

  - Visual Studio 2010 で作成され、Visual Basic 式が使用されている C# ワークフロー プロジェクトは、C# 式を使用する C# ワークフロー プロジェクトと互換性があります。

- バージョン管理機能の強化

  - 永続化されたワークフロー インスタンスとワークフロー定義間のマッピングを提供する新しい <xref:System.Activities.WorkflowIdentity> クラス。

  - <xref:System.ServiceModel.Activities.WorkflowServiceHost> を含む同じホスト内の複数のワークフローのバージョンの並列実行。

  - 動的更新プログラムで、永続化されたワークフロー インスタンスの定義を変更する機能。

- 既存のサービス コントラクトに合わせて自動的にアクティビティを生成するサポートが提供されるコントラクト優先のワークフロー サービス開発。

詳細については、「[.NET 4.5 での Windows Workflow Foundation の新機能](../windows-workflow-foundation/whats-new-in-wf-in-dotnet.md)」を参照してください。

<a name="tailored" />

### <a name="net-for-windows-8x-store-apps"></a>Windows 8.x ストア アプリ用 .NET

Windows 8.x ストア アプリは、特定のフォーム ファクターに合わせて設計されており、Windows オペレーティング システムの機能を利用します。 C# または Visual Basic を使って Windows 用の Windows 8.x ストア アプリをビルドするために、.NET Framework 4.5 または 4.5.1 のサブセットを使うことができます。 このサブセットは Windows 8.x ストア アプリ用 .NET と呼ばれ、[概要](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))のページで説明されています。

### <a name="portable-class-libraries-a-nameportable-"></a>ポータブル クラス ライブラリ <a name="portable" />

Visual Studio 2012 (および以降のバージョン) のポータブル クラス ライブラリ プロジェクトを使うと、複数の .NET Framework プラットフォームで動作するマネージド アセンブリを作成してビルドできます。 ポータブル クラス ライブラリ プロジェクトを使って、対象とするプラットフォーム (Windows Phone や Windows 8.x ストア アプリ用 .NET など) を選択できます。 プロジェクトで使用できる型およびメンバーは、自動的にこれらのプラットフォーム間で共通の型とメンバーに制限されます。 詳細については、[ポータブル クラス ライブラリ](../../standard/cross-platform/cross-platform-development-with-the-portable-class-library.md)に関するページを参照してください。

## <a name="see-also"></a>関連項目

- [NET Framework および特別なリリース](../get-started/the-net-framework-and-out-of-band-releases.md)
- [.NET Framework のアクセシビリティの新機能](whats-new-in-accessibility.md)
- [Visual Studio 2017 の新機能](/visualstudio/ide/whats-new-visual-studio-2017)
- [Visual Studio 2019 の新機能](/visualstudio/ide/whats-new-visual-studio-2019)
- [ASP.NET](/aspnet)
- [Visual Studio での C++ の新機能](/cpp/what-s-new-for-visual-cpp-in-visual-studio)
