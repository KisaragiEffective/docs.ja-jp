---
ms.openlocfilehash: 0036fc2b03dde64d24d883e55b9f00c931f0c218
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "70997652"
---
### <a name="net-interop-will-now-queryinterface-for-iagileobject-a-winrt-interface"></a>.NET 相互運用で IAgileObject に対して QueryInterface が呼び出されるようになる (WinRT インターフェイス)

|   |   |
|---|---|
|説明|.NET デリゲートで WinRT イベントを使用する場合、.NET Framework 4.8 以降では、Windows で IAgileObject に対して QI が呼び出されます。  .NET Framework の以前のバージョンでは、ランタイムでその QI が失敗し、イベントをサブスクライブできませんでした。<ul><li>[ x ] 後方互換</li></ul>|
|提案される解決策|IAgileObject に対して QI を有効にすると実行が中断される場合は、次の構成を設定し、このコードを無効にすることができます。 <h4>方法 1:環境変数</h4> 環境変数を COMPLUS_DisableCCWSupportIAgileObject=1 に設定します。この方法は、この環境変数を継承するすべての環境に影響します。 この場合、コンソール セッションが 1 つのみになる可能性があります。あるいは、環境変数をグローバルに設定した場合、コンピューター全体に影響する可能性があります。 環境変数の名前では大文字と小文字は区別されません。 <h4>方法 2:レジストリ</h4> レジストリ エディター (regedit.exe) を使用して、サブキーの HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft.NETFramework または HKEY_CURRENT_USER\SOFTWARE\Microsoft.NETFramework を見つけます。その後、次のように追加します。値の名前:DisableCCWSupportIAgileObject 種類:DWORD (32 ビット) 値 (REG_WORD ともいう) 値:1。Windows REG.EXE ツールを使用して、コマンド ラインまたはスクリプト環境から、この値を追加することができます。 次に例を示します。<pre><code class="lang-console">reg add HKLM\SOFTWARE\Microsoft\.NETFramework /v DisableCCWSupportIAgileObject /t REG_DWORD /d 1&#13;&#10;</code></pre>ここでは、<code>HKLM</code> が <code>HKEY_LOCAL_MACHINE</code> の代わりに使用されています。 この構文のヘルプを表示するには、<code>reg add /?</code> を使用します。 レジストリ値の名前では大文字と小文字は区別されません。|
|スコープ|エッジ|
|バージョン|4.8|
|種類|ランタイム|
