---
title: Certmgr.exe (証明書マネージャー ツール)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- certificates, managing
- CRLs
- certificate trust lists
- Certmgr.exe
- Certificate Manager tool
- CTLs
- certificate revocation lists
ms.assetid: 7e953b43-1374-4bbc-814f-53ca1b6b52bb
ms.openlocfilehash: 06fe3a78d0b19720d4f83111980b88806312205f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73129872"
---
# <a name="certmgrexe-certificate-manager-tool"></a>Certmgr.exe (証明書マネージャー ツール)
証明書マネージャー ツール (Certmgr.exe) は、証明書、証明書信頼リスト (CTL: Certificate Trust List)、および証明書失効リスト (CRL: Certificate Revocation List) を管理します。  
  
 証明書マネージャーは Visual Studio と共に自動的にインストールされます。 ツールを開始するには、[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)を使用します。  
  
> [!NOTE]
> 証明書マネージャー ツール (Certmgr.exe) はコマンド ライン ユーティリティですが、証明書 (Certmgr.msc) は Microsoft 管理コンソール (MMC: Microsoft Management Console) スナップインです。 通常、Certmgr.msc は Windows のシステム ディレクトリにあるので、コマンド ラインで「`certmgr`」と入力すると、Visual Studio 用開発者コマンド プロンプトを開いていた場合でも証明書 MMC スナップインが読み込まれることがあります。 これは、PATH 環境変数で、スナップインのパスが証明書マネージャー ツールのパスよりも前に指定されているためです。 この問題が発生した場合は、実行可能ファイルのパスを指定して Certmgr.exe コマンドを実行できます。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
 X.509 証明書の概要については、「[証明書の使用](../wcf/feature-details/working-with-certificates.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
      certmgr [/add | /del | /put] [options]  
[/s[/r registryLocation]] [sourceStorename]  
[/s[/r registryLocation]] [destinationStorename]  
```  
  
## <a name="parameters"></a>パラメーター  
  
|引数|説明|  
|--------------|-----------------|  
|*sourceStorename*|追加、削除、保存、または表示する既存の証明書、CTL、または CRL を含む証明書ストア。 ストア ファイルまたはシステム ストアを指定できます。|  
|*destinationStorename*|出力証明書ストアまたはファイル。|  
  
|オプション|説明|  
|------------|-----------------|  
|**/add**|証明書、CTL、および CRL を証明書ストアに追加します。|  
|**/all**|**/add** を指定して使用した場合は、すべてのエントリを追加します。 **/del** を指定して使用した場合は、すべてのエントリを削除します。 **/add**、 **/del** のいずれのオプションも指定せずに使用した場合は、すべてのエントリを表示します。 **/all** オプションは、 **/put** オプションと共に指定できません。|  
|**/c**|**/add** を指定して使用した場合は、証明書を追加します。 **/del** を指定して使用した場合は、証明書を削除します。 **/put** を指定して使用した場合は、証明書を保存します。 **/add**、 **/del**、 **/put** のいずれのオプションも指定せずに使用した場合は、証明書を表示します。|  
|**/CRL**|**/add** を指定して使用した場合は、CRL を追加します。 **/del** を指定して使用した場合は、CRL を削除します。 **/put** を指定して使用した場合は、CRL を保存します。 **/add**、 **/del**、 **/put** のいずれのオプションも指定せずに使用した場合は、CRL を表示します。|  
|**/CTL**|**/add** を指定して使用した場合は、CTL を追加します。 **/del** を指定して使用した場合は、CTL を削除します。 **/put** を指定して使用した場合は、CTL を保存します。 **/add**、 **/del**、 **/put** のいずれのオプションも指定せずに使用した場合は、CTL を表示します。|  
|**/del**|証明書、CTL、および CRL を証明書ストアから削除します。|  
|**/e** *encodingType*|証明書のエンコード タイプを指定します。 既定値は、`X509_ASN_ENCODING` です。|  
|**/f** *dwFlags*|ストア オープン フラグを指定します。 これは **CertOpenStore** に渡される *dwFlags* パラメーターです。 既定値は CERT_SYSTEM_STORE_CURRENT_USER です。 このオプションは **/y** オプションを使用した場合にだけ有効です。|  
|**/h** **[elp]**|このツールのコマンド構文とオプションを表示します。|  
|**/n** *nam*|追加、削除、または保存する証明書の共通名を指定します。 このオプションは証明書についてだけ使用できます。CTL または CRL については使用できません。|  
|**/put**|証明書ストアに含まれている X.509 証明書、CTL、または CRL をファイルに保存します。 ファイルは X.509 形式で保存されます。 **/7** オプションを **/put** オプションと一緒に使用すると、ファイルを PKCS #7 形式で保存できます。 **/put** オプションの後ろには、 **/c**、 **/CTL**、 **/CRL** のいずれかのオプションを指定する必要があります。 **/all** オプションは、 **/put** オプションと共に指定できません。|  
|**/r** *location*|システム ストアのレジストリの位置を指定します。 このオプションは、 **/s** オプションを指定した場合にだけ有効です。 *location* には、次のいずれかを指定する必要があります。<br /><br /> -   証明書ストアが HKEY_CURRENT_USER キーに含まれる場合は `currentUser` を指定します。 既定値です。<br />-   証明書ストアが HKEY_LOCAL_MACHINE キーに含まれる場合は `localMachine` を指定します。|  
|**/s**|証明書ストアがシステム ストアであることを指定します。 このオプションを指定しない場合、ストアは **StoreFile** と見なされます。|  
|**/sha1** *sha1Hash*|追加、削除、または保存する証明書、CTL、または CRL の SHA1 ハッシュを指定します。|  
|**/v**|詳細出力モードを指定します。このモードでは、証明書、CTL、および CRL に関する詳細情報が表示されます。 このオプションは、 **/add**、 **/del**、または **/put** のオプションと共に指定できません。|  
|**/y** *provider*|ストア プロバイダー名を指定します。|  
|**/7**|出力証明書ストアを PKCS #7 オブジェクトとして保存します。|  
|**/?**|このツールのコマンド構文とオプションを表示します。|  
  
## <a name="remarks"></a>解説  
 Certmgr.exe は次の基本的な機能を実行します。  
  
- 証明書、CTL、および CRL をコンソールに表示します。  
  
- 証明書、CTL、および CRL を証明書ストアに追加します。  
  
- 証明書、CTL、および CRL を証明書ストアから削除します。  
  
- 証明書ストアに含まれている X.509 証明書、CTL、または CRL をファイルに保存します。  
  
 Certmgr.exe は、**StoreFile** とシステム ストアという 2 つの種類の証明書ストアに対して機能します。 証明書ストアの種類を指定する必要はありません。Certmgr.exe がストアの種類を識別し、適切な操作を実行します。  
  
 オプションを何も指定せずに Certmgr.exe を実行すると、証明書の管理タスクの実行時に役立つ GUI を備えた certmgr.msc スナップインが起動します。これらのタスクはコマンド ラインでも実行できます。 この GUI には、証明書、CTL、および CRL をディスクから証明書ストアへとコピーする、インポート ウィザードが用意されています。  
  
 次のコードをコンパイルおよび実行することによって、`sourceStorename` パラメーターおよび `destinationStorename` パラメーターについて X509Certificate ストアの名前を検索できます。  
  
 [!code-csharp[Tools.CertMgr#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tools.certmgr/cs/storenames1.cs#1)]
 [!code-vb[Tools.CertMgr#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tools.certmgr/vb/storenames1.vb#1)]  
  
 証明書の詳細については、「[証明書の使用](../wcf/feature-details/working-with-certificates.md)」を参照してください。  
  
## <a name="examples"></a>使用例  
 既定のシステム ストアである `my` を詳細出力モードで表示するコマンドを次に示します。  
  
```console  
certmgr /v /s my  
```  
  
 `myFile.ext` という名前のファイルに含まれるすべての証明書を `newFile.ext` という名前の新しいファイルに追加するコマンドを次に示します。  
  
```console  
certmgr /add /all /c myFile.ext newFile.ext  
```  
  
 `testcert.cer` という名前のファイルに含まれる証明書をシステム ストア `my` に追加するコマンドを次に示します。  
  
```console  
certmgr /add /c testcert.cer /s my  
```  
  
 `TrustedCert.cer` という名前のファイルに含まれる証明書をルート証明書ストアに追加するコマンドを次に示します。  
  
```console  
certmgr /c /add TrustedCert.cer /s root  
```  
  
 システム ストア `myCert` に含まれていて共通名が `my` の証明書を `newCert.cer` という名前のファイルに保存するコマンドを次に示します。  
  
```console  
certmgr /add /c /n myCert /s my newCert.cer  
```  
  
 システム ストア `my` に含まれるすべての CTL を削除し、その結果得られるストアを `newStore.str` という名前のファイルに格納するコマンドを次に示します。  
  
```console  
certmgr /del /all /ctl /s my newStore.str  
```  
  
 システム ストア `my` に含まれる証明書をファイル `newFile` に保存するコマンドを次に示します。 `my` から `newFile` に保存する証明書の番号を入力するためのプロンプトが表示されます。  
  
```console  
certmgr /put /c /s my newFile  
```  
  
## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [Makecert.exe (証明書作成ツール)](/windows/desktop/SecCrypto/makecert)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
