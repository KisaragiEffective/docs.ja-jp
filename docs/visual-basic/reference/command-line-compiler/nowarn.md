---
title: -nowarn
ms.date: 07/20/2015
helpviewer_keywords:
- nowarn compiler option [Visual Basic]
- /nowarn compiler option [Visual Basic]
- -nowarn compiler option [Visual Basic]
ms.assetid: 7ebf2106-0652-4fdc-bf60-70fc86465d83
ms.openlocfilehash: 880fdf4931dadea547d64d0506bd3e978956468e
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005397"
---
# <a name="-nowarn"></a>-nowarn
警告を生成するコンパイラの機能を無効にします。  
  
## <a name="syntax"></a>構文  
  
```console  
-nowarn[:numberList]  
```  
  
## <a name="arguments"></a>引数  
  
|項目|定義|  
|---|---|  
|`numberList`|任意。 コンパイラによって抑制される警告 ID 番号のコンマ区切りの一覧。 警告 Id が指定されていない場合は、すべての警告が抑制されます。|  
  
## <a name="remarks"></a>コメント  
 @No__t-0 オプションを指定すると、コンパイラは警告を生成しません。 個々の警告を非表示にするには、警告 ID をコロンの後の `-nowarn` オプションに指定します。 複数の警告番号はコンマで区切ります。  
  
 警告 id の数値部分のみを指定する必要があります。 たとえば、BC42024 を抑制する場合は、使用されていないローカル変数の警告として、`-nowarn:42024` を指定します。  
  
 警告 ID 番号の詳細については、「[Visual Basic での警告の構成](/visualstudio/ide/configuring-warnings-in-visual-basic)」を参照してください。  
  
|Visual Studio 統合開発環境で-nowarn を設定するには|  
|---|  
|1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。 <br />2. **[コンパイル]** タブをクリックします。<br />3.すべての警告を無効にするには、[**すべての警告を無効**にする] チェックボックスをオンにします。<br />     または<br />     特定の警告を無効にするには、警告の隣にあるドロップダウンリストから **[なし]** をクリックします。|  
  
## <a name="example"></a>例  
 次のコードは `T2.vb` をコンパイルし、警告を表示しません。  
  
```console
vbc -nowarn t2.vb  
```  
  
## <a name="example"></a>例  
 次のコードは `T2.vb` をコンパイルし、使用されていないローカル変数 (42024) の警告を表示しません。  
  
```console
vbc -nowarn:42024 t2.vb  
```  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [Visual Basic での警告の構成](/visualstudio/ide/configuring-warnings-in-visual-basic)
