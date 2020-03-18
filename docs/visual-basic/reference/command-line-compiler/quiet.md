---
title: -quiet
ms.date: 07/20/2015
f1_keywords:
- -quiet
- quiet
helpviewer_keywords:
- -quiet compiler option [Visual Basic]
- /quiet compiler option [Visual Basic]
- quiet compiler option [Visual Basic]
ms.assetid: 5d77fa23-4c50-4708-8535-649912b098e8
ms.openlocfilehash: 6e773c60469e8426956c92a5aa377741ba5af4d3
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005283"
---
# <a name="-quiet"></a>-quiet

コンパイラで構文関連のエラーと警告のコードが表示されないようにします。

## <a name="syntax"></a>構文

```console
-quiet
```

## <a name="remarks"></a>コメント

既定では、`-quiet` は無効です。 コンパイラは、構文に関連するエラーまたは警告を報告するときに、ソースコードから行を出力します。 コンパイラの出力を解析するアプリケーションでは、コンパイラが診断のテキストのみを出力する方が便利な場合があります。

次の例では、`Module1` は、`-quiet` を指定せずにコンパイルされた場合にソースコードを含むエラーを出力します。

```vb
Module Module1
    Sub Main()
        x()
    End Sub
End Module
```

Output:

```console
C:\projects\vb2.vb(3) : error BC30451: 'x' is not declared. It may be inaccessible due to its protection level.

        x()
        ~
```

`-quiet` を使用してコンパイルされた場合、コンパイラは次のものだけを出力します。

```console
E:\test\t2.vb(3) : error BC30451: Name 'x' is not declared.
```

> [!NOTE]
> `-quiet` オプションは Visual Studio の開発環境内からは利用できません。このオプションを利用できるのは、コマンド ラインからコンパイルするときだけです。

## <a name="example"></a>例

次に示すのは `T2.vb` をコンパイルし、構文関連のコンパイラ診断に対してコードを表示しない場合のコード例です。

```console
vbc -quiet t2.vb
```

## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
