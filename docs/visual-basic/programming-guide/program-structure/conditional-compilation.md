---
title: 条件付きコンパイル
ms.date: 07/20/2015
helpviewer_keywords:
- conditional compilation [Visual Basic], about conditional compilation
- compilation [Visual Basic], conditional
ms.assetid: 9c35e55e-7eee-44fb-a586-dad1f1884848
ms.openlocfilehash: 19a2c70941a9a72574f7e624743def74b80c4e39
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347460"
---
# <a name="conditional-compilation-in-visual-basic"></a>Visual Basic での条件付きコンパイル
*条件付きコンパイル*では、プログラム内の特定のコードブロックが選択的にコンパイルされ、他のコードは無視されます。  
  
 たとえば、同じプログラミングタスクに対するさまざまなアプローチの速度を比較するデバッグステートメントを記述したり、アプリケーションを複数の言語用にローカライズしたりすることができます。 条件付きコンパイルステートメントは、実行時ではなくコンパイル時に実行するように設計されています。  
  
 `#If...Then...#Else` ディレクティブで条件付きでコンパイルされるコードブロックを示します。 たとえば、同じソースコードから、同じアプリケーションのフランス語とドイツ語の言語バージョンを作成するには、定義済みの定数 `FrenchVersion` と `GermanVersion`を使用して、`#If...Then` ステートメントにプラットフォーム固有のコードセグメントを埋め込みます。 次の例では、その方法を示します。  
  
 [!code-vb[VbVbalrConditionalComp#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#5)]  
  
 コンパイル時に `FrenchVersion` 条件付きコンパイル定数の値を `True` に設定すると、フランス語バージョンの条件付きコードがコンパイルされます。 `GermanVersion` 定数の値を `True`に設定すると、コンパイラはドイツ語バージョンを使用します。 どちらも `True`に設定されていない場合は、最後の `Else` ブロックのコードが実行されます。  
  
> [!NOTE]
> コードを編集し、現在のブランチに含まれていない場合、条件付きコンパイルディレクティブを使用すると、オートコンプリートは機能しません。  
  
## <a name="declaring-conditional-compilation-constants"></a>条件付きコンパイル定数の宣言  
 条件付きコンパイル定数は、次の3つの方法のいずれかで設定できます。  
  
- **プロジェクトデザイナー**で  
  
- コマンドラインコンパイラを使用する場合のコマンドライン  
  
- コード内  
  
 条件付きコンパイル定数には特別なスコープがあり、標準コードからアクセスすることはできません。 条件付きコンパイル定数のスコープは、設定方法によって異なります。 次の表は、前述の3つの方法のそれぞれを使用して宣言された定数の範囲を示しています。  
  
|定数の設定方法|定数のスコープ|  
|---|---|  
|**プロジェクトデザイナー**|プロジェクト内のすべてのファイルに対してパブリック|  
|[パッケージ実行ユーティリティ]|コマンドラインコンパイラに渡されるすべてのファイルに対してパブリック|  
|コード内の `#Const` ステートメント|宣言されているファイルに対してプライベート|  
  
|プロジェクトデザイナーで定数を設定するには|  
|---|  
|-実行可能ファイルを作成する前に、「[プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)」に記載されている手順に従って、**プロジェクトデザイナー**で定数を設定します。|  
  
|コマンドラインで定数を設定するには|  
|---|  
|-次の例に示すように、 **-d**スイッチを使用して条件付きコンパイル定数を入力します。<br />     `vbc MyProj.vb /d:conFrenchVersion=–1:conANSI=0`<br />     **-D**スイッチと最初の定数の間にスペースは必要ありません。 詳細については、「 [-define (Visual Basic)](../../../visual-basic/reference/command-line-compiler/define.md)」を参照してください。<br />     コマンドライン宣言は、**プロジェクトデザイナー**で入力された宣言をオーバーライドしますが、削除はしません。 **プロジェクトデザイナー**で設定された引数は、後続のコンパイルに有効なままです。<br />     コード自体に定数を記述する場合、そのスコープが宣言されているモジュール全体であるため、配置に関して厳密な規則はありません。|  
  
|コードに定数を設定するには|  
|---|  
|-使用されているモジュールの宣言ブロックに定数を配置します。 これにより、コードの整理と読み取りが容易になります。|  
  
## <a name="related-topics"></a>関連トピック  
  
|タイトル|説明|  
|---|---|  
|[プログラム構造とコード規則](../../../visual-basic/programming-guide/program-structure/program-structure-and-code-conventions.md)|コードを読みやすく、保守しやすいようにするための推奨事項を示します。|  
  
## <a name="reference"></a>参照  
 [#Const ディレクティブ](../../../visual-basic/language-reference/directives/const-directive.md)  
  
 [#If...Then...#Else ディレクティブ](../../../visual-basic/language-reference/directives/if-then-else-directives.md)  
  
 [-define (Visual Basic)](../../../visual-basic/reference/command-line-compiler/define.md)
