---
title: 定数の概要
ms.date: 07/20/2015
helpviewer_keywords:
- constants [Visual Basic]
ms.assetid: 29016fe8-78b3-4dc8-90b8-1cfec2fa8ac9
ms.openlocfilehash: 9ccddfe44757c76992d641094e21ec8c2110ef83
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74338343"
---
# <a name="constants-overview-visual-basic"></a>定数の概要 (Visual Basic)
定数は、変更されない数値または文字列の代わりとなるわかりやすい名前です。 定数は、名前が示すように、アプリケーションの実行全体で同じままになる値を格納します。 コードの読みやすさを大幅に向上させ、定数を使用して保守を容易にすることができます。 再表示される値を含むコードで使用するか、覚えにくい特定の数値に依存するか、明確な意味を持たないコードで使用します。  
  
## <a name="how-to-create-and-use-constants"></a>定数を作成して使用する方法  
 Visual Basic には、主に印刷と表示のためにを使用する多数の定義済み定数が含まれています。 変数名を作成する場合と同じガイドラインを使用して、`Const` ステートメントを使用して独自の定数を作成することもできます。 `Option Strict` が `On`場合は、定数型を明示的に宣言する必要があります。  
  
 定数のスコープは、その名前を修飾せずに参照できるすべてのコードのセットであり、同じ場所で宣言された変数のものと同じです。 特定のプロシージャのスコープ内に存在する定数を作成するには、そのプロシージャ内で宣言します。 アプリケーション全体で使用できる定数を作成するには、クラスの宣言セクションで `Public` キーワードを使用して宣言します。  
  
> [!NOTE]
> 定数は変数に似ていますが、変数に対して変更を加えたり、変数に新しい値を割り当てたりすることはできません。  
  
 コードで使用する定数は、操作するコントロールまたはコンポーネントのオブジェクトモデルによって定義できます。また、ユーザー定義 (自分で作成したもの) にすることもできます。  
  
## <a name="compile-time-and-run-time-constants"></a>コンパイル時と実行時の定数  
 コンパイル時の定数は、コードのコンパイル時に計算されますが、実行時の定数は、アプリケーションの実行中にのみ計算できます。 コンパイル時の定数は、アプリケーションが実行されるたびに同じ値を持ちますが、実行時の定数は毎回変わる可能性があります。 コンパイル時定数は、配列の境界、case 式、列挙子初期化子などの場合に必要です。  
  
## <a name="in-this-section"></a>このセクションの内容  
  
|定義|用語|  
|---|---|  
|[方法 : 定数を宣言する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-a-constant.md)|`Const` ステートメントを使用して定数を宣言し、その値を設定する方法について説明します。定数を宣言することで、値にわかりやすい名前を割り当てます。|  
|[ユーザー定義定数](../../../../visual-basic/programming-guide/language-features/constants-enums/user-defined-constants.md)|スコープに関する情報や循環参照を回避する方法など、独自の定数を作成する方法について説明します。|  
|[定数とリテラルのデータ型](../../../../visual-basic/programming-guide/language-features/constants-enums/constant-and-literal-data-types.md)|`Option Explicit` がオフになっている場合に Visual Basic コンパイラが定数を初期化する方法について説明します。|  
|[方法 : 関連する定数値をまとめてグループ化する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-group-related-constant-values-together.md)|関連する定数値をグループ化する方法を示します。|  
  
## <a name="reference"></a>参照  
  
|定義|用語|  
|---|---|  
|[定数と列挙体](../../../../visual-basic/language-reference/constants-and-enumerations.md)|Visual Basic によって定義済みの定数を一覧表示します。|  
|[Const ステートメント](../../../../visual-basic/language-reference/statements/const-statement.md)|`Const` ステートメントとその使用方法について説明します。|  
|[Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)|`Option Strict` ステートメントとその使用方法について説明します。|  
  
## <a name="see-also"></a>関連項目

- [列挙型の概要](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-overview.md)
- [方法: Visual Basic で配列変数を初期化する](../../../../visual-basic/programming-guide/language-features/arrays/how-to-initialize-an-array-variable.md)
