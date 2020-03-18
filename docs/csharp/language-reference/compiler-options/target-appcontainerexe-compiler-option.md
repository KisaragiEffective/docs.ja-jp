---
title: -target:appcontainerexe (C# コンパイラ オプション)
ms.date: 07/20/2015
ms.assetid: e7e62229-23ea-4e53-bef5-380d951bf95f
ms.openlocfilehash: 64661e72f9efe190606cadd93558678cb849e8cc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74204533"
---
# <a name="-targetappcontainerexe-c-compiler-options"></a>-target:appcontainerexe (C# コンパイラ オプション)
**-target:appcontainerexe** コンパイラ オプションを使用すると、アプリケーション コンテナーで実行する必要のある Windows 実行可能ファイル (.exe) がコンパイラによって作成されます。 このオプションは [-target:winexe](./target-winexe-compiler-option.md) に相当しますが、Windows 8.x Store アプリ用に設計されています。  
  
## <a name="syntax"></a>構文  
  
```console  
-target:appcontainerexe  
```  
  
## <a name="remarks"></a>解説  
 アプリケーション コンテナーでのアプリケーションの実行を要求するために、このオプションは、[Portable Executable](/windows/desktop/Debug/pe-format) (PE) ファイルでビットを設定します。 このビットが設定されている場合、CreateProcess メソッドがアプリケーション コンテナー外の実行可能ファイルを起動しようとすると、エラーが発生します。  
  
 [-out](./out-compiler-option.md) オプションを使用しない限り、出力ファイル名は [Main](../../programming-guide/main-and-command-args/index.md) メソッドを含む入力ファイルと同じになります。  
  
 コマンド ラインで指定すると、次の **-out** オプションまたは **-target** オプションまでのすべてのファイルが、実行可能ファイルの作成に使用されます。  
  
### <a name="to-set-this-compiler-option-in-the-ide"></a>IDE でこのコンパイラ オプションを設定するには  
  
1. **ソリューション エクスプローラー**で、プロジェクトのショートカット メニューを開き、 **[プロパティ]** を選択します。  
  
2. **[アプリケーション]** タブの **[出力の種類]** ボックスの一覧で、 **[Windows ストア アプリ]** をクリックします。  
  
     このオプションは、Windows 8.x Store アプリ テンプレートでのみ使用できます。  
  
 このコンパイラ オプションをプログラムで設定する方法については、「<xref:VSLangProj80.ProjectProperties3.OutputType%2A>」を参照してください。  
  
## <a name="example"></a>例  
 次のコマンドは、アプリケーション コンテナーでのみ実行できる Windows 実行可能ファイルに `filename.cs` をコンパイルします。  
  
```console  
csc -target:appcontainerexe filename.cs  
```  
  
## <a name="see-also"></a>参照

- [-target (C# コンパイラ オプション)](./target-compiler-option.md)
- [-target:winexe (C# コンパイラ オプション)](./target-winexe-compiler-option.md)
- [C# コンパイラ オプション](./index.md)
