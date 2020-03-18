---
title: -doc (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- FileProperties.BuildAction
- /doc
helpviewer_keywords:
- comments, C# code
- XML documentation [C#], comments in source files
- doc compiler option [C#]
- Visual C#, XML documentation for
- -doc compiler option [C#]
- /doc compiler option [C#]
ms.assetid: 849eea59-c936-4311-bad8-d07404480f2a
ms.openlocfilehash: 01ea71f3de9e30abe25184e38a59f3707b54bd5a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73422973"
---
# <a name="-doc-c-compiler-options"></a>-doc (C# コンパイラ オプション)
**-doc** オプションを使用すると、XML ファイル内にドキュメント コメントを含めることができます。  
  
## <a name="syntax"></a>構文  
  
```console  
-doc:file  
```  
  
## <a name="arguments"></a>引数  
 `file`  
 XML の出力ファイル。コンパイルのソース コード ファイルのコメントが入力されます。  
  
## <a name="remarks"></a>解説  
 ソース コード ファイルで、次の項目の前にあるドキュメント コメントを処理し、XML ファイルに追加できます。  
  
- [クラス](../keywords/class.md)、[デリゲート](../builtin-types/reference-types.md#the-delegate-type)、[インターフェイス](../keywords/interface.md)などのユーザー定義型  
  
- フィールド、[イベント](../keywords/event.md)、[プロパティ](../../programming-guide/classes-and-structs/using-properties.md)、メソッドなどのメンバー  
  
 Main を含むソース コード ファイルが最初に XML に出力されます。  
  
 生成された .xml ファイルで [IntelliSense](/visualstudio/ide/using-intellisense) 機能を使用するには、サポートするアセンブリの名前と .xml ファイル名を同じにして、その .xml ファイルをアセンブリと同じディレクトリに置きます。 これで、アセンブリが Visual Studio プロジェクトで参照されると、.xml ファイルも同様に検出されます。 詳細については、[コード コメントの追加](/visualstudio/ide/reference/generate-xml-documentation-comments)に関するページを参照してください。  
  
 [-target:module](./target-module-compiler-option.md) でコンパイルしない限り、`file` には \<assembly>\</assembly> タグが追加されます。コンパイルの出力ファイルのアセンブリ マニフェストを含むファイルの名前が指定されます。  
  
> [!NOTE]
> -doc オプションは、すべての入力ファイル (プロジェクトの設定で設定された場合、そのプロジェクト内のすべてのファイル) に適用されます。 特定のファイルまたはコードの特定のセクションについて、ドキュメントのコメントに関する警告を無効にするには、[#pragma warning](../preprocessor-directives/preprocessor-pragma-warning.md) を使用します。  
  
 コードのコメントからドキュメントを生成する方法については、「[ドキュメント コメント用の推奨タグ](../../programming-guide/xmldoc/recommended-tags-for-documentation-comments.md)」を参照してください。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1. プロジェクトの **[プロパティ]** ページを開きます。  
  
2. **[ビルド]** タブをクリックします。  
  
3. **[XML ドキュメント ファイル]** プロパティを変更します。  
  
 このコンパイラ オプションをプログラムで設定する方法については、「 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.DocumentationFile%2A>」をご覧ください。  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
