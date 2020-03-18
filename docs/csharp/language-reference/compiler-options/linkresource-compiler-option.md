---
title: -linkresource (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /linkresource
helpviewer_keywords:
- /linkresource compiler option [C#]
- linkres compiler option [C#]
- /linkres compiler option [C#]
- -linkres compiler option [C#]
- -linkresource compiler option [C#]
- linkresource compiler option [C#]
ms.assetid: 440c26c2-77c1-4811-a0a3-57cce3f5fc96
ms.openlocfilehash: 41af8e0ba8ffebd07d3cb1d2bc5fbc04b8cd595d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173732"
---
# <a name="-linkresource-c-compiler-options"></a>-linkresource (C# コンパイラ オプション)
.NET Framework のリソースへのリンクを出力ファイルに作成します。 リソース ファイルが出力ファイルに追加されることはありません。 これに対し、[-resource](./resource-compiler-option.md) オプションはリソース ファイルを出力ファイルに埋め込みます。  
  
## <a name="syntax"></a>構文  
  
```console  
-linkresource:filename[,identifier[,accessibility-modifier]]  
```  
  
## <a name="arguments"></a>引数  
 `filename`  
 アセンブリからリンクする .NET Framework のリソース ファイル。  
  
 `identifier` (省略可)  
 リソースの論理名。リソースを読み込むために使われる名前です。 既定値は、ファイルの名前です。  
  
 `accessibility-modifier` (省略可)  
 リソースのアクセシビリティ。パブリックまたはプライベートです。 既定値はパブリックです。  
  
## <a name="remarks"></a>解説  
 既定では、リンクされたリソースは、C# コンパイラで作成されるときにアセンブリ内でパブリックになります。 リソースをプライベートにするには、アクセシビリティ修飾子として `private` を指定します。 `public` または `private` 以外の他の修飾子は許可されません。  
  
 **-linkresource** には、[-target:module](./target-compiler-option.md) 以外のいずれかの **-target** オプションが必要です。  
  
 `filename` が [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) や開発環境などで作成された .NET Framework リソース ファイルである場合は、<xref:System.Resources> 名前空間のメンバーを使ってそのファイルにアクセスできます。 詳細については、<xref:System.Resources.ResourceManager?displayProperty=nameWithType> を参照してください。 それ以外のすべてのリソースに対しては、`GetManifestResource` クラスの <xref:System.Reflection.Assembly> メソッドを使用して、実行時にリソースにアクセスします。  
  
 `filename` で指定するファイルはどのような形式でもかまいません。 たとえば、ネイティブ DLL をアセンブリの一部にすることで、グローバル アセンブリ キャッシュにインストールして、アセンブリ内のマネージド コードからアクセスできるようにすることができます。 以下の例の 2 番目で、その方法を示します。 同じことをアセンブリ リンカーで行うことができます。 以下の例の 3 番目で、その方法を示します。 詳しくは、「[Al.exe (アセンブリ リンカー)](../../../framework/tools/al-exe-assembly-linker.md)」および「[アセンブリとグローバル アセンブリ キャッシュの使用](../../../framework/app-domains/working-with-assemblies-and-the-gac.md)」をご覧ください。  
  
 **-linkres** は **-linkresource** の省略形式です。  
  
 このコンパイラ オプションは Visual Studio では使用できず、プログラムで変更することはできません。  
  
## <a name="example"></a>例  
 `in.cs` をコンパイルして、リソース ファイル `rf.resource` にリンクします。  
  
```console  
csc -linkresource:rf.resource in.cs  
```  
  
## <a name="example"></a>例  
 `A.cs` をコンパイルして DLL を作成し、ネイティブ DLL N.dll にリンクして、出力をグローバル アセンブリ キャッシュ (GAC) に配置します。 この例では、A.dll と N.dll の両方を GAC に置きます。  
  
```console  
csc -linkresource:N.dll -t:library A.cs  
gacutil -i A.dll  
```  
  
## <a name="example"></a>例  
 この例では、前の例と同じことを行いますが、アセンブリ リンカー オプションを使います。  
  
```console  
csc -t:module A.cs  
al -out:A.dll A.netmodule -link:N.dll
gacutil -i A.dll  
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [Al.exe (アセンブリ リンカー)](../../../framework/tools/al-exe-assembly-linker.md)
- [アセンブリとグローバル アセンブリ キャッシュの使用](../../../framework/app-domains/working-with-assemblies-and-the-gac.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
