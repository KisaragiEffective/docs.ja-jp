---
title: Sn.exe (厳密名ツール)
ms.date: 03/30/2017
helpviewer_keywords:
- public keys, signing files
- Strong Name tool
- Sn.exe
- assemblies [.NET Framework], signing
- signing files
- strong-named assemblies, signing files
- key pairs for signing files
ms.assetid: c1d2b532-1b8e-4c7a-8ac5-53b801135ec6
ms.openlocfilehash: b5eee15a08dcae42263e06939c197ec0848816a0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79180314"
---
# <a name="snexe-strong-name-tool"></a>Sn.exe (厳密名ツール)
厳密名ツール (Sn.exe) は、[厳密な名前](../../standard/assembly/strong-named.md)を使用してアセンブリに署名する場合に役立ちます。 Sn.exe には、キーの管理、署名の生成、署名の検査に関する各オプションが用意されています。  
  
> [!WARNING]
> セキュリティに関しては、厳格な名前に依存しないでください。 厳格な名前は、一意の ID を提供するだけです。

 厳密な名前付けと厳密な名前付きアセンブリについて詳しくは、「[厳密な名前付きアセンブリ](../../standard/assembly/strong-named.md)」 と「[方法: 厳密な名前でアセンブリに署名する](../../standard/assembly/sign-strong-name.md)」 を参照してください。  
  
 厳密名ツールは Visual Studio と共に自動的にインストールされます。 このツールを開始するには、開発者コマンド プロンプト (または、Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  

> [!NOTE]
> 64 ビット コンピューターでは、Visual Studio 用開発者コマンド プロンプトを使用して 32 ビット バージョンの Sn.exe を、Visual Studio x64 Win64 コマンド プロンプトを使用して 64 ビット バージョンを実行してください。
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
sn [-quiet][option [parameter(s)]]  
```  
  
## <a name="parameters"></a>パラメーター  
  
|オプション|説明|  
|------------|-----------------|  
|`-a identityKeyPairFile signaturePublicKeyFile`|<xref:System.Reflection.AssemblySignatureKeyAttribute> データを生成して、ID キーをファイルからの署名キーに移行します。|  
|`-ac identityPublicKeyFile identityKeyPairContainer signaturePublicKeyFile`|<xref:System.Reflection.AssemblySignatureKeyAttribute> データを生成して、ID キーをキー コンテナーからの署名キーに移行します。|  
|`-c [csp]`|厳密な名前による署名に使用する既定の暗号サービス プロバイダー (CSP: Cryptographic Service Provider) を設定します。 この設定は、コンピューター全体に適用されます。 CSP 名を指定しない場合は、現在の設定がクリアされます。|  
|`-d container`|指定したキー コンテナーを厳密な名前 CSP から削除します。|  
|`-D assembly1 assembly2`|2 つのアセンブリの違いが、署名だけかどうかを検査します。 通常は、このオプションは、別のキー ペアを使用してアセンブリに再署名した後のチェック用として使用されます。|  
|`-e assembly outfile`|*assembly* から公開キーを抽出し、*outfile* に格納します。|  
|`-h`|このツールのコマンド構文とオプションを表示します。|  
|`-i infile container`|指定したキー コンテナーに、*infile* に含まれるキー ペアをインストールします。 キー コンテナーは厳密な名前 CSP 内に常駐します。|  
|`-k [keysize] outfile`|指定したサイズの新しい <xref:System.Security.Cryptography.RSACryptoServiceProvider> キーを生成し、指定したファイルに書き込みます。  公開キーと秘密キーの両方がファイルに書き込みまれます。<br /><br /> キーのサイズを指定しない場合、Microsoft Enhanced Cryptographic Provider をインストールしている場合は既定で 1,024 ビットのキーが生成され、インストールしていない場合は既定で 512 ビットのキーが生成されます。<br /><br /> Microsoft Enhanced Cryptographic Provider をインストールしている場合、*keysize* パラメーターは、8 ビット間隔で 384 ビットから 16,384 ビットまでの長さのキーをサポートします。  Microsoft Enhanced Cryptographic Provider をインストールしていない場合は、8 ビット間隔で 384 ビットから 512 ビットまでの長さのキーをサポートします。|  
|`-m [y|n]`|キー コンテナーがコンピューターに固有であるか、ユーザーに固有であるかを指定します。 *y* を指定した場合、キー コンテナーはコンピューターに固有です。 *n* を指定した場合、キー コンテナーはユーザーに固有です。<br /><br /> y も n も指定しない場合は、現在の設定が表示されます。|  
|`-o infile [outfile]`|*infile* から公開キーを抽出し、.csv ファイルに格納します。 公開キーの各バイトはコンマで区切られます。 この形式は、ソース コード内で、キーへの参照を初期化済みの配列としてハードコーディングする場合に便利です。 *outfile* を指定しない場合、出力はクリップボードに保存されます。 **注:** このオプションでは、入力が公開キーのみであるかどうかの検証は行われません。 `infile` に秘密キーとのキー ペアが格納されていた場合、秘密キーも抽出されます。|  
|`-p infile outfile [hashalg]`|*infile* 内のキー ペアから公開キーを抽出し、その公開キーを *outfile* に格納します。オプションで *hashalg* で指定された RSA アルゴリズムを使用します。 この公開キーは、[アセンブリ リンカー (Al.exe)](al-exe-assembly-linker.md) の **/delaysign+** オプションと **/keyfile** オプションを使用して、アセンブリへの署名を遅らせるときに使用できます。 アセンブリの署名を遅らせた場合、コンパイル時には公開キーだけが設定され、後で秘密キーが判明したときに追加される署名用にファイル内の領域が予約されます。|  
|`-pc container outfile [hashalg]`|*container* 内のキー ペアから公開キーを抽出し、*outfile* に格納します。 *hashalg* オプションを使用する場合、RSA アルゴリズムにより公開キーが抽出されます。|  
|`-Pb [y|n]`|厳密な名前を省略するポリシーが強制されるかどうかを指定します。 *y* を指定すると、完全に信頼されている <xref:System.AppDomain> に完全信頼アセンブリが読み込まれるとき、アセンブリの厳密な名前の検証は行われません。 *n* を指定した場合は、厳密な名前が正しいかどうかのみ検証されますが、特定の厳密な名前については確認されません。 <xref:System.Security.Permissions.StrongNameIdentityPermission> は、完全に信頼されているアセンブリには効果がありません。 厳密な名前の一致については、独自のチェックを実行する必要があります。<br /><br /> `y` も `n` も指定しない場合は、現在の設定が表示されます。 既定値は、`y` です。 **注:** 64 ビット コンピューターでは、Sn.exe の 32 ビットのインスタンスと 64 ビットのインスタンスの両方にこのパラメーターを設定する必要があります。|  
|`-q[uiet]`|クワイエット モードを指定します。このモードでは、成功メッセージは表示されません。|  
|`-R[a] assembly infile`|以前に署名したアセンブリ、または署名を遅らせたアセンブリに、*infile* 内のキー ペアを使用して再署名します。<br /><br /> **-Ra** を使用すると、アセンブリ内のすべてのファイルについてハッシュが再計算されます。|  
|`-Rc[a] assembly container`|以前に署名したアセンブリ、または署名を遅らせたアセンブリに、*container* 内のキー ペアを使用して再署名します。<br /><br /> **-Rca** を使用すると、アセンブリ内のすべてのファイルについてハッシュが再計算されます。|  
|`-Rh assembly`|アセンブリ内のすべてのファイルについてハッシュを再計算します。|  
|`-t[p] infile`|*infile* に格納されている公開キーに関するトークンを表示します。 *infile* には、以前に **-p** を使用してキー ペア ファイルから生成された公開キーが含まれている必要があります。  **-t[p]** オプションを使用して、トークンをキー ペア ファイルから直接抽出しないでください。<br /><br /> トークンは、ハッシュ関数によって公開キーから算出されます。 領域を節約するために、共通言語ランタイムは厳密な名前を持つアセンブリへの依存度を記録するときに、別のアセンブリへ参照の一部として公開キー トークンをマニフェスト内に格納します。 **-tp** オプションを指定すると、トークンの他に公開キーも表示されます。 <xref:System.Reflection.AssemblySignatureKeyAttribute> 属性がアセンブリに適用されている場合、トークンは ID キー用であり、ハッシュ アルゴリズムの名前と ID キーが表示されます。<br /><br /> このオプションはアセンブリ署名を検証しないため、信頼の決定には使用しないでください。  このオプションは、生の公開キー トークン データのみを表示します。|  
|`-T[p] assembly`|*assembly* に関する公開キー トークンを表示します。 *assembly* には、アセンブリ マニフェストを含むファイルの名前を指定する必要があります。<br /><br /> トークンは、ハッシュ関数によって公開キーから算出されます。 領域を節約するために、共通言語ランタイムは厳密な名前を持つアセンブリへの依存度を記録するときに、別のアセンブリへ参照の一部として公開キー トークンをマニフェスト内に格納します。 **-Tp** オプションを指定すると、トークンの他に公開キーも表示されます。 <xref:System.Reflection.AssemblySignatureKeyAttribute> 属性がアセンブリに適用されている場合、トークンは ID キー用であり、ハッシュ アルゴリズムの名前と ID キーが表示されます。<br /><br /> このオプションはアセンブリ署名を検証しないため、信頼の決定には使用しないでください。  このオプションは、生の公開キー トークン データのみを表示します。|  
|`-TS assembly infile`|*入力ファイル*内のキー ペアを使い、完全または不完全に署名された*アセンブリ*をテスト署名します。|  
|`-TSc assembly container`|キー コンテナー *コンテナー* 内のキー ペアを使い、完全または不完全に署名された*アセンブリ*をテスト署名します。|
|`-v assembly`|*assembly* 内の厳密な名前を検査します。ここで、*assembly* はアセンブリ マニフェストを含むファイルの名前です。|  
|`-vf assembly`|*assembly* 内の厳密な名前を検査します。 **-v** オプションとは異なり、 **-vf** では **-Vr** オプションで無効化した場合であっても、検査を強制的に実行します。|  
|`-Vk regfile.reg assembly [userlist] [infile]`|指定したアセンブリを登録して検証をスキップするために使用できる登録エントリ (.reg) ファイルを作成します。 **-Vr** オプションに適用されるアセンブリ名前付け規則が **-Vk** オプションにも適用されます。 *userlist* オプションと *infile* オプションについては、 **-Vr** オプションを参照してください。|  
|`-Vl`|コンピューター上の厳密な名前検査に関する、現在の設定を一覧表示します。|  
|`-Vr  assembly [userlist] [infile]`|検査をスキップする *assembly* を登録します。 必要に応じて、検証の省略を適用する必要があるコンマで区切られたユーザー名のリストを指定できます。 *infile* を指定した場合、検査は実行されますが、検査時には *infile* 内の公開キーが使用されます。 指定した厳密な名前を持つアセンブリをすべて登録するには、 *\*, strongname* という形式で *assembly* を指定します。 *strongname* は、トークン化した形式の公開キーを表す 16 進形式の文字列として指定します。 公開キー トークンの表示については、 **-t** オプションと **-T** オプションを参照してください。 **注意:** このオプションは開発時だけ使用します。 検証省略リストにアセンブリを追加すると、セキュリティ上の脆弱性が生じます。 悪意のあるアセンブリは、検証省略リストに追加されたアセンブリの完全限定アセンブリ名 (アセンブリ名、バージョン、カルチャ、および公開キー トークン) を使用することによって、その ID を偽装できます。 これによって、悪意のあるアセンブリの検証も省略できます。|  
|||  
|`-Vu  assembly`|検査をスキップする *assembly* の登録を解除します。 **-Vr** オプションに適用される規則と同じアセンブリ名前付け規則が **-Vu** オプションにも適用されます。|  
|`-Vx`|検査をスキップするエントリをすべて削除します。|  
|`-?`|このツールのコマンド構文とオプションを表示します。|  
  
> [!NOTE]
> Sn.exe の全オプションでは大文字と小文字が区別されます。また、オプションが正しく認識されるためには、表記されたとおりに正確に入力する必要があります。  
  
## <a name="remarks"></a>Remarks  
 **-R** オプションと **-Rc** オプションは、署名を遅らせたアセンブリを処理する場合に便利です。 その場合、コンパイル時には公開キーだけが設定され、後で秘密キーが判明したときに署名が実行されます。  
  
> [!NOTE]
> レジストリなどの保護されたリソースに書き込むパラメーター (例: -**Vr)** の場合は、管理者として SN.exe を実行してください。  
  
厳密な名前ツールでは、公開キーと秘密キーの組が `AT_SIGNATURE` アルゴリズム識別子を使用して生成されると想定されます。 `AT_KEYEXCHANGE` アルゴリズムを使用して生成された公開キー/秘密キーのペアでは、エラーが生成されます。

## <a name="examples"></a>使用例  
 新しいランダム キー ペアを作成し、`keyPair.snk` に格納するコマンドを次に示します。  
  
```console  
sn -k keyPair.snk  
```  
  
 厳密な名前 CSP 内のコンテナー `keyPair.snk` 内の `MyContainer` にキーを格納するコマンドを次に示します。  
  
```console  
sn -i keyPair.snk MyContainer  
```  
  
 `keyPair.snk` から公開キーを抽出し、`publicKey.snk` に格納するコマンドを次に示します。  
  
```console  
sn -p keyPair.snk publicKey.snk  
```  
  
 公開キーおよび `publicKey.snk` に含まれている公開キーのトークンを表示するコマンドを次に示します。  
  
```console  
sn -tp publicKey.snk  
```  
  
 アセンブリ `MyAsm.dll` を検査するコマンドを次に示します。  
  
```console  
sn -v MyAsm.dll  
```  
  
 既定の CSP から `MyContainer` を削除するコマンドを次に示します。  
  
```console  
sn -d MyContainer  
```  
  
## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [Al.exe (アセンブリ リンカー)](al-exe-assembly-linker.md)
- [厳密な名前付きアセンブリ](../../standard/assembly/strong-named.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
