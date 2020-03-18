---
title: Peverify.exe (PEVerify ツール)
ms.date: 03/30/2017
helpviewer_keywords:
- portable executable files, PEVerify
- verifying MSIL and metadata
- PEVerify tool
- type safety requirements
- MSIL
- PEverify.exe
- PE files, PEVerify
ms.assetid: f4f46f9e-8d08-4e66-a94b-0c69c9b0bbfa
ms.openlocfilehash: 9d5f8c80937c36e975d42d6efb0a83295cb28be9
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73104978"
---
# <a name="peverifyexe-peverify-tool"></a>Peverify.exe (PEVerify ツール)
PEVerify ツールは、Microsoft Intermediate Language (MSIL) を生成する開発者 (コンパイラの作成者、スクリプト エンジンの開発者など) が、生成する MSIL コードと関連メタデータがタイプ セーフ要件を満たしているかどうかを確認する場合に役立ちます。 一部のコンパイラでは、特定の言語構成を使用しなかった場合にだけ、検査可能でタイプ セーフなコードが生成されます。 このようなコンパイラを使用している場合、生成したコードがタイプ セーフかどうかを確認する必要があると思う開発者もいるはずです。 そのような場合は、ファイルに対して PEVerify ツールを実行し、MSIL とメタデータを検査できます。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
peverify filename [options]  
```  
  
## <a name="parameters"></a>パラメーター  
  
|引数|説明|  
|--------------|-----------------|  
|*ファイル名*|MSIL とメタデータを検査する対象のポータブル実行可能 (PE) ファイル。|  
  
|オプション|説明|  
|------------|-----------------|  
|**/break=** *maxErrorCount*|*maxErrorCount* エラーが発生した後で検査を中止します。<br /><br /> このパラメーターは、.NET Framework Version 2.0 以降ではサポートされません。|  
|**/clock**|次の検査時間をミリ秒単位で計測して報告します。<br /><br /> **MD Val. cycle**<br /> メタデータ検証サイクル<br /><br /> **MD Val. pure**<br /> メタデータ検証のみ<br /><br /> **IL Ver. cycle**<br /> Microsoft Intermediate Language (MSIL) 検証サイクル<br /><br /> **IL Ver pure**<br /> MSIL 検証のみ<br /><br /> **MD Val. cycle** 時間および **IL Ver. cycle** 時間には、必要なスタートアップ手順およびシャットダウン手順を実行するために必要な時間が含まれます。 **MD Val. pure** 時間および **IL Ver pure** 時間は、検査または検証だけを行うために要する時間を反映します。|  
|**/help**|このツールのコマンド構文とオプションを表示します。|  
|**/hresult**|エラーコードを 16 進形式で表示します。|  
|**/ignore=** *hex.code* [, *hex.code*]|指定したエラー コードを無視します。|  
|**/ignore=@** *responseFile*|指定した応答ファイル内に一覧表示されているエラー コードを無視します。|  
|**/il**|*filename* で指定したアセンブリに実装されているメソッドに対して、MSIL がタイプ セーフかどうかを検査します。 **/quiet** オプションを指定した場合を除き、検出された問題ごとに詳細な説明が返されます。|  
|**/md**|*filename* で指定したアセンブリに対して、メタデータを検証します。 この場合、ファイル内のすべてのメタデータ構造が検証され、検出された問題がすべて報告されます。|  
|**/nologo**|製品バージョンと著作権情報を表示しません。|  
|**/nosymbols**|.NET Framework Version 2.0 で、後方互換性のために行番号を表示しません。|  
|**/quiet**|クワイエット モードを指定します。このモードでは、検査により検出された問題の報告が簡略化されます。 ファイルがタイプ セーフかどうかは報告されますが、タイプ セーフでない場合に、その問題に関する情報は報告されません。|  
|`/transparent`|透過的なメソッドのみを検証します。|  
|**/unique**|繰り返し発生するエラー コードを無視します。|  
|**/verbose**|.NET Framework Version 2.0 で、MSIL 検証メッセージに追加情報を表示します。|  
|**/?**|このツールのコマンド構文とオプションを表示します。|  
  
## <a name="remarks"></a>解説  
 共通言語ランタイムは、セキュリティ機構や分離機構の実施を簡単にするために、アプリケーション コードがタイプ セーフに実行されることに依存しています。 通常、[検査可能でタイプ セーフ](../../standard/security/key-security-concepts.md#type-safety-and-security)なコード以外のコードは実行できません。しかし、信頼できるが検査を実行できないコードを実行可能にするセキュリティ ポリシーを設定することはできます。  
  
 **/md** と **/il** のいずれのオプションも指定しない場合は、両方の種類の検査が実行されます。 まず、 **/md** オプションによる検査が実行されます。 エラーが検出されない場合は、 **/il** オプションによる検査が実行されます。 **/md** と **/il** の両方のオプションを指定した場合は、メタデータの検査でエラーが検出された場合でも、 **/il** オプションによる検査が実行されます。 つまり、メタデータの検査でエラーが検出されないときは、**peverify** *filename* と **peverify** *filename* **/md** **/il** のいずれを指定しても同じ検査が実行されます。  
  
 Peverify.exe は、データ フローの分析と、メタデータの有効性に関する多数の規則のリストに基づいて、MSIL に対する包括的な検査を実行します。 Peverify.exe によって実行される検査の詳細については、Windows SDK の Tools Developers Guide フォルダー内にある "Metadata Validation Specification" (メタデータ検証仕様) と "MSIL Instruction Set Specification" (MSIL 命令セット仕様) を参照してください。  
  
 .NET Framework Version 2.0 以降では、`byref`、`dup`、`ldsflda`、`ldflda`、`ldelema`、および `call` の各 MSIL 命令を使用して指定する検証可能な `unbox` 戻り値をサポートします。  
  
## <a name="examples"></a>使用例  
 アセンブリ `myAssembly.exe` に実装されているメソッドに対して、メタデータの有効性および MSIL がタイプ セーフかどうかについて検査するコマンドを次に示します。  
  
```console  
peverify myAssembly.exe /md /il  
```  
  
 上記の要求が正常に終了した場合は、次のメッセージが表示されます。  
  
```output
All classes and methods in myAssembly.exe Verified  
```  
  
 アセンブリ `myAssembly.exe` に実装されているメソッドに対して、メタデータの有効性および MSIL がタイプ セーフかどうかについて検査するコマンドを次に示します。 このツールは、これらの検査に要する時間を表示します。  
  
```console  
peverify myAssembly.exe /md /il /clock  
```  
  
 上記の要求が正常に終了した場合は、次のメッセージが表示されます。  
  
```output
All classes and methods in myAssembly.exe Verified  
Timing: Total run     320 msec  
        MD Val.cycle  40 msec  
        MD Val.pure   10 msec  
        IL Ver.cycle  270 msec  
        IL Ver.pure   230 msec  
```  
  
 アセンブリ `myAssembly.exe` に実装されているメソッドに対して、メタデータの有効性および MSIL がタイプ セーフかどうかについて検査するコマンドを次に示します。 しかし、Peverify.exe は、最大エラー カウントである 100 に達すると、停止します。 このツールは、指定されたエラー コードも無視します。  
  
```console  
peverify myAssembly.exe /break=100 /ignore=0x12345678,0xABCD1234  
```  
  
 上記の例と結果は同じですが、無視するエラー コードを応答ファイル `ignoreErrors.rsp` に指定するコマンドを次に示します。  
  
```console  
peverify myAssembly.exe /break=100 /ignore@ignoreErrors.rsp  
```  
  
 この応答ファイルには、エラー コードの一覧をコンマで区切って指定できます。  
  
```text
0x12345678, 0xABCD1234  
```  
  
 また、エラー コードを 1 行に 1 つという形式で指定することもできます。  
  
```text
0x12345678  
0xABCD1234  
```  
  
## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [検証可能なタイプ セーフ コードの作成](../misc/code-access-security-basics.md#typesafe_code)
- [タイプ セーフとセキュリティ](../../standard/security/key-security-concepts.md#type-safety-and-security)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
