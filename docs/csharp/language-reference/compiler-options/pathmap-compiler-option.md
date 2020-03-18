---
title: -pathmap (C# コンパイラ オプション)
ms.date: 05/16/2018
f1_keywords:
- /pathmap
helpviewer_keywords:
- -pathmap compiler option [C#]
- pathmap compiler option [C#]
- /pathmap compiler option [C#]
ms.openlocfilehash: 48e96d2ec2ccbea83d573c0eb3630b1591c407a9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69606630"
---
# <a name="-pathmap-c-compiler-options"></a>-pathmap (C# コンパイラ オプション)

**-pathmap** コンパイラ オプションは、コンパイラによるソース パス名出力への物理パスのマップ方法を指定します。

## <a name="syntax"></a>構文

```console
-pathmap:path1=sourcePath1,path2=sourcePath2
```

## <a name="arguments"></a>引数

 `path1` 現在の環境でのソース ファイルへの完全なパス

 `sourcePath1` 出力ファイルにおいて `path1` の代わりに使用されるソース パス。

複数のマップされるソース パスを指定するには、コンマで区切ります。

## <a name="remarks"></a>解説

コンパイラは、次の理由でその出力にソース パスを書き込みます。

1. <xref:System.Runtime.CompilerServices.CallerFilePathAttribute> が省略可能なパラメーターに適用される場合、引数はソース パスに置き換えられます。
1. ソース パスは、PDB ファイルに埋め込まれます。
1. PDB ファイルのパスは、PE (ポータブル実行可能) ファイルに埋め込まれます。

このオプションは、コンパイラが実行されるコンピューター上の各物理パスを、出力ファイルに書き込まれる必要がある対応するパスにマップします。

## <a name="example"></a>例

ディレクトリ `t.cs`C:**work\\tests\\ 内の**  をコンパイルし、出力ではそのディレクトリを **\publish** にマップします。

```console
csc -pathmap:C:\work\tests=\publish t.cs
```

## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
