---
title: -pdb (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /pdb
helpviewer_keywords:
- -pdb compiler option [C#]
- pdb compiler option [C#]
- /pdb compiler option [C#]
ms.assetid: e9d0f96a-5b75-45d6-9765-92538dd5f823
ms.openlocfilehash: 3081f4716e8cd858d789db6050e635af941aa05c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69602566"
---
# <a name="-pdb-c-compiler-options"></a>-pdb (C# コンパイラ オプション)
**-pdb** コンパイラ オプションは、デバッグ シンボル ファイルの場所と名前を指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-pdb:filename  
```  
  
## <a name="arguments"></a>引数  
 `filename`  
 デバッグ シンボル ファイルの名前と場所です。  
  
## <a name="remarks"></a>解説  
 [-debug (C# コンパイラ オプション)](./debug-compiler-option.md) を指定すると、コンパイラは、コンパイラが出力ファイル (.exe または .dll) を作成するのと同じディレクトリに、出力ファイルと同じ名前のファイル名で .pdb ファイルを作成します。  
  
 **-pdb** では、.pdb ファイルに対し、既定以外のファイル名と場所を指定することができます。  
  
 このコンパイラ オプションは、Visual Studio 開発環境で設定することはできません。また、プログラムで変更することもできません。  
  
## <a name="example"></a>例  
 `t.cs` をコンパイルして、tt.pdb と呼ばれる .pdb ファイルを作成します。  
  
```console  
csc -debug -pdb:tt t.cs  
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
