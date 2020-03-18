---
title: 'チュートリアル: COM オブジェクトの作成'
ms.date: 07/20/2015
helpviewer_keywords:
- COM interop [Visual Basic], creating COM objects
- COM objects, creating
- COM interop [Visual Basic], walkthroughs
- object creation [Visual Basic], COM objects
- COM objects, walkthroughs
ms.assetid: 7b07a463-bc72-4392-9ba0-9dfcb697a44f
ms.openlocfilehash: 5d00aff07358a0c40159fde9c12c70e0842d848b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74338616"
---
# <a name="walkthrough-creating-com-objects-with-visual-basic"></a>チュートリアル : Visual Basic での COM オブジェクトの作成
新しいアプリケーションまたはコンポーネントを作成する場合は、.NET Framework アセンブリを作成することをお勧めします。 ただし、Visual Basic を使用すると、.NET Framework コンポーネントを COM に簡単に公開できます。 これにより、COM コンポーネントを必要とする以前のアプリケーションスイートに新しいコンポーネントを提供できるようになります。 このチュートリアルでは、Visual Basic を使用して、COM クラステンプレートを使用するかどうかにかかわらず、.NET Framework オブジェクトを COM オブジェクトとして公開する方法について説明します。  
  
 Com オブジェクトを公開する最も簡単な方法は、COM クラステンプレートを使用することです。 COM クラステンプレートは新しいクラスを作成し、クラスと相互運用性レイヤーを COM オブジェクトとして生成し、オペレーティングシステムに登録するようにプロジェクトを構成します。  
  
> [!NOTE]
> Visual Basic で作成されたクラスをアンマネージコードで使用する COM オブジェクトとして公開することもできますが、これは真の COM オブジェクトではなく、Visual Basic では使用できません。 詳細については、「 [.NET Framework アプリケーションでの COM 相互運用性](../../../visual-basic/programming-guide/com-interop/com-interoperability-in-net-framework-applications.md)」を参照してください。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-create-a-com-object-by-using-the-com-class-template"></a>COM クラステンプレートを使用して COM オブジェクトを作成するには  
  
1. **[ファイル]** メニューの **[新しいプロジェクト]** をクリックして、新しい Windows アプリケーションプロジェクトを開きます。  
  
2. **新しいプロジェクト** ダイアログボックスの **プロジェクトの種類** フィールドで、Windows が選択されていることを確認します。 **[テンプレート]** ボックスの一覧から **[クラスライブラリ]** を選択し、[ **OK]** をクリックします。 新しいプロジェクトが表示されます。  
  
3. **[プロジェクト]** メニューの **[新しい項目の追加]** をクリックします。 **[新しい項目の追加]** ダイアログ ボックスが表示されます。  
  
4. **[テンプレート]** ボックスの一覧から **[COM クラス]** を選択し、 **[追加]** をクリックします。 Visual Basic は、新しいクラスを追加し、COM 相互運用のために新しいプロジェクトを構成します。  
  
5. プロパティ、メソッド、イベントなどのコードを COM クラスに追加します。  
  
6. **[ビルド]** メニューの **[ビルドと]** を選択します。 Visual Basic は、アセンブリをビルドし、オペレーティングシステムに COM オブジェクトを登録します。  
  
## <a name="creating-com-objects-without-the-com-class-template"></a>Com クラステンプレートを使用しない COM オブジェクトの作成  
 Com クラステンプレートを使用する代わりに、手動で COM クラスを作成することもできます。 この手順は、コマンドラインから作業する場合や、COM オブジェクトの定義方法をより細かく制御する必要がある場合に役立ちます。  
  
#### <a name="to-set-up-your-project-to-generate-a-com-object"></a>COM オブジェクトを生成するようにプロジェクトを設定するには  
  
1. **[ファイル]** メニューの **[NewProject]** をクリックして、新しい Windows アプリケーションプロジェクトを開きます。  
  
2. **新しいプロジェクト** ダイアログボックスの **プロジェクトの種類** フィールドで、Windows が選択されていることを確認します。 **[テンプレート]** ボックスの一覧から **[クラスライブラリ]** を選択し、[ **OK]** をクリックします。 新しいプロジェクトが表示されます。  
  
3. **ソリューション エクスプローラー**で、プロジェクトを右クリックして、 **[プロパティ]** をクリックします。 **プロジェクトデザイナー**が表示されます。  
  
4. **[コンパイル]** タブをクリックします。  
  
5. [ **COM 相互運用機能に登録**する] チェックボックスをオンにします。  
  
#### <a name="to-set-up-the-code-in-your-class-to-create-a-com-object"></a>COM オブジェクトを作成するためにクラス内のコードを設定するには  
  
1. **ソリューションエクスプローラー**で、 **[Class1]** をダブルクリックしてコードを表示します。  
  
2. クラスの名前を `ComClass1` に変更します。  
  
3. `ComClass1`に次の定数を追加します。 これらは、COM オブジェクトに必要なグローバル一意識別子 (GUID) 定数を格納します。  
  
     [!code-vb[VbVbalrInterop#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#2)]  
  
4. **[ツール]** メニューの **[GUID の作成]** をクリックします。 **[GUID の作成]** ダイアログ ボックスで、 **[レジストリ形式]** をクリックし、 **[コピー]** をクリックします。 **[終了]** をクリックします。  
  
5. `ClassId` の空の文字列を GUID に置き換え、先頭と末尾の中かっこを削除します。 たとえば、Guidgen.exe によって提供された GUID が `"{2C8B0AEE-02C9-486e-B809-C780A11530FE}"` 場合、コードは次のようになります。  
  
     [!code-vb[VbVbalrInterop#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#3)]  
  
6. 次の例に示すように、`InterfaceId` と `EventsId` の定数に対して前の手順を繰り返します。  
  
     [!code-vb[VbVbalrInterop#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#4)]  
  
    > [!NOTE]
    > Guid が新規で一意であることを確認します。それ以外の場合、COM コンポーネントは他の COM コンポーネントと競合する可能性があります。  
  
7. 次の例に示すように、`ComClass` 属性を `ComClass1`に追加して、クラス ID、インターフェイス ID、およびイベント ID の Guid を指定します。  
  
     [!code-vb[VbVbalrInterop#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#5)]  
  
8. COM クラスにはパラメーターなしの `Public Sub New()` コンストラクターが必要です。または、クラスが正しく登録されません。 パラメーターなしのコンストラクターをクラスに追加します。  
  
     [!code-vb[VbVbalrInterop#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#6)]  
  
9. プロパティ、メソッド、およびイベントをクラスに追加し、`End Class` ステートメントで終了します。 **[ビルド]** メニューの **[ソリューションのビルド]** を選択します。 Visual Basic は、アセンブリをビルドし、オペレーティングシステムに COM オブジェクトを登録します。  
  
    > [!NOTE]
    > Visual Basic で生成した COM オブジェクトは、実際の COM オブジェクトではないため、他の Visual Basic アプリケーションでは使用できません。 そのような COM オブジェクトへの参照を追加しようとすると、エラーが発生します。 詳細については、「 [.NET Framework アプリケーションでの COM 相互運用性](../../../visual-basic/programming-guide/com-interop/com-interoperability-in-net-framework-applications.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.ComClassAttribute>
- [COM 相互運用](../../../visual-basic/programming-guide/com-interop/index.md)
- [チュートリアル : COM オブジェクトによる継承の実装](../../../visual-basic/programming-guide/com-interop/walkthrough-implementing-inheritance-with-com-objects.md)
- [#Region ディレクティブ](../../../visual-basic/language-reference/directives/region-directive.md)
- [.NET Framework アプリケーションにおける COM 相互運用性](../../../visual-basic/programming-guide/com-interop/com-interoperability-in-net-framework-applications.md)
- [相互運用性のトラブルシューティング](../../../visual-basic/programming-guide/com-interop/troubleshooting-interoperability.md)
