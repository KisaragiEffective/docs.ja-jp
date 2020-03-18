---
title: コンポジション分析ツール (Mefx)
ms.date: 03/30/2017
helpviewer_keywords:
- Composition Analysis Tool [MEF]
- MEF, Composition Analysis Tool
- Mefx [MEF], Composition Analysis Tool
ms.assetid: c48a7f93-83bb-4a06-aea0-d8e7bd1502ad
ms.openlocfilehash: bb2748b16a16d7d01b076402889829f5b31a1912
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73126369"
---
# <a name="composition-analysis-tool-mefx"></a>コンポジション分析ツール (Mefx)
合成分析ツール (Mefx) は、Managed Extensibility Framework (MEF) のパートが含まれたライブラリ (.dll) ファイルとアプリケーション (.exe) ファイルを分析するコマンド ライン アプリケーションです。 Mefx の主な目的は、開発者が煩雑なトレース コードをアプリケーション自体に追加することなく、MEF アプリケーションの合成エラーを診断できるようにすることです。 また、Mefx は、サード パーティが提供するライブラリのパートについて理解する際にも役立ちます ここでは、Mefx の使用方法について説明し、構文のリファレンスを示します。  
  
<a name="getting_mefx"></a>   
## <a name="getting-mefx"></a>Mefx の入手  
 Mefx は、GitHub の [Managed Extensibility Framework](https://github.com/MicrosoftArchive/mef/releases/tag/4.0) で入手できます。 ツールをダウンロードして解凍してください。  
  
<a name="basic_syntax"></a>   
## <a name="basic-syntax"></a>基本構文  
 Mefx は、次の形式でコマンド ラインから起動します。  
  
```console
mefx [files and directories] [action] [options]  
```  
  
 引数の最初のセットでは、分析対象のパートの読み込み元となるファイルとディレクトリを指定します。 `/file:` スイッチを使用してファイルを指定し、 `/directory:` スイッチを使用してディレクトリを指定します。 次の例に示すように、複数のファイルまたはディレクトリを指定できます。  
  
```console  
mefx /file:MyAddIn.dll /directory:Program\AddIns [action...]  
```  
  
> [!NOTE]
> 各 .dll または .exe は一度だけ読み込むようにしてください。 1 つのファイルを何度も読み込むと、ツールから間違った情報が返されることがあります。  
  
 ファイルとディレクトリを指定した後、コマンドと、そのコマンドのオプションを指定する必要があります。  
  
<a name="listing_available_parts"></a>   
## <a name="listing-available-parts"></a>使用可能なパートの一覧を表示する  
 `/parts` アクションを使用すると、読み込んだファイルで宣言されているすべてのパートの一覧が表示されます。 結果は、パート名の単純なリストです。  
  
```console
mefx /file:MyAddIn.dll /parts  
MyAddIn.AddIn  
MyAddIn.MemberPart  
```  
  
 パートの詳細を参照する場合は、 `/verbose` オプションを使用します。 利用可能なすべてのパートの詳細が出力されます。 1 つのパートに関する詳細情報を入手する場合は、 `/type` アクションではなく `/parts`アクションを使用します。  
  
```console  
mefx /file:MyAddIn.dll /type:MyAddIn.AddIn /verbose  
[Part] MyAddIn.MemberPart from: AssemblyCatalog (Assembly=" MyAddIn, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Export] MyAddIn.MemberPart (ContractName=" MyAddIn.MemberPart")  
```  
  
<a name="listing_imports_and_exports"></a>   
## <a name="listing-imports-and-exports"></a>インポートとエクスポートの一覧表示  
 `/imports` アクションと `/exports` アクションでは、インポートされたすべてのパートと、エクスポートされたすべてのパートがそれぞれ一覧表示されます。 `/importers` アクションまたは `/exporters` アクションを使用して、特定の型をインポートまたはエクスポートするパートを一覧表示することもできます。  
  
```console  
mefx /file:MyAddIn.dll /importers:MyAddin.MemberPart  
MyAddin.AddIn  
```  
  
 これらのアクションに `/verbose` オプションを適用することもできます。  
  
<a name="finding_rejected_parts"></a>   
## <a name="finding-rejected-parts"></a>拒否されたパートの検索  
 使用可能なパートを読み込んだ後、Mefx は MEF 合成エンジンを使用してそれらのパートを合成します。 正常に構成できないパートのことを、 *拒否された*パートと呼びます。 拒否されたすべてのパートの一覧を表示するには、 `/rejected` アクションを使用します。  
  
 `/verbose` アクションで `/rejected` オプションを使用すると、拒否されたパートに関する詳細情報が出力されます。 次の例では、 `ClassLibrary1` という DLL に `AddIn` パートが含まれています。このパートは、 `MemberPart` パートと `ChainOne` パートをインポートします。 `ChainOne` は `ChainTwo`をインポートしますが、 `ChainTwo` が存在しません。 そのため、 `ChainOne` が拒否され、この拒否が原因で `AddIn` も拒否されることになります。  
  
```console  
mefx /file:ClassLibrary1.dll /rejected /verbose  
```  
  
 前のコマンドの完全な出力を次に示します。  
  
```output
[Part] ClassLibrary1.AddIn from: AssemblyCatalog (Assembly="ClassLibrary1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Export] ClassLibrary1.AddIn (ContractName="ClassLibrary1.AddIn")  
  [Import] ClassLibrary1.AddIn.memberPart (ContractName="ClassLibrary1.MemberPart")  
    [SatisfiedBy] ClassLibrary1.MemberPart (ContractName="ClassLibrary1.MemberPart") from: ClassLibrary1.MemberPart from: AssemblyCatalog (Assembly="ClassLibrar  
y1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Import] ClassLibrary1.AddIn.chain (ContractName="ClassLibrary1.ChainOne")  
    [Exception] System.ComponentModel.Composition.ImportCardinalityMismatchException: No valid exports were found that match the constraint '((exportDefinition.ContractName == "ClassLibrary1.ChainOne") AndAlso (exportDefinition.Metadata.ContainsKey("ExportTypeIdentity") AndAlso "ClassLibrary1.ChainOne".Equals(exportDefinition.Metadata.get_Item("ExportTypeIdentity"))))', invalid exports may have been rejected.  
   at System.ComponentModel.Composition.Hosting.ExportProvider.GetExports(ImportDefinition definition, AtomicComposition atomicComposition)  
   at Microsoft.ComponentModel.Composition.Diagnostics.CompositionInfo.AnalyzeImportDefinition(ExportProvider host, IEnumerable`1 availableParts, ImportDefinition id)  
    [Unsuitable] ClassLibrary1.ChainOne (ContractName="ClassLibrary1.ChainOne")  
from: ClassLibrary1.ChainOne from: AssemblyCatalog (Assembly="ClassLibrary1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
      [Because] PartDefinitionIsRejected, The part providing the export is rejected because of other issues.  
  
[Part] ClassLibrary1.ChainOne from: AssemblyCatalog (Assembly="ClassLibrary1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Primary Rejection]  
  [Export] ClassLibrary1.ChainOne (ContractName="ClassLibrary1.ChainOne")  
  [Import] ClassLibrary1.ChainOne.chain (ContractName="ClassLibrary1.ChainTwo")  
    [Exception] System.ComponentModel.Composition.ImportCardinalityMismatchException: No valid exports were found that match the constraint '((exportDefinition.ContractName == "ClassLibrary1.ChainTwo") AndAlso (exportDefinition.Metadata.ContainsKey("ExportTypeIdentity") AndAlso "ClassLibrary1.ChainTwo".Equals(exportDefinition.Metadata.get_Item("ExportTypeIdentity"))))', invalid exports may have been rejected.  
   at System.ComponentModel.Composition.Hosting.ExportProvider.GetExports(ImportDefinition definition, AtomicComposition atomicComposition)  
   at Microsoft.ComponentModel.Composition.Diagnostics.CompositionInfo.AnalyzeImportDefinition(ExportProvider host, IEnumerable`1 availableParts, ImportDefinition id)  
```  
  
 `[Exception]` と `[Unsuitable]` の結果に、有用な情報が含まれています。 `[Exception]` の結果には、パートが拒否された理由に関する情報が示されています。 `[Unsuitable]` の結果には、他の点では一致するパートを使用してインポートを満たせなかった理由が示されています。ここでは、インポートが見つからないために、パートそのものが拒否されたということが分かります。  
  
<a name="analyzing_primary_causes"></a>   
## <a name="analyzing-primary-causes"></a>主要な原因を分析する  
 長い依存関係チェーンで複数のパートがリンクされている場合、最下位付近のパートに関係する問題によって、チェーン全体が拒否されることがあります。 エラーの根本原因が必ずしも明らかであるとは限らないため、このような問題は診断が難しい場合があります。 問題解決に役立てるために、 `/causes` アクションを使用できます。このアクションでは、拒否の連鎖の根本原因を見つけることを試みます。  
  
 前の例で `/causes` アクションを使用すると、 `ChainOne`に関する情報だけが示されます。このパートのインポートが満たされなかったことが、 `AddIn`が拒否された根本原因であるためです。 `/causes` アクションは、通常のオプションと `/verbose` オプションの両方で使用できます。  
  
> [!NOTE]
> ほとんどの場合、連鎖するエラーの根本原因を Mefx で診断できます。 ただし、パートがプログラムによってコンテナーに追加される場合、階層コンテナーが関係している場合、またはカスタムの `ExportProvider` 実装が関係している場合には、Mefx によって原因を診断することができません これらの状況では一般にエラーの診断が難しいため、できるだけ避けることをお勧めします。  
  
<a name="white_lists"></a>   
## <a name="white-lists"></a>ホワイト リスト  
 `/whitelist` オプションでは、拒否されることが予想されるパートの一覧を示すテキスト ファイルを指定できます。 予期されない拒否にはフラグが設定されます。 これは、一部の依存関係が見つからない不完全なライブラリまたはサブライブラリを分析する際に役立ちます。 `/whitelist` オプションは、 `/rejected` アクションまたは `/causes` アクションに適用できます。  
  
 "ClassLibrary1.ChainOne" というテキストが含まれた test.txt という名前のファイルがあるとします。 前の例で、 `/rejected` アクションに `/whitelist` オプションを指定して実行すると、次の出力が生成されます。  
  
```console
mefx /file:ClassLibrary1.dll /rejected /whitelist:test.txt  
[Unexpected] ClassLibrary1.AddIn  
ClassLibrary1.ChainOne  
```
