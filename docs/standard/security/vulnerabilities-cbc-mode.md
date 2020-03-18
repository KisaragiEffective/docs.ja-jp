---
title: CBC 暗号化解除の脆弱性
description: パディングを使用して暗号ブロックチェーン (CBC) モードの対称復号化を使用してタイミングの脆弱性を検出し、軽減する方法について説明します。
ms.date: 06/12/2018
author: blowdart
ms.openlocfilehash: 4616ef9015b47ff232a17f058c7a0f1449f42e81
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159963"
---
# <a name="timing-vulnerabilities-with-cbc-mode-symmetric-decryption-using-padding"></a>パディングを使用した CBC モードの対称復号化に関するタイミングの脆弱性

検証可能な埋め込みが適用されている場合は、特に限定的なものを除き、暗号ブロックチェーン (CBC) モードで暗号化されたデータの暗号化を解除しても安全ではなくなります。環境. この略しは、現在認識されている暗号調査に基づいています。

## <a name="introduction"></a>はじめに

埋め込み oracle 攻撃とは、暗号化されたデータに対する攻撃の一種であり、攻撃者はキーを知らなくてもデータの内容を解読することができます。

Oracle は、実行中のアクションが正しいかどうかについての情報を攻撃者に示す "通知" を参照します。 子供とともにボードまたはカードゲームをプレイしてみてください。 私たちが大きな気に入った機能を持っている場合は、これが oracle であると考えています。 相手は、この oracle を使用して、次の移動を適切に計画することができます。

パディングは特定の暗号化用語です。 一部の暗号は、データの暗号化に使用されるアルゴリズムであり、各ブロックが固定サイズであるデータのブロックに対して機能します。 暗号化するデータがブロックを埋めるための適切なサイズではない場合、データはそのデータに埋め込まれます。 多くの形式の埋め込みでは、元の入力が適切なサイズであったとしても、常に埋め込みが存在している必要があります。 これにより、復号化時には、常に埋め込みを安全に削除できます。

この2つを組み合わせると、ソフトウェアの実装と埋め込み oracle によって、暗号化解除されたデータに有効な埋め込みがあるかどうかが明らかになります。 Oracle は、無効なブロックではなく、有効なブロックを処理するためにある程度までの時間を取るなど、"無効な埋め込み" という値を返すような単純なものになる可能性があります。

ブロックベースの暗号には、モードと呼ばれるもう1つのプロパティがあります。これは、最初のブロックのデータと2番目のブロックのデータとの関係を決定します。 最も一般的に使用されるモードの1つは、CBC です。 CBC は初期化ベクター (IV) と呼ばれる初期のランダムブロックを導入し、前のブロックと静的暗号化の結果を組み合わせて、同じキーを使用して同じメッセージを暗号化しても、必ずしも同じ暗号化された出力を生成しないようにします。

攻撃者は、データが正しいことを oracle が通知するまで、oracle の埋め込みを使用して、CBC データをどのように構造化し、oracle を公開するコードにわずかに変更されたメッセージを送信し、データを送信し続けることができます。 この応答から、攻撃者はメッセージバイトをバイト単位で復号化できます。

最新のコンピューターネットワークは、このような高品質なので、攻撃者はリモートシステムの実行時間の差が非常に小さく (0.1 ミリ秒未満) 検出される可能性があります。 暗号化が正常に行われなかったと想定されているアプリケーションは、データが改ざんされていない場合にのみ発生する可能性があり、復号化が成功したか失敗したかを確認するように設計されているツールからの攻撃に対して脆弱 このタイミングの違いは、言語やライブラリによっては、他の言語やライブラリによってはさらに重要になる場合がありますが、アプリケーションの障害への対応が考慮されると、すべての言語とライブラリに対して実際の脅威であると考えられるようになりました。

この攻撃は、暗号化されたデータを変更し、oracle を使用して結果をテストする機能に依存しています。 攻撃を完全に軽減する唯一の方法は、暗号化されたデータへの変更を検出し、それに対するアクションの実行を拒否することです。 これを行うための標準的な方法は、データの署名を作成し、操作を実行する前にその署名を検証することです。 署名は検証可能である必要があります。攻撃者が作成することはできません。それ以外の場合は、暗号化されたデータを変更し、変更されたデータに基づいて新しい署名を計算します。 適切な署名の1つの一般的な型は、キー付きハッシュメッセージ認証コード (HMAC) と呼ばれます。 HMAC は、HMAC を生成したユーザーとそれを検証しているユーザーだけが認識する秘密キーを取得するという点で、チェックサムとは異なります。 キーを所有していない場合、正しい HMAC を生成することはできません。 データを受信したら、暗号化されたデータを取得し、秘密キーと送信者共有を使用して HMAC を個別に計算した後、送信した HMAC を計算したものと比較します。 この比較は一定の時間にする必要があります。そうしないと、別の検出可能な oracle を追加して、異なる種類の攻撃を行うことができます。

要約すると、埋め込まれている CBC ブロック暗号を安全に使用するには、データの暗号化を解除する前に一定時間の比較を使用して検証する HMAC (または他のデータ整合性チェック) と組み合わせて使用する必要があります。 変更されたすべてのメッセージは、応答を生成するのに同じ時間を要するため、攻撃を防ぐことができます。

## <a name="who-is-vulnerable"></a>脆弱なユーザー

この脆弱性は、独自の暗号化と復号化を実行しているマネージアプリケーションとネイティブアプリケーションの両方に適用されます。 たとえば、次のようになります。

- 後でサーバーで暗号化を解除するためにクッキーを暗号化するアプリケーション。
- 列が後で復号化されるテーブルにユーザーがデータを挿入する機能を提供するデータベースアプリケーション。
- 転送中のデータを保護するために共有キーを使用する暗号化に依存するデータ転送アプリケーション。
- TLS トンネルの "内部" メッセージを暗号化および復号化するアプリケーション。

TLS だけを使用すると、これらのシナリオでは保護されない場合があることに注意してください。

脆弱なアプリケーション:

- PKCS # 7 や ANSI 923 などの検証可能なパディングモードで、CBC 暗号モードを使用してデータを復号化します。
- (MAC または非対称デジタル署名を使用して) データ整合性チェックを実行せずに、暗号化解除を実行します。

これは、暗号メッセージ構文 (PKCS # 7/CMS) EnvelopedData 構造体など、これらのプリミティブの上に構築されたアプリケーションにも当てはまります。

## <a name="related-areas-of-concern"></a>関連する関連領域

調査により、Microsoft は、メッセージに既知または予測可能なフッター構造が設定されている場合に、ISO 10126 と同等のパディングで埋め込まれる CBC メッセージについてさらに懸念しています。 たとえば、W3C XML 暗号化の構文と処理の推奨事項 (xmlenc、EncryptedXml) の規則に基づいてコンテンツが準備されているとします。 メッセージに署名するための W3C ガイダンスでは、暗号化は常に適切と見なされていましたが、Microsoft は常に暗号化を行うことをお勧めします。

非対称キーと任意のメッセージの間には固有の信頼関係がないため、アプリケーション開発者は常に非対称署名キーの適用性を検証することを考慮する必要があります。

## <a name="details"></a>詳細

これまで、HMAC や RSA 署名などの方法を使用して、重要なデータを暗号化して認証することが重要であるという合意がありました。 ただし、暗号化と認証の操作をシーケンス処理する方法については、明確なガイダンスはありません。 この記事で説明されている脆弱性により、Microsoft のガイダンスは、常に "暗号化-再署名" パラダイムを使用するようになりました。 つまり、まず対称キーを使用してデータを暗号化した後、暗号化テキスト (暗号化されたデータ) で MAC または非対称署名を計算します。 データの暗号化を解除する場合は、逆の操作を実行します。 まず、暗号化テキストの MAC または署名を確認してから、暗号化を解除します。

"Oracle 攻撃の埋め込み" と呼ばれる脆弱性のクラスは、10年以上にわたって存在することがわかっています。 これらの脆弱性により、攻撃者は、AES や3DES などの対称ブロックアルゴリズムによって暗号化されたデータの暗号化を解除できます。この場合、データのブロックあたり4096回以下の試行が行われます。 これらの脆弱性は、ブロック暗号が最後に検証可能な埋め込みデータで最も頻繁に使用されるという事実を活用します。 攻撃者が暗号文を改ざんし、改ざんによって最後に埋め込みの形式でエラーが発生したかどうかを確認できた場合、攻撃者はデータの暗号化を解除できます。

最初に、実際の攻撃は、埋め込みが有効かどうかに基づいて異なるエラーコードを返すサービス (ASP.NET 脆弱性[MS10](/security-updates/SecurityBulletins/2010/ms10-070)など) に基づいていました。 しかし、Microsoft は、有効な埋め込みと無効な埋め込みを処理するタイミングの違いのみを使用して、同様の攻撃を行うことが実用的であると考えています。

暗号化スキームによって署名が使用され、署名の検証が特定のデータ長 (内容にかかわらず) に対して実行される場合、データの整合性は、[サイドチャネル](https://en.wikipedia.org/wiki/Side-channel_attack)経由で攻撃者に情報を出力することなく検証できます。 整合性チェックでは改ざんされたメッセージが拒否されるため、oracle の脅威の脅威が軽減されます。

## <a name="guidance"></a>ガイダンス

まず、機密性を持つデータをトランスポート層セキュリティ (TLS) を介して送信することをお勧めします。これは、Secure Sockets Layer (SSL) の後継となります。

次に、次のようにアプリケーションを分析します。

- どのような暗号化が実行されているか、および使用しているプラットフォームと Api によってどのような暗号化が提供されているかを正確に把握します。
- 対称[ブロック暗号アルゴリズム](https://en.wikipedia.org/wiki/Block_cipher#Notable_block_ciphers)の各レイヤー (AES や3des など) での各使用方法には、秘密キー付きのデータ整合性チェック (非対称署名、HMAC、または暗号モードを GCM や CCM などの[認証済み暗号化](https://en.wikipedia.org/wiki/Authenticated_encryption)(AE) モードに変更する) の使用が組み込まれていることを確認してください。

現在の調査に基づき、一般的には、認証と暗号化の手順が非 AE 暗号化のモードに対して個別に実行されるときに、暗号化テキスト (encrypt-sign) の認証が最適なオプションであると考えられます。 ただし、暗号化には1つのサイズに適合するすべての答えはありません。この汎化は、professional 熟練からのアドバイスとして適切ではありません。

メッセージング形式を変更できないが、認証されていない CBC 復号化を実行するアプリケーションは、次のような軽減策を組み込むことをお勧めします。

- 復号化による埋め込みの検証または削除を許可せずに暗号化を解除します。
  - 適用された埋め込みを削除または無視する必要がある場合でも、アプリケーションに負荷を移すことになります。
  - その利点は、埋め込みの検証と削除を他のアプリケーションデータ検証ロジックに組み込むことができることです。 埋め込みの検証とデータの検証を一定の時間に実行できる場合は、脅威が軽減されます。
  - パディングの解釈によって認識されるメッセージ長が変わるため、このアプローチから生成されたタイミング情報が残っている可能性があります。
- 復号化パディングモードを ISO10126 に変更します。
  - ISO10126 復号化埋め込みは、PKCS7 暗号化パディングと ANSIX923 暗号化埋め込みの両方と互換性があります。
  - モードを変更すると、oracle ナレッジがブロック全体ではなく1バイトに変換されます。 ただし、コンテンツに既知のフッター (XML の終了要素など) が含まれている場合、関連する攻撃によってメッセージの残りの部分が攻撃される可能性があります。
  - これにより、攻撃者が同じプレーンテキストを別のメッセージオフセットを使用して複数回暗号化する場合でも、プレーンテキストの回復を防ぐことはできません。
- Dampen に対する復号化呼び出しの評価を、タイミングシグナルにします。
  - ホールド時間の計算には、埋め込みを含むデータセグメントに対して復号化操作が行う最大時間を超える最小値が必要です。
  - 時間計算は、<xref:System.Environment.TickCount?displayProperty=nameWithType> (ロールオーバー/オーバーフロー) を使用したり、2つのシステムタイムスタンプ (NTP 調整エラーが発生) を引いたりするのではなく、[高解像度のタイムスタンプを取得する方法](/windows/desktop/sysinfo/acquiring-high-resolution-time-stamps)に従って実行する必要があります。
  - 時間の計算は、最後に埋め込まれるだけでなく、マネージまたC++はアプリケーションで発生する可能性のあるすべての例外を含む、復号化操作を含む必要があります。
  - 成功または失敗が判明した場合、タイミングゲートは、期限切れになったときにエラーを返す必要があります。
- 認証されていない暗号化解除を実行するサービスでは、"無効な" メッセージが大量に発生していることを検出するための監視を実施する必要があります。
  - この信号には誤検知 (正当な破損データ) と偽陽性の両方が含まれていることに注意してください (回避検出に十分に長い時間をかけて攻撃を拡散します)。

## <a name="finding-vulnerable-code---native-applications"></a>脆弱なコードの検索-ネイティブアプリケーション

Windows Cryptography: Next Generation (CNG) ライブラリに対してビルドされたプログラムの場合:

- 暗号化解除呼び出しは、`BCRYPT_BLOCK_PADDING` フラグを指定して、 [Bcryptdecrypt 化](/windows/desktop/api/bcrypt/nf-bcrypt-bcryptdecrypt)を行います。
- キーハンドルは、 [BCRYPT_CHAINING_MODE](/windows/desktop/SecCNG/cng-property-identifiers#BCRYPT_CHAINING_MODE)が `BCRYPT_CHAIN_MODE_CBC`に設定された[bcryptsetproperty](/windows/desktop/api/bcrypt/nf-bcrypt-bcryptsetproperty)を呼び出すことによって初期化されています。
  - `BCRYPT_CHAIN_MODE_CBC` が既定値であるため、影響を受けるコードには、`BCRYPT_CHAINING_MODE`の値が割り当てられていない可能性があります。

以前の Windows 暗号化 API に対してビルドされたプログラムの場合:

- 復号化の呼び出しは、`Final=TRUE`を使用して暗号[解除](/windows/desktop/api/wincrypt/nf-wincrypt-cryptdecrypt)を行うことです。
- キーハンドルは、 [KP_MODE](/windows/desktop/api/wincrypt/nf-wincrypt-cryptgetkeyparam)を `CRYPT_MODE_CBC`に設定して[CryptSetKeyParam](/windows/desktop/api/wincrypt/nf-wincrypt-cryptsetkeyparam)を呼び出すことによって初期化されています。
  - `CRYPT_MODE_CBC` が既定値であるため、影響を受けるコードには、`KP_MODE`の値が割り当てられていない可能性があります。

## <a name="finding-vulnerable-code---managed-applications"></a>脆弱なコード管理アプリケーションの検索

- 復号化呼び出しは <xref:System.Security.Cryptography.SymmetricAlgorithm?displayProperty=nameWithType>の <xref:System.Security.Cryptography.SymmetricAlgorithm.CreateDecryptor> または <xref:System.Security.Cryptography.SymmetricAlgorithm.CreateDecryptor(System.Byte[],System.Byte[])> メソッドに対して行います。
  - これには、.NET 内の次の派生型が含まれますが、サードパーティの型が含まれる場合もあります。
    - <xref:System.Security.Cryptography.Aes>
    - <xref:System.Security.Cryptography.AesCng>
    - <xref:System.Security.Cryptography.AesCryptoServiceProvider>
    - <xref:System.Security.Cryptography.AesManaged>
    - <xref:System.Security.Cryptography.DES>
    - <xref:System.Security.Cryptography.DESCryptoServiceProvider>
    - <xref:System.Security.Cryptography.RC2>
    - <xref:System.Security.Cryptography.RC2CryptoServiceProvider>
    - <xref:System.Security.Cryptography.Rijndael>
    - <xref:System.Security.Cryptography.RijndaelManaged>
    - <xref:System.Security.Cryptography.TripleDES>
    - <xref:System.Security.Cryptography.TripleDESCng>
    - <xref:System.Security.Cryptography.TripleDESCryptoServiceProvider>
- <xref:System.Security.Cryptography.SymmetricAlgorithm.Padding?displayProperty=nameWithType> プロパティが <xref:System.Security.Cryptography.PaddingMode.PKCS7?displayProperty=nameWithType>、<xref:System.Security.Cryptography.PaddingMode.ANSIX923?displayProperty=nameWithType>、または <xref:System.Security.Cryptography.PaddingMode.ISO10126?displayProperty=nameWithType>に設定されました。
  - <xref:System.Security.Cryptography.PaddingMode.PKCS7?displayProperty=nameWithType> が既定値であるため、影響を受けるコードに <xref:System.Security.Cryptography.SymmetricAlgorithm.Padding?displayProperty=nameWithType> プロパティが割り当てられていない可能性があります。
- <xref:System.Security.Cryptography.SymmetricAlgorithm.Mode?displayProperty=nameWithType> プロパティがに設定されました <xref:System.Security.Cryptography.CipherMode.CBC?displayProperty=nameWithType>
  - <xref:System.Security.Cryptography.CipherMode.CBC?displayProperty=nameWithType> が既定値であるため、影響を受けるコードに <xref:System.Security.Cryptography.SymmetricAlgorithm.Mode?displayProperty=nameWithType> プロパティが割り当てられていない可能性があります。

## <a name="finding-vulnerable-code---cryptographic-message-syntax"></a>脆弱性のあるコードの検出-暗号化メッセージの構文

暗号化されたコンテンツが、AES の CBC モード (2.16.840.1.101.3.4.1.2、2.16.840.1.101.3.4.1.22、2.16.840.1.101.3.4.1.42)、DES (1.3.14.3.2.7)、3DES (1.2.840.113549.3.7)、または RC2 (1.2.840.113549.3.2) を使用する、認証されていない CMS EnvelopedData メッセージCBC モードでその他のブロック暗号アルゴリズムを使用したメッセージを脆弱にします。

ストリーム暗号はこの特定の脆弱性の影響を受けませんが、Microsoft では、ContentEncryptionAlgorithm 値を検査することによって常にデータを認証することをお勧めします。

マネージアプリケーションの場合、CMS EnvelopedData blob は <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.Decode(System.Byte[])?displayProperty=fullName>に渡される任意の値として検出できます。

ネイティブアプリケーションの場合、cms EnvelopedData blob は、 [Cryptmsgupdate](/windows/desktop/api/wincrypt/nf-wincrypt-cryptmsgupdate)を介して cms ハンドルに提供される任意の値として検出できます。この場合、結果として得られる[CMSG_TYPE_PARAM](/windows/desktop/api/wincrypt/nf-wincrypt-cryptmsggetparam)が `CMSG_ENVELOPED` されます。また、cms ハンドルは、後で、 [cryptmsgupdate](/windows/desktop/api/wincrypt/nf-wincrypt-cryptmsgcontrol)経由で `CMSG_CTRL_DECRYPT` 命令を送信します。

## <a name="vulnerable-code-example---managed"></a>脆弱なコードの例-マネージド

このメソッドは、cookie を読み取り、復号化します。データ整合性チェックは表示されません。 このため、このメソッドによって読み取られたクッキーの内容は、その cookie を受け取ったユーザー、または暗号化された cookie 値を取得した攻撃者によって攻撃される可能性があります。

```csharp
private byte[] DecryptCookie(string cookieName)
{
    HttpCookie cookie = Request.Cookies[cookieName];

    if (cookie == null)
    {
        return null;
    }

    using (ICryptoTransform decryptor = _aes.CreateDecryptor())
    using (MemoryStream memoryStream = new MemoryStream())
    using (CryptoStream cryptoStream =
        new CryptoStream(memoryStream, decryptor, CryptoStreamMode.Write))
    {
        byte[] readCookie = Convert.FromBase64String(cookie.Value);
        cryptoStream.Write(readCookie, 0, readCookie.Length);
        cryptoStream.FlushFinalBlock();
        return memoryStream.ToArray();
    }
}
```

## <a name="example-code-following-recommended-practices---managed"></a>推奨される運用方法に従うコード例-マネージド

次のサンプルコードでは、標準ではないメッセージ形式のを使用します。

`cipher_algorithm_id || hmac_algorithm_id || hmac_tag || iv || ciphertext`

`cipher_algorithm_id` アルゴリズム識別子と `hmac_algorithm_id` アルゴリズム識別子は、これらのアルゴリズムのアプリケーションローカル (非標準) 表現です。 これらの識別子は、ベア連結されたバイトストリームとしてではなく、既存のメッセージングプロトコルの他の部分でも意味があります。

また、この例では、1つのマスターキーを使用して、暗号化キーと HMAC キーの両方を派生させます。 これは、シングルキーアプリケーションをデュアルキーアプリケーションに変換し、2つのキーを異なる値として保持するための便利な方法として提供されています。 さらに、HMAC キーと暗号化キーが同期されないことを保証します。

このサンプルでは、暗号化または復号化のための <xref:System.IO.Stream> は受け入れられません。 現在のデータ形式では、`hmac_tag` 値が暗号化テキストの前にあるため、ワンパス暗号化が困難になります。 ただし、この形式が選択されました。これは、パーサーをより単純にするために、すべての固定サイズの要素を先頭に維持するためです。 このデータ形式では、1回だけの暗号化解除が可能です。ただし、cautioned は、GetHashAndReset を呼び出して、TransformFinalBlock を呼び出す前に結果を確認することができます。 ストリーミング暗号化が重要な場合は、異なる AE モードが必要になることがあります。

```csharp
// ==++==
//
//   Copyright (c) Microsoft Corporation.  All rights reserved.
//
//   Shared under the terms of the Microsoft Public License,
//   https://opensource.org/licenses/MS-PL
//
// ==--==

using System;
using System.Diagnostics;
using System.IO;
using System.Runtime.CompilerServices;
using System.Security.Cryptography;

namespace Microsoft.Examples.Cryptography
{
    public enum AeCipher : byte
    {
        Unknown,
        Aes256CbcPkcs7,
    }

    public enum AeMac : byte
    {
        Unknown,
        HMACSHA256,
        HMACSHA384,
    }

    /// <summary>
    /// Provides extension methods to make HashAlgorithm look like .NET Core's
    /// IncrementalHash
    /// </summary>
    internal static class IncrementalHashExtensions
    {
        public static void AppendData(this HashAlgorithm hash, byte[] data)
        {
            hash.TransformBlock(data, 0, data.Length, null, 0);
        }

        public static void AppendData(
            this HashAlgorithm hash,
            byte[] data,
            int offset,
            int length)
        {
            hash.TransformBlock(data, offset, length, null, 0);
        }

        public static byte[] GetHashAndReset(this HashAlgorithm hash)
        {
            hash.TransformFinalBlock(Array.Empty<byte>(), 0, 0);
            return hash.Hash;
        }
    }

    public static partial class AuthenticatedEncryption
    {
        /// <summary>
        /// Use <paramref name="masterKey"/> to derive two keys (one cipher, one HMAC)
        /// to provide authenticated encryption for <paramref name="message"/>.
        /// </summary>
        /// <param name="masterKey">The master key from which other keys derive.</param>
        /// <param name="message">The message to encrypt</param>
        /// <returns>
        /// A concatenation of
        /// [cipher algorithm+chainmode+padding][mac algorithm][authtag][IV][ciphertext],
        /// suitable to be passed to <see cref="Decrypt"/>.
        /// </returns>
        /// <remarks>
        /// <paramref name="masterKey"/> should be a 128-bit (or bigger) value generated
        /// by a secure random number generator, such as the one returned from
        /// <see cref="RandomNumberGenerator.Create()"/>.
        /// This implementation chooses to block deficient inputs by length, but does not
        /// make any attempt at discerning the randomness of the key.
        ///
        /// If the master key is being input by a prompt (like a password/passphrase)
        /// then it should be properly turned into keying material via a Key Derivation
        /// Function like PBKDF2, represented by Rfc2898DeriveBytes. A 'password' should
        /// never be simply turned to bytes via an Encoding class and used as a key.
        /// </remarks>
        public static byte[] Encrypt(byte[] masterKey, byte[] message)
        {
            if (masterKey == null)
                throw new ArgumentNullException(nameof(masterKey));
            if (masterKey.Length < 16)
                throw new ArgumentOutOfRangeException(
                    nameof(masterKey),
                    "Master Key must be at least 128 bits (16 bytes)");
            if (message == null)
                throw new ArgumentNullException(nameof(message));

            // First, choose an encryption scheme.
            AeCipher aeCipher = AeCipher.Aes256CbcPkcs7;

            // Second, choose an authentication (message integrity) scheme.
            //
            // In this example we use the master key length to change from HMACSHA256 to
            // HMACSHA384, but that is completely arbitrary. This mostly represents a
            // "cryptographic needs change over time" scenario.
            AeMac aeMac = masterKey.Length < 48 ? AeMac.HMACSHA256 : AeMac.HMACSHA384;

            // It's good to be able to identify what choices were made when a message was
            // encrypted, so that the message can later be decrypted. This allows for
            // future versions to add support for new encryption schemes, but still be
            // able to read old data. A practice known as "cryptographic agility".
            //
            // This is similar in practice to PKCS#7 messaging, but this uses a
            // private-scoped byte rather than a public-scoped Object IDentifier (OID).
            // Please note that the scheme in this example adheres to no particular
            // standard, and is unlikely to survive to a more complete implementation in
            // the .NET Framework.
            //
            // You may be well-served by prepending a version number byte to this
            // message, but may want to avoid the value 0x30 (the leading byte value for
            // DER-encoded structures such as X.509 certificates and PKCS#7 messages).
            byte[] algorithmChoices = { (byte)aeCipher, (byte)aeMac };
            byte[] iv;
            byte[] cipherText;
            byte[] tag;

            // Using our algorithm choices, open an HMAC (as an authentication tag
            // generator) and a SymmetricAlgorithm which use different keys each derived
            // from the same master key.
            //
            // A custom implementation may very well have distinctly managed secret keys
            // for the MAC and cipher, this example merely demonstrates the master to
            // derived key methodology to encourage key separation from the MAC and
            // cipher keys.
            using (HMAC tagGenerator = GetMac(aeMac, masterKey))
            {
                using (SymmetricAlgorithm cipher = GetCipher(aeCipher, masterKey))
                using (ICryptoTransform encryptor = cipher.CreateEncryptor())
                {
                    // Since no IV was provided, a random one has been generated
                    // during the call to CreateEncryptor.
                    //
                    // But note that it only does the auto-generation once. If the cipher
                    // object were used again, a call to GenerateIV would have been
                    // required.
                    iv = cipher.IV;

                    cipherText = Transform(encryptor, message, 0, message.Length);
                }

                // The IV and ciphertest both need to be included in the MAC to prevent
                // tampering.
                //
                // By including the algorithm identifiers, we have technically moved from
                // simple Authenticated Encryption (AE) to Authenticated Encryption with
                // Additional Data (AEAD). By including the algorithm identifiers in the
                // MAC, it becomes harder for an attacker to change them as an attempt to
                // perform a downgrade attack.
                //
                // If you've added a data format version field, it can also be included
                // in the MAC to further inhibit an attacker's options for confusing the
                // data processor into believing the tampered message is valid.
                tagGenerator.AppendData(algorithmChoices);
                tagGenerator.AppendData(iv);
                tagGenerator.AppendData(cipherText);
                tag = tagGenerator.GetHashAndReset();
            }

            // Build the final result as the concatenation of everything except the keys.
            int totalLength =
                algorithmChoices.Length +
                tag.Length +
                iv.Length +
                cipherText.Length;

            byte[] output = new byte[totalLength];
            int outputOffset = 0;

            Append(algorithmChoices, output, ref outputOffset);
            Append(tag, output, ref outputOffset);
            Append(iv, output, ref outputOffset);
            Append(cipherText, output, ref outputOffset);

            Debug.Assert(outputOffset == output.Length);
            return output;
        }

        /// <summary>
        /// Reads a message produced by <see cref="Encrypt"/> after verifying it hasn't
        /// been tampered with.
        /// </summary>
        /// <param name="masterKey">The master key from which other keys derive.</param>
        /// <param name="cipherText">
        /// The output of <see cref="Encrypt"/>: a concatenation of a cipher ID, mac ID,
        /// authTag, IV, and cipherText.
        /// </param>
        /// <returns>The decrypted content.</returns>
        /// <exception cref="ArgumentNullException">
        /// <paramref name="masterKey"/> is <c>null</c>.
        /// </exception>
        /// <exception cref="ArgumentNullException">
        /// <paramref name="cipherText"/> is <c>null</c>.
        /// </exception>
        /// <exception cref="CryptographicException">
        /// <paramref name="cipherText"/> identifies unknown algorithms, is not long
        /// enough, fails a data integrity check, or fails to decrypt.
        /// </exception>
        /// <remarks>
        /// <paramref name="masterKey"/> should be a 128-bit (or larger) value
        /// generated by a secure random number generator, such as the one returned from
        /// <see cref="RandomNumberGenerator.Create()"/>. This implementation chooses to
        /// block deficient inputs by length, but doesn't make any attempt at
        /// discerning the randomness of the key.
        ///
        /// If the master key is being input by a prompt (like a password/passphrase),
        /// then it should be properly turned into keying material via a Key Derivation
        /// Function like PBKDF2, represented by Rfc2898DeriveBytes. A 'password' should
        /// never be simply turned to bytes via an Encoding class and used as a key.
        /// </remarks>
        public static byte[] Decrypt(byte[] masterKey, byte[] cipherText)
        {
            // This example continues the .NET practice of throwing exceptions for
            // failures. If you consider message tampering to be normal (and thus
            // "not exceptional") behavior, you may like the signature
            // bool Decrypt(byte[] messageKey, byte[] cipherText, out byte[] message)
            // better.
            if (masterKey == null)
                throw new ArgumentNullException(nameof(masterKey));
            if (masterKey.Length < 16)
                throw new ArgumentOutOfRangeException(
                    nameof(masterKey),
                    "Master Key must be at least 128 bits (16 bytes)");
            if (cipherText == null)
                throw new ArgumentNullException(nameof(cipherText));

            // The format of this message is assumed to be public, so there's no harm in
            // saying ahead of time that the message makes no sense.
            if (cipherText.Length < 2)
            {
                throw new CryptographicException();
            }

            // Use the message algorithm headers to determine what cipher algorithm and
            // MAC algorithm are going to be used. Since the same Key Derivation
            // Functions (KDFs) are being used in Decrypt as Encrypt, the keys are also
            // the same.
            AeCipher aeCipher = (AeCipher)cipherText[0];
            AeMac aeMac = (AeMac)cipherText[1];

            using (SymmetricAlgorithm cipher = GetCipher(aeCipher, masterKey))
            using (HMAC tagGenerator = GetMac(aeMac, masterKey))
            {
                int blockSizeInBytes = cipher.BlockSize / 8;
                int tagSizeInBytes = tagGenerator.HashSize / 8;
                int headerSizeInBytes = 2;
                int tagOffset = headerSizeInBytes;
                int ivOffset = tagOffset + tagSizeInBytes;
                int cipherTextOffset = ivOffset + blockSizeInBytes;
                int cipherTextLength = cipherText.Length - cipherTextOffset;
                int minLen = cipherTextOffset + blockSizeInBytes;

                // Again, the minimum length is still assumed to be public knowledge,
                // nothing has leaked out yet. The minimum length couldn't just be calculated
                // without reading the header.
                if (cipherText.Length < minLen)
                {
                    throw new CryptographicException();
                }

                // It's very important that the MAC be calculated and verified before
                // proceeding to decrypt the ciphertext, as this prevents any sort of
                // information leaking out to an attacker.
                //
                // Don't include the tag in the calculation, though.

                // First, everything before the tag (the cipher and MAC algorithm ids)
                tagGenerator.AppendData(cipherText, 0, tagOffset);

                // Skip the data before the tag and the tag, then read everything that
                // remains.
                tagGenerator.AppendData(
                    cipherText,
                    tagOffset + tagSizeInBytes,
                    cipherText.Length - tagSizeInBytes - tagOffset);

                byte[] generatedTag = tagGenerator.GetHashAndReset();

                // The time it took to get to this point has so far been a function only
                // of the length of the data, or of non-encrypted values (e.g. it could
                // take longer to prepare the *key* for the HMACSHA384 MAC than the
                // HMACSHA256 MAC, but the algorithm choice wasn't a secret).
                //
                // If the verification of the authentication tag aborts as soon as a
                // difference is found in the byte arrays then your program may be
                // acting as a timing oracle which helps an attacker to brute-force the
                // right answer for the MAC. So, it's very important that every possible
                // "no" (2^256-1 of them for HMACSHA256) be evaluated in as close to the
                // same amount of time as possible. For this, we call CryptographicEquals
                if (!CryptographicEquals(
                    generatedTag,
                    0,
                    cipherText,
                    tagOffset,
                    tagSizeInBytes))
                {
                    // Assuming every tampered message (of the same length) took the same
                    // amount of time to process, we can now safely say
                    // "this data makes no sense" without giving anything away.
                    throw new CryptographicException();
                }

                // Restore the IV into the symmetricAlgorithm instance.
                byte[] iv = new byte[blockSizeInBytes];
                Buffer.BlockCopy(cipherText, ivOffset, iv, 0, iv.Length);
                cipher.IV = iv;

                using (ICryptoTransform decryptor = cipher.CreateDecryptor())
                {
                    return Transform(
                        decryptor,
                        cipherText,
                        cipherTextOffset,
                        cipherTextLength);
                }
            }
        }

        private static byte[] Transform(
            ICryptoTransform transform,
            byte[] input,
            int inputOffset,
            int inputLength)
        {
            // Many of the implementations of ICryptoTransform report true for
            // CanTransformMultipleBlocks, and when the entire message is available in
            // one shot this saves on the allocation of the CryptoStream and the
            // intermediate structures it needs to properly chunk the message into blocks
            // (since the underlying stream won't always return the number of bytes
            // needed).
            if (transform.CanTransformMultipleBlocks)
            {
                return transform.TransformFinalBlock(input, inputOffset, inputLength);
            }

            // If our transform couldn't do multiple blocks at once, let CryptoStream
            // handle the chunking.
            using (MemoryStream messageStream = new MemoryStream())
            using (CryptoStream cryptoStream =
                new CryptoStream(messageStream, transform, CryptoStreamMode.Write))
            {
                cryptoStream.Write(input, inputOffset, inputLength);
                cryptoStream.FlushFinalBlock();
                return messageStream.ToArray();
            }
        }

        /// <summary>
        /// Open a properly configured <see cref="SymmetricAlgorithm"/> conforming to the
        /// scheme identified by <paramref name="aeCipher"/>.
        /// </summary>
        /// <param name="aeCipher">The cipher mode to open.</param>
        /// <param name="masterKey">The master key from which other keys derive.</param>
        /// <returns>
        /// A SymmetricAlgorithm object with the right key, cipher mode, and padding
        /// mode; or <c>null</c> on unknown algorithms.
        /// </returns>
        private static SymmetricAlgorithm GetCipher(AeCipher aeCipher, byte[] masterKey)
        {
            SymmetricAlgorithm symmetricAlgorithm;

            switch (aeCipher)
            {
                case AeCipher.Aes256CbcPkcs7:
                    symmetricAlgorithm = Aes.Create();
                    // While 256-bit, CBC, and PKCS7 are all the default values for these
                    // properties, being explicit helps comprehension more than it hurts
                    // performance.
                    symmetricAlgorithm.KeySize = 256;
                    symmetricAlgorithm.Mode = CipherMode.CBC;
                    symmetricAlgorithm.Padding = PaddingMode.PKCS7;
                    break;
                default:
                    // An algorithm we don't understand
                    throw new CryptographicException();
            }

            // Instead of using the master key directly, derive a key for our chosen
            // HMAC algorithm based upon the master key.
            //
            // Since none of the symmetric encryption algorithms currently in .NET
            // support key sizes greater than 256-bit, we can use HMACSHA256 with
            // NIST SP 800-108 5.1 (Counter Mode KDF) to derive a value that is
            // no smaller than the key size, then Array.Resize to trim it down as
            // needed.

            using (HMAC hmac = new HMACSHA256(masterKey))
            {
                // i=1, Label=ASCII(cipher)
                byte[] cipherKey = hmac.ComputeHash(
                    new byte[] { 1, 99, 105, 112, 104, 101, 114 });

                // Resize the array to the desired keysize. KeySize is in bits,
                // and Array.Resize wants the length in bytes.
                Array.Resize(ref cipherKey, symmetricAlgorithm.KeySize / 8);

                symmetricAlgorithm.Key = cipherKey;
            }

            return symmetricAlgorithm;
        }

        /// <summary>
        /// Open a properly configured <see cref="HMAC"/> conforming to the scheme
        /// identified by <paramref name="aeMac"/>.
        /// </summary>
        /// <param name="aeMac">The message authentication mode to open.</param>
        /// <param name="masterKey">The master key from which other keys derive.</param>
        /// <returns>
        /// An HMAC object with the proper key, or <c>null</c> on unknown algorithms.
        /// </returns>
        private static HMAC GetMac(AeMac aeMac, byte[] masterKey)
        {
            HMAC hmac;

            switch (aeMac)
            {
                case AeMac.HMACSHA256:
                    hmac = new HMACSHA256();
                    break;
                case AeMac.HMACSHA384:
                    hmac = new HMACSHA384();
                    break;
                default:
                    // An algorithm we don't understand
                    throw new CryptographicException();
            }

            // Instead of using the master key directly, derive a key for our chosen
            // HMAC algorithm based upon the master key.
            // Since the output size of the HMAC is the same as the ideal key size for
            // the HMAC, we can use the master key over a fixed input once to perform
            // NIST SP 800-108 5.1 (Counter Mode KDF):
            hmac.Key = masterKey;

            // i=1, Context=ASCII(MAC)
            byte[] newKey = hmac.ComputeHash(new byte[] { 1, 77, 65, 67 });

            hmac.Key = newKey;
            return hmac;
        }

        // A simple helper method to ensure that the offset (writePos) always moves
        // forward with new data.
        private static void Append(byte[] newData, byte[] combinedData, ref int writePos)
        {
            Buffer.BlockCopy(newData, 0, combinedData, writePos, newData.Length);
            writePos += newData.Length;
        }

        /// <summary>
        /// Compare the contents of two arrays in an amount of time which is only
        /// dependent on <paramref name="length"/>.
        /// </summary>
        /// <param name="a">An array to compare to <paramref name="b"/>.</param>
        /// <param name="aOffset">
        /// The starting position within <paramref name="a"/> for comparison.
        /// </param>
        /// <param name="b">An array to compare to <paramref name="a"/>.</param>
        /// <param name="bOffset">
        /// The starting position within <paramref name="b"/> for comparison.
        /// </param>
        /// <param name="length">
        /// The number of bytes to compare between <paramref name="a"/> and
        /// <paramref name="b"/>.</param>
        /// <returns>
        /// <c>true</c> if both <paramref name="a"/> and <paramref name="b"/> have
        /// sufficient length for the comparison and all of the applicable values are the
        /// same in both arrays; <c>false</c> otherwise.
        /// </returns>
        /// <remarks>
        /// An "insufficient data" <c>false</c> response can happen early, but otherwise
        /// a <c>true</c> or <c>false</c> response take the same amount of time.
        /// </remarks>
        [MethodImpl(MethodImplOptions.NoInlining | MethodImplOptions.NoOptimization)]
        private static bool CryptographicEquals(
            byte[] a,
            int aOffset,
            byte[] b,
            int bOffset,
            int length)
        {
            Debug.Assert(a != null);
            Debug.Assert(b != null);
            Debug.Assert(length >= 0);

            int result = 0;

            if (a.Length - aOffset < length || b.Length - bOffset < length)
            {
                return false;
            }

            unchecked
            {
                for (int i = 0; i < length; i++)
                {
                    // Bitwise-OR of subtraction has been found to have the most
                    // stable execution time.
                    //
                    // This cannot overflow because bytes are 1 byte in length, and
                    // result is 4 bytes.
                    // The OR propagates all set bytes, so the differences are only
                    // present in the lowest byte.
                    result = result | (a[i + aOffset] - b[i + bOffset]);
                }
            }

            return result == 0;
        }
    }
}
```
