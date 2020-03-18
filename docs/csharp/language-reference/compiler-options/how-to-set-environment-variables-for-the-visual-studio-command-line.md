---
title: Visual Studio のコマンドラインのための環境変数を設定する方法
ms.date: 12/20/2019
f1_keywords:
- cs.build.commandline
helpviewer_keywords:
- csc.exe, command-line builds
- Visual C#, command-line builds
- command-line compilers, Visual C#
- Vsvars32.bat
- builds [C#], command-line
- compilers, invoking from command line
- Vcvars32.bat file
- command line [C#], building from
- Visual C# compiler, enabling
- compiling source code, from command line
ms.assetid: 7ec09480-5612-4f6a-8d00-ad90ea9bca5d
ms.openlocfilehash: 99e2a837877494dd4c7e0106047bce3cc39a9282
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75342364"
---
# <a name="how-to-set-environment-variables-for-the-visual-studio-command-line"></a>Visual Studio のコマンドラインのための環境変数を設定する方法

VsDevCmd.bat ファイルは、適切な環境変数を設定してコマンド ライン ビルドを有効にします。

> [!NOTE]
> Visual Studio 2015 とそれ以前のバージョンでは、同じ目的で VsDevCmd.bat ではなく VSVARS32.bat を使用しました。 このファイルは \Program Files\Microsoft Visual Studio\\*Version*\Common7\Tools または Program Files (x86)\Microsoft Visual Studio\\*Version*\Common7\Tools に格納されていました。

以前のバージョンの Visual Studio と最新バージョンの Visual Studio の両方がコンピューターにインストールされている場合は、同じコマンド プロンプト ウィンドウから異なるバージョンの VsDevCmd.bat または VSVARS32.BAT を実行しないでください。 代わりに、独自のウィンドウで、各バージョンのコマンドを実行する必要があります。

### <a name="to-run-vsdevcmdbat"></a>VsDevCmd.BAT を実行するには

1. **[スタート]** メニューから、**VS 2019 の開発者コマンド プロンプト**を開きます。  これは、 **[Visual Studio 2019]** フォルダーにあります。

2. インストールの \Program Files\Microsoft Visual Studio\\*Version*\\*Offering*\Common7\Tools または \Program Files (x86)\Microsoft Visual Studio\\*Version*\\*Offering*\Common7\Tools サブディレクトリに移動します。  (*Version* は最新バージョンの *2019* です。 *Offering* は *Enterprise*、*Professional* または *Community* のいずれかです。)

3. 「**VsDevCmd**」と入力して、VsDevCmd.bat を実行します。

    > [!CAUTION]
    > VsDevCmd.bat の場所は、コンピューターによって異なる場合があります。 VsDevCmd.bat ファイルが見つからない場合や破損している場合でも、別のコンピューターの VsDevCmd.bat と置き換えないでください。 その場合は、セットアップ プログラムを再実行してファイルを置き換えてください。

### <a name="available-options-for-vsdevcmdbat"></a>VsDevCmd.BAT に使用できるオプション

VsDevCmd.BAT に使用できるオプションを確認するには、`-help` オプションを使ってコマンドを実行します。

```console
VsDevCmd.bat -help
```

## <a name="see-also"></a>参照

- [csc.exe を使用したコマンド ラインからのビルド](./command-line-building-with-csc-exe.md)
