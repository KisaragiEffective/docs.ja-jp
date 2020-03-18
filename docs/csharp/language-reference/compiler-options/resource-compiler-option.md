---
title: -resource (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /resource
helpviewer_keywords:
- -resource compiler option [C#]
- /resource compiler option [C#]
- -res compiler option [C#]
- /res compiler option [C#]
- res compiler option [C#]
- resource compiler option [C#]
ms.assetid: 5212666e-98ab-47e4-a497-b5545ab15c7f
ms.openlocfilehash: e14bf59f5922a918b627af22c052c8efd9081e84
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69602530"
---
# <a name="-resource-c-compiler-options"></a>-resource (C# コンパイラ オプション)
指定されたリソースを出力ファイルに埋め込みます。  
  
## <a name="syntax"></a>構文  
  
```console  
-resource:filename[,identifier[,accessibility-modifier]]  
```  
  
## <a name="arguments"></a>引数  
 `filename`  
 出力ファイルに埋め込む .NET Framework リソース ファイルです。  
  
 `identifier` (省略可)  
 リソースの論理名。リソースを読み込むために使われる名前です。 既定値はファイル名です。  
  
 `accessibility-modifier` (省略可)  
 リソースのアクセシビリティ。パブリックまたはプライベートです。 既定値はパブリックです。  
  
## <a name="remarks"></a>解説  
 [-linkresource](./linkresource-compiler-option.md) を使用すると、リソースがアセンブリにリンクされ、リソース ファイルは出力ファイルに追加されません。  
  
 既定では、リソースは、C# コンパイラを使用して作成されるときにアセンブリ内でパブリックになります。 リソースをプライベートにするには、アクセシビリティ修飾子として `private` を指定します。 `public` と `private` 以外のアクセシビリティは使用できません。  
  
 `filename` が [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) や開発環境などで作成された .NET Framework リソース ファイルである場合は、<xref:System.Resources> 名前空間のメンバーを使ってそのファイルにアクセスできます。 詳細については、<xref:System.Resources.ResourceManager?displayProperty=nameWithType> を参照してください。 それ以外のすべてのリソースに対しては、`GetManifestResource` クラスの <xref:System.Reflection.Assembly> メソッドを使用して、実行時にリソースにアクセスします。  
  
 **-res** は **-resource** の省略形です。  
  
 出力ファイルにおけるリソースの順序は、コマンド ラインでの指定順序に基づいて決定されます。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1. プロジェクトにリソース ファイルを追加します。  
  
2. **ソリューション エクスプローラー**で、埋め込むファイルを選択します。  
  
3. 選択したファイルの **[プロパティ]** ウィンドウで、 **[ビルド アクション]** を選択します。  
  
4. **[ビルド アクション]** を **[埋め込まれたリソース]** に設定します。  
  
 このコンパイラ オプションをプログラムで設定する方法については、「<xref:VSLangProj80.FileProperties2.BuildAction%2A>」を参照してください。  
  
## <a name="example"></a>例  
 `in.cs` をコンパイルし、リソース ファイル `rf.resource` をアタッチします。  
  
```console  
csc -resource:rf.resource in.cs  
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
