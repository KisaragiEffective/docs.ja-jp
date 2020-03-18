---
title: FindPrivateKey サンプル
ms.date: 12/04/2017
helpviewer_keywords:
- FindPrivateKey
ms.assetid: 16b54116-0ceb-4413-af0c-753bb2a785a6
ms.openlocfilehash: 0ed1e5e81a5d2f7f3586e5dce306e8244b5ebd48
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75346013"
---
# <a name="findprivatekey-sample"></a>FindPrivateKey サンプル

証明書ストア内の特定の X.509 証明書に関連付けられている秘密キー ファイルの場所と名前を見つけることが困難な場合があります。 FindPrivateKey.exe ツールを使用すると、この処理を容易に実行できます。

> [!IMPORTANT]
> FindPrivateKey は、使用する前にコンパイルする必要があるサンプルです。 FindPrivateKey ツールのビルド方法については、「 [FindPrivateKey プロジェクトをビルドするに](#to-build-the-findprivatekey-project)は」セクションを参照してください。

X.509 証明書は、コンピューターの管理者または任意のユーザーによってインストールされます。 ただし、別のアカウントで実行されているサービスが証明書にアクセスする可能性があります。 たとえば、NETWORK SERVICE アカウントなどです。

別のアカウントでは、秘密キー ファイルへアクセスできない場合があります。これは、証明書が最初にこのアカウントによってインストールされていないからです。 FindPrivateKey ツールでは、指定された X.509 証明書の秘密キー ファイルの場所を検索できます。 特定の X.509 証明書の秘密キー ファイルの場所がわかれば、このファイルに対するアクセス許可の追加または削除を実行できます。

セキュリティのために証明書を使用するサンプルでは、*セットアップの .bat*ファイルで FindPrivateKey ツールを使用します。 秘密キーファイルが見つかったら、 *cacls.exe*などの他のツールを使用して、ファイルに対する適切なアクセス権を設定できます。

自己ホスト型実行可能ファイルなどのユーザーアカウントで Windows Communication Foundation (WCF) サービスを実行する場合は、そのユーザーアカウントにファイルへの読み取り専用アクセス権があることを確認します。 インターネットインフォメーションサービス (IIS) で WCF サービスを実行する場合、サービスを実行する既定のアカウントは、IIS 7 以前のバージョンのネットワークサービス、または IIS 7.5 以降のバージョンのアプリケーションプール Id です。 詳細については、「[アプリケーションプール id](/iis/manage/configuring-security/application-pool-identities)」を参照してください。

## <a name="examples"></a>使用例

プロセスに読み取り権限がない証明書にアクセスすると、次の例のような例外メッセージが表示されます。

```output
System.ArgumentException was unhandled
Message="The certificate 'CN=localhost' must have a private key that is capable of key exchange.  The process must have access rights for the private key."
Source="System.ServiceModel"
```

この場合は、FindPrivateKey ツールを使用して秘密キーファイルを検索し、サービスが実行されているプロセスのアクセス権を設定します。 たとえば、次の例に示すように、Cacls.exe ツールを使用してこれを行うことができます。

```console
cacls.exe "C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys\8aeda5eb81555f14f8f9960745b5a40d_38f7de48-5ee9-452d-8a5a-92789d7110b1" /E /G "NETWORK SERVICE":R
```

#### <a name="to-build-the-findprivatekey-project"></a>FindPrivateKey プロジェクトをビルドするには

プロジェクトをダウンロードするには、 [Windows Communication Foundation (WCF) および Windows Workflow Foundation (WF) のサンプル .NET Framework 4 を](https://www.microsoft.com/download/details.aspx?id=21459)参照してください。

1. ファイルエクスプローラーを開き、サンプルをインストールしたディレクトリの場所にある*WF_WCF_Samples \wcf\setup\findprivatekey\cs*フォルダーに移動します。

2. .sln ファイルのアイコンをダブルクリックして、このファイルを Visual Studio で開きます。

3. **[ビルド]** メニューの **[ソリューションのリビルド]** をクリックします。

4. ソリューションをビルドすると、FindPrivateKey.exe ファイルが生成されます。

## <a name="conventionscommand-line-entries"></a>規則-コマンドラインエントリ

 "[*option*]" は省略可能なパラメーターのセットを表します。

 "{*option*}" は、必須パラメーターのセットを表します。

 "*オプション 1* &#124; *option2*" は、オプションのセット間の選択を表します。

 "\<*値*>" は、入力するパラメーター値を表します。

## <a name="usage"></a>使用状況

```console
FindPrivateKey <storeName> <storeLocation> [{ {-n <subjectName>} | {-t <thumbprint>} } [-f | -d | -a]]
```

ここで:

| パラメータ         | 説明                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| `<subjectName>` | 証明書のサブジェクト名                                               |
| `<thumbprint>`  | 証明書のサムプリント (この検索には、Certmgr.exe ツールを使用できます) |
| `-f`            | 出力ファイル名のみ                                                             |
| `-d`            | 出力ディレクトリのみ                                                             |
| `-a`            | 出力の絶対ファイル名                                                         |

コマンドプロンプトでパラメーターが指定されていない場合は、この情報を含むヘルプテキストが表示されます。

## <a name="examples"></a>使用例

この例では、現在のユーザーの個人用ストアで、サブジェクト名が "CN = localhost" の証明書のファイル名を検索します。

```console
FindPrivateKey My CurrentUser -n "CN=localhost"
```

この例では、"CN = localhost" というサブジェクト名を持つ証明書のファイル名を現在のユーザーの個人用ストアに検索し、完全なディレクトリパスを出力します。

```console
FindPrivateKey My CurrentUser -n "CN=localhost" -a
```

この例では、ローカル コンピューターの Personal ストアで、"03 33 98 63 d0 47 e7 48 71 33 62 64 76 5c 4c 9d 42 1d 6b 52" というサムプリントを持つ証明書のファイル名を検索します。

```console
FindPrivateKey My LocalMachine -t "03 33 98 63 d0 47 e7 48 71 33 62 64 76 5c 4c 9d 42 1d 6b 52"
```
