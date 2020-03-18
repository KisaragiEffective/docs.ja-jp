---
title: '@ (C# コンパイラ オプション)'
ms.date: 07/20/2015
f1_keywords:
- '@'
helpviewer_keywords:
- response files, specifying for compilation [C#]
- '@ compiler option'
ms.assetid: dda4fa9f-a02c-400f-8b6a-d58834e13d7f
ms.openlocfilehash: d8e5c0ec148754c3e4cebfa32ad9f44a0bb0119e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "70202914"
---
# <a name="-c-compiler-options"></a>@ (C# コンパイラ オプション)
@ オプションを使用すると、コンパイラ オプションおよびコンパイルするソース コード ファイルを含むファイルを指定できます。  
  
## <a name="syntax"></a>構文  
  
```console  
@response_file  
```  
  
## <a name="arguments"></a>引数  
 `response_file`  
 コンパイラ オプションやコンパイルするソース コード ファイルの一覧を含むファイルです。  
  
## <a name="remarks"></a>解説  
 コンパイラ オプションとソース コード ファイルは、コマンド ラインで指定した場合と同じように、コンパイラによって処理されます。  
  
 コンパイル時に複数の応答ファイルを指定するには、複数の応答ファイル オプションを指定します。 次に例を示します。  
  
```console  
@file1.rsp @file2.rsp  
```  
  
 応答ファイルでは、複数のコンパイラ オプションとソース コード ファイルを 1 行に記述できます。 1 つのコンパイラ オプションは 1 行に指定する必要があり、複数行にわたって指定できません。 応答ファイルには、シャープ記号 (#) で始まるコメントを記述できます。  
  
 応答ファイルでのコンパイラ オプションの指定方法は、コマンド ラインでのコンパイラ オプションの指定方法と同じです。 詳細については、[コマンド ラインからのビルド](./how-to-set-environment-variables-for-the-visual-studio-command-line.md) に関するページを参照してください。  
  
 コンパイラは、検出した順にコマンド オプションを処理します。 このため、コマンド ライン引数によって、応答ファイルで先に指定したオプションをオーバーライドできます。 逆に、応答ファイルのオプションが、コマンド ラインや他の応答ファイルで先に指定したオプションをオーバーライドすることもあります。  
  
 C# では、csc.exe ファイルと同じディレクトリに csc.rsp ファイルがあります。 csc.rsp の詳細については、「[-noconfig](./noconfig-compiler-option.md)」を参照してください。  
  
 このコンパイラ オプションは、Visual Studio 開発環境で設定することはできません。また、プログラムで変更することもできません。  
  
## <a name="example"></a>例  
 サンプルの応答ファイルの一部を次に示します。  
  
```console  
# build the first output file  
-target:exe -out:MyExe.exe source1.cs source2.cs  
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
