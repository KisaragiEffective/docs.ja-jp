---
title: IEnumerable の実装
ms.date: 07/31/2018
helpviewer_keywords:
- control flow [Visual Basic]
- enumerable interfaces
- loop structures [Visual Basic], optimizing performance
- control flow [Visual Basic]
ms.assetid: c60d7589-51f2-4463-a2d5-22506bbc1554
ms.openlocfilehash: f40fcf7e0724addc478b261dcd36d09e1d8a751a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74333688"
---
# <a name="walkthrough-implementing-ienumerableof-t-in-visual-basic"></a>チュートリアル: Visual Basic での IEnumerable(Of T) の実装
<xref:System.Collections.Generic.IEnumerable%601> インターフェイスは、一度に1つの値のシーケンスを返すことができるクラスによって実装されます。 データを一度に1つずつ返す利点は、データの完全なセットをメモリに読み込んで使用する必要がないことです。 データから1つの項目を読み込むには、十分なメモリを使用する必要があります。 `IEnumerable(T)` インターフェイスを実装するクラスは、`For Each` ループまたは LINQ クエリで使用できます。  
  
 たとえば、大きなテキストファイルを読み取って、特定の検索条件に一致するファイルから各行を返すアプリケーションがあるとします。 アプリケーションでは、LINQ クエリを使用して、指定した条件に一致する行をファイルから取得します。 LINQ クエリを使用してファイルの内容を照会するために、アプリケーションでは、ファイルの内容を配列またはコレクションに読み込むことができます。 ただし、ファイル全体を配列またはコレクションに読み込むと、必要以上に多くのメモリが消費されます。 LINQ クエリでは、列挙可能なクラスを使用してファイルの内容に対してクエリを実行し、検索条件に一致する値のみを返すこともできます。 一致する値をいくつか返すクエリでは、使用するメモリがはるかに少なくなります。  
  
 <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装するクラスを作成して、ソースデータを列挙可能なデータとして公開することができます。 `IEnumerable(T)` インターフェイスを実装するクラスには、ソースデータを反復処理するために <xref:System.Collections.Generic.IEnumerator%601> インターフェイスを実装する別のクラスが必要です。 これらの2つのクラスを使用すると、特定の型としてデータの項目を順番に返すことができます。  
  
 このチュートリアルでは、`IEnumerable(Of String)` インターフェイスを実装するクラスと、テキストファイルを一度に1行読み取るための `IEnumerator(Of String)` インターフェイスを実装するクラスを作成します。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="creating-the-enumerable-class"></a>列挙可能なクラスの作成  
  
**列挙可能なクラスプロジェクトを作成する**

1. Visual Basic で、 **[ファイル]** メニューの **[新規作成]** をポイントし、 **[プロジェクト]** をクリックします。

1. **[新しいプロジェクト]** ダイアログ ボックスの **[プロジェクトの種類]** ペインで、 **[Windows]** が選択されていることを確認します。 **[テンプレート]** ペインで **[クラス ライブラリ]** を選択します。 **[名前]** ボックスに `StreamReaderEnumerable` と入力して、 **[OK]** をクリックします。 新しいプロジェクトが表示されます。

1. **ソリューションエクスプローラー**で、Class1 ファイルを右クリックし、[名前の**変更**] をクリックします。 ファイルの名前を `StreamReaderEnumerable.vb` に変更し、Enter キーを押します。 ファイルの名前を変更すると、クラスの名前も `StreamReaderEnumerable` に変更されます。 このクラスが `IEnumerable(Of String)` インターフェイスを実装します。

1. StreamReaderEnumerable プロジェクトを右クリックし、 **[追加]** をポイントして、 **[新しい項目]** をクリックします。 **クラス**テンプレートを選択します。 **[名前]** ボックスに「`StreamReaderEnumerator.vb`」と入力し、[ **OK]** をクリックします。

 このプロジェクトの最初のクラスは列挙可能なクラスであり、`IEnumerable(Of String)` インターフェイスを実装します。 このジェネリックインターフェイスは <xref:System.Collections.IEnumerable> インターフェイスを実装し、このクラスのコンシューマーが `String`として型指定された値にアクセスできることを保証します。  
  
**IEnumerable を実装するコードを追加します。**

1. StreamReaderEnumerable ファイルを開きます。

2. `Public Class StreamReaderEnumerable`の後の行で、次のように入力し、enter キーを押します。

     [!code-vb[VbVbalrIteratorWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/StreamReaderIterator.vb#1)]

   Visual Basic は、`IEnumerable(Of String)` インターフェイスに必要なメンバーをクラスに自動的に設定します。
  
3. この列挙可能なクラスは、一度に1行ずつテキストファイルから行を読み取ります。 次のコードをクラスに追加して、入力パラメーターとしてファイルパスを受け取るパブリックコンストラクターを公開します。

     [!code-vb[VbVbalrIteratorWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/StreamReaderIterator.vb#2)]

4. `IEnumerable(Of String)` インターフェイスの <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> メソッドを実装すると、`StreamReaderEnumerator` クラスの新しいインスタンスが返されます。 `IEnumerable(Of String)` インターフェイスのメンバーのみを公開する必要があるため、`IEnumerable` インターフェイスの `GetEnumerator` メソッドの実装は `Private`できます。 `GetEnumerator` メソッドに対して生成された Visual Basic コードを次のコードに置き換えます。

     [!code-vb[VbVbalrIteratorWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/StreamReaderIterator.vb#3)]  
  
**IEnumerator を実装するコードを追加する**

1. StreamReaderEnumerator .vb ファイルを開きます。

2. `Public Class StreamReaderEnumerator`の後の行で、次のように入力し、enter キーを押します。

     [!code-vb[VbVbalrIteratorWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/StreamReaderIterator.vb#4)]

   Visual Basic は、`IEnumerator(Of String)` インターフェイスに必要なメンバーをクラスに自動的に設定します。

3. 列挙子クラスはテキストファイルを開き、ファイルの行を読み取るためにファイル i/o を実行します。 次のコードをクラスに追加して、ファイルパスを入力パラメーターとして受け取り、テキストファイルを読み取り用に開くパブリックコンストラクターを公開します。

     [!code-vb[VbVbalrIteratorWalkthrough#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/StreamReaderIterator.vb#5)]

4. `IEnumerator(Of String)` インターフェイスと `IEnumerator` インターフェイスの両方の `Current` プロパティは、テキストファイルから現在の項目を `String`として返します。 `IEnumerator(Of String)` インターフェイスのメンバーのみを公開する必要があるため、`IEnumerator` インターフェイスの `Current` プロパティの実装は `Private`できます。 `Current` のプロパティに対して生成 Visual Basic コードを次のコードに置き換えます。

     [!code-vb[VbVbalrIteratorWalkthrough#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/StreamReaderIterator.vb#6)]

5. `IEnumerator` インターフェイスの `MoveNext` メソッドは、テキストファイル内の次の項目に移動し、`Current` プロパティによって返される値を更新します。 読み取る項目がそれ以上ない場合、`MoveNext` メソッドは `False`を返します。それ以外の場合、`MoveNext` メソッドは `True`を返します。 `MoveNext` メソッドに次のコードを追加します。

     [!code-vb[VbVbalrIteratorWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/StreamReaderIterator.vb#7)]

6. `IEnumerator` インターフェイスの `Reset` メソッドは、テキストファイルの先頭を指すように反復子に指示し、現在の項目の値をクリアします。 `Reset` メソッドに次のコードを追加します。

     [!code-vb[VbVbalrIteratorWalkthrough#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/StreamReaderIterator.vb#8)]

7. `IEnumerator` インターフェイスの `Dispose` メソッドは、反復子が破棄される前にすべてのアンマネージリソースが解放されることを保証します。 `StreamReader` オブジェクトによって使用されるファイルハンドルはアンマネージリソースであり、反復子インスタンスが破棄される前に閉じる必要があります。 `Dispose` メソッドに対して生成 Visual Basic コードを次のコードに置き換えます。

     [!code-vb[VbVbalrIteratorWalkthrough#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/StreamReaderIterator.vb#9)] 
  
## <a name="using-the-sample-iterator"></a>サンプル反復子の使用

 コード内で列挙可能なクラスを、`For Next` ループや LINQ クエリなどの `IEnumerable`を実装するオブジェクトを必要とする制御構造と共に使用できます。 次の例は、LINQ クエリの `StreamReaderEnumerable` を示しています。  
  
 [!code-vb[VbVbalrIteratorWalkthrough#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIteratorWalkthrough/VB/Module1.vb#10)]  
  
## <a name="see-also"></a>参照

- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [制御フロー](../../../../visual-basic/programming-guide/language-features/control-flow/index.md)
- [ループ構造](../../../../visual-basic/programming-guide/language-features/control-flow/loop-structures.md)
- [For Each...Next ステートメント](../../../../visual-basic/language-reference/statements/for-each-next-statement.md)
