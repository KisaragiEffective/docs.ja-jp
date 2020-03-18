---
title: -libpath
ms.date: 03/10/2018
helpviewer_keywords:
- libpath compiler option [Visual Basic]
- /libpath compiler option [Visual Basic]
- -libpath compiler option [Visual Basic]
ms.assetid: 5f1c26c9-3455-4e89-bdf3-b12d6c2e655b
ms.openlocfilehash: 9a5822a097828f818da020735c3822e86eb3236b
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716636"
---
# <a name="-libpath"></a>-libpath
参照アセンブリの場所を指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-libpath:dirList  
```  
  
## <a name="arguments"></a>引数  
  
|用語|Definition|  
|---|---|  
|`dirList`|必ず指定します。 現在の作業ディレクトリ (コンパイラの呼び出し元のディレクトリ) または共通言語ランタイムのシステムディレクトリのいずれかで参照されているアセンブリが見つからない場合に、コンパイラが検索するディレクトリのセミコロン区切りのリスト。 ディレクトリ名にスペースが含まれている場合は、名前を引用符 ("") で囲みます。|  
  
## <a name="remarks"></a>Remarks  
 `-libpath` オプションは、 [-reference](../../../visual-basic/reference/command-line-compiler/reference.md)オプションによって参照されるアセンブリの場所を指定します。  
  
 コンパイラは、完全に修飾されていないアセンブリ参照を次の順序で検索します。  
  
1. 現在の作業ディレクトリ。 これは、コンパイラを起動したディレクトリです。  
  
2. 共通言語ランタイムのシステム ディレクトリ。  
  
3. `-libpath`によって指定されたディレクトリ。  
  
4. LIB 環境変数によって指定されているディレクトリ。  
  
 `-libpath` オプションは加法です。これを複数回指定すると、前の値が追加されます。  
  
 アセンブリ参照を指定するには、`-reference` を使用します。  
  
|Visual Studio 統合開発環境で libpath を設定するには|  
|---|  
|1.**ソリューションエクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。 <br />2. **[参照]** タブをクリックします。<br />3. **[参照パス...]** ボタンをクリックします。<br />4. **[参照パス]** ダイアログボックスの **[フォルダー:]** ボックスにディレクトリ名を入力します。<br />5. **[フォルダーの追加]** をクリックします。|  
  
## <a name="example"></a>使用例  
 次のコードでは、`T2.vb` をコンパイルして .exe ファイルを作成します。 コンパイラは、作業ディレクトリ、C: ドライブのルートディレクトリ、およびアセンブリ参照の C: ドライブの新しい Assemblies ディレクトリ内を検索します。  
  
```console  
vbc -libpath:c:\;"c:\New Assemblies" -reference:t2.dll t2.vb  
```  
  
## <a name="see-also"></a>関連項目

- [.NET のアセンブリ](../../../standard/assembly/index.md)
- [Visual Basic のコマンド ライン コンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイル コマンド ラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
