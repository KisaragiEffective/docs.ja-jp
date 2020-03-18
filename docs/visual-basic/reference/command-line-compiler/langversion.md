---
title: -langversion
ms.date: 03/10/2018
helpviewer_keywords:
- /langversion compiler option [Visual Basic]
- langversion compiler option [Visual Basic]
- -langversion compiler option [Visual Basic]
ms.assetid: 59b7b0c8-2dde-4e9b-94e7-0237f7e0bafb
ms.openlocfilehash: 72a5638a5c5364381ffd68604b0d44830d53f365
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344212"
---
# <a name="-langversion-visual-basic"></a>-langversion (Visual Basic)
指定された Visual Basic 言語バージョンに含まれている構文のみをコンパイラが受け入れるようにします。  
  
## <a name="syntax"></a>構文  
  
```console  
-langversion:version  
```  
  
## <a name="arguments"></a>引数  
 `version`  
 必須。 コンパイル中に使用される言語バージョン。 許容される値は、`9`、`10`、`11`、`12`、`14`、`15`、`15.3`、`15.5`、`default`、および `latest`です。

 `.0` をマイナーバージョン (`11.0`など) として使用して、整数値を指定することもできます。

 使用可能なすべての値の一覧を表示するには、コマンドラインで `-langversion:?` を指定します。  
  
## <a name="remarks"></a>コメント  
 `-langversion` オプションは、コンパイラが受け入れる構文を指定します。 たとえば、言語バージョンが9.0 であることを指定した場合、コンパイラはバージョン10.0 以降でのみ有効な構文に対してエラーを生成します。  
  
 このオプションは、異なるバージョンの .NET Framework を対象とするアプリケーションを開発する場合に使用できます。 たとえば、.NET Framework 3.5 を対象としている場合は、このオプションを使用して、言語バージョン10.0 の構文を使用しないようにすることができます。  
  
 `-langversion` を直接設定するには、コマンドラインを使用する必要があります。 詳細については、「[対象となる特定の .NET Framework のバージョンの指定](/visualstudio/ide/visual-studio-multi-targeting-overview)」を参照してください。  
  
## <a name="example"></a>例  
 次のコードは、Visual Basic 9.0 の `sample.vb` をコンパイルします。  
  
```console  
vbc -langversion:9.0 sample.vb  
```  
  
## <a name="see-also"></a>参照

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [対象となる特定の .NET Framework バージョンの指定](/visualstudio/ide/visual-studio-multi-targeting-overview)
