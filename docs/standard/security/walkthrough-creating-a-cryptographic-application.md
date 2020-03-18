---
title: 'チュートリアル : 暗号化アプリケーションの作成'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptography [NET Framework], example
- cryptography [NET Framework], cryptographic application example
- cryptography [NET Framework], application example
ms.assetid: abf48c11-1e72-431d-9562-39cf23e1a8ff
ms.openlocfilehash: 6e2d9b8bebdfd2ea5d5507cc73d444fa8bf785fb
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75705835"
---
# <a name="walkthrough-creating-a-cryptographic-application"></a>チュートリアル : 暗号化アプリケーションの作成
このチュートリアルでは、コンテンツの暗号化および復号化の方法を示します。 コード例は、Windows フォーム アプリケーション向けに設計されています。 このアプリケーションは、スマート カードを使用するなどの実際のシナリオは示していません。 代わりに、暗号化と復号化の基礎を示しています。  
  
 このチュートリアルでは、次の暗号化のガイドラインを使用します。  
  
- 自動生成される <xref:System.Security.Cryptography.SymmetricAlgorithm.Key%2A> と <xref:System.Security.Cryptography.SymmetricAlgorithm.IV%2A> を使用すると、対称アルゴリズムである <xref:System.Security.Cryptography.RijndaelManaged> クラスでデータの暗号化と復号化を行えます。  
  
- 非対称アルゴリズムである <xref:System.Security.Cryptography.RSACryptoServiceProvider> を使用すると、<xref:System.Security.Cryptography.RijndaelManaged> で暗号化されたデータのキーの暗号化と復号化を行えます。 非対称アルゴリズムは、キーなどの少量のデータに最適です。  
  
    > [!NOTE]
    > 暗号化されたコンテンツを他のユーザーと交換するのではなく、コンピューター上のデータを保護する場合は、<xref:System.Security.Cryptography.ProtectedData> クラスまたは <xref:System.Security.Cryptography.ProtectedMemory> クラスの使用を検討してください。  
  
 次の表は、このトピックの暗号化のタスクをまとめたものです。  
  
|タスク|説明|  
|----------|-----------------|  
|Windows フォーム アプリケーションの作成|アプリケーションを実行するために必要なコントロールの一覧を表示します。|  
|グローバル オブジェクトの宣言|文字列のパスの変数、<xref:System.Security.Cryptography.CspParameters>、および <xref:System.Security.Cryptography.RSACryptoServiceProvider> を宣言して、<xref:System.Windows.Forms.Form> クラスのグローバル コンテキストを指定します。|  
|非対称キーの作成|非対称の公開キーと秘密キーの値のペアを作成し、これにキー コンテナー名を割り当てます。|  
|ファイルの暗号化|暗号化するファイルを選択するダイアログ ボックスを表示し、ファイルを暗号化します。|  
|ファイルの復号化|復号化する暗号化されたファイルを選択するダイアログ ボックスを表示し、ファイルを復号化します。|  
|秘密キーの取得|キー コンテナー名を使用して、完全なキーのペアを取得します。|  
|公開キーのエクスポート|パブリック パラメーターのみで XML ファイルにキーを保存します。|  
|公開キーのインポート|キーを XML ファイルからキー コンテナーに読み込みます。|  
|アプリケーションのテスト|このアプリケーションをテストするための手順を一覧に示します。|  
  
## <a name="prerequisites"></a>[前提条件]  
 このチュートリアルを実行するには、次のコンポーネントが必要です。  
  
- <xref:System.IO> 名前空間と <xref:System.Security.Cryptography> 名前空間への参照。  
  
## <a name="creating-a-windows-forms-application"></a>Windows フォーム アプリケーションの作成  
 このチュートリアルにあるほとんどのコード例は、ボタン コントロールのイベント ハンドラーとして設計されています。 次の表は、サンプル アプリケーションに必要なコントロールと、コード例に一致する必要な名前を示しています。  
  
|Control|[名前]|テキストのプロパティ (必要に応じて)|  
|-------------|----------|---------------------------------|  
|<xref:System.Windows.Forms.Button>|`buttonEncryptFile`|ファイルの暗号化|  
|<xref:System.Windows.Forms.Button>|`buttonDecryptFile`|ファイルの復号化|  
|<xref:System.Windows.Forms.Button>|`buttonCreateAsmKeys`|キーの作成|  
|<xref:System.Windows.Forms.Button>|`buttonExportPublicKey`|公開キーのエクスポート|  
|<xref:System.Windows.Forms.Button>|`buttonImportPublicKey`|公開キーのインポート|  
|<xref:System.Windows.Forms.Button>|`buttonGetPrivateKey`|秘密キーの取得|  
|<xref:System.Windows.Forms.Label>|`label1`|キーが設定されていません|  
|<xref:System.Windows.Forms.OpenFileDialog>|`openFileDialog1`||  
|<xref:System.Windows.Forms.OpenFileDialog>|`openFileDialog2`||  
  
 ボタンのイベント ハンドラーを作成するには、Visual Studio デザイナーでボタンをダブルクリックします。  
  
## <a name="declaring-global-objects"></a>グローバル オブジェクトの宣言  
 次のコードをフォームのコンストラクターに追加します。 環境とユーザー設定のための文字列変数を編集します。  
  
 [!code-csharp[CryptoWalkThru#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#1)]
 [!code-vb[CryptoWalkThru#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#1)]  
  
## <a name="creating-an-asymmetric-key"></a>非対称キーの作成  
 この作業では、<xref:System.Security.Cryptography.RijndaelManaged> キーの暗号化と復号化を行う非対称キーを作成します。 このキーは、コンテンツの暗号化に使用されたもので、ラベル コントロールにキー コンテナー名を表示します。  
  
 次のコードを [`Create Keys`] ボタン (`buttonCreateAsmKeys_Click`) の `Click` イベント ハンドラーとして追加します。  
  
 [!code-csharp[CryptoWalkThru#2](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#2)]
 [!code-vb[CryptoWalkThru#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#2)]  
  
## <a name="encrypting-a-file"></a>ファイルの暗号化  
 このタスクには、`Encrypt File` ボタン (`buttonEncryptFile_Click`) のイベントハンドラーメソッドと `EncryptFile` メソッドの2つのメソッドが含まれます。 最初のメソッドは、ファイルを選択するためのダイアログ ボックスを表示し、暗号化を実行する 2 番目のメソッドにファイル名を渡します。  
  
 暗号化されたコンテンツ、キー、および IV は、すべて 1 つの <xref:System.IO.FileStream> に保存されます。これを暗号化パッケージといいます。  
  
 `EncryptFile` メソッドは以下を実行します。  
  
1. コンテンツを暗号化する <xref:System.Security.Cryptography.RijndaelManaged> 対称アルゴリズムを作成します。  
  
2. <xref:System.Security.Cryptography.RijndaelManaged> キーを暗号化する <xref:System.Security.Cryptography.RSACryptoServiceProvider> オブジェクトを作成します。  
  
3. ソース ファイルの <xref:System.IO.FileStream> の読み取りと暗号化を行う <xref:System.Security.Cryptography.CryptoStream> オブジェクト (バイトのブロック) を使用して、暗号化ファイルの対象の <xref:System.IO.FileStream> オブジェクトにします。  
  
4. 暗号化されたキーと IV の長さを決定し、長さの値のバイト配列を作成します。  
  
5. 暗号化されたパッケージにキー、IV、および長さの値を書き込みます。  
  
 暗号化パッケージでは、次の形式を使用します。  
  
- キーの長さ: 0 - 3 バイト  
  
- IV の長さ: 4 - 7 バイト  
  
- 暗号化されたキー  
  
- IV  
  
- 暗号化テキスト  
  
 キーと IV の長さを使用すると、暗号化パッケージのすべての部分の開始点と長さを決定することができます。これを使用してファイルを復号化します。  
  
 次のコードを [`Encrypt File`] ボタン (`buttonEncryptFile_Click`) の `Click` イベント ハンドラーとして追加します。  
  
 [!code-csharp[CryptoWalkThru#3](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#3)]
 [!code-vb[CryptoWalkThru#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#3)]  
  
 フォームに次の `EncryptFile` メソッドを追加します。  
  
 [!code-csharp[CryptoWalkThru#5](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#5)]
 [!code-vb[CryptoWalkThru#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#5)]  
  
## <a name="decrypting-a-file"></a>ファイルの復号化  
 このタスクには、[`Decrypt File`] ボタン (`buttonDecryptFile_Click`) のイベント ハンドラー メソッドと `DecryptFile` メソッドという 2 つのメソッドが含まれています。 最初のメソッドは、ファイルを選択するためのダイアログ ボックスを表示し、復号化を実行する 2 番目のメソッドにファイル名を渡します。  
  
 `Decrypt` メソッドは以下を実行します。  
  
1. コンテンツを復号化する <xref:System.Security.Cryptography.RijndaelManaged> の対称アルゴリズムを作成します。  
  
2. 暗号化されたキーと IV の長さを取得するには、暗号化パッケージの <xref:System.IO.FileStream> の最初の 8 バイトを読み取ってバイト配列にします。  
  
3. キーと IV を暗号化パッケージから抽出してバイト配列にします。  
  
4. <xref:System.Security.Cryptography.RijndaelManaged> キーを復号化する <xref:System.Security.Cryptography.RSACryptoServiceProvider> オブジェクトを作成します。  
  
5. <xref:System.Security.Cryptography.CryptoStream> オブジェクトを使用して、<xref:System.IO.FileStream> の暗号化パッケージの暗号テキスト セクションをバイトのブロックで読み取って復号化し、復号化されたファイルの <xref:System.IO.FileStream> オブジェクトにします。 これが終了すると、復号化は完了です。  
  
 次のコードを、[`Decrypt File`] ボタンの `Click` イベント ハンドラーとして追加します。  
  
 [!code-csharp[CryptoWalkThru#4](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#4)]
 [!code-vb[CryptoWalkThru#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#4)]  
  
 フォームに次の `DecryptFile` メソッドを追加します。  
  
 [!code-csharp[CryptoWalkThru#6](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#6)]
 [!code-vb[CryptoWalkThru#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#6)]  
  
## <a name="exporting-a-public-key"></a>公開キーのエクスポート  
 この作業では、[`Create Keys`] ボタンによって作成されたキーをファイルに保存します。 パブリック パラメーターのみがエクスポートされます。  
  
 このタスクでは、アリスがボブに公開キーを与えるシナリオをシミュレーションします。そうすることで、ボブはアリスのファイルを暗号化できるようになります。 ボブとその公開キーを持つ他のユーザーはファイルを復号化できなくなります。彼らはプライベート パラメーターを持つ完全なキーのペアを持っていないためです。  
  
 次のコードを [`Export Public Key`] ボタン (`buttonExportPublicKey_Click`) の `Click` イベント ハンドラーとして追加します。  
  
 [!code-csharp[CryptoWalkThru#8](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#8)]
 [!code-vb[CryptoWalkThru#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#8)]  
  
## <a name="importing-a-public-key"></a>公開キーのインポート  
 このタスクでは、[`Export Public Key`] ボタンによって作成されたパブリック パラメーターのみを持つキーを読み込み、キー コンテナー名として設定します。  
  
 この作業では、ボブがアリスのファイルを暗号化できるように、ボブがパブリック パラメーターのみを持つアリスのキーを読み込むシナリオをシミュレーションします。  
  
 次のコードを [`Import Public Key`] ボタン (`buttonImportPublicKey_Click`) の `Click` イベント ハンドラーとして追加します。  
  
 [!code-csharp[CryptoWalkThru#9](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#9)]
 [!code-vb[CryptoWalkThru#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#9)]  
  
## <a name="getting-a-private-key"></a>秘密キーの取得  
 この作業では、[`Create Keys`] ボタンを使用して作成されたキーの名前にキー コンテナー名を設定します。 キー コンテナーには、プライベート パラメーターを持つ完全なキーのペアが格納されます。  
  
 この作業では、アリスが自分の秘密キーを使用してボブが暗号化したファイルを復号化するシナリオをシミュレーションします。  
  
 次のコードを [`Get Private Key`] ボタン (`buttonGetPrivateKey_Click`) の `Click` イベント ハンドラーとして追加します。  
  
 [!code-csharp[CryptoWalkThru#7](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#7)]
 [!code-vb[CryptoWalkThru#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#7)]  
  
## <a name="testing-the-application"></a>アプリケーションのテスト  
 アプリケーションをビルドしたら、次のテスト シナリオを実行します。  
  
#### <a name="to-create-keys-encrypt-and-decrypt"></a>キーの作成、暗号化、および復号化を行うには  
  
1. [`Create Keys`] ボタンをクリックします。 ラベルはキー名を表示し、完全なキーのペアであることを示します。  
  
2. [`Export Public Key`] ボタンをクリックします。 公開キーのパラメーターのエクスポートにより現在のキーは変更されないことに注意してください。  
  
3. [`Encrypt File`] ボタンをクリックして、ファイルを選択します。  
  
4. [`Decrypt File`] ボタンをクリックし、暗号化したファイルを選択します。  
  
5. 復号化したファイルを調べます。  
  
6. 次のシナリオで保持されたキー コンテナーを取得するテストを実行するには、アプリケーションを閉じて再起動します。  
  
#### <a name="to-encrypt-using-the-public-key"></a>公開キーを使用して暗号化するには  
  
1. [`Import Public Key`] ボタンをクリックします。 ラベルはキー名を表示し、公開キーのみであることを示します。  
  
2. [`Encrypt File`] ボタンをクリックして、ファイルを選択します。  
  
3. [`Decrypt File`] ボタンをクリックし、暗号化したファイルを選択します。 復号化するには秘密キーが必要であるため、これは失敗します。  
  
 このシナリオでは、公開キーのみによる別のユーザーのファイルの暗号化をデモンストレーションします。 通常、そのユーザーは公開キーのみを与え、秘密キーは復号化のため与えないでおきます。  
  
#### <a name="to-decrypt-using-the-private-key"></a>秘密キーを使用して復号化するには  
  
1. [`Get Private Key`] ボタンをクリックします。 ラベルはキー名を表示し、完全なキーのペアであるかどうかを示します。  
  
2. [`Decrypt File`] ボタンをクリックし、暗号化したファイルを選択します。 復号化するための完全なキーのペアがあるため、これは成功します。  
  
## <a name="see-also"></a>関連項目

- [Cryptographic Services](../../../docs/standard/security/cryptographic-services.md)
