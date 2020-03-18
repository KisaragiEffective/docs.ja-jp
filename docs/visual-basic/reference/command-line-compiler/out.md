---
title: -out
ms.date: 07/20/2015
helpviewer_keywords:
- /out compiler option [Visual Basic]
- -out compiler option [Visual Basic]
- out compiler option [Visual Basic]
ms.assetid: 9f148c15-0909-4cb8-a2db-777f8a8b45ae
ms.openlocfilehash: 67366e13e4dceea4772d0730222413cb25b4e8b7
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352390"
---
# <a name="-out-visual-basic"></a>-out (Visual Basic)
出力ファイルの名前を指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-out:filename  
```  
  
## <a name="arguments"></a>引数  
  
|用語|Definition|  
|---|---|  
|`filename`|必須。 コンパイラによって作成される出力ファイルの名前。 ファイル名にスペースが含まれている場合は、名前を引用符 ("") で囲みます。|  
  
## <a name="remarks"></a>コメント  
 作成するファイルの完全な名前と拡張子を指定します。 そうしないと、.exe ファイルの名前が `Sub Main` プロシージャを含むソースコードファイルから取得され、.dll ファイルの名前が最初のソースコードファイルの名前になります。  
  
 .Exe または .dll 拡張子のないファイル名を指定すると、コンパイラは、`-target` コンパイラオプションに指定された値に応じて、自動的に拡張機能を追加します。  
  
|Visual Studio 統合開発環境でを設定するには|  
|---|  
|1.**ソリューションエクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。 <br />2. **[アプリケーション]** タブをクリックします。<br />3. **[アセンブリ名]** ボックスの値を変更します。|  
  
## <a name="example"></a>例  
 次のコードは `T2.vb` をコンパイルし、出力ファイル `T2.exe`を作成します。  
  
```console
vbc t2.vb -out:t3.exe  
```  
  
## <a name="see-also"></a>参照

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
