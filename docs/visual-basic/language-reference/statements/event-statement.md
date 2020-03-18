---
title: Event ステートメント
ms.date: 05/12/2018
f1_keywords:
- vb.Event
- vb.Custom
helpviewer_keywords:
- Event statement [Visual Basic]
- declaring events [Visual Basic], syntax
- Public keyword [Visual Basic], Event statements
- Custom keyword [Visual Basic]
- declarations [Visual Basic], events
- event keyword [Visual Basic]
- WithEvents keyword [Visual Basic], Event statements
- events [Visual Basic], declaring
- ByVal keyword [Visual Basic], Event statements
- events [Visual Basic], custom
- ByRef keyword [Visual Basic], Event statements
- declaring user-defined events
ms.assetid: 306ff8ed-74dd-4b6a-bd2f-e91b17474042
ms.openlocfilehash: dd42e3364d96a8a9b3800b3d1f5e94b2fa25bad4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351224"
---
# <a name="event-statement"></a>Event ステートメント
ユーザー定義イベントを宣言します。  
  
## <a name="syntax"></a>構文  
  
```vb  
[ <attrlist> ] [ accessmodifier ] _  
[ Shared ] [ Shadows ] Event eventname[(parameterlist)] _  
[ Implements implementslist ]  
' -or-  
[ <attrlist> ] [ accessmodifier ] _  
[ Shared ] [ Shadows ] Event eventname As delegatename _  
[ Implements implementslist ]  
' -or-  
 [ <attrlist> ] [ accessmodifier ] _  
[ Shared ] [ Shadows ] Custom Event eventname As delegatename _  
[ Implements implementslist ]  
   [ <attrlist> ] AddHandler(ByVal value As delegatename)  
      [ statements ]  
   End AddHandler  
   [ <attrlist> ] RemoveHandler(ByVal value As delegatename)  
      [ statements ]  
   End RemoveHandler  
   [ <attrlist> ] RaiseEvent(delegatesignature)  
      [ statements ]  
   End RaiseEvent  
End Event  
```  
  
## <a name="parts"></a>指定項目  
  
|要素|説明|  
|---|---|  
|`attrlist`|省略可。 このイベントに適用される属性の一覧です。 複数の属性を指定するときは、コンマで区切ります。 [属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)は山かっこ ("`<`" と "`>`") で囲む必要があります。|  
|`accessmodifier`|省略可。 どのようなコードからイベントにアクセスできるのかを指定します。 次のいずれかになります。<br /><br /> -   [パブリック](../../../visual-basic/language-reference/modifiers/public.md): 宣言する要素にアクセスできるすべてのコードがアクセスできます。<br />-   [プロテクト](../../../visual-basic/language-reference/modifiers/protected.md): クラスまたは派生クラス内のコードだけがアクセスできます。<br />-   [Friend](../../../visual-basic/language-reference/modifiers/friend.md): 同じアセンブリ内のコードだけがアクセスできます。<br />-   [プライベート](../../../visual-basic/language-reference/modifiers/private.md): 宣言する要素内のコードだけがアクセスできます。<br /> イベントのクラス、派生クラス、または同じアセンブリ内の[保護されたフレンド](../../language-reference/modifiers/protected-friend.md)のみのコード -   、そのコードにアクセスできます。 <br />イベントのクラスまたは同じアセンブリ内の派生クラス内の[プライベートに保護された](../../language-reference/modifiers/private-protected.md)専用コードを - 、そのコードにアクセスできます。|  
|`Shared`|省略可。 このイベントがクラスまたは構造体の特定のインスタンスに関連付けられないことを指定します。|  
|`Shadows`|省略可。 このイベントが、基本クラスにある、同じ名前を持つプログラミング要素、またはオーバーロードされる要素のセットを宣言し直して隠ぺいすることを示します。 宣言された要素は、他の任意の種類の要素でシャドウできます。<br /><br /> シャドウされた要素は、その要素をシャドウする派生クラスからは使用できません。ただし、シャドウする要素がアクセスできない要素の場合は例外です。 たとえば、`Private` 要素が基本クラスの要素をシャドウすると、`Private` 要素へのアクセス許可を持たないコードは、代わりに基本クラスにアクセスします。|  
|`eventname`|必須。 イベントの名前です。変数の標準的な名前付け規則に従って名前を付けます。|  
|`parameterlist`|省略可。 このイベントのパラメーターを表すローカル変数のリストです。 [パラメーターリスト](../../../visual-basic/language-reference/statements/parameter-list.md)はかっこで囲む必要があります。|  
|`Implements`|省略可。 このイベントがインターフェイスのイベントを実装することを示します。|  
|`implementslist`|`Implements` を指定する場合は、必ず指定します。 実装される `Sub` プロシージャのリストです。 複数のプロシージャを指定するときは、コンマで区切ります。<br /><br /> *implementedprocedure* [, *implementedprocedure* ...]<br /><br /> `implementedprocedure` の構文と指定項目は次のとおりです。<br /><br /> `interface`.`definedname`<br /><br /> -   `interface`-必須。 このプロシージャの包含クラスまたは包含構造体が実装しているインターフェイスの名前です。<br />-   `Definedname`-必須。 `interface` の中でプロシージャを定義するために使用する名前。 これは、`name` (定義されているプロシージャを実装するためにこのプロシージャが使用している名前) と同じである必要はありません。|  
|`Custom`|必須。 `Custom` として宣言されたイベントでは、`AddHandler`、`RemoveHandler`、および `RaiseEvent` の各カスタム アクセサーを定義する必要があります。|  
|`delegatename`|省略可。 イベント ハンドラーの署名を指定するデリゲートの名前。|  
|`AddHandler`|必須。 `AddHandler` アクセサーを宣言します。ここでは、イベント ハンドラーが追加されたとき実行するステートメントを、`AddHandler` ステートメントを使って明示的に指定するか、`Handles` 句を使って暗黙的に指定します。|  
|`End AddHandler`|必須。 `AddHandler` ブロックを終了します。|  
|`value`|必須。 パラメーター名。|  
|`RemoveHandler`|必須。 `RemoveHandler` アクセサーを宣言します。ここでは、イベント ハンドラーが削除されたときに実行するステートメントを、`RemoveHandler` ステートメントを使って指定します。|  
|`End RemoveHandler`|必須。 `RemoveHandler` ブロックを終了します。|  
|`RaiseEvent`|必須。 `RaiseEvent` アクセサーを宣言します。ここでは、イベントが発生したときに実行するステートメントを、`RaiseEvent` ステートメントを使って指定します。 通常は、`AddHandler` アクセサーと `RemoveHandler` アクセサーによって管理されるデリゲートのリストが呼び出されます。|  
|`End RaiseEvent`|必須。 `RaiseEvent` ブロックを終了します。|  
|`delegatesignature`|必須。 `delegatename` デリゲートに必要なパラメーターと一致するパラメーターのリストです。 [パラメーターリスト](../../../visual-basic/language-reference/statements/parameter-list.md)はかっこで囲む必要があります。|  
|`statements`|省略可。 `AddHandler`、`RemoveHandler`、および `RaiseEvent` の各メソッドの本体が含まれるステートメントです。|  
|`End Event`|必須。 `Event` ブロックを終了します。|  
  
## <a name="remarks"></a>コメント  
 宣言したイベントは、`RaiseEvent` ステートメントを使って発生させます。 通常、イベントの宣言と発生は、次のように行われます。  
  
 [!code-vb[VbVbalrEvents#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#13)]  
  
> [!NOTE]
> イベントの引数は、プロシージャの引数と同様に宣言できます。ただし、イベントに対して名前付き引数、`ParamArray` 引数、または `Optional` 引数を指定することはできません。 イベントは値を返しません。  
  
 イベントを処理するためには、`Handles` ステートメントまたは `AddHandler` ステートメントを使用して、イベントをイベント ハンドラー サブルーチンに関連付ける必要があります。 サブルーチンとイベントの署名が一致する必要があります。 共有イベントを処理するには、`AddHandler` ステートメントを使う必要があります。  
  
 `Event` は、モジュール レベルでのみ使用できます。 つまり、イベントの*宣言コンテキスト*はクラス、構造体、モジュール、またはインターフェイスである必要があり、ソースファイル、名前空間、プロシージャ、またはブロックにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。  
  
 ほとんどの状況で、このトピックの「構文」のセクションにある最初の構文を使ってイベントを宣言できますが、 一部のシナリオでは、イベントの動作をより詳細に制御することが必要になります。 このトピックの「構文」セクションの最後には、`Custom` キーワードを使用した構文があります。これを使用すると、カスタム イベントを定義してイベントを詳細に制御できます。 カスタム イベントでは、コードでイベント ハンドラーを追加または削除するときに、つまりコードでイベントを生成するときに、何が起こるかを正確に指定します。 例については、「[方法: カスタムイベントを宣言してメモリを節約する](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-conserve-memory.md)」および「[方法: カスタムイベントを宣言](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-avoid-blocking.md)してブロックを回避する」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、イベントを使用して 10 秒から 0 秒までカウント ダウンします。 このコードは、イベント関連のいくつかのメソッド、プロパティ、およびステートメントの例を示しています。 `RaiseEvent` ステートメントの使用例も含まれています。  
  
 イベントを発生させるクラスをイベント ソース、イベントを処理するメソッドをイベント ハンドラーと呼びます。 イベント ソースでは、生成されるイベントに対して複数のイベント ハンドラーを設定できます。 クラスでイベントが発生すると、そのイベントは、オブジェクトのインスタンスに対するイベントを処理するために選択されたすべてのクラスで発生します。  
  
 また、この例では、ボタン (`Form1`) とテキスト ボックス (`Button1`) を含んだフォーム (`TextBox1`) も使用しています。 ボタンをクリックすると、1 つ目のテキスト ボックスに 10 秒から 0 秒までのカウントダウンが表示されます。 カウントダウンが終わると (10 秒が経過すると)、1 つ目のテキスト ボックスに "Done" と表示されます。  
  
 `Form1` のコードでは、フォームの初期状態と終了状態を指定しています。 イベント発生時に実行されるコードも含まれています。  
  
 この例を使用するには、新しい Windows フォーム プロジェクトを開きます。 次に、`Button1` という名前のボタンと `TextBox1` という名前のテキスト ボックスを、`Form1` という名前のメイン フォームに追加します。 次に、フォームを右クリックし、 **[コードの表示]** をクリックしてコードエディターを開きます。  
  
 `WithEvents` クラスの宣言セクションに、`Form1` 変数を追加します。  
  
 [!code-vb[VbVbalrEvents#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#14)]  
  
 `Form1` のコードに次のコードを追加します。 `Form_Load` や `Button_Click` など、重複して存在する可能性のあるプロシージャを置き換えます。  
  
 [!code-vb[VbVbalrEvents#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#15)]  
  
 F5 キーを押して前の例を実行し、 **[開始]** というラベルのボタンをクリックします。 最初のテキスト ボックスで、秒のカウント ダウンが開始されます。 カウントダウンが終わると (10 秒が経過すると)、1 つ目のテキスト ボックスに "Done" と表示されます。  
  
> [!NOTE]
> `My.Application.DoEvents` メソッドがイベントを処理する方法は、フォームと同じではありません。 フォームでイベントを直接処理するには、マルチスレッドを使用します。 詳細については、「[マネージスレッド処理](../../../standard/threading/index.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [RaiseEvent ステートメント](../../../visual-basic/language-reference/statements/raiseevent-statement.md)
- [Implements ステートメント](../../../visual-basic/language-reference/statements/implements-statement.md)
- [イベント](../../../visual-basic/programming-guide/language-features/events/index.md)
- [AddHandler ステートメント](../../../visual-basic/language-reference/statements/addhandler-statement.md)
- [RemoveHandler ステートメント](../../../visual-basic/language-reference/statements/removehandler-statement.md)
- [!](../../../visual-basic/language-reference/statements/handles-clause.md)
- [Delegate ステートメント](../../../visual-basic/language-reference/statements/delegate-statement.md)
- [方法: カスタム イベントを宣言してメモリを節約する](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-conserve-memory.md)
- [方法: カスタム イベントを宣言してブロックを回避する](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-avoid-blocking.md)
- [Shared](../../../visual-basic/language-reference/modifiers/shared.md)
- [Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)
