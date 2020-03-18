---
title: 有効期間
ms.date: 07/20/2015
helpviewer_keywords:
- static variables [Visual Basic], lifetime
- static variables [Visual Basic], Visual Basic
- declared elements [Visual Basic], lifetime
- Shared variable lifetime [Visual Basic]
- lifetime [Visual Basic], declared elements
- lifetime [Visual Basic], Visual Basic
- lifetime [Visual Basic]
ms.assetid: bd91e390-690a-469a-9946-8dca70bc14e7
ms.openlocfilehash: 293537ad33c8e751d49d820fc57ea525e68bc203
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347774"
---
# <a name="lifetime-in-visual-basic"></a>Visual Basic における有効期間
宣言された要素の*有効*期間は、その要素が使用可能になるまでの期間です。 変数は、有効期間が設定されている唯一の要素です。 このため、コンパイラはプロシージャのパラメーターと関数の戻り値を変数の特殊なケースとして扱います。 変数の有効期間は、値を保持できる期間を表します。 その値は有効期間を通じて変化することがありますが、常に値が保持されます。  
  
## <a name="different-lifetimes"></a>異なる有効期間  
 *メンバー変数*(モジュールレベルではプロシージャ以外) は、通常、宣言されている要素と同じ有効期間を持ちます。 クラスまたは構造体で宣言された非共有変数は、宣言されているクラスまたは構造体のインスタンスごとに個別のコピーとして存在します。 各変数には、インスタンスと同じ有効期間があります。 ただし、`Shared` 変数の有効期間は1つだけであり、アプリケーションの実行時間全体にわたって存続します。  
  
 *ローカル変数*(プロシージャ内で宣言) は、宣言されているプロシージャが実行されている場合にのみ存在します。 これは、そのプロシージャのパラメーターと関数の戻り値にも適用されます。 ただし、そのプロシージャが他のプロシージャを呼び出す場合、呼び出されたプロシージャが実行されている間、ローカル変数の値は保持されます。  
  
## <a name="beginning-of-lifetime"></a>有効期間の開始  
 ローカル変数の有効期間は、コントロールが宣言されているプロシージャに制御が入ると開始されます。 各ローカル変数は、プロシージャの実行が開始されるとすぐに、そのデータ型の既定値に初期化されます。 プロシージャが初期値を指定する `Dim` ステートメントを検出すると、コードに他の値が既に割り当てられていたとしても、これらの変数はこれらの値に設定されます。  
  
 構造体変数の各メンバーは、個別の変数であるかのように初期化されます。 同様に、配列変数の各要素は個別に初期化されます。  
  
 プロシージャ内のブロック内で宣言された変数 (`For` ループなど) は、プロシージャのエントリで初期化されます。 これらの初期化は、コードがブロックを実行したかどうかにかかわらず有効になります。  
  
## <a name="end-of-lifetime"></a>有効期間の終了  
 プロシージャが終了すると、ローカル変数の値は保持されず、Visual Basic はメモリを解放します。 次にプロシージャを呼び出すときに、すべてのローカル変数が新しく作成され、再初期化されます。  
  
 クラスまたは構造体のインスタンスが終了すると、その共有されていない変数はメモリとその値を失います。 クラスまたは構造体の新しい各インスタンスは、非共有変数を作成して再初期化します。 ただし、`Shared` 変数は、アプリケーションが実行を停止するまで保持されます。  
  
## <a name="extension-of-lifetime"></a>有効期間の延長  
 `Static` キーワードを使用してローカル変数を宣言する場合、その有効期間は、そのプロシージャの実行時間よりも長くなります。 次の表は、プロシージャの宣言によって、`Static` 変数が存在する期間を決定する方法を示しています。  
  
|プロシージャの場所と共有|静的変数の有効期間の開始|静的変数の有効期間の終了|  
|------------------------------------|-------------------------------------|-----------------------------------|  
|(既定では共有される) モジュール内|プロシージャが初めて呼び出されたとき|アプリケーションの実行が停止したとき|  
|クラスでは、`Shared` (プロシージャはインスタンスメンバーではありません)|プロシージャが最初に呼び出されたときに、特定のインスタンスまたはクラスまたは構造体の名前自体で呼び出されます。|アプリケーションの実行が停止したとき|  
|`Shared` ではなく、クラスのインスタンス内で (プロシージャはインスタンスメンバー)|特定のインスタンスでプロシージャが初めて呼び出されたとき|インスタンスがガベージコレクション (GC) 用に解放されたとき|  
  
## <a name="static-variables-of-the-same-name"></a>同じ名前の静的変数  
 複数のプロシージャで、同じ名前の静的変数を宣言できます。 この場合、Visual Basic コンパイラは、このような各変数を個別の要素と見なします。 これらの変数のいずれかを初期化しても、他の変数の値には影響しません。 オーバーロードのセットを使用してプロシージャを定義し、各オーバーロードで同じ名前の静的変数を宣言する場合も同じことが当てはまります。  
  
## <a name="containing-elements-for-static-variables"></a>静的変数の要素を含む  
 静的ローカル変数は、クラス内で、つまり、そのクラスのプロシージャ内で宣言できます。 ただし、構造体の内部で静的ローカル変数を宣言することはできません。構造体のメンバーであるか、その構造内のプロシージャのローカル変数として宣言することはできません。  
  
## <a name="example"></a>使用例  
  
### <a name="description"></a>説明  
 次の例では、 [Static](../../../../visual-basic/language-reference/modifiers/static.md)キーワードを使用して変数を宣言しています。 ( [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)で `Static`などの修飾子が使用されている場合は、`Dim` キーワードは必要ありません)。  
  
### <a name="code"></a>コード  
 [!code-vb[VbVbalrKeywords#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/class7.vb#13)]  
  
### <a name="comments"></a>コメント  
 前の例では、プロシージャ `runningTotal` 呼び出し元のコードに戻ると、変数 `applesSold` が引き続き存在します。 次回 `runningTotal` が呼び出されたときに、`applesSold` は以前に計算された値を保持します。  
  
 `Static`を使用せずに `applesSold` が宣言されている場合、以前に蓄積された値は `runningTotal`の呼び出し間で保持されません。 次に `runningTotal` が呼び出されたときに、`applesSold` は再作成されて0に初期化され、`runningTotal` は呼び出されたのと同じ値を返します。  
  
### <a name="compile-the-code"></a>コードのコンパイル  
 静的ローカル変数の値は、その宣言の一部として初期化できます。 `Static`する配列を宣言する場合は、そのランク (次元の数)、各次元の長さ、および個々の要素の値を初期化できます。  
  
### <a name="security"></a>セキュリティ  
 前の例では、モジュールレベルで `applesSold` を宣言することで、同じ有効期間を生成できます。 ただし、このように変数のスコープを変更した場合、プロシージャに排他的にアクセスすることはできなくなります。 他のプロシージャが `applesSold` にアクセスしてその値を変更する可能性があるため、実行中の合計が不安定になり、コードの保守が困難になる可能性があります。  
  
## <a name="see-also"></a>関連項目

- [Shared](../../../../visual-basic/language-reference/modifiers/shared.md)
- [Nothing](../../../../visual-basic/language-reference/nothing.md)
- [宣言された要素の名前](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)
- [宣言された要素の参照](../../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [Visual Basic 内のスコープ](../../../../visual-basic/programming-guide/language-features/declared-elements/scope.md)
- [Visual Basic でのアクセス レベル](../../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
- [変数](../../../../visual-basic/programming-guide/language-features/variables/index.md)
- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [Static](../../../../visual-basic/language-reference/modifiers/static.md)
