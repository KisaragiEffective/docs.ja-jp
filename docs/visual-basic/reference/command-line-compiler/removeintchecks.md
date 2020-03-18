---
title: -removeintchecks
ms.date: 03/13/2018
f1_keywords:
- removeintchecks
- -removeintchecks
helpviewer_keywords:
- removeintchecks compiler option [Visual Basic]
- /removeintchecks compiler option [Visual Basic]
- -removeintchecks compiler option [Visual Basic]
ms.assetid: c1835bd5-1e38-4fba-bd2f-6984774765d4
ms.openlocfilehash: bea6ca24ea6da9000267e754d52fe0ca152f7d7f
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005228"
---
# <a name="-removeintchecks"></a>-removeintchecks
整数演算のオンまたはオフのオーバーフローエラーチェックをオンまたはオフにします。  
  
## <a name="syntax"></a>構文  
  
```console  
-removeintchecks[+ | -]  
```  
  
## <a name="arguments"></a>引数  
  
|項目|定義|  
|---|---|  
|`+` &#124; `-`|任意。 @No__t-0 オプションを指定すると、コンパイラはすべての整数計算でオーバーフローエラーをチェックします。 既定値は `-removeintchecks-` です。<br /><br /> @No__t-0 または `-removeintchecks+` を指定すると、エラーチェックが行われず、整数計算が高速になります。 ただし、エラーチェックが行われず、データ型の容量がオーバーフローした場合は、エラーが発生することなく誤った結果が格納される可能性があります。|  
  
|Visual Studio 統合開発環境で-removeintchecks を設定するには|  
|---|  
|1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。 <br />2. **[コンパイル]** タブをクリックします。<br />3. **[詳細設定]** ボタンをクリックします。<br />4。[**整数オーバーフローを削除**する] チェックボックスの値を変更します。|  
  
## <a name="example"></a>例  
 次のコードは、`Test.vb` をコンパイルし、整数オーバーフローエラーチェックをオフにします。  
  
```console
vbc -removeintchecks+ test.vb  
```  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
