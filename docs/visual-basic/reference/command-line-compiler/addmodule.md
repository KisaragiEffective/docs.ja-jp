---
title: -addmodule
ms.date: 03/09/2018
helpviewer_keywords:
- /addmodule compiler option [Visual Basic]
- addmodule compiler option [Visual Basic]
- -addmodule compiler option [Visual Basic]
ms.assetid: fb4b89d4-4926-4f20-868d-427fa28497b2
ms.openlocfilehash: dd98b45d75ff421dc81666ed47695132a49bfa3a
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72524482"
---
# <a name="-addmodule"></a>-addmodule
指定ファイル内のすべての型情報を現在のコンパイル対象のプロジェクトで使用できるようにします。  
  
## <a name="syntax"></a>構文  
  
```console  
-addmodule:fileList  
```  
  
## <a name="arguments"></a>引数  
 `fileList`  
 必須です。 メタデータが含まれているものの、アセンブリマニフェストが含まれていないファイルのコンマ区切りのリスト。 スペースを含むファイル名は引用符 ("") で囲む必要があります。  
  
## <a name="remarks"></a>Remarks  
 @No__t_0 パラメーターによって指定されたファイルは、`-target:module` オプションを使用して作成するか、または `-target:module` に相当する別のコンパイラを使用して作成する必要があります。  
  
 @No__t_0 で追加されるすべてのモジュールは、実行時に出力ファイルと同じディレクトリに存在する必要があります。 つまり、コンパイル時に任意のディレクトリにモジュールを指定できますが、モジュールは実行時にアプリケーションディレクトリに存在する必要があります。 そうでない場合は、<xref:System.TypeLoadException> エラーが発生します。  
  
 @No__t_2 で `-target:module` 以外の[ターゲット (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)オプションを (暗黙的または明示的に) 指定すると、`-addmodule` に渡すファイルはプロジェクトのアセンブリの一部になります。 @No__t_0 で追加された1つ以上のファイルを含む出力ファイルを実行するには、アセンブリが必要です。  
  
 アセンブリを含むファイルからメタデータをインポートするには、[-reference (Visual Basic)](../../../visual-basic/reference/command-line-compiler/reference.md)を使用します。  
  
> [!NOTE]
> @No__t_0 オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。  
  
## <a name="example"></a>例  
 次のコードでは、モジュールを作成します。  
  
 [!code-vb[VbVbalrCompiler#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#47)]  
  
 次のコードは、モジュールの型をインポートします。  
  
 [!code-vb[VbVbalrCompiler#48](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#48)]  
  
 `t1` を実行すると、`802` が出力されます。  
  
## <a name="see-also"></a>関連項目

- [Visual Basic のコマンド ライン コンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
- [-reference (Visual Basic)](../../../visual-basic/reference/command-line-compiler/reference.md)
- [コンパイル コマンド ラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
