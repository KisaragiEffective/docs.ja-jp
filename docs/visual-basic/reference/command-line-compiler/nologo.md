---
title: -nologo
ms.date: 03/13/2018
helpviewer_keywords:
- -nologo compiler option [Visual Basic]
- banners [Visual Basic], suppressing startup
- nologo compiler option [Visual Basic]
- /nologo compiler option [Visual Basic]
ms.assetid: 25ef54b6-d676-4639-a2d2-a747a158bc07
ms.openlocfilehash: 03dc98c45a894304a67765c3e49cd19bbb089c8c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74335435"
---
# <a name="-nologo-visual-basic"></a>-nologo (Visual Basic)
コンパイル中に著作権情報のバナーおよび情報メッセージを表示しません。  
  
## <a name="syntax"></a>構文  
  
```console  
-nologo  
```  
  
## <a name="remarks"></a>コメント  
 `-nologo`を指定した場合、コンパイラは著作権情報のバナーを表示しません。 既定では、`-nologo` は無効です。  
  
> [!NOTE]
> `-nologo` オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。  
  
## <a name="example"></a>例  
 次のコードは `T2.vb` をコンパイルし、著作権のバナーを表示しません。  
  
```console
vbc -nologo t2.vb  
```  
  
## <a name="see-also"></a>参照

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
