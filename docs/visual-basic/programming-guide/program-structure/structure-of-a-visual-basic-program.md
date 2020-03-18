---
title: Visual Basic プログラムの構造
ms.date: 07/20/2015
helpviewer_keywords:
- conditional compilation [Visual Basic], Visual Basic
- program structure [Visual Basic], Visual Basic
- procedures [Visual Basic], structure
- Visual Basic code, program structure
ms.assetid: ad0c6531-d762-4c77-a700-de16b07b6119
ms.openlocfilehash: 4f4136a2c8fb7ca98ff22aa6a5fc676f30cd1c5d
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2019
ms.locfileid: "64624306"
---
# <a name="structure-of-a-visual-basic-program"></a>Visual Basic プログラムの構造
Visual Basic プログラムは、標準の構成ブロックから構築します。 *ソリューション*は 1 つまたは複数のプロジェクトで構成されます *プロジェクト*は、1 つまたは複数のアセンブリを含めることができます。 各*アセンブリ*が 1 つまたは複数のソース ファイルからコンパイルします。 A*ソース ファイル*は、クラス、構造体、モジュール、およびインターフェイスの定義と実装を提供し、最終的にはすべてのコードが含まれます。  
  
 これらのビルディング ブロックの Visual Basic プログラムの詳細については、[ソリューションとプロジェクト](/visualstudio/ide/solutions-and-projects-in-visual-studio)と [.NET のアセンブリ](../../../standard/assembly/index.md)を参照してください。  
  
## <a name="file-level-programming-elements"></a>ファイル レベルのプログラミング要素  
 プロジェクトまたはファイルを開始し、コード エディターを開き、既にとを正しい順序でいくつかのコードが表示されます。 作成するすべてのコードは、次のシーケンスに従う必要があります。  
  
1. `Option` ステートメント  
  
2. `Imports` ステートメント  
  
3. `Namespace` ステートメントと名前空間レベル要素  
  
 別の順序でステートメントを入力すると、コンパイル エラーが発生することができます。  
  
 プログラムは、条件付きコンパイル ステートメントを含めることもできます。 上記の一連のステートメントの間でソース ファイルでこれらを挿入することができます。  
  
### <a name="option-statements"></a>ステートメントのオプション  
 `Option` ステートメントは、構文とロジックのエラーを防ぐ、後続のコードの基盤となる規則を確立します。 [Option Explicit ステートメント](../../../visual-basic/language-reference/statements/option-explicit-statement.md)ことにより、すべての変数宣言なりのスペルをデバッグする時間を短縮します。 [Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)別のデータ型の変数を使用するときに発生する可能性がロジックのエラーとデータの損失を最小限に抑えるのに役立ちます。 [Option Compare ステートメント](../../../visual-basic/language-reference/statements/option-compare-statement.md)方法文字列は、相互にいずれかに基づく比較を指定します。 その`Binary`または`Text`値。  
  
### <a name="imports-statements"></a>Imports ステートメント  
 [Imports ステートメント (.NET 名前空間および型)](../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)をプロジェクトの外で定義されたインポート名に含めることができます。 `Imports` ステートメントにより、コードはクラスや他のインポートされた名前空間内で定義された型を修飾することなく参照できます。 `Imports` ステートメントは必要な分だけ使用できます。 詳細は「[参照と Imports ステートメント](../../../visual-basic/programming-guide/program-structure/references-and-the-imports-statement.md)」を参照してください。  
  
### <a name="namespace-statements"></a>Namespace ステートメント  
 名前空間のヘルプを整理し、プログラミングの要素のグループ化とアクセスに容易にするための分類します。 使用する、 [Namespace ステートメント](../../../visual-basic/language-reference/statements/namespace-statement.md)を特定の名前空間内で、次のステートメントを分類します。 詳細については、「[Visual Basic における名前空間](../../../visual-basic/programming-guide/program-structure/namespaces.md)」を参照してください。  
  
### <a name="conditional-compilation-statements"></a>条件付きコンパイル ステートメント  
 条件付きコンパイル ステートメントは、ソース ファイルにどこに表示できます。 含まれるまたは特定の条件によってコンパイル時に除外するコードの部分が発生するとします。 できます使用することも、アプリケーションのデバッグの条件付きコードがデバッグ モードのみで実行されるためです。 詳細については、次を参照してください。[条件付きコンパイル](../../../visual-basic/programming-guide/program-structure/conditional-compilation.md)します。  
  
## <a name="namespace-level-programming-elements"></a>Namespace レベルのプログラミング要素  
 クラス、構造、およびモジュールは、ソース ファイル内のすべてのコードを含めることができます。 *名前空間レベル*要素、または、ソース ファイル レベルで、名前空間内に表示されることができます。 その他のすべてのプログラミング要素の宣言を保持します。 要素のシグネチャを定義して実装を提供しないには、インターフェイスは、モジュール レベルも表示されます。 モジュール レベルの要素の詳細については、次を参照してください。  
  
- [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)  
  
- [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)  
  
- [Module ステートメント](../../../visual-basic/language-reference/statements/module-statement.md)  
  
- [Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md)  
  
 名前空間レベルでのデータ要素は、列挙体およびデリゲートです。  
  
## <a name="module-level-programming-elements"></a>モジュール レベルのプログラミング要素  
 プロシージャ、演算子、プロパティ、およびイベントは、実行可能コード (実行時にアクションを実行するステートメント) を保持できる唯一のプログラミング要素です。 *モジュール レベル*プログラムの要素。 プロシージャ レベルの要素の詳細については、次を参照してください。  
  
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)  
  
- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)  
  
- [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)  
  
- [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)  
  
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)  
  
- [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)  
  
 モジュール レベルのデータ要素は、変数、定数、列挙型、およびデリゲート。  
  
## <a name="procedure-level-programming-elements"></a>プロシージャ レベルのプログラミング要素  
 内容のほとんど*プロシージャ レベル*要素は、実行可能ステートメントは、プログラムの実行時のコードを構成します。 すべての実行可能コードがいくつかの手順である必要があります (`Function`、 `Sub`、 `Operator`、 `Get`、 `Set`、 `AddHandler`、 `RemoveHandler`、 `RaiseEvent`)。 詳細については、「[ステートメント](../../../visual-basic/programming-guide/language-features/statements.md)」を参照してください。  
  
 プロシージャ レベルでのデータ要素は、ローカル変数および定数に限定されます。  
  
## <a name="the-main-procedure"></a>メインのプロシージャ  
 `Main`プロシージャは、アプリケーションが読み込まれたときに実行する最初のコード。 `Main` 開始点と、アプリケーションの全体的なコントロールとして機能します。 次の 4 種類がある`Main`:  
  
- `Sub Main()`  
  
- `Sub Main(ByVal cmdArgs() As String)`  
  
- `Function Main() As Integer`  
  
- `Function Main(ByVal cmdArgs() As String) As Integer`  
  
 最も一般的なプロシージャの種類は、`Sub Main()`です。 詳細については、「[Visual basic の Main プロシージャ](../../../visual-basic/programming-guide/program-structure/main-procedure.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [Visual Basic の main プロシージャ](../../../visual-basic/programming-guide/program-structure/main-procedure.md)
- [Visual Basic の名前付け規則](../../../visual-basic/programming-guide/program-structure/naming-conventions.md)
- [Visual Basic の制限事項](../../../visual-basic/programming-guide/program-structure/limitations.md)
