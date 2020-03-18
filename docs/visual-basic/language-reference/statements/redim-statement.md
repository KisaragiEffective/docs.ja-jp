---
title: ReDim ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.ReDim
- vb.Preserve
helpviewer_keywords:
- fixed-length strings [Visual Basic], declaring
- ReDim statement [Visual Basic], syntax
- dynamic arrays [Visual Basic], ReDim statement
- arrays [Visual Basic], reallocating
- arrays [Visual Basic], reinitializing
- arrays [Visual Basic], dimensions
- scalars, and arrays
- scalars
- declarations [Visual Basic], dynamic arrays
- variables [Visual Basic], scalar
- ReDim statement [Visual Basic]
- data types [Visual Basic], assigning
- As keyword [Visual Basic], in ReDim statement
- To keyword [Visual Basic], ReDim statement
- arrays [Visual Basic], declaring
- Preserve keyword [Visual Basic], ReDim statement
- storage [Visual Basic], allocating
- arrays [Visual Basic], and scalars
- declaration statements [Visual Basic]
- scalar variables [Visual Basic]
ms.assetid: ad1c5e07-dcd7-4ae1-a79e-ad3f2dcc2083
ms.openlocfilehash: fabfd9a45d47cc1b881b3743181a03e89158f939
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346747"
---
# <a name="redim-statement-visual-basic"></a>ReDim ステートメント (Visual Basic)
配列変数の記憶域を再割り当てします。  
  
## <a name="syntax"></a>構文  
  
```vb  
ReDim [ Preserve ] name(boundlist) [ ,  name(boundlist) [, ... ] ]  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|----------|----------------|  
|`Preserve`|省略可。 最後の次元のサイズだけを変更したときに、既存の配列のデータを保持するために使用する修飾子|  
|`name`|必須。 配列変数名。 「 [Declared Element Names](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。|  
|`boundlist`|必須。 再定義された配列の各次元の境界を一覧表示します。|  
  
## <a name="remarks"></a>コメント  
 `ReDim` ステートメントを使用し、既に宣言されている配列の 1 つまたは複数の次元のサイズを変更できます。 大きな配列があり、その要素の一部が必要ない場合、`ReDim` で配列のサイズを減らし、メモリを解放できます。 一方で、配列に要素を追加する必要がある場合、`ReDim` は要素を追加できます。  
  
 `ReDim` ステートメントは配列のみを対象としています。 スカラー (単一の値のみが含まれる変数)、コレクション、構造体では有効ではありません。 `Array` 型の変数を宣言する場合、`ReDim` ステートメントには新しい配列を作成するために十分な型情報が与えられません。  
  
 `ReDim` はプロシージャ レベルでのみ使用できます。 そのため、変数の宣言コンテキストはプロシージャにする必要があります。ソース ファイル、名前空間、インターフェイス、クラス、構造体、モジュール、ブロックにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。  
  
## <a name="rules"></a>ルール  
  
- **複数の変数。** 同じ宣言ステートメントで複数の配列変数のサイズを変更し、各変数の `name` と `boundlist` の部分を指定できます。 複数の変数を指定するときは、コンマで区切ります。  
  
- **配列の境界。** `boundlist` 内の各エントリは、そのディメンションの下限と上限を指定できます。 下限は常に 0 (ゼロ) です。 上限は次元の長さ (上限に 1 を足したもの) ではなく、その次元で可能な最大インデックス値です。 各次元のインデックスは 0 ～ 上限値の範囲で変わります。  
  
     `boundlist` の次元数は配列の次元の元の数 (r順位) に一致する必要があります。  
  
- **データ型。** `ReDim` ステートメントでは、配列変数またはその要素のデータ型を変更することはできません。  
  
- **初期化。  **   `ReDim` ステートメントでは、配列要素の新しい初期化値を指定できません。  
  
- **ランク.** `ReDim` ステートメントでは、配列のランク (次元数) を変更することはできません。  
  
- **保持してサイズを変更します。** `Preserve`を使用する場合は、配列の最後の次元だけのサイズを変更できます。 他のすべての次元については、既存の配列の境界を指定する必要があります。  
  
     たとえば、配列に次元が 1 つだけある場合、その次元のサイズを変更し、配列のすべてのコンテンツを保持できます。最後で唯一の次元を変更するためです。 ただし、配列に次元が 2 つ以上あるときは、`Preserve` を使用する場合、最後の次元だけのサイズを変更できます。  
  
- **属性.** 値の配列を保持するプロパティで `ReDim` を使用できます。  
  
## <a name="behavior"></a>動作  
  
- **配列の置換。** `ReDim` は、既存の配列を解放し、同じランクの新しい配列を作成します。 新しい配列は配列変数で解放された配列に取って代わります。  
  
- **保持せずに初期化します。** `Preserve`を指定しない場合、`ReDim` データ型の既定値を使用して、新しい配列の要素を初期化します。  
  
- **Preserve を使用した初期化。** `Preserve`を指定した場合、は既存の配列から新しい配列に要素をコピー Visual Basic ます。  
  
## <a name="example"></a>例  
 次の例では、配列の既存データを失うことなく動的配列の最後の次元のサイズを増やし、その後、一部のデータを損失しサイズを減らします。 最後に、サイズを減らして元の値に戻し、すべての配列要素を再初期化します。  
  
 [!code-vb[VbVbalrStatements#52](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#52)]  
  
 `Dim` ステートメントは次の 3 つの次元を持つ新しい配列を作成します。 各次元は 10 の境界で宣言されます。そのため、各次元の配列インデックスの範囲は 0 ～ 10 になります。 次の説明では、3 つの次元が「層」、「行」、「列」になります。  
  
 最初の `ReDim` は、変数 `intArray` の既存の配列を置換する新しい配列を作成します。 `ReDim` 既存の配列のすべての要素を新しい配列にコピーします。 また、さらに 10 個の列を各層の各行の終わりに追加し、これらの列の要素を 0 (配列の要素型である `Integer` の初期値) に初期化します。  
  
 2 番目の `ReDim` は新しい配列をもう 1 つ作成し、適合するすべての要素をコピーします。 ただし、5 つの列がすべての層のすべての行の終わりから失われます。 これらの列を使用して完了した場合、これは問題ではありません。 大きな配列のサイズを減らし、不要になったメモリを解放できます。  
  
 3 番目の `ReDim` は新しい配列をもう 1 つ作成し、すべての層のすべての行の終わりから別の 5 つの列を削除します。 このとき、既存の要素はコピーされません。 このステートメントは配列を元のサイズに戻します。 ステートメントに `Preserve` 修飾子が含まれないため、すべての配列要素が元の初期値に設定されます。  
  
 その他の例については、「[配列](../../../visual-basic/programming-guide/language-features/arrays/index.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.IndexOutOfRangeException>
- [Const ステートメント](../../../visual-basic/language-reference/statements/const-statement.md)
- [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)
- [Erase ステートメント](../../../visual-basic/language-reference/statements/erase-statement.md)
- [Nothing](../../../visual-basic/language-reference/nothing.md)
- [配列](../../../visual-basic/programming-guide/language-features/arrays/index.md)
