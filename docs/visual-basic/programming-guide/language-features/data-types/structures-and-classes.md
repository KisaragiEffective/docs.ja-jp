---
title: 構造体とクラス
ms.date: 07/20/2015
helpviewer_keywords:
- classes [Visual Basic], vs. structures
- structures [Visual Basic]
- classes [Visual Basic]
- structures [Visual Basic], compared to classes
- structures [Visual Basic], structure variables
- structure variables [Visual Basic]
ms.assetid: a221e74a-ffcf-4bdc-a0f6-a088a9bf26cc
ms.openlocfilehash: 3353935a74bb77fa4a630e706aa425063c7a610a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346326"
---
# <a name="structures-and-classes-visual-basic"></a>構造体とクラス (Visual Basic)
Visual Basic は構造体とクラスの構文を統一します。結果として、両方のエンティティが同じ機能の大部分をサポートします。 ただし、構造体とクラスには重要な違いもあります。  
  
 クラスには参照型を使用できるという利点があります。参照を渡すことは、構造体変数をすべてのデータと共に渡すよりも効率的です。 一方、構造体では、グローバルヒープにメモリを割り当てる必要はありません。  
  
 構造体から継承することはできないため、構造体は拡張する必要のないオブジェクトに対してのみ使用してください。 作成するオブジェクトのインスタンスサイズが小さい場合は構造体を使用し、クラスと構造体のパフォーマンス特性を考慮してください。  
  
## <a name="similarities"></a>共通  
 構造体とクラスは、次の点で似ています。  
  
- どちらも*コンテナー*型であり、他の型をメンバーとして含むことを意味します。  
  
- どちらにもメンバーがあります。これには、コンストラクター、メソッド、プロパティ、フィールド、定数、列挙体、イベント、およびイベントハンドラーを含めることができます。 ただし、これらのメンバーは、構造体の宣言された*要素*と混同しないでください。  
  
- どちらのメンバーも、個別のアクセスレベルを持つことができます。 たとえば、1つのメンバーを `Public` および別の `Private`として宣言できます。  
  
- どちらもインターフェイスを実装できます。  
  
- どちらも、パラメーターの有無にかかわらず、共有コンストラクターを持つことができます。  
  
- プロパティが少なくとも1つのパラメーターを受け取る場合は、どちらも*既定のプロパティ*を公開できます。  
  
- どちらもイベントを宣言して発生させることができ、どちらもデリゲートを宣言できます。  
  
## <a name="differences"></a>[残領域]  
 構造体とクラスは、次の点で異なります。  
  
- 構造体は*値型*です。クラスは*参照型*です。 構造体型の変数は、データへの参照をクラス型として格納するのではなく、構造体のデータを格納します。  
  
- 構造体はスタック割り当てを使用します。クラスは、ヒープ割り当てを使用します。  
  
- 既定では、すべての構造体要素が `Public` されます。クラス変数および定数は既定では `Private` ますが、他のクラスメンバーは既定では `Public` ます。 クラスメンバーのこの動作は、既定の Visual Basic 6.0 システムとの互換性を提供します。  
  
- 構造体には、少なくとも1つの非共有変数または非共有のカスタム event 要素が必要です。クラスは完全に空にすることができます。  
  
- 構造体要素を `Protected`として宣言することはできません。クラスメンバーは、を使用できます。  
  
- 構造体プロシージャでイベントを処理できるのは、それが[共有](../../../../visual-basic/language-reference/modifiers/shared.md)`Sub` プロシージャであり、かつ、 [AddHandler ステートメント](../../../../visual-basic/language-reference/statements/addhandler-statement.md)を使用した場合のみです。すべてのクラスプロシージャは、 [Handles](../../../../visual-basic/language-reference/statements/handles-clause.md)キーワードまたは `AddHandler` ステートメントを使用して、イベントを処理できます。 詳細については、「 [イベント](../../../../visual-basic/programming-guide/language-features/events/index.md)で定義されているインターフェイスのプライベート C++ 固有の実装です。  
  
- 構造体変数の宣言では、配列の初期化子または初期サイズを指定できません。クラス変数宣言では、を使用できます。  
  
- 構造体は <xref:System.ValueType?displayProperty=nameWithType> クラスから暗黙的に継承し、他の型から継承することはできません。クラスは、<xref:System.ValueType?displayProperty=nameWithType>以外のクラスまたはクラスから継承できます。  
  
- 構造体は継承できません。クラスはです。  
  
- 構造体は終了されないため、共通言語ランタイム (CLR) は、どの構造体でも <xref:System.Object.Finalize%2A> メソッドを呼び出すことはありません。クラスは、ガベージコレクター (GC) によって終了されます。これは、アクティブな参照が残っていないことを検出したときに、クラスの <xref:System.Object.Finalize%2A> を呼び出します。  
  
- 構造体はコンストラクターを必要としません。クラスは、を行います。  
  
- 構造体は、パラメーターを受け取る場合にのみ、非共有コンストラクターを持つことができます。クラスは、パラメーターの有無にかかわらず、これらを持つことができます。  
  
 すべての構造体には、パラメーターを持たない暗黙のパブリックコンストラクターがあります。 このコンストラクターは、すべての構造体のデータ要素を既定値に初期化します。 この動作を再定義することはできません。  
  
## <a name="instances-and-variables"></a>インスタンスと変数  
 構造体は値型であるため、各構造体変数は個々の構造体インスタンスに永続的にバインドされます。 ただし、クラスは参照型であり、オブジェクト変数はさまざまなタイミングでさまざまなクラスインスタンスを参照できます。 この区別は、次の方法で構造体とクラスの使用に影響します。  
  
- **イニシャライズ.** 構造体変数には、構造体のパラメーターなしのコンストラクターを使用した要素の初期化が暗黙的に含まれます。 したがって、`Dim s As struct1` は `Dim s As struct1 = New struct1()`と同じです。  
  
- **変数を割り当てています。** ある構造体変数を別の構造体変数に割り当てるか、または構造体インスタンスをプロシージャ引数に渡すと、すべての変数要素の現在の値が新しい構造体にコピーされます。 あるオブジェクト変数を別のオブジェクト変数に割り当てるか、またはオブジェクト変数をプロシージャに渡すと、参照ポインターだけがコピーされます。  
  
- **何も割り当てません。** 構造体変数に値[Nothing](../../../../visual-basic/language-reference/nothing.md)を割り当てることができますが、インスタンスは引き続き変数に関連付けられます。 変数要素は割り当てによって再初期化されますが、そのメソッドを呼び出してデータ要素にアクセスすることはできます。  
  
     これに対して、オブジェクト変数を `Nothing`に設定した場合、どのクラスインスタンスとも関連付けを解除すると、別のインスタンスを割り当てるまで変数を介してメンバーにアクセスすることはできません。  
  
- **複数のインスタンス。** オブジェクト変数は、異なるタイミングで異なるクラスインスタンスを割り当てることができ、複数のオブジェクト変数が同じクラスインスタンスを同時に参照できます。 クラスメンバーの値に加えた変更は、同じインスタンスを指す別の変数を介してアクセスされた場合に、それらのメンバーに影響を与えます。  
  
     ただし、構造体要素は、独自のインスタンス内で分離されます。 その値に対する変更は、同じ `Structure` 宣言の他のインスタンスであっても、他の構造体変数には反映されません。  
  
- **等.** 2つの構造体の等価テストは、要素ごとのテストで実行する必要があります。 <xref:System.Object.Equals%2A> メソッドを使用して、2つのオブジェクト変数を比較できます。 <xref:System.Object.Equals%2A> 2 つの変数が同じインスタンスを指しているかどうかを示します。  
  
## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [複合データ型](../../../../visual-basic/programming-guide/language-features/data-types/composite-data-types.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [構造体](../../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [構造体とその他のプログラミング要素](../../../../visual-basic/programming-guide/language-features/data-types/structures-and-other-programming-elements.md)
- [クラスとオブジェクト](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
