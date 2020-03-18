---
title: 'アセンブリを作成できません : <error message>'
ms.date: 08/14/2018
f1_keywords:
- vbc30145
- bc30145
helpviewer_keywords:
- BC30145
ms.assetid: 2e7eb2b9-eda6-4bdb-95cc-72c7f0be7528
ms.openlocfilehash: 5776755a57fbc2b0086b1c9b6cfbb2f2b7eb03fa
ms.sourcegitcommit: 5a28f8eb071fcc09b045b0c4ae4b96898673192e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2019
ms.locfileid: "73197275"
---
# <a name="unable-to-emit-assembly-error-message"></a>アセンブリを生成できません: \<エラーメッセージ >

Visual Basic コンパイラは、アセンブリリンカー (Al.exe、Alink とも呼ばれ*ます*) を呼び出してマニフェストを持つアセンブリを生成します。リンカーは、アセンブリの作成の出力段階でエラーを報告します。

**エラー ID:** BC30145

## <a name="to-correct-this-error"></a>このエラーを解決するには

1. 引用符で囲まれたエラーメッセージを調べ、詳細な説明とアドバイスについては、「 [al.exe](../../../framework/tools/al-exe-assembly-linker.md) 」を参照してください。

2. [Al.exe](../../../framework/tools/al-exe-assembly-linker.md)または[Sn.exe (厳密名ツール)](../../../framework/tools/sn-exe-strong-name-tool.md)を使用して、手動でアセンブリに署名してみます。

3. エラーが続く場合は、状況に関する情報を収集し、マイクロソフト プロダクト サポート サービスに通知してください。

### <a name="to-sign-the-assembly-manually"></a>アセンブリを手動で署名するには

1. [Sn.exe (厳密名ツール)](../../../framework/tools/sn-exe-strong-name-tool.md)) を使用して、公開キーと秘密キーのペアファイルを作成します。

   このファイルの拡張子は *.snk*です。

2. エラーが発生している COM 参照をプロジェクトから削除します。

3. [Visual Studio の開発者コマンドプロンプト](../../../framework/tools/developer-command-prompt-for-vs.md)を開きます。

   Windows 10 では、タスクバーの検索ボックスに「**開発者コマンドプロンプト**」と入力します。 次に、結果一覧から**開発者コマンドプロンプト [FOR VS 2017** ] を選択します。

4. ディレクトリを、アセンブリラッパーを配置するディレクトリに変更します。

5. 次のコマンドを入力します。

    ```cmd
    tlbimp <path to COM reference file> /out:<output assembly name> /keyfile:<path to .snk file>
    ```

   実際に入力するコマンドの例を次に示します。

    ```cmd
    tlbimp c:\windows\system32\msi.dll /out:Interop.WindowsInstaller.dll /keyfile:"c:\documents and settings\mykey.snk"
    ```

   > [!TIP]
   > パスまたはファイルにスペースが含まれている場合は、二重引用符を使用します。

6. Visual Studio で、先ほど作成したファイルに .NET アセンブリ参照を追加します。

## <a name="see-also"></a>関連項目

- [Al.exe](../../../framework/tools/al-exe-assembly-linker.md)
- [Sn.exe (厳密名ツール)](../../../framework/tools/sn-exe-strong-name-tool.md)
- [方法: 公開キーと秘密キーのキー ペアを作成する](../../../standard/assembly/create-public-private-key-pair.md)
- [ご意見](/visualstudio/ide/feedback-options)
