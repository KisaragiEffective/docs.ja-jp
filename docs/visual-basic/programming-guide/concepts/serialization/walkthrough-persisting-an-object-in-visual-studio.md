---
title: オブジェクトの永続化 (Visual Basic)
ms.date: 07/20/2015
ms.assetid: f1d0b562-e349-4dce-ab5f-c05108467030
ms.openlocfilehash: fbd342c929e8519571c0f6bb76d4091efcfe4476
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350395"
---
# <a name="walkthrough-persisting-an-object-in-visual-studio-visual-basic"></a>チュートリアル: Visual Studio でのオブジェクトの永続化 (Visual Basic)
オブジェクトのプロパティはデザイン時に既定値に設定できますが、そのオブジェクトが破棄されると、実行時に入力した値はすべて失われます。 シリアル化によってインスタンス間でオブジェクトのデータを永続化すると、値を保存しておき、次にそのオブジェクトをインスタンス化するときに、その値を取得することができます。  
  
> [!NOTE]
> Visual Basic では、`My.Settings` オブジェクトを使用して、名前や数値などの単純なデータを保存できます。 詳細については、「[My.Settings オブジェクト](../../../../visual-basic/language-reference/objects/my-settings-object.md)」を参照してください。  
  
 このチュートリアルでは、簡単な `Loan` オブジェクトを作成し、そのデータをファイルに永続化します。 その後、オブジェクトを再作成するときに、そのファイルからデータを取得します。  
  
> [!IMPORTANT]
> 次のコード例では、ファイルが存在しない場合は新規にファイルを作成します。 アプリケーションでファイルを作成する必要がある場合、そのアプリケーションには、フォルダーに対する `Create` アクセス許可が必要です。 アクセス許可は、アクセス制御リストを使用して設定します。 ファイルが既に存在する場合、アプリケーションに必要なのは下位の `Write` アクセス許可だけです。 可能な場合は、(フォルダーに対して Create アクセス許可を付与するのではなく) 配置時にファイルを作成し、1 つのファイルに対してのみ `Read` アクセス許可を付与する方が安全です。 また、ルート フォルダーや Program Files フォルダーにデータを書き込むよりも、ユーザー フォルダーに書き込む方が安全です。  
  
> [!IMPORTANT]
> この例では、バイナリにデータを格納します。 この形式は、パスワードやクレジット カード情報などの重要情報には使用しないでください。  
  
> [!NOTE]
> 実際に画面に表示されるダイアログ ボックスとメニュー コマンドは、アクティブな設定またはエディションによっては、ヘルプの説明と異なる場合があります。 設定を変更するには、 **[ツール]** メニューの **[設定のインポートとエクスポート]** をクリックします。 詳細については、「[Visual Studio IDE のカスタマイズ](/visualstudio/ide/personalizing-the-visual-studio-ide)」を参照してください。  
  
## <a name="creating-the-loan-object"></a>Loan オブジェクトの作成  
 まず、`Loan` クラスとそのクラスを使用するテスト アプリケーションを作成します。  
  
### <a name="to-create-the-loan-class"></a>Loan クラスを作成するには  
  
1. 新しいクラス ライブラリ プロジェクトを作成して、"LoanClass" という名前を付けます。 詳細については、「[ソリューションとプロジェクトの作成](https://docs.microsoft.com/visualstudio/ide/creating-solutions-and-projects)」を参照してください。  
  
2. **ソリューション エクスプローラー**で、Class1 ファイルのショートカット メニューを開き、 **[名前の変更]** を選択します。 ファイルの名前を `Loan` に変更し、Enter キーを押します。 ファイルの名前を変更すると、クラスの名前も `Loan` に変更されます。  
  
3. クラスに次のパブリック メンバーを追加します。  
  
    ```vb  
    Public Class Loan  
        Implements System.ComponentModel.INotifyPropertyChanged  
  
        Public Property LoanAmount As Double  
        Public Property InterestRate As Double  
        Public Property Term As Integer  
  
        Private p_Customer As String  
        Public Property Customer As String  
            Get  
                Return p_Customer  
            End Get  
            Set(ByVal value As String)  
                p_Customer = value  
                RaiseEvent PropertyChanged(Me,  
                  New System.ComponentModel.PropertyChangedEventArgs("Customer"))  
            End Set  
        End Property  
  
        Event PropertyChanged As System.ComponentModel.PropertyChangedEventHandler _  
          Implements System.ComponentModel.INotifyPropertyChanged.PropertyChanged  
  
        Public Sub New(ByVal loanAmount As Double,  
                       ByVal interestRate As Double,  
                       ByVal term As Integer,  
                       ByVal customer As String)  
  
            Me.LoanAmount = loanAmount  
            Me.InterestRate = interestRate  
            Me.Term = term  
            p_Customer = customer  
        End Sub  
    End Class  
    ```  
  
 また、`Loan` クラスを使用する簡単なアプリケーションも作成する必要があります。  
  
### <a name="to-create-a-test-application"></a>テスト アプリケーションを作成するには  
  
1. **[ファイル]** メニューで **[追加]** 、 **[新しいプロジェクト]** の順に選択して、Windows フォーム アプリケーション プロジェクトをソリューションに追加します。  
  
2. **[新しいプロジェクトの追加]** ダイアログ ボックスで、 **[Windows フォーム アプリケーション]** を選択し、プロジェクト名として「`LoanApp`」と入力します。次に、 **[OK]** をクリックしてダイアログ ボックスを閉じます。  
  
3. **ソリューション エクスプローラー** で LoanApp プロジェクトを選択します。  
  
4. **[プロジェクト]** メニューの **[スタートアップ プロジェクトに設定]** をクリックします。  
  
5. **[プロジェクト]** メニューの **[参照の追加]** をクリックします。  
  
6. **[参照の追加]** ダイアログ ボックスで、 **[プロジェクト]** タブをクリックし、LoanClass プロジェクトを選択します。  
  
7. **[OK]** をクリックして、ダイアログ ボックスを閉じます。  
  
8. デザイナーで、フォームに <xref:System.Windows.Forms.TextBox> コントロールを 4 つ追加します。  
  
9. コード エディターで、次のコードを追加します。  
  
    ```vb  
    Private WithEvents TestLoan As New LoanClass.Loan(10000.0, 0.075, 36, "Neil Black")  
  
    Private Sub Form1_Load() Handles MyBase.Load  
        TextBox1.Text = TestLoan.LoanAmount.ToString  
        TextBox2.Text = TestLoan.InterestRate.ToString  
        TextBox3.Text = TestLoan.Term.ToString  
        TextBox4.Text = TestLoan.Customer  
    End Sub  
    ```  
  
10. 次のコードを使用して、`PropertyChanged` イベントのイベント ハンドラーをフォームに追加します。  
  
    ```vb  
    Public Sub CustomerPropertyChanged(  
          ByVal sender As Object,  
          ByVal e As System.ComponentModel.PropertyChangedEventArgs  
        ) Handles TestLoan.PropertyChanged  
  
        MsgBox(e.PropertyName & " has been changed.")  
    End Sub  
    ```  
  
 この時点で、アプリケーションをビルドして実行できます。 `Loan` クラスの既定値が、テキスト ボックスに表示されます。 利率の値を 7.5 から 7.1 に変更し、アプリケーションをいったん閉じてから、再び実行してください。値が既定値の 7.5 に戻ります。  
  
 実際には利率は定期的に変わりますが、アプリケーションを実行するたびに変わるとは限りません。 アプリケーションを実行するたびにユーザーが利率を更新するのではなく、アプリケーションのインスタンス間で最新の利率を保持できるようにすると便利です。 次の手順では、Loan クラスにシリアル化を追加して、利率を保持できるようにします。  
  
## <a name="using-serialization-to-persist-the-object"></a>シリアル化を使用したオブジェクトの永続化  
 Loan クラスの値を永続化するには、まず、クラスを `Serializable` 属性でマークする必要があります。  
  
### <a name="to-mark-a-class-as-serializable"></a>クラスをシリアル化可能としてマークするには  
  
- Loan クラスのクラス宣言を次のように変更します。  
  
    ```vb  
    <Serializable()>  
    Public Class Loan  
    ```  
  
 `Serializable` 属性は、クラス内のすべての要素がファイルに永続化できることをコンパイラに示します。 `PropertyChanged` イベントは Windows フォーム オブジェクトで処理されるためシリアル化できません。 永続化しないクラス メンバーは、`NonSerialized` 属性でマークできます。  
  
### <a name="to-prevent-a-member-from-being-serialized"></a>メンバーをシリアル化の対象から除外するには  
  
- `PropertyChanged` イベントの宣言を次のように変更します。  
  
    ```vb  
    <NonSerialized()>  
    Event PropertyChanged As System.ComponentModel.PropertyChangedEventHandler _  
      Implements System.ComponentModel.INotifyPropertyChanged.PropertyChanged  
    ```  
  
 次に、LoanApp アプリケーションにシリアル化コードを追加します。 クラスをシリアル化してファイルに書き込むには、<xref:System.IO> 名前空間と <xref:System.Xml.Serialization> 名前空間を使用します。 必要なクラス ライブラリへの参照を追加すると、完全修飾名の入力が不要になります。  
  
### <a name="to-add-references-to-namespaces"></a>名前空間に参照を追加するには  
  
- `Form1` クラスの先頭に、次のステートメントを追加します。  
  
    ```vb  
    Imports System.IO  
    Imports System.Runtime.Serialization.Formatters.Binary  
    ```  
  
     この場合は、バイナリ フォーマッタを使用して、バイナリ形式でオブジェクトを保存します。  
  
 次の手順では、オブジェクトの作成時にファイルからオブジェクトを逆シリアル化するコードを追加します。  
  
### <a name="to-deserialize-an-object"></a>オブジェクトを逆シリアル化するには  
  
1. シリアル化されたデータのファイル名を定数としてクラスに追加します。  
  
    ```vb  
    Const FileName As String = "..\..\SavedLoan.bin"  
    ```  
  
2. `Form1_Load` イベント プロシージャのコードを次のように変更します。  
  
    ```vb  
    Private WithEvents TestLoan As New LoanClass.Loan(10000.0, 0.075, 36, "Neil Black")  
  
    Private Sub Form1_Load() Handles MyBase.Load  
        If File.Exists(FileName) Then  
            Dim TestFileStream As Stream = File.OpenRead(FileName)  
            Dim deserializer As New BinaryFormatter  
            TestLoan = CType(deserializer.Deserialize(TestFileStream), LoanClass.Loan)  
            TestFileStream.Close()  
        End If  
  
        AddHandler TestLoan.PropertyChanged, AddressOf Me.CustomerPropertyChanged  
  
        TextBox1.Text = TestLoan.LoanAmount.ToString  
        TextBox2.Text = TestLoan.InterestRate.ToString  
        TextBox3.Text = TestLoan.Term.ToString  
        TextBox4.Text = TestLoan.Customer  
    End Sub  
    ```  
  
     まず、ファイルが存在することを確認する必要があります。 ファイルが存在する場合は、バイナリ ファイルを読み取る <xref:System.IO.Stream> クラスと、ファイルを変換する <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> クラスを作成します。 ストリーム型を Loan オブジェクト型に変換する必要もあります。  
  
 次に、テキスト ボックスに入力されたデータを `Loan` クラスに保存するコードを追加します。その後、クラスをファイルにシリアル化する必要があります。  
  
### <a name="to-save-the-data-and-serialize-the-class"></a>データを保存してクラスをシリアル化するには  
  
- `Form1_FormClosing` イベント プロシージャに次のコードを追加します。  
  
    ```vb  
    Private Sub Form1_FormClosing() Handles MyBase.FormClosing  
        TestLoan.LoanAmount = CDbl(TextBox1.Text)  
        TestLoan.InterestRate = CDbl(TextBox2.Text)  
        TestLoan.Term = CInt(TextBox3.Text)  
        TestLoan.Customer = TextBox4.Text  
  
        Dim TestFileStream As Stream = File.Create(FileName)  
        Dim serializer As New BinaryFormatter  
        serializer.Serialize(TestFileStream, TestLoan)  
        TestFileStream.Close()  
    End Sub  
    ```  
  
 この時点で、アプリケーションを再度ビルドして実行できます。 最初に既定値がテキスト ボックスに表示されます。 値を変更して、4 番目のテキスト ボックスに名前を入力します。 いったんアプリケーションを閉じて、再び実行します。 これで、新しい値がテキスト ボックスに表示されます。  
  
## <a name="see-also"></a>関連項目

- [シリアル化 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/index.md)
- [Visual Basic のプログラミング ガイド](../../../../visual-basic/programming-guide/index.md)
