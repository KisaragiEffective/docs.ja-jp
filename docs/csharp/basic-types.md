---
title: 基本型 - C# ガイド
description: すべての C# プログラムの中核となる型 (数値、文字列、オブジェクト) について説明します
ms.date: 10/10/2016
ms.technology: csharp-fundamentals
ms.assetid: 95c686ba-ae4f-440e-8e94-0dbd6e04d11f
ms.openlocfilehash: bb2177026afb2eef2e14ece0c306bfd3ffe7af39
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77673265"
---
# <a name="types-variables-and-values"></a>型、変数、および値

C# は、厳密に型指定された言語です。 すべての変数および定数は、値に評価されるすべての式がそうであるように、型を持ちます。 すべてのメソッド シグネチャで、各入力パラメーターの型と戻り値の型が指定されます。 .NET Framework クラス ライブラリでは、一連の組み込みの数値型が定義され、さらにファイル システム、ネットワーク接続、オブジェクトのコレクション、オブジェクトの配列、日付など、さまざまな論理構造を表すより複雑な型も定義されています。 一般的な C# プログラムでは、クラス ライブラリで定義されている型と、そのプログラムの問題領域に固有の概念をモデル化するユーザー定義の型が使用されます。  
  
型には、次のような情報が保存されます。  
  
- その型の変数が必要とする記憶領域。  
  
- 表すことができる最大値と最小値。  
  
- 含まれるメンバー (メソッド、フィールド、イベントなど)。  
  
- 継承元となった基本型。  
  
- 実行時に変数に割り当てられるメモリの場所。  
  
- 許可される演算の種類。  
  
コンパイラは型情報を使用して、コード内で実行されるすべての演算が "*タイプ セーフ*" であることを確認します。 たとえば、[int](language-reference/builtin-types/integral-numeric-types.md) 型の変数を宣言すると、その変数は加算演算と減算演算で使用できます。 同じ演算を [bool](language-reference/builtin-types/bool.md) 型の変数に対して実行しようとすると、コンパイラで次の例のようなエラーが発生します。  
  
[!code-csharp[Type Safety](../../samples/snippets/csharp/concepts/basic-types/type-safety.cs)]  
  
> [!NOTE]  
> C や C++ と異なり、C# では、[bool](language-reference/builtin-types/bool.md) を [int](language-reference/builtin-types/integral-numeric-types.md) に変換することはできません。  
  
コンパイラは、型情報を実行可能ファイル内にメタデータとして埋め込みます。 共通言語ランタイム (CLR: Common Language Runtime) は、実行時にこのメタデータを使用して、メモリの割り当て時および再要求時に、タイプ セーフであるかどうかを再度確認します。  

## <a name="specifying-types-in-variable-declarations"></a>変数宣言での型の指定

プログラム内で変数や定数を宣言するときは、その型を指定するか、[var](language-reference/keywords/var.md) キーワードを使用して、コンパイラが型を推論できるようにする必要があります。 次の例では、組み込みの数値型と複雑なユーザー定義の型の両方を使用する変数宣言を示します。  
  
[!code-csharp[Variable Declaration](../../samples/snippets/csharp/concepts/basic-types/variable-declaration.cs)]  
  
メソッドのパラメーターおよび戻り値の型は、メソッド シグネチャで指定します。 入力引数として [int](language-reference/builtin-types/integral-numeric-types.md) を使用する必要があり、戻り値として文字列を返すメソッドのシグネチャを次に示します。  
  
[!code-csharp[Method Signature](../../samples/snippets/csharp/concepts/basic-types/method-signature.cs)]  
  
変数を宣言すると、新しい型を使用してその変数を再度宣言することはできず、宣言された型と互換性のない値をその変数に代入することはできません。 たとえば、[int](language-reference/builtin-types/integral-numeric-types.md) を宣言してから、それに `true` のブール値を代入することはできません。 ただし、たとえば新しい変数に代入するときや、メソッドの引数として渡すときに、値を他の型に変換することは可能です。 データの損失を伴わない "*型変換*" は、コンパイラによって自動的に実行されます。 データの損失を伴う可能性のある変換には、ソース コードに *cast* を記述する必要があります。

詳細については、「[キャストと型変換](programming-guide/types/casting-and-type-conversions.md)」を参照してください。

## <a name="built-in-types"></a>組み込み型

C# には、整数、浮動小数点値、ブール式、テキスト文字、10 進数値などのデータを表現するための標準的な組み込みの数値型が用意されています。 また、組み込みの **string** 型と **object** 型もあります。 これらの型は、すべての C# プログラムで使用できます。 組み込み型の完全な一覧については、[組み込みの型](language-reference/builtin-types/built-in-types.md)に関するページを参照してください。
  
## <a name="custom-types"></a>カスタム型

カスタムの型を独自に作成するには、[struct](language-reference/keywords/class.md)、[class](language-reference/keywords/class.md)、[interface](language-reference/keywords/interface.md)、[enum](language-reference/builtin-types/enum.md) の各構成要素を使用します。 .NET Framework のクラス ライブラリ自体が、マイクロソフトによって提供された、ユーザーが独自のアプリケーションで使用できるカスタムの型のコレクションです。 既定では、クラス ライブラリで最も頻繁に使用される型は任意の C# プログラムで使用可能になっています。 その他の型は、その型が定義されているアセンブリへのプロジェクト参照を明示的に追加した場合にのみ使用可能になります。 コンパイラがアセンブリを参照できるようになると、そのアセンブリ内で宣言されている型の変数 (および定数) をソース コード内で宣言できるようになります。
  
## <a name="generic-types"></a>ジェネリック型

クライアント コードが型のインスタンスを作成したときに提供される実際の型 (*具象型*) のプレースホルダーとして使用される 1 つ以上の*型パラメーター*で、型を宣言することもできます。 このような型は、*ジェネリック型*と呼ばれます。 たとえば、.NET Framework の型 <xref:System.Collections.Generic.List%601> には、慣例により *T* という名前が与えられる 1 つの型パラメーターがあります。この型のインスタンスを作成するときには、たとえば文字列の場合なら、リストに含まれるオブジェクトの型を次のように指定します。  
  
[!code-csharp[Generic types](../../samples/snippets/csharp/concepts/basic-types/generic-type.cs)]
  
型パラメーターを使用することで、同じクラスを再利用して任意の型の要素を格納できます。このとき、各要素を[オブジェクト](language-reference/builtin-types/reference-types.md#the-object-type)に変換する必要はありません。 ジェネリック コレクション クラスが "*厳密に型指定されたコレクション*" と呼ばれるのは、コレクションの要素の固有の型をコンパイラが認識しているためで、たとえば、前の例の `strings` オブジェクトに整数を追加しようとすると、コンパイル時にエラーが発生します。 詳細については、「[ジェネリック](programming-guide/generics/index.md)」を参照してください。

## <a name="implicit-types-anonymous-types-and-tuple-types"></a>暗黙の型、匿名型、および Null 許容型

前に説明したように、[var](language-reference/keywords/var.md) キーワードを使用すると、ローカル変数 (クラスのメンバーではない) の型を暗黙的に指定できます。 変数の型はコンパイル時に決定されますが、その型はコンパイラによって指定されます。 詳細については、「[暗黙的に型指定されたローカル変数](programming-guide/classes-and-structs/implicitly-typed-local-variables.md)」を参照してください。  
  
場合によっては、メソッドの境界を越えて格納したり受け渡したりする予定のない単純な一連の関連値に名前付きの型を作成するのは便利ではないこともあります。 このような場合は、*匿名型*を作成できます。 詳細については、「[匿名型](programming-guide/classes-and-structs/anonymous-types.md)」を参照してください。

１ つのメソッドから複数の値を返したいというのはよくあることです。 そのような場合は、１ つのメソッド呼び出しで複数の値を返す*タプル型*を作成できます。 詳細については、「[タプル](tuples.md)」を参照してください

## <a name="the-common-type-system"></a>共通型システム

.NET Framework で型システムを使用する場合は、次の 2 つの基本事項を理解しておく必要があります。  
  
- 継承の原則がサポートされています。 他の型から型を派生させることができます。派生元の型は "*基本型*" と呼ばれます。 派生した型は、基本型のメソッド、プロパティ、およびその他のメンバーを (若干の制限付きで) 継承します。 基本型もなんらかの他の型から派生できます。この場合、派生した型はその継承階層内の両方の基本型のメンバーを継承します。 <xref:System.Int32> (C# のキーワード: `int`) などの組み込み数値型を含むすべての型は、最終的に <xref:System.Object> (C# のキーワード: `object`) という単一の基本型から派生します。 この一元化された型階層は、[共通型システム](../standard/common-type-system.md) (CTS) と呼ばれます。 C# での継承の詳細については、「[継承](programming-guide/classes-and-structs/inheritance.md)」を参照してください。  
  
- CTS の各型は、"*値型*" または "*参照型*" として定義されます。 これは、.NET クラス ライブラリのすべてのカスタムの型や、ユーザーが独自に定義した型にも当てはまります。 `struct` または `enum` キーワードを使用して定義する型は値型です。 値型の詳細については、[値型](language-reference/builtin-types/value-types.md)に関するページを参照してください。 [class](language-reference/keywords/class.md) キーワードを使用して定義した型は、参照型です。 参照型の詳細については、「[Classes](programming-guide/classes-and-structs/classes.md)」を参照してください。 参照型と値型では、コンパイル時の規則や実行時の動作が異なります。

## <a name="see-also"></a>関連項目

- [構造体型](language-reference/builtin-types/struct.md)
- [列挙型](language-reference/builtin-types/enum.md)
- [クラス](programming-guide/classes-and-structs/classes.md)
