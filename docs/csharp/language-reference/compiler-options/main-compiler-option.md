---
title: -main (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /main
helpviewer_keywords:
- -main compiler option [C#]
- main compiler option [C#]
- /main compiler option [C#]
ms.assetid: 975cf4d5-36ac-4530-826c-4aad0c7f2049
ms.openlocfilehash: 6c842abc1423e7ee0d98b71392e02410c6cf9172
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69602726"
---
# <a name="-main-c-compiler-options"></a>-main (C# コンパイラ オプション)
このオプションは、**Main** メソッドを含むクラスが複数ある場合に、プログラムへのエントリ ポイントを含むクラスを指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-main:class  
```  
  
## <a name="arguments"></a>引数  
 `class`  
 **Main** メソッドを含む型です。  
 指定するクラス名は完全に修飾する必要があります。クラスを含む完全な名前空間を指定し、そのあとにクラス名を続ける必要があります。 たとえば、`Main` メソッドが、`Program` 名前空間の `MyApplication.Core` クラス内に置かれている場合、コンパイラ オプションは `-main:MyApplication.Core.Program` とする必要があります。
  
## <a name="remarks"></a>解説  
 [Main](../../programming-guide/main-and-command-args/index.md) メソッドを使用した型がコンパイル対象に 2 つ以上含まれている場合には、プログラムへのエントリ ポイントとして使用する **Main** メソッドがどの型に含まれているかを指定できます。  
  
 このオプションは、.exe ファイルをコンパイルする際に使用されます。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1. プロジェクトの **[プロパティ]** ページを開きます。  
  
2. **[アプリケーション]** プロパティ ページをクリックします。  
  
3. **[スタートアップ オブジェクト]** プロパティを変更します。  
  
     このコンパイラ オプションをプログラムによって設定するには、「<xref:VSLangProj80.ProjectProperties3.StartupObject%2A>」を参照してください。  
  
## <a name="example"></a>例  
 `t2.cs`Main`t3.cs` メソッドが **にあることを指定して、** と `Test2` をコンパイルします。  
  
```console  
csc t2.cs t3.cs -main:Test2  
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
