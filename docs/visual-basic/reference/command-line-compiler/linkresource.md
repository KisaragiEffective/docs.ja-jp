---
title: -linkresource
ms.date: 03/10/2018
helpviewer_keywords:
- /linkresource compiler option [Visual Basic]
- -linkresource compiler option [Visual Basic]
- linkresource compiler option [Visual Basic]
- /linkres compiler option [Visual Basic]
- linkres compiler option [Visual Basic]
- -linkres compiler option [Visual Basic]
ms.assetid: cf4dcad8-17b7-404c-9184-29358aa05b15
ms.openlocfilehash: 0315645eccdc899ac9cf4d0be105297e1fa2a4c4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74335482"
---
# <a name="-linkresource-visual-basic"></a>-linkresource (Visual Basic)
マネージド リソースへのリンクを作成します。  
  
## <a name="syntax"></a>構文  
  
```console  
-linkresource:filename[,identifier[,public|private]]  
```

または  

```console
-linkres:filename[,identifier[,public|private]]  
```  
  
## <a name="arguments"></a>引数  
 `filename`  
 必須。 アセンブリにリンクするリソースファイル。 ファイル名にスペースが含まれている場合は、名前を引用符 ("") で囲みます。  
  
 `identifier`  
 省略可。 リソースの論理名。 リソースの読み込みに使用される名前。 既定値は、ファイルの名前です。 必要に応じて、ファイルがアセンブリマニフェスト内でパブリックであるかプライベートであるかを指定できます (例: `-linkres:filename.res,myname.res,public`)。 既定では、`filename` はアセンブリで公開されています。  
  
## <a name="remarks"></a>コメント  
 `-linkresource` オプションでは、リソースファイルは出力ファイルに埋め込まれません。これを行うには、`-resource` オプションを使用します。  
  
 `-linkresource` オプションでは、`-target:module`以外の `-target` オプションのいずれかが必要です。  
  
 たとえば、 [resgen.exe (リソースファイルジェネレーター)](../../../framework/tools/resgen-exe-resource-file-generator.md)や開発環境などによって作成された .NET Framework のリソースファイル `filename` 場合は、<xref:System.Resources> 名前空間のメンバーを使用してアクセスできます。 (詳細については、「<xref:System.Resources.ResourceManager>」を参照してください)。実行時に他のすべてのリソースにアクセスするには、<xref:System.Reflection.Assembly> クラスの `GetManifestResource` で始まるメソッドを使用します。  
  
 ファイル名には任意のファイル形式を使用できます。 たとえば、ネイティブ DLL をアセンブリの一部にすることで、グローバル アセンブリ キャッシュにインストールして、アセンブリ内のマネージド コードからアクセスできるようにすることができます。  
  
 `-linkresource` の省略形は `-linkres` です。  
  
> [!NOTE]
> `-linkresource` オプションは、Visual Studio 開発環境では使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。  
  
## <a name="example"></a>例  
 次のコードでは、`in.vb` とリソースファイル `rf.resource`へのリンクをコンパイルします。  
  
```console  
vbc -linkresource:rf.resource in.vb  
```  
  
## <a name="see-also"></a>参照

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
- [-resource (Visual Basic)](../../../visual-basic/reference/command-line-compiler/resource.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
