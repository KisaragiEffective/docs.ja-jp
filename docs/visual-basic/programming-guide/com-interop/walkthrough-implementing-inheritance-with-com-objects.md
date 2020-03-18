---
title: 'チュートリアル : COM オブジェクトによる継承の実装'
ms.date: 07/20/2015
helpviewer_keywords:
- inheritance [Visual Basic], COM reusability
- base classes [Visual Basic], COM reusability
- inheritance [Visual Basic], walkthroughs
- derived classes [Visual Basic], COM reusability
ms.assetid: f8e7263a-de13-48d1-b67c-ca1adf3544d9
ms.openlocfilehash: 209e1005b9f944bf4883e8406031fb17d4d60df1
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347992"
---
# <a name="walkthrough-implementing-inheritance-with-com-objects-visual-basic"></a>チュートリアル: COM オブジェクトによる継承の実装 (Visual Basic)

以前のバージョンの Visual Basic で作成されたクラスも含めて、COM オブジェクトの `Public` クラスから Visual Basic クラスを派生させることができます。 COM オブジェクトから継承されたクラスのプロパティとメソッドは、他の基底クラスのプロパティおよびメソッドがオーバーライドまたはオーバーロードできるのと同様にオーバーライドまたはオーバーロードできます。 COM オブジェクトからの継承は、再コンパイルしたくない既存のクラスライブラリがある場合に便利です。

次の手順では、Visual Basic 6.0 を使用して、クラスを含む COM オブジェクトを作成し、それを基底クラスとして使用する方法を示します。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-build-the-com-object-that-is-used-in-this-walkthrough"></a>このチュートリアルで使用する COM オブジェクトをビルドするには

1. Visual Basic 6.0 で、新しい ActiveX DLL プロジェクトを開きます。 `Project1` という名前のプロジェクトが作成されます。 このクラスには、`Class1`という名前のクラスがあります。

2. **Project Explorer**で、 **[project1]** を右クリックし、 **[project1 のプロパティ]** をクリックします。 **[プロジェクトのプロパティ]** ダイアログボックスが表示されます。

3. **[プロジェクトのプロパティ]** ダイアログボックスの **[全般**] タブで、 **[プロジェクト名]** フィールドに「`ComObject1`」と入力して、プロジェクト名を変更します。

4. **Project Explorer**で、[`Class1`] を右クリックし、 **[プロパティ]** をクリックします。 クラスの **[プロパティ]** ウィンドウが表示されます。

5. `Name` プロパティを `MathFunctions`に変更します。

6. **Project Explorer**で、[`MathFunctions`] を右クリックし、 **[コードの表示]** をクリックします。 **コードエディター**が表示されます。

7. プロパティ値を保持するローカル変数を追加します。

    ```vb
    ' Local variable to hold property value
    Private mvarProp1 As Integer
    ```

8. プロパティ `Let` プロパティとプロパティ `Get` プロパティプロシージャを追加します。

    ```vb
    Public Property Let Prop1(ByVal vData As Integer)
       'Used when assigning a value to the property.
       mvarProp1 = vData
    End Property
    Public Property Get Prop1() As Integer
       'Used when retrieving a property's value.
       Prop1 = mvarProp1
    End Property
    ```

9. 関数を追加します。

    ```vb
    Function AddNumbers(
       ByVal SomeNumber As Integer,
       ByVal AnotherNumber As Integer) As Integer

       AddNumbers = SomeNumber + AnotherNumber
    End Function
    ```

10. **[ファイル]** メニューの **[ComObject1]** の作成 をクリックして、COM オブジェクトを作成し、登録します。

    > [!NOTE]
    > Visual Basic で作成されたクラスを COM オブジェクトとして公開することもできますが、これは真の COM オブジェクトではなく、このチュートリアルでは使用できません。 詳細については、「 [.NET Framework アプリケーションでの COM 相互運用性](../../../visual-basic/programming-guide/com-interop/com-interoperability-in-net-framework-applications.md)」を参照してください。

## <a name="interop-assemblies"></a>相互運用機能アセンブリ

次の手順では、アンマネージコード (COM オブジェクトなど) と Visual Studio で使用されるマネージコードの間のブリッジとして機能する相互運用機能アセンブリを作成します。 Visual Basic に*よって作成*された相互運用機能アセンブリは、com オブジェクトとの間で移動するときに、パラメーターと戻り値を同等のデータ型にパッケージ化するプロセスなど、com オブジェクトの操作の詳細の多くを処理します。 Visual Basic アプリケーション内の参照は、実際の COM オブジェクトではなく、相互運用機能アセンブリを指します。

### <a name="to-use-a-com-object-with-visual-basic-2005-and-later-versions"></a>Visual Basic 2005 以降のバージョンで COM オブジェクトを使用するには

1. 新しい Visual Basic Windows アプリケーション プロジェクトを開きます。

2. **[プロジェクト]** メニューの **[参照の追加]** をクリックします。

     **[参照の追加]** ダイアログ ボックスが表示されます。

3. **[COM]** タブで、 **[コンポーネント名]** ボックスの一覧の `ComObject1` をダブルクリックし、[ **OK]** をクリックします。

4. **[プロジェクト]** メニューの **[新しい項目の追加]** をクリックします。

     **[新しい項目の追加]** ダイアログ ボックスが表示されます。

5. **[テンプレート]** ペインで、 **[クラス]** をクリックします。

     既定のファイル名 `Class1.vb`が **[名前]** フィールドに表示されます。 このフィールドを MathClass に変更し、 **[追加]** をクリックします。 これにより `MathClass`という名前のクラスが作成され、そのコードが表示されます。

6. COM クラスを継承するために、`MathClass` の先頭に次のコードを追加します。

     [!code-vb[VbVbalrInterop#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#31)]

7. `MathClass`に次のコードを追加して、基底クラスのパブリックメソッドをオーバーロードします。

     [!code-vb[VbVbalrInterop#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#32)]

8. `MathClass`に次のコードを追加して、継承されたクラスを拡張します。

     [!code-vb[VbVbalrInterop#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#33)]

新しいクラスは、COM オブジェクトの基底クラスのプロパティを継承し、メソッドをオーバーロードして、クラスを拡張する新しいメソッドを定義します。

### <a name="to-test-the-inherited-class"></a>継承されたクラスをテストするには

1. スタートアップフォームにボタンを追加し、それをダブルクリックしてコードを表示します。

2. ボタンの `Click` イベントハンドラープロシージャに次のコードを追加して、`MathClass` のインスタンスを作成し、オーバーロードされたメソッドを呼び出します。

     [!code-vb[VbVbalrInterop#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#34)]

3. F5 キーを押してプロジェクトを実行します。

フォームのボタンをクリックすると、`AddNumbers` メソッドが最初に `Short` データ型の数値で呼び出され、Visual Basic 基本クラスから適切なメソッドが選択されます。 `AddNumbers` の2回目の呼び出しは `MathClass`からのオーバーロードメソッドに送られます。 3番目の呼び出しは、クラスを拡張する `SubtractNumbers` メソッドを呼び出します。 基本クラスのプロパティが設定され、値が表示されます。

## <a name="next-steps"></a>次の手順

オーバーロードされた `AddNumbers` 関数は、COM オブジェクトの基底クラスから継承されたメソッドと同じデータ型を持つように見えます。 これは、基底クラスのメソッドの引数とパラメーターが Visual Basic 6.0 で16ビット整数として定義されているのに対し、Visual Basic の以降のバージョンでは `Short` 型の16ビット整数として公開されているためです。 新しい関数は、32ビットの整数を受け取り、基底クラスの関数をオーバーロードします。

COM オブジェクトを使用する場合は、パラメーターのサイズとデータ型を必ず確認してください。 たとえば、引数として Visual Basic 6.0 collection オブジェクトを受け取る COM オブジェクトを使用している場合、Visual Basic の新しいバージョンのコレクションを指定することはできません。

COM クラスから継承されたプロパティとメソッドはオーバーライドできます。つまり、基本 COM クラスから継承されたプロパティまたはメソッドを置き換えるローカルプロパティまたはメソッドを宣言できます。 継承された COM プロパティをオーバーライドするための規則は、次の例外を除き、他のプロパティおよびメソッドをオーバーライドするための規則に似ています。

- COM クラスから継承されたプロパティまたはメソッドをオーバーライドする場合は、継承された他のすべてのプロパティとメソッドをオーバーライドする必要があります。

- `ByRef` パラメーターを使用するプロパティをオーバーライドすることはできません。

## <a name="see-also"></a>参照

- [.NET Framework アプリケーションにおける COM 相互運用性](../../../visual-basic/programming-guide/com-interop/com-interoperability-in-net-framework-applications.md)
- [Inherits ステートメント](../../../visual-basic/language-reference/statements/inherits-statement.md)
- [Short データ型](../../../visual-basic/language-reference/data-types/short-data-type.md)
