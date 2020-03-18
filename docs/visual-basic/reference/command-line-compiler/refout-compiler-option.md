---
title: -refout
ms.date: 03/16/2018
f1_keywords:
- /refout
helpviewer_keywords:
- refout compiler option [Visual Basic]
- /refout compiler option [Visual Basic]
- -refout compiler option [Visual Basic]
ms.openlocfilehash: 3649a24a52cc6a448ea7cf4d850915adf02147fb
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348650"
---
# <a name="-refout-visual-basic"></a>-refout (Visual Basic)

**-refout** オプションは、参照アセンブリを出力するファイル パスを指定します。

[!INCLUDE[compiler-options](~/includes/compiler-options.md)]

## <a name="syntax"></a>構文

```console
-refout:filepath
```

## <a name="arguments"></a>引数

`filepath`  
参照アセンブリのパスとファイル名。 通常、プライマリアセンブリのサブフォルダーに存在する必要があります。 (MSBuild で使用される) 推奨規則は、プライマリ アセンブリに相対する "ref/" サブ フォルダー内に参照アセンブリを配置することです。 `filepath` 内のすべてのフォルダーが存在している必要があります。コンパイラでは、これらは作成されません。

## <a name="remarks"></a>コメント

Visual Basic では、バージョン15.3 以降の `-refout` スイッチがサポートされています。

参照アセンブリは、ライブラリのパブリック API サーフェイスを表すために必要最小限のメタデータのみを含む特殊なアセンブリです。 これには、ビルド ツールでアセンブリを参照するときに重要なすべてのメンバーの宣言が含まれます。ただし、すべてのメンバーの実装と、その API コントラクトに影響を与えないプライベート メンバーの宣言は除外されます。 詳細については、.NET のガイドの「[参照アセンブリ](../../../standard/assembly/reference-assemblies.md)」を参照してください。

`-refout` オプションと [`-refonly`](refonly-compiler-option.md) オプションは同時に指定できません。

## <a name="see-also"></a>関連項目

- [/refonly](refonly-compiler-option.md)
- [Visual Basic コマンドラインコンパイラ](index.md)
- [コンパイルコマンドラインのサンプル](sample-compilation-command-lines.md)
