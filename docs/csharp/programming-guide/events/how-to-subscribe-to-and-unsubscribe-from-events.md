---
title: イベントのサブスクリプションとサブスクリプション解除を行う方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- event handlers [C#], creating
- Code Editor, event handlers
- events [C#], creating using the IDE
ms.assetid: 6319f39f-282c-4173-8a62-6c4657cf51cd
ms.openlocfilehash: 3df357cb15f7f77cefbf360dd9615ce246afe2ea
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75705328"
---
# <a name="how-to-subscribe-to-and-unsubscribe-from-events-c-programming-guide"></a>イベントのサブスクリプションとサブスクリプション解除を行う方法 (C# プログラミング ガイド)
別のクラスによってパブリッシュされるイベントが発生したときに呼び出されるカスタム コードを作成するときは、そのイベントをサブスクライブします。 たとえば、ユーザーがボタンをクリックしたらアプリケーションで何かを行うには、ボタンの `click` イベントをサブスクライブします。  
  
### <a name="to-subscribe-to-events-by-using-the-visual-studio-ide"></a>Visual Studio IDE を使ってイベントをサブスクライブするには  
  
1. **デザイン** ビューに **[プロパティ]** ウィンドウが表示されない場合は、イベント ハンドラーを作成するフォームまたはコントロールを右クリックして、 **[プロパティ]** を選びます。  
  
2. **[プロパティ]** ウィンドウの **[イベント]** ボタンをクリックします。  
  
3. 作成するイベントをダブルクリックします (`Load` イベントなど)。  
  
     Visual C# によって空のイベント ハンドラー メソッドを作成され、コードに追加されます。 または、**コード** ビューを使って手動でコードを追加することもできます。 たとえば、次のコード行では、`Form` クラスで `Load` イベントが発生すると呼び出されるイベント ハンドラー メソッドを宣言しています。  
  
     [!code-csharp[csProgGuideEvents#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#11)]  
  
     イベントをサブスクライブするために必要なコード行も、プロジェクトの Form1.Designer.cs ファイルの `InitializeComponent` メソッドに自動的に生成されます。 次のようなコードです。  
  
    ```csharp
    this.Load += new System.EventHandler(this.Form1_Load);  
    ```  
  
### <a name="to-subscribe-to-events-programmatically"></a>プログラムでイベントをサブスクライブするには  
  
1. シグネチャがイベントのデリゲート シグネチャと一致するイベント ハンドラー メソッドを定義します。 たとえば、イベントが <xref:System.EventHandler> デリゲート型に基づいている場合は、次のコードがメソッド スタブを表します。  
  
    ```csharp
    void HandleCustomEvent(object sender, CustomEventArgs a)  
    {  
       // Do something useful here.  
    }  
    ```  
  
2. 加算代入演算子 (`+=`) を使って、イベントにイベント ハンドラーをアタッチします。 次の例では、`publisher` オブジェクトに `RaiseCustomEvent` という名前のイベントがあるものとします。 イベントをサブスクライブするには、サブスクライバー クラスがそのパブリッシャー クラスを参照する必要があることに注意してください。  
  
    ```csharp
    publisher.RaiseCustomEvent += HandleCustomEvent;  
    ```  
  
     上の構文は、C# 2.0 で新しく追加されたものです。 これは、`new` キーワードを使ってカプセル化するデリゲートを明示的に作成する必要がある C# 1.0 の構文と完全に同等です。  
  
    ```csharp
    publisher.RaiseCustomEvent += new CustomEventHandler(HandleCustomEvent);  
    ```  
  
     また、[ラムダ式](../statements-expressions-operators/lambda-expressions.md)を使用してイベント ハンドラーを指定することもできます。
  
    ```csharp
    public Form1()  
    {  
        InitializeComponent();  
        this.Click += (s,e) =>
            {
                MessageBox.Show(((MouseEventArgs)e).Location.ToString());
            };
    }  
    ```  
  
### <a name="to-subscribe-to-events-by-using-an-anonymous-method"></a>匿名メソッドを使ってイベントをサブスクライブするには  
  
- 後でイベントのサブスクリプションを解除する必要がない場合は、加算代入演算子 (`+=`) を使って匿名メソッドをイベントにアタッチできます。 次の例では、`publisher` オブジェクトに `RaiseCustomEvent` という名前のイベントがあり、`CustomEventArgs` クラスもある種の特別なイベント情報を保持するように定義されているものとします。 イベントをサブスクライブするには、サブスクライバー クラスがその `publisher` クラスを参照する必要があることに注意してください。  
  
    ```csharp
    publisher.RaiseCustomEvent += delegate(object o, CustomEventArgs e)  
    {  
      string s = o.ToString() + " " + e.ToString();  
      Console.WriteLine(s);  
    };  
    ```  
  
     匿名関数を使ってイベントをサブスクライブした場合、簡単にはイベントのサブスクリプションを解除できないことに注意することが重要です。 このシナリオでサブスクリプションを解除するには、イベントをサブスクライブするコードに戻り、匿名メソッドをデリゲート変数に格納して、イベントにデリゲートを追加する必要があります。 一般に、後のコードでイベントのサブスクリプションを解除する必要がある場合は、イベントのサブスクライブに匿名関数を使わないことをお勧めします。 匿名関数について詳しくは、「[匿名関数](../statements-expressions-operators/anonymous-functions.md)」をご覧ください。  
  
## <a name="unsubscribing"></a>サブスクリプションの解除  
 イベントが発生したときにイベント ハンドラーが呼び出されないようにするには、イベントからサブスクリプションを解除します。 リソースのリークを防ぐには、サブスクライバー オブジェクトを破棄する前に、イベントのサブスクリプションを解除する必要があります。 イベントのサブスクリプションを解除するまで、パブリッシュ側オブジェクトでイベントの基盤になっているマルチキャスト デリゲートは、サブスクライバーのイベント ハンドラーをカプセル化するデリゲートへの参照を保持しています。 パブリッシュ側オブジェクトがその参照を保持している限り、ガベージ コレクションはサブスクライバー オブジェクトを削除しません。  
  
#### <a name="to-unsubscribe-from-an-event"></a>イベントのサブスクリプションを解除するには  
  
- イベントのサブスクリプションを解除するには、減算代入演算子 (`-=`) を使います。  
  
    ```csharp
    publisher.RaiseCustomEvent -= HandleCustomEvent;  
    ```  
  
     すべてのサブスクライバーがイベントのサブスクリプションを解除すると、パブリッシャー クラスのイベント インスタンスは `null` に設定されます。  
  
## <a name="see-also"></a>参照

- [イベント](./index.md)
- [event](../../language-reference/keywords/event.md)
- [.NET Framework ガイドラインに準拠したイベントを発行する方法](./how-to-publish-events-that-conform-to-net-framework-guidelines.md)
- [- および -= 演算子](../../language-reference/operators/subtraction-operator.md)
- [+ および += 演算子](../../language-reference/operators/addition-operator.md)
