---
title: -imports
ms.date: 03/10/2018
helpviewer_keywords:
- /imports compiler option [Visual Basic]
- imports compiler option [Visual Basic]
- -imports compiler option [Visual Basic]
ms.assetid: 9a93fb53-c080-497b-bf9b-441022dbbc39
ms.openlocfilehash: 2a1dd19189ff65413255b9bc137e1a7f0227bbe1
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716648"
---
# <a name="-imports-visual-basic"></a>-imports (Visual Basic)
指定したアセンブリから名前空間をインポートします。  
  
## <a name="syntax"></a>構文  
  
```console  
-imports:namespaceList  
```  
  
## <a name="arguments"></a>引数  
  
|用語|Definition|  
|---|---|  
|`namespaceList`|必ず指定します。 インポートする名前空間のコンマ区切りの一覧。|  
  
## <a name="remarks"></a>Remarks  
 `-imports` オプションは、ソースファイルの現在のセットまたは参照されているアセンブリの中で定義されているすべての名前空間をインポートします。  
  
 `-imports` で指定された名前空間のメンバーは、コンパイル時にすべてのソースコードファイルで使用できます。 [Imports ステートメント (.Net 名前空間と型)](../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)を使用して、1つのソースコードファイルで名前空間を使用します。  
  
|Visual Studio 統合開発環境でインポートを設定するには|  
|---|  
|1.**ソリューションエクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。 <br />2. **[参照]** タブをクリックします。<br />3. **[ユーザーインポートの追加]** ボタンの横にあるボックスに名前空間名を入力します。<br />4. **[ユーザーインポートの追加]** ボタンをクリックします。|  
  
## <a name="example"></a>使用例  
 `-imports:system.globalization` を指定すると、次のコードがコンパイルされます。 それがない場合、コンパイルが成功するには、ソースコードファイルの先頭に `Imports System.Globalization` ステートメントが含まれているか、プロパティが `System.Globalization.CultureInfo.CurrentCulture.Name`として完全に修飾されている必要があります。

```vb
Module Example
   Public Sub Main()
      Console.WriteLine($"The current culture is {CultureInfo.CurrentCulture.Name}")
   End Sub
End Module
```

## <a name="see-also"></a>関連項目

- [Visual Basic のコマンド ライン コンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [参照と Imports ステートメント](../../../visual-basic/programming-guide/program-structure/references-and-the-imports-statement.md)
- [コンパイル コマンド ラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
