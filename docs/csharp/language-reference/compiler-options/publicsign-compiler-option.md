---
title: -publicsign (C# コンパイラ オプション)
ms.date: 05/15/2018
f1_keywords:
- /publicsign
helpviewer_keywords:
- -publicsign compiler option [C#]
- publicsign compiler option [C#]
- /publicsign compiler option [C#]
ms.openlocfilehash: de7d9c98b0f279b52bc93711c5b986a2b2e57215
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61662531"
---
# <a name="-publicsign-c-compiler-options"></a>-publicsign (C# コンパイラ オプション)

このオプションを指定すると、コンパイラは公開キーを適用しますが、実際にはアセンブリに署名しません。 **-publicsign** オプションは、ファイルが実際に署名されていることをランタイムに伝えるアセンブリのビットも設定します。

## <a name="syntax"></a>構文

```console
-publicsign
```

## <a name="arguments"></a>引数

[なし] :

## <a name="remarks"></a>解説

**-publicsign** オプションを使うには、[-keyfile](keyfile-compiler-option.md) または [-keycontainer](keycontainer-compiler-option.md) を使う必要があります。 **keyfile** または **keycontainer** オプションは、公開キーを指定します。

**-publicsign** オプションと **-delaysign** オプションは相互に排他的です。

"フェイク署名" または "OSS 署名" と呼ばれることもあり、公開署名には出力アセンブリ内の公開キーが含まれ、"署名済み" フラグは設定されますが、実際には秘密キーによるアセンブリの署名は行われません。 これは、リリースされている "完全に署名された" アセンブリと互換性を持ちながら、アセンブリの署名に使われた秘密キーへはアクセスできない、そのようなアセンブリをビルドするオープン ソース プロジェクトに役立ちます。 アセンブリが完全に署名されているかどうかをコンシューマーが実際に確認する必要があることはほとんどないので、このようなパブリックにビルドされたアセンブリは、完全に署名されたアセンブリが使われるほとんどすべてのシナリオにおいて使用可能です。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには

1. プロジェクトの **[プロパティ]** ページを開きます。
1. **[遅延署名のみ]** プロパティを変更します。

## <a name="see-also"></a>参照

- [C# コンパイラの -delaysign オプション](delaysign-compiler-option.md)
- [C# コンパイラの -keyfile オプション](keyfile-compiler-option.md)
- [C# コンパイラの -keycontainer オプション](keycontainer-compiler-option.md)
- [C# コンパイラ オプション](index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
