---
title: Aximp.exe (Windows フォーム ActiveX コントロール インポーター)
ms.date: 03/30/2017
helpviewer_keywords:
- ActiveX controls, hosting in Windows Forms
- ActiveX Control Importer
- type definitions, converting
- Aximp.exe
- Windows Forms ActiveX Control Importer
ms.assetid: 482c0d83-7144-4497-b626-87d2351b78d0
ms.openlocfilehash: 6d58d1df81780c3033eab7c1ac3e860adeb374b1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79180424"
---
# <a name="aximpexe-windows-forms-activex-control-importer"></a>Aximp.exe (Windows フォーム ActiveX コントロール インポーター)
ActiveX コントロール インポーターは、ActiveX コントロール用の COM タイプ ライブラリに属する型定義を Windows フォーム コントロールに変換します。  
  
 Windows フォームがホストできるのは、<xref:System.Windows.Forms.Control> から派生するクラスである Windows フォームのコントロールだけです。 Aximp.exe は Windows フォーム上でホストできる ActiveX コントロール用のラッパー クラスを生成します。 このため、他の Windows フォーム コントロールに適用できるのと同じデザイン時サポートとプログラミング手順を使用できます。  
  
 ActiveX コントロールをホストするには、<xref:System.Windows.Forms.AxHost> から派生するラッパー コントロールを生成する必要があります。 ラッパー コントロールには、基になる ActiveX コントロールのインスタンスが 1 つ含まれます。 このインスタンスは、ActiveX コントロールとの通信方法を認識しますが、Windows フォーム コントロールとして表示されます。 生成されたコントロールは、ActiveX コントロールをホストし、そのプロパティ、メソッド、およびイベントを、生成されたコントロールのプロパティ、メソッド、およびイベントとして公開します。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
aximp [options]{file.dll | file.ocx}  
```  
  
## <a name="remarks"></a>解説  
  
|引数|[説明]|  
|--------------|-----------------|  
|*file*|変換する ActiveX コントロールを含むソース ファイルの名前。 引数 file の拡張子は .dll または .ocx であることが必要です。|  
  
|オプション|[説明]|  
|------------|-----------------|  
|`/delaysign`|Aximp.exe が遅延署名を使用して、生成されたコントロールに名前を割り当てるように指定します。 `/keycontainer:`、`/keyfile:`、または `/publickey:` オプションのいずれかで、このオプションを指定する必要があります。 遅延署名プロセスの詳細については、「[アセンブリへの遅延署名](../../standard/assembly/delay-sign.md)」を参照してください。|  
|`/help`|このツールのコマンド構文とオプションを表示します。|  
|`/keycontainer:` *containerName*|*containerName* で指定されたキー コンテナーの公開キーと秘密キーのペアを使用して、生成されたコントロールに厳密な名前で署名します。|  
|`/keyfile:` *filename*|*filename* にある発行者の正式な公開キーと秘密キーのペアを使用して、生成されたコントロールに厳密な名前で署名します。|  
|`/nologo`|Microsoft 著作権情報を表示しません。|  
|`/out:` *filename*|作成するアセンブリの名前を指定します。|  
|`/publickey:` *filename*|*filename* で指定されたファイルの公開キーを使用して、生成されたコントロールに厳密な名前で署名します。|  
|`/rcw:` *filename*|新しいものを生成する代わりに、指定したランタイム呼び出し可能ラッパーを使用します。 複数のインスタンスを指定できます。 現在のディレクトリは相対パスに使用されます。 詳細については、「[ランタイム呼び出し可能ラッパー](../../standard/native-interop/runtime-callable-wrapper.md)」 を参照してください。|  
|`/silent`|成功メッセージを表示しません。|  
|`/source`|Windows フォーム ラッパーの C# ソース コードを生成します。|  
|`/verbose`|詳細出力モードを指定します。進行状況に関する追加情報が表示されます。|  
|`/?`|このツールのコマンド構文とオプションを表示します。|  
  
 Aximp.exe は、ActiveX コントロール タイプ ライブラリの全体を一度に変換し、元のタイプ ライブラリの中で定義されていたタイプのための共通言語ランタイム メタデータとコントロール実装を含む、アセンブリのセットを生成します。 生成されるファイルには、次のパターンに従って名前が付けられます。  
  
 COM タイプ用の共通言語ランタイム プロキシ: *progid*.dll  
  
 ActiveX コントロール用の Windows フォーム プロキシ (Ax は ActiveX を表します): Ax*progid*.dll  
  
> [!NOTE]
> ActiveX コントロールのメンバーの名前が .NET Framework で定義された名前と一致する場合、Aximp.exe は、AxHost 派生クラスを作成するときにメンバー名の前に "Ctl" を付けます。 たとえば、ActiveX コントロールに "Layout" というメンバーが含まれている場合、.NET Framework 内で Layout イベントが定義されているため、メンバー名は、AxHost 派生クラスでは "CtlLayout" に変更されます。  
  
 これらの生成されたファイルは、[Ildasm.exe (IL 逆アセンブラー)](ildasm-exe-il-disassembler.md) などのツールでチェックできます。  
  
 Aximp.exe を使用して ActiveX WebBrowser コントロール (shdocvw.dll) 用の .NET アセンブリを生成する方法はサポートされていません。  
  
 shdocvw.dll に対して Aximp.exe を実行すると、ツールが実行されるディレクトリに shdocvw.dll という名前の別のファイルが常に作成されます。 この生成されたファイルが Documents ディレクトリおよび Settings ディレクトリに置かれると、Microsoft Internet Explorer や Windows エクスプローラーに対して問題を引き起こします。 コンピューターを再起動すると、Windows は shdocvw.dll のコピーを見つけるために、system32 ディレクトリの前に Documents and Settings ディレクトリを調べます。 Windows は Documents and Settings で見つけたコピーを使用して、マネージド ラッパーを読み込もうとします。 Internet Explorer と Windows エクスプローラーは、system32 ディレクトリにある shdocvw.dll のバージョンのレンダリング エンジンに依存しているため、正しく機能しません。 このような問題が発生したら、Documents ディレクトリおよび Settings ディレクトリにある shdocvw.dll を削除して、コンピューターを再起動します。  
  
 shdocvw.dll で Aximp.exe を使用して、アプリケーション開発で使用する .NET アセンブリを作成する場合にも、問題が発生する可能性があります。 この場合、アプリケーションは、システム バージョンと生成されたバージョンの両方の shdocvw.dll を読み込み、システム バージョンを優先します。 このとき、WebBrowser ActiveX コントロール内部の Web ページを読み込もうとすると、[開いて保存] ダイアログ ボックスが表示される場合があります。 ユーザーが **[開く]** をクリックすると、Internet Explorer で Web ページが開きます。 これは、Internet Explorer version 6 以前を実行しているコンピューターでのみ発生します。 この問題の発生を防ぐには、「<xref:System.Windows.Forms.WebBrowser>方法: タイプ ライブラリへの参照を追加する[」に説明されているように、マネージド ](../interop/how-to-add-references-to-type-libraries.md) コントロールまたは Visual Studio を使用してマネージド shdocvw.dll を生成します。  
  
## <a name="example"></a>例  
 Media Player コントロール `msdxm.ocx` の MediaPlayer.dll と AxMediaPlayer.dll を生成するコマンドを次に示します。  
  
```console
aximp c:\systemroot\system32\msdxm.ocx  
```  
  
## <a name="see-also"></a>参照

- [ツール](index.md)
- [Ildasm.exe (IL 逆アセンブラー)](ildasm-exe-il-disassembler.md)
