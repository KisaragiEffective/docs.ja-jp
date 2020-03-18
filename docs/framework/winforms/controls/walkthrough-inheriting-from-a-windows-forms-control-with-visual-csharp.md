---
title: コントロールから継承する
ms.date: 03/30/2017
helpviewer_keywords:
- inheritance [Windows Forms], custom controls
- inheritance [Windows Forms], control
- Windows Forms controls, inheritance
- inheritance [Windows Forms], walkthroughs
- custom controls [Windows Forms], inheritance
ms.assetid: 09476da0-8d4c-4a4c-b969-649519dfb438
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 713ccf97a73ce9684b9124a121369f22751861d0
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740140"
---
# <a name="walkthrough-inherit-from-a-windows-forms-control-with-c"></a>チュートリアル: Windows フォームコントロールから C\# を継承する

でC#は、*継承*によって強力なカスタムコントロールを作成できます。 継承を使用すると、標準の Windows フォーム コントロールの固有の機能をすべて保持しながら、カスタム機能も組み込んだコントロールを作成できます。 このチュートリアルでは、`ValueButton` という単純な継承されたコントロールを作成します。 このボタンは、標準の Windows フォーム <xref:System.Windows.Forms.Button> コントロールから機能を継承し、`ButtonValue`というカスタムプロパティを公開します。

## <a name="create-the-project"></a>プロジェクトを作成する

新しいプロジェクトを作成するときは、ルート名前空間、アセンブリ名、プロジェクト名を設定し、既定のコンポーネントが適切な名前空間に含まれるようにするために、プロジェクトの名前を指定します。

### <a name="to-create-the-valuebuttonlib-control-library-and-the-valuebutton-control"></a>ValueButtonLib コントロール ライブラリと ValueButton コントロールを作成するには

1. Visual Studio で、新しい**Windows フォームコントロールライブラリ**プロジェクトを作成し、 **valuebuttonlib**という名前を指定します。

     プロジェクト名 `ValueButtonLib` は、既定でルート名前空間にも割り当てられます。 ルート名前空間は、アセンブリ内のコンポーネント名の修飾に使用されます。 たとえば、`ValueButton` という名前のコンポーネントが 2 つのアセンブリに含まれている場合、`ValueButton` を使用して目的の `ValueButtonLib.ValueButton` コンポーネントを指定できます。 詳細については、「[名前空間](../../../csharp/programming-guide/namespaces/index.md)」を参照してください。

2. **ソリューション エクスプローラー**で、 **[UserControl1.cs]** を右クリックし、ショートカット メニューの **[名前の変更]** をクリックします。 ファイル名を**ValueButton.cs**に変更します。 コード要素 " **" へのすべての参照の名前を変更するかどうかをたずねられたら、** [はい]`UserControl1` をクリックします。

3. **ソリューション エクスプローラー**で、 **[ValueButton.cs]** を右クリックし、 **[コードの表示]** をクリックします。

4. `class` ステートメント行、`public partial class ValueButton`を見つけて、このコントロールが継承する <xref:System.Windows.Forms.UserControl> の型を <xref:System.Windows.Forms.Button>に変更します。 これにより、継承されたコントロールが <xref:System.Windows.Forms.Button> コントロールのすべての機能を継承できるようになります。

5. **ソリューション エクスプローラー**で、 **[ValueButton.cs]** ノードを開いて、デザイナーによって生成されたコード ファイル (**ValueButton.Designer.cs**) を表示します。 このファイルを**コード エディター**で開きます。

6. `InitializeComponent` メソッドを見つけて、<xref:System.Windows.Forms.ContainerControl.AutoScaleMode%2A> プロパティを割り当てる行を削除します。 このプロパティは、<xref:System.Windows.Forms.Button> コントロールに存在しません。

7. **[ファイル]** メニューの **[すべて保存]** をクリックして、プロジェクトを保存します。

    > [!NOTE]
    > ビジュアル デザイナーは使用できなくなっています。 <xref:System.Windows.Forms.Button> コントロールは独自の描画を行うため、デザイナーでその外観を変更することはできません。 そのビジュアル表現は、コードで変更されない限り、から継承したクラス (つまり、<xref:System.Windows.Forms.Button>) とまったく同じになります。 UI 要素のないコンポーネントをデザイン サーフェイスに追加することは可能です。

## <a name="add-a-property-to-your-inherited-control"></a>継承されたコントロールにプロパティを追加する

継承された Windows フォーム コントロールの考えられる用途の 1 つとして、外観は標準の Windows フォーム コントロールと同じでありながら、カスタム プロパティを公開するコントロールの作成があります。 このセクションでは、`ButtonValue` というプロパティをコントロールに追加します。

### <a name="to-add-the-value-property"></a>Value プロパティを追加するには

1. **ソリューション エクスプローラー**で、 **[ValueButton.cs]** を右クリックし、ショートカット メニューの **[コードの表示]** をクリックします。

2. `class` ステートメントを見つけます。 `{` の直後に次のコードを入力します。

    ```csharp
    // Creates the private variable that will store the value of your
    // property.
    private int varValue;
    // Declares the property.
    public int ButtonValue
    {
       // Sets the method for retrieving the value of your property.
       get
       {
          return varValue;
       }
       // Sets the method for setting the value of your property.
       set
       {
          varValue = value;
       }
    }
    ```

     このコードでは、`ButtonValue` プロパティを格納し、取得するメソッドを設定しています。 `get` ステートメントは、返された値を `varValue` プライベート変数に格納されている値に設定します。`set` ステートメントは、`value` キーワードを使用してプライベート変数の値を設定します。

3. **[ファイル]** メニューの **[すべて保存]** をクリックして、プロジェクトを保存します。

## <a name="test-the-control"></a>コントロールをテストする

コントロールはスタンドアロン プロジェクトではないため、コンテナー内でホストする必要があります。 コントロールをテストするには、コントロールを実行するテスト プロジェクトを指定する必要があります。 また、コントロールをビルド (コンパイル) して、テスト プロジェクトからアクセスできるようにする必要があります。 このセクションでは、コントロールをビルドし、Windows フォームでテストします。

### <a name="to-build-your-control"></a>コントロールをビルドするには

**[ビルド]** メニューの **[ソリューションのビルド]** をクリックします。 コンパイル エラーも警告も発生せずに、ビルドが正常に完了します。

### <a name="to-create-a-test-project"></a>テスト プロジェクトを作成するには

1. **[ファイル]** メニューの **[追加]** をポイントし、 **[新しいプロジェクト]** をクリックして **[新しいプロジェクトの追加]** ダイアログ ボックスを開きます。

2. **[Visual C#]** ノードの下の **[Windows]** ノードを選択し、 **[Windows フォーム アプリケーション]** をクリックします。

3. **[名前]** ボックスに「 **Test**」と入力します。

4. **ソリューション エクスプローラー**で、テスト プロジェクトの **[参照設定]** ノードを右クリックし、ショートカット メニューの **[参照の追加]** をクリックして **[参照の追加]** ダイアログ ボックスを表示します。

5. **[プロジェクト]** というラベルのタブをクリックします。 ValueButtonLib プロジェクトが**プロジェクト名**の下に一覧表示されます。 プロジェクトをダブルクリックして、テスト プロジェクトへの参照を追加します。

6. **ソリューション エクスプローラー**で、 **[Test]** を右クリックし、 **[ビルド]** をクリックします。

### <a name="to-add-your-control-to-the-form"></a>コントロールをフォームに追加するには

1. **ソリューション エクスプローラー**で、 **[Form1.cs]** を右クリックし、ショートカット メニューの **[デザイナーの表示]** をクリックします。

2. **ツールボックス**で、 **[Valuebuttonlib コンポーネント]** を選択します。 **[ValueButton]** をダブルクリックします。

     **ValueButton** がフォームに表示されます。

3. **[ValueButton]** を右クリックし、ショートカット メニューの **[プロパティ]** をクリックします。

4. **[プロパティ]** ウィンドウで、このコントロールのプロパティを調べます。 これらは、追加のプロパティ ButtonValue がある点を除いて、標準ボタンによって公開されるプロパティと同じであることに注意してください。

5. **Buttonvalue**プロパティを**5**に設定します。

6. **ツールボックス**の **[すべての Windows フォーム]** タブで、 **[ラベル]** をダブルクリックして <xref:System.Windows.Forms.Label> コントロールをフォームに追加します。

7. ラベルをフォームの中央に配置し直します。

8. `valueButton1` をダブルクリックします。

     **コード エディター**が開き、`valueButton1_Click` イベントが表示されます。

9. 次のコード行を挿入します。

    ```csharp
    label1.Text = valueButton1.ButtonValue.ToString();
    ```

10. **ソリューション エクスプローラー**で、 **[Test]** を右クリックし、ショートカット メニューの **[スタートアップ プロジェクトに設定]** をクリックします。

11. **[デバッグ]** メニューの **[デバッグの開始]** をクリックします。

     `Form1` が表示されます。

12. [`valueButton1`] をクリックします。

     `label1` に数字の "5" が表示されます。これは、継承されたコントロールの `ButtonValue` プロパティが、`label1` メソッドによって `valueButton1_Click` に渡されたことを示しています。 このようにして、`ValueButton` コントロールは標準の Windows フォーム ボタンの機能をすべて継承しながら、追加のカスタム プロパティを公開します。

## <a name="see-also"></a>参照

- [方法: [ツールボックス アイテムの選択] ダイアログ ボックスにコントロールを表示する](how-to-display-a-control-in-the-choose-toolbox-items-dialog-box.md)
- [チュートリアル : Visual C# による複合コントロールの作成](walkthrough-authoring-a-composite-control-with-visual-csharp.md)
