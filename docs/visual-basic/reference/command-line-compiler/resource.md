---
title: -resource
ms.date: 03/13/2018
helpviewer_keywords:
- /resource compiler option [Visual Basic]
- -resource compiler option [Visual Basic]
- /res compiler option [Visual Basic]
- res compiler option [Visual Basic]
- -res compiler option [Visual Basic]
- resource compiler option [Visual Basic]
ms.assetid: eee2f227-91f2-4f2b-a9d6-1c51c5320858
ms.openlocfilehash: a781d543dd32ffb3d0ac0b11c544dbfd8cd5d806
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348568"
---
# <a name="-resource-visual-basic"></a>-resource (Visual Basic)
マネージド リソースをアセンブリに埋め込みます。  
  
## <a name="syntax"></a>構文  
  
```console  
-resource:filename[,identifier[,public|private]]  
```

または  

```console
-res:filename[,identifier[,public|private]]  
```  
  
## <a name="arguments"></a>引数  
  
|用語|Definition|  
|---|---|  
|`filename`|必須。 出力ファイルに埋め込むリソースファイルの名前。 既定では、`filename` はアセンブリで公開されています。 ファイル名にスペースが含まれている場合は、ファイル名を引用符 ("") で囲みます。|  
|`identifier`|省略可。 リソースの論理名。読み込みに使用される名前。 既定値は、ファイルの名前です。 必要に応じて、次のように、アセンブリマニフェストでリソースがパブリックであるかプライベートであるかを指定できます。 `-res:filename.res, myname.res, public`|  
  
## <a name="remarks"></a>コメント  
 リソースファイルを出力ファイルに配置せずに、リソースをアセンブリにリンクするには、`-linkresource` を使用します。  
  
 たとえば、 [resgen.exe (リソースファイルジェネレーター)](../../../framework/tools/resgen-exe-resource-file-generator.md)や開発環境などによって作成された .NET Framework のリソースファイル `filename` 場合は、<xref:System.Resources> 名前空間のメンバーを使用してアクセスできます (詳細については、「<xref:System.Resources.ResourceManager>」を参照してください)。 実行時に他のすべてのリソースにアクセスするには、次のいずれかの方法を使用します。 <xref:System.Reflection.Assembly.GetManifestResourceInfo%2A>、<xref:System.Reflection.Assembly.GetManifestResourceNames%2A>、または <xref:System.Reflection.Assembly.GetManifestResourceStream%2A>。  
  
 `-resource` の省略形は `-res` です。  
  
 Visual Studio IDE で `-resource` を設定する方法の詳細については、「[アプリケーションリソースの管理 (.NET)](/visualstudio/ide/managing-application-resources-dotnet)」を参照してください。  
  
## <a name="example"></a>例  
 次のコードでは、`In.vb` をコンパイルし、リソースファイル `Rf.resource`をアタッチします。  
  
```console
vbc -res:rf.resource in.vb  
```  
  
## <a name="see-also"></a>参照

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-win32resource](../../../visual-basic/reference/command-line-compiler/win32resource.md)
- [-linkresource (Visual Basic)](../../../visual-basic/reference/command-line-compiler/linkresource.md)
- [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
