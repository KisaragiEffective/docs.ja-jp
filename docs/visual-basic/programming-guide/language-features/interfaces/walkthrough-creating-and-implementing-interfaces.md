---
title: インターフェイスの作成と実装
ms.date: 07/20/2015
helpviewer_keywords:
- interfaces [Visual Basic], walkthroughs
- interfaces [Visual Basic], testing
- interface implementation [Visual Basic], walkthrough
- interfaces [Visual Basic], creating
ms.assetid: ded82af2-9f52-4232-98ef-fe458180f112
ms.openlocfilehash: 47176d2e7a512d8e8c27a90ac04d2a2a2af274b5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345040"
---
# <a name="walkthrough-creating-and-implementing-interfaces-visual-basic"></a>チュートリアル: インターフェイスの作成と実装 (Visual Basic)

インターフェイスには、プロパティ、メソッド、およびイベントの特性が記述されていますが、実装の詳細は構造体またはクラスにはありません。  
  
 このチュートリアルでは、インターフェイスを宣言して実装する方法について説明します。  
  
> [!NOTE]
> このチュートリアルでは、ユーザーインターフェイスの作成方法については説明しません。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="to-define-an-interface"></a>インターフェイスを定義するには
  
1. 新しい Visual Basic Windows アプリケーション プロジェクトを開きます。  
  
2. **[プロジェクト]** メニューの **[モジュールの追加]** をクリックして、新しいモジュールをプロジェクトに追加します。  
  
3. 新しいモジュールに `Module1.vb` 名前を指定し、 **[追加]** をクリックします。 新しいモジュールのコードが表示されます。  
  
4. `Module` と `End Module` のステートメントの間に `Interface TestInterface` を入力し、ENTER キーを押して、`Module1` 内の `TestInterface` という名前のインターフェイスを定義します。 **コードエディター**によって `Interface` キーワードがインデントされ、コードブロックを形成するための `End Interface` ステートメントが追加されます。  
  
5. `Interface` と `End Interface` ステートメントの間に次のコードを配置して、インターフェイスのプロパティ、メソッド、およびイベントを定義します。  
  
     [!code-vb[VbVbalrOOP#98](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#98)]
  
## <a name="implementation"></a>実装

 インターフェイスメンバーの宣言に使用される構文は、クラスメンバーの宣言に使用される構文とは異なることがあります。 この違いは、インターフェイスに実装コードを含めることができないという事実を反映しています。  
  
### <a name="to-implement-the-interface"></a>インターフェイスを実装するには
  
1. `Module1`に次のステートメントを追加して、`ImplementationClass` という名前のクラスを追加します。そのためには、`End Interface` ステートメントの後、`End Module` ステートメントの前に、ENTER キーを押します。  
  
     [!code-vb[VbVbalrOOP#99](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#99)]
  
     統合開発環境内で作業している場合は、ENTER キーを押すと、一致する `End Class` ステートメントが**コードエディター**に表示されます。  
  
2. 次の `Implements` ステートメントを `ImplementationClass`に追加します。これは、クラスが実装するインターフェイスの名前を指定します。  
  
     [!code-vb[VbVbalrOOP#100](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#100)]
  
     クラスまたは構造体の先頭にある他の項目とは別にリストされている場合、`Implements` ステートメントは、クラスまたは構造体がインターフェイスを実装していることを示します。  
  
     統合開発環境内で作業している場合、ENTER キーを押したときに `TestInterface` に必要なクラスメンバーが**コードエディター**に実装され、次の手順は省略できます。  
  
3. 統合開発環境内で作業していない場合は、`MyInterface`インターフェイスのすべてのメンバーを実装する必要があります。 `ImplementationClass` に次のコードを追加して、`Event1`、`Method1`、および `Prop1`を実装します。  
  
     [!code-vb[VbVbalrOOP#101](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#101)]
  
     `Implements` ステートメントは、実装されているインターフェイスとインターフェイスメンバーの名前を記述します。  
  
4. プロパティ値を格納したクラスにプライベートフィールドを追加して、`Prop1` の定義を完了します。  
  
     [!code-vb[VbVbalrOOP#102](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#102)]
  
     プロパティ get アクセサーから `pval` の値を返します。  
  
     [!code-vb[VbVbalrOOP#103](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#103)]
  
     プロパティセットアクセサーの `pval` の値を設定します。  
  
     [!code-vb[VbVbalrOOP#104](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#104)]
  
5. 次のコードを追加して、`Method1` の定義を完了します。  
  
     [!code-vb[VbVbalrOOP#105](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#105)]
  
### <a name="to-test-the-implementation-of-the-interface"></a>インターフェイスの実装をテストするには
  
1. **ソリューションエクスプローラー**でプロジェクトのスタートアップフォームを右クリックし、 **[コードの表示]** をクリックします。 エディターに、スタートアップフォームのクラスが表示されます。 既定では、スタートアップフォームは `Form1`と呼ばれます。  
  
2. 次の `testInstance` フィールドを `Form1` クラスに追加します。  
  
     [!code-vb[VbVbalrOOP#120](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#120)]
  
     `testInstance` を `WithEvents`として宣言することで、`Form1` クラスはそのイベントを処理できます。  
  
3. `testInstance`によって発生したイベントを処理するために、次のイベントハンドラーを `Form1` クラスに追加します。  
  
     [!code-vb[VbVbalrOOP#106](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#106)]
  
4. `Test` という名前のサブルーチンを `Form1` クラスに追加して、実装クラスをテストします。  
  
     [!code-vb[VbVbalrOOP#107](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#107)]
  
     `Test` プロシージャは、`MyInterface`を実装するクラスのインスタンスを作成し、そのインスタンスを `testInstance` フィールドに割り当て、プロパティを設定して、インターフェイスを介してメソッドを実行します。  
  
5. スタートアップフォームの `Form1 Load` プロシージャから `Test` プロシージャを呼び出すコードを追加します。  
  
     [!code-vb[VbVbalrOOP#108](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#108)]
  
6. F5 キーを押して `Test` プロシージャを実行します。 "Prop1 が9に設定されました" というメッセージが表示されます。 [OK] をクリックすると、"Method1 の X パラメーターは 5" というメッセージが表示されます。 [OK] をクリックすると、"イベントをキャッチしたイベントハンドラー" というメッセージが表示されます。  
  
## <a name="see-also"></a>参照

- [Implements ステートメント](../../../../visual-basic/language-reference/statements/implements-statement.md)
- [インターフェイス](../../../../visual-basic/programming-guide/language-features/interfaces/index.md)
- [Interface ステートメント](../../../../visual-basic/language-reference/statements/interface-statement.md)
- [Event ステートメント](../../../../visual-basic/language-reference/statements/event-statement.md)
