---
title: -win32icon
ms.date: 03/13/2018
helpviewer_keywords:
- win32icon compiler option [Visual Basic]
- -win32icon compiler option [Visual Basic]
- /win32icon compiler option [Visual Basic]
ms.assetid: aecaab01-9353-46c5-941c-6edabd4eff92
ms.openlocfilehash: 6b4b69d227c857442de6857fac023090b3698e81
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72004632"
---
# <a name="-win32icon"></a>-win32icon
.Ico ファイルを出力ファイルに挿入します。 この .ico ファイルは、**ファイルエクスプローラー**の出力ファイルを表します。  
  
## <a name="syntax"></a>構文  
  
```console  
-win32icon:filename  
```  
  
## <a name="arguments"></a>引数  
  
|項目|定義|  
|---|---|  
|`filename`|出力ファイルに追加する .ico ファイル。 ファイル名にスペースが含まれている場合は、ファイル名を引用符 ("") で囲みます。|  
  
## <a name="remarks"></a>コメント  
 Microsoft Windows リソースコンパイラ (RC) を使用して .ico ファイルを作成できます。 リソースコンパイラは、ビジュアルC++プログラムをコンパイルすると呼び出されます.ico ファイルは、.rc ファイルから作成されます。 @No__t-0 および `-win32resource` オプションは相互に排他的です。  
  
 .NET Framework リソースファイルを参照する場合は[-linkresource (Visual Basic)](../../../visual-basic/reference/command-line-compiler/linkresource.md)を参照し、.NET Framework リソースファイルをアタッチする場合は[-resource (Visual Basic)](../../../visual-basic/reference/command-line-compiler/resource.md)を参照してください。 .Res ファイルをインポートするには、「[-win32resource](../../../visual-basic/reference/command-line-compiler/win32resource.md) 」を参照してください。  
  
|Visual Studio IDE で-win32icon を設定するには|  
|---|  
|1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。 <br />2. **[アプリケーション]** タブをクリックします。<br />3. **[アイコン]** ボックスの値を変更します。|  
  
## <a name="example"></a>例  
 次のコードは `In.vb` をコンパイルし、.ico ファイルをアタッチします。 `Rf.ico` です。  
  
```console
vbc -win32icon:rf.ico in.vb  
```  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
