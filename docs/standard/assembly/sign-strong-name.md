---
title: '方法: 厳密な名前でアセンブリに署名する'
ms.date: 08/20/2019
helpviewer_keywords:
- strong-named assemblies, signing with strong names
- signing assemblies
- assemblies [.NET Framework], signing
- assemblies [.NET Framework], strong-named
ms.assetid: 2c30799a-a826-46b4-a25d-c584027a6c67
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 9998e69e8bf1505bcfc7a9103e9d89616dad9633
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160314"
---
# <a name="how-to-sign-an-assembly-with-a-strong-name"></a>方法: 厳密な名前でアセンブリに署名する

> [!NOTE]
> .NET Core では厳密な名前のアセンブリがサポートされており、.NET Core ライブラリ内のすべてのアセンブリは署名されていますが、ほとんどのサードパーティ アセンブリには厳密な名前は必要ありません。 詳細については、GitHub の「[厳密な名前の署名](https://github.com/dotnet/runtime/blob/master/docs/project/strong-name-signing.md)」を参照してください。

厳密な名前でアセンブリに署名するには、いくつかの方法があります。  
  
- Visual Studio でプロジェクトの **[プロパティ]** ダイアログ ボックスにある **[署名]** タブを使用する。 これは、厳密な名前でアセンブリに署名するための最も簡単で便利な方法です。  
  
- [アセンブリ リンカー (Al.exe)](../../framework/tools/al-exe-assembly-linker.md) を使用して、.NET Framework のコード モジュール ( *.netmodule* ファイル) をキー ファイルにリンクする。  
  
- アセンブリ属性を使用して、厳密な名前情報をコードに挿入する。 使用するキー ファイルが配置されている場所に応じて、 <xref:System.Reflection.AssemblyKeyFileAttribute> または <xref:System.Reflection.AssemblyKeyNameAttribute> 属性を使用できます。  
  
- コンパイラ オプションを使用する。  
  
 厳密な名前でアセンブリに署名するには、暗号キー ペアが必要です。 キー ペアの作成の詳細については、「[方法:公開キーと秘密キーのキー ペアを作成する](create-public-private-key-pair.md)」を参照してください。  
  
## <a name="create-and-sign-an-assembly-with-a-strong-name-by-using-visual-studio"></a>Visual Studio を使用してアセンブリを作成し、厳密な名前でそのアセンブリに署名する  
  
1. **ソリューション エクスプローラー**で、プロジェクトのショートカット メニューを開き、 **[プロパティ]** を選択します。  
  
2. **[署名]** タブを選択します。  
  
3. **[アセンブリの署名]** ボックスを選択します。  
  
4. **[厳密な名前のキー ファイルを選択してください]** ボックスで **[参照]** をクリックし、キー ファイルに移動します。 新しいキー ファイルを作成するには、 **[新規作成]** を選択し、 **[厳密な名前キーの作成]** ダイアログ ボックスでその名前を入力します。  
  
> [!NOTE]
> [アセンブリの遅延署名](delay-sign.md)を行うには、公開キー ファイルを選択します。  
  
### <a name="create-and-sign-an-assembly-with-a-strong-name-by-using-the-assembly-linker"></a>アセンブリ リンカーを使用してアセンブリを作成し、厳密な名前でそのアセンブリに署名する  
  
[Visual Studio 用開発者コマンド プロンプト](../../framework/tools/developer-command-prompt-for-vs.md)で次のコマンドを入力します。  

**al** **/out:** \<*assemblyName*>  *\<moduleName>* **/keyfile:** \<*keyfileName*>  

この場合、  

- *assemblyName*は、アセンブリ リンカーが生成する、厳密な名前で署名されたアセンブリ ( *.dll* ファイルまたは *.exe* ファイル) の名前です。  
  
- *moduleName* は、1 つ以上の型を含む、.NET Framework コード モジュール ( *.netmodule* ファイル) の名前です。 C# または Visual Basic で `/target:module` スイッチを使ってコードをコンパイルすることにより、 *.netmodule* ファイルを作成できます。
  
- *keyfileName* は、キー ペアを格納しているコンテナーまたはファイルの名前です。 アセンブリ リンカーは、現在のディレクトリとの関連で相対パスを解釈します。  

次の例では、キー ファイル *sgKey.snk* を使用して、厳密な名前でアセンブリ *MyAssembly.dll* に署名します。  

```console
al /out:MyAssembly.dll MyModule.netmodule /keyfile:sgKey.snk  
```  
  
このツールの詳細については、「 [アセンブリ リカー](../../framework/tools/al-exe-assembly-linker.md)」を参照してください。  
  
## <a name="sign-an-assembly-with-a-strong-name-by-using-attributes"></a>属性を使用して厳密な名前でアセンブリに署名する  
  
1. <xref:System.Reflection.AssemblyKeyFileAttribute?displayProperty=nameWithType> または <xref:System.Reflection.AssemblyKeyNameAttribute> 属性をソース コード ファイルに追加して、厳密な名前でアセンブリに署名するときに使用するキー ペアを格納するファイルまたはコンテナーの名前を指定します。  

2. ソース コード ファイルを通常どおりにコンパイルします。  

   > [!NOTE]
   > C# および Visual Basic コンパイラは、ソース コードで <xref:System.Reflection.AssemblyKeyFileAttribute> または <xref:System.Reflection.AssemblyKeyNameAttribute> 属性が見つかった場合、コンパイラ警告を発行します (CS1699 と BC41008)。 この警告は無視してかまいません。  

次の例は、*keyfile.snk* という名前のキー ファイルを指定して、<xref:System.Reflection.AssemblyKeyFileAttribute> 属性を使用します。このキー ファイルは、アセンブリがコンパイルされるディレクトリにあります。  

```cpp
[assembly:AssemblyKeyFileAttribute("keyfile.snk")];
```

```csharp
[assembly:AssemblyKeyFileAttribute("keyfile.snk")]
```

```vb
<Assembly:AssemblyKeyFileAttribute("keyfile.snk")>
```

ソース ファイルのコンパイル時に、アセンブリに遅延署名することもできます。 詳細については、「[アセンブリへの遅延署名](delay-sign.md)」を参照してください。  

## <a name="sign-an-assembly-with-a-strong-name-by-using-the-compiler"></a>コンパイラを使用して厳密な名前でアセンブリに署名する  

C# と Visual Basic では `/keyfile` または `/delaysign` コンパイラ オプションを使用し、C++ では `/KEYFILE` または `/DELAYSIGN` リンカー オプションを使用して、ソース コード ファイルをコンパイルします。 オプション名の後に、コロンと、キー ファイルの名前を追加します。 コマンド ライン コンパイラを使用する場合は、ソース コード ファイルを格納するディレクトリにキー ファイルをコピーできます。  

遅延署名については、「[アセンブリへの遅延署名](delay-sign.md)」を参照してください。  

次の例は、C# コンパイラを使用し、アセンブリ *UtilityLibrary.dll* にキー ファイル *sgKey.snk* を使用して厳密な名前で署名します。  

```cmd
csc /t:library UtilityLibrary.cs /keyfile:sgKey.snk  
```  

## <a name="see-also"></a>関連項目

- [厳密な名前付きアセンブリの作成と使用](create-use-strong-named.md)
- [方法: 公開キーと秘密キーのキー ペアを作成する](create-public-private-key-pair.md)
- [Al.exe (アセンブリ リンカー)](../../framework/tools/al-exe-assembly-linker.md)
- [アセンブリへの遅延署名](delay-sign.md)
- [アセンブリおよびマニフェストへの署名の管理](/visualstudio/ide/managing-assembly-and-manifest-signing)
- [[署名] ページ (プロジェクト デザイナー)](/visualstudio/ide/reference/signing-page-project-designer)
