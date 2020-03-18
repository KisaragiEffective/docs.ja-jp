---
title: -langversion (C# コンパイラ オプション)
ms.date: 08/23/2019
f1_keywords:
- /langversion
helpviewer_keywords:
- /langversion compiler option [C#]
- -langversion compiler option [C#]
- langversion compiler option [C#]
ms.assetid: 3fb00b05-a0ff-4782-b313-13a4c0f62d94
ms.openlocfilehash: 007b10f6f27233c43caad4c1910e3d1158682950
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76920367"
---
# <a name="-langversion-c-compiler-options"></a>-langversion (C# コンパイラ オプション)

コンパイラが、選択した C# 言語仕様に含まれている構文のみを受け入れるようにします。

## <a name="syntax"></a>構文

```console
-langversion:option
```

## <a name="arguments"></a>引数

`option`

有効な値は、

|オプション|意味|
|------------|-------------|
|preview|コンパイラは、サポート可能な最新のプレビュー バージョンの有効な言語構文をすべて受け入れます。|
|latest|コンパイラは、サポート可能な最新バージョン (マイナー リリースを含む) の有効な言語構文をすべて受け入れます。|
|latestMajor|コンパイラは、最新のメジャー バージョンからサポートできるすべての有効な言語構文を受け入れます。|
|8.0|コンパイラは、C# 8.0 以下に含まれている構文のみを受け入れます。|
|7.3|コンパイラは、C# 7.3 以下に含まれている構文のみを受け入れます。|
|7.2|コンパイラは、C# 7.2 以下に含まれている構文のみを受け入れます。|
|7.1|コンパイラは、C# 7.1 以下に含まれている構文のみを受け入れます。|
|7|コンパイラは、C# 7.0 以下に含まれている構文のみを受け入れます。|
|6|コンパイラは、C# 6.0 以下に含まれている構文のみを受け入れます。|
|5|コンパイラは、C# 5.0 以下に含まれている構文のみを受け入れます。|
|4|コンパイラは、C# 4.0 以下に含まれている構文のみを受け入れます。|
|3|コンパイラは、C# 3.0 以下に含まれている構文のみを受け入れます。|
|ISO-2|コンパイラは、ISO/IEC 23270:2006 C# (2.0) に含まれている構文のみを受け入れます。|
|ISO-1|コンパイラは、ISO/IEC 23270:2003 C# (1.0/1.2) に含まれている構文のみを受け入れます。|

既定の言語バージョンは、アプリケーションのターゲット フレームワークやインストールされている SDK または Visual Studio のバージョンに依存します。 これらの規則は、[言語バージョンの構成](../configure-language-version.md#defaults)に関する記事の中で定義されています。

## <a name="remarks"></a>解説

C# アプリケーションで参照されるメタデータは、 **-langversion** コンパイラ オプションの対象になりません。

C# コンパイラのバージョンごとに言語仕様の拡張機能が含まれているため、 **-langversion** は、コンパイラの以前のバージョンと同じ機能を提供しません。

さらに、C# バージョンの更新は、一般的に主要な .NET Framework のリリースと一致しますが、新しい構文および機能は必ずしも特定のフレームワーク バージョンに関連付けられていません。 新機能では、C# リビジョンと共にリリースされる新しいコンパイラの更新プログラムを確実に必要としますが、各特定機能には、独自の最小の .NET API または共通言語ランタイムの要件があり、この要件によって、NuGet パッケージやその他のライブラリを含めることで下位レベルのフレームワークで実行できるようになります。

使用する **-langversion** の設定に関係なく、現在のバージョンの共通言語ランタイムを使用して .exe や .dll を作成します。 1 つの例外は、[-langversion:ISO-1](./moduleassemblyname-compiler-option.md) の下で機能する、フレンド アセンブリと **-moduleassemblyname (C# コンパイラ オプション)** です。

C# 言語バージョンを指定するその他の方法については、[C# 言語のバージョンの選択](../configure-language-version.md)に関するトピックを参照してください。

このコンパイラ オプションをプログラムで設定する方法については、「<xref:VSLangProj80.CSharpProjectConfigurationProperties3.LanguageVersion%2A>」を参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

|バージョン|リンク|[説明]|
|-------|----|-----------|
|C# 7.0 以降||現在使用できません|
|C# 6.0|[リンク](/dotnet/csharp/language-reference/language-specification/introduction)|C# 言語仕様バージョン 6 - 非公式ドラフト: .NET Foundation|
|C# 5.0|[[Download PDF]\(PDF をダウンロード\)](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf)|Standard ECMA-334 5th Edition|
|C# 3.0|[DOC のダウンロード](https://download.microsoft.com/download/3/8/8/388e7205-bc10-4226-b2a8-75351c669b09/CSharp%20Language%20Specification.doc)|C# 言語仕様バージョン 3.0: Microsoft Corporation|
|C# 2.0|[[Download PDF]\(PDF をダウンロード\)](https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%204th%20edition%20June%202006.pdf)|Standard ECMA-334 4th Edition|
|C# 1.2|[DOC のダウンロード](https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%202nd%20edition%20December%202002.pdf)|C# 言語仕様バージョン 1.2: Microsoft Corporation|
|C# 1.0|[DOC のダウンロード](https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%201st%20edition%20December%202001.pdf)|C# 言語仕様バージョン 1.0: Microsoft Corporation|

## <a name="minimum-sdk-version-needed-to-support-all-language-features"></a>すべての言語機能をサポートするために必要な SDK の最小バージョン

次の表は、SDK の最小バージョンに対応する言語バージョンをサポートする C# コンパイラを示しています。

|C# バージョン|SDK の最小バージョン|
|----------|-------------------|
|C# 8.0| Microsoft Visual Studio/Build Tools 2019、バージョン 16.3 または .NET Core 3.0 SDK |
|C# 7.3| Microsoft Visual Studio/Build Tools 2017、バージョン 15.7 |
|C# 7.2| Microsoft Visual Studio/Build Tools 2017、バージョン 15.5 |
|C# 7.1| Microsoft Visual Studio/Build Tools 2017、バージョン 15.3 |
|C# 7.0| Microsoft Visual Studio/Build Tools 2017 |
|C# 6| Microsoft Visual Studio/Build Tools 2015 |
|C# 5| Microsoft Visual Studio/Build Tools 2012、またはバンドルされている .Net Framework 4.5 コンパイラ |
|C# 4| Microsoft Visual Studio/Build Tools 2010、またはバンドルされている .Net Framework 4.0 コンパイラ |
|C# 3| Microsoft Visual Studio/Build Tools 2008、またはバンドルされている .Net Framework 3.5 コンパイラ |
|C# 2| Microsoft Visual Studio/Build Tools 2005、またはバンドルされている .Net Framework 2.0 コンパイラ |
|C# 1.0/1.2 | Microsoft Visual Studio/Build Tools .NET 2002 またはバンドルされている .NET Framework 1.0 コンパイラ |

## <a name="see-also"></a>参照

- [C# コンパイラ オプション](index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
