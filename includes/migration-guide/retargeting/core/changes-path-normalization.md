---
ms.openlocfilehash: 994876457f9745de99be30e567f400f69a0192fa
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "70259799"
---
### <a name="changes-in-path-normalization"></a>パスの正規化の変更

|   |   |
|---|---|
|説明|.NET Framework 4.6.2 を対象とするアプリより、ランタイムによってパスを正規化する方法が変わりました。パスの正規化では、パスまたはファイルを識別する文字列を変更し、対象のオペレーティング システムの有効なパスに準拠するようにします。 通常、正規化では次のことを行います。<ul><li>コンポーネントとディレクトリの区切り記号を正規化する。</li><li>現在のディレクトリを相対パスに適用する。</li><li>パスの相対ディレクトリ (.) または親ディレクトリ (..) を評価する。</li><li>指定した文字をトリミングする。</li></ul>.NET Framework 4.6.2 を対象とするアプリより、パス正規化の次の変更が既定で有効になっています。<ul><li>ランタイムはオペレーティング システムの [GetFullPathName](https://docs.microsoft.com/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamew) 関数に従って、パスを正規化します。</li><li>正規化では、ディレクトリ セグメントの末尾 (ディレクトリ名の末尾のスペースなど) がトリミングされなくなりました。</li><li>`\\.\` や `\\?\` (mscorlib.dll のファイル I/O API の場合) を含む、完全に信頼できるデバイス パス構文がサポートされます。</li><li>ランタイムではデバイス構文パスは検証されません。</li><li>代替データ ストリームにアクセスするためのデバイス構文の使用はサポートされています。</li></ul>これらの変更でパフォーマンスが向上すると同時に、以前はアクセス不可だったパスにメソッドでアクセスできるようになります。 .NET Framework 4.6.1 以前のバージョンを対象とするアプリが .NET Framework 4.6.2 以降で実行される場合、この変更の影響は受けません。|
|提案される解決策|.NET Framework 4.6.2 以降を対象とするアプリの場合、アプリケーション構成ファイルの <code>&lt;runtime&gt;</code> セクションに次の行を追加することでこの変更を無効にし、従来の正規化を使用できます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.IO.UseLegacyPathHandling=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>.NET Framework 4.6.1 以前を対象とするが、.NET Framework 4.6.2 以降で実行されるアプリの場合、アプリケーション構成ファイルの <code>&lt;runtime&gt;</code> セクションに次の行を追加することで、パス正規化の変更を有効にできます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.IO.UseLegacyPathHandling=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.6.2|
|[種類]|再ターゲット中|
