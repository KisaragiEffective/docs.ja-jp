---
title: 'チュートリアル: Visual Studio でマネージド アセンブリからの型を埋め込む'
ms.date: 08/19/2019
ms.assetid: 55ed13c9-c5bb-4bc2-bcd8-0587eb568864
dev_langs:
- csharp
- vb
ms.openlocfilehash: f11fbedad766753ee462c5f597b823493cdaf7cf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75338552"
---
# <a name="walkthrough-embed-types-from-managed-assemblies-in-visual-studio"></a>チュートリアル: Visual Studio でマネージド アセンブリからの型を埋め込む

厳密な名前を持つマネージド アセンブリから型情報を埋め込むと、アプリケーション内で型を疎結合して、バージョンに依存しないプログラムを実現できます。 つまり、任意のバージョンのマネージド ライブラリの型を使用してプログラムを記述でき、新しいバージョンごとに再コンパイルする必要はありません。

型の埋め込みは、COM 相互運用と共によく使用されます (Microsoft Office からのオートメーション オブジェクトを使用するアプリケーションなど)。 型情報を埋め込むと、同じビルドのプログラムを、別のコンピューター上にある別バージョンの Microsoft Office と連携させることができます。 ただし、フル マネージド ソリューションで型の埋め込みを使用することもできます。

埋め込み可能なパブリック インターフェイスを指定した後、それらのインターフェイスを実装するランタイム クラスを作成します。 クライアント プログラムでは、パブリック インターフェイスが含まれるアセンブリを参照し、参照の `Embed Interop Types` プロパティを `True` に設定することで、設計時にインターフェイスの型情報を埋め込むことができます。 クライアント プログラムでは、その後、それらのインターフェイスとして型指定されたランタイム オブジェクトのインスタンスを読み込むことができます。 これは、コマンド ライン コンパイラを使用し、[-link コンパイラ オプション](../../csharp/language-reference/compiler-options/link-compiler-option.md)を使用してアセンブリを参照することに相当します。

厳密な名前を持つランタイム アセンブリの新バージョンを作成した場合でも、クライアント プログラムを再コンパイルする必要はありません。 クライアント プログラムでは、パブリック インターフェイスの埋め込まれた型情報を使用して、利用可能などのバージョンでも引き続き使用できます。

このチュートリアルでは、次の作業を行います。

1. 埋め込み可能な型情報が含まれるパブリック インターフェイスを使用して、厳密な名前を持つアセンブリを作成する。
1. そのパブリック インターフェイスを実装する、厳密な名前のランタイム アセンブリを作成する。
1. パブリック インターフェイスから型情報を埋め込み、ランタイム アセンブリからクラスのインスタンスを作成する、クライアント プログラムを作成する。
1. ランタイム アセンブリを変更し、リビルドする。
1. クライアント プログラムを実行し、クライアント プログラムを再コンパイルしなくても、新バージョンのランタイム アセンブリが使用されることを確認する。

[!INCLUDE[note_settings_general](../../../includes/note-settings-general-md.md)]

## <a name="conditions-and-limitations"></a>条件と制限

次の条件が満たされていると、アセンブリから型情報を埋め込むことができます。

- アセンブリが、少なくとも 1 つのパブリック インターフェイスを公開している。
- 埋め込みインターフェイスに、`ComImport` 属性および一意の GUID の `Guid` 属性で注釈が付けられている。
- アセンブリに、`ImportedFromTypeLib` 属性または `PrimaryInteropAssembly` 属性、およびアセンブリ レベルの `Guid` 属性が付けられている。 既定では、Visual C# と Visual Basic のプロジェクト テンプレートには、アセンブリ レベルの `Guid` 属性が含まれています。

型の埋め込みの主な機能は、COM 相互運用アセンブリをサポートすることなので、フル マネージドのソリューションに型情報を埋め込む場合は、次の制限が適用されます。

- COM 相互運用に固有の属性のみが埋め込まれます。 他の属性は無視されます。
- 型でジェネリック パラメーターが使用されていて、ジェネリック パラメーターの型が埋め込まれる型である場合、その型をアセンブリの境界を越えて使用することはできません。 アセンブリの境界を越える場合の例としては、別のアセンブリからメソッドを呼び出す場合や、別のアセンブリで定義されている型から型を派生させる場合です。
- 定数は埋め込まれません。
- <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> クラスでは、埋め込み型をキーとして利用できません。 埋め込み型をキーとしてサポートするために、独自のディクショナリ型を実装することは可能です。

## <a name="create-an-interface"></a>インターフェイスの作成

最初のステップは、型等価性インターフェイス プロジェクトを作成することです。

1. Visual Studio で、 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** の順に選択します。

1. **[新しいプロジェクトの作成]** ダイアログ ボックスで、 **[Search for templates]\(テンプレートの検索\)** ボックスに「*クラス ライブラリ*」と入力します。 一覧から C# または Visual Basic の**クラス ライブラリ (.NET Framework)** テンプレートを選択し、 **[次へ]** を選択します。

1. **[新しいプロジェクトの構成]** ダイアログ ボックスの **[プロジェクト名]** に「*TypeEquivalenceInterface*」と入力し、 **[作成]** を選択します。 新しいプロジェクトが作成されます。

1. **ソリューション エクスプローラー**で、*Class1.cs* ファイルまたは *Class1.vb* ファイルを右クリックし、 **[名前の変更]** を選択して、ファイル名を *Class1* から *ISampleInterface* に変更します。 クラスの名前も `ISampleInterface` 変更するかどうかを確認を求めるメッセージが表示されたら、 **[はい]** と応答します。 このクラスは、クラスのパブリック インターフェイスを表します。

1. **ソリューション エクスプローラー**で **TypeEquivalenceInterface** プロジェクトを右クリックし、 **[プロパティ]** を選択します。

1. **[プロパティ]** 画面の左側のウィンドウで **[ビルド]** を選択し、 **[出力パス]** をお使いのコンピューター上の場所 (*C:\TypeEquivalenceSample* など) に設定します。 このチュートリアル全体で同じ場所を使用します。

1. **[プロパティ]** 画面の左側のウィンドウで **[署名]** を選択し、 **[アセンブリに署名する]** チェック ボックスをオンにします。 **[厳密な名前のキー ファイルを選択してください]** のドロップダウンで、 **[新規作成]** を選択します。

1. **[厳密な名前キーの作成]** ダイアログで、 **[キー ファイル名]** に「*key.snk*」と入力します。 **[キーファイルをパスワードで保護する]** チェック ボックスをオフにし、 **[OK]** を選択します。

1. コード エディターで *ISampleInterface* クラス ファイルを開き、その内容を次のコードに置き換えて、`ISampleInterface` インターフェイスを作成します。

   ```csharp
   using System;
   using System.Runtime.InteropServices;

   namespace TypeEquivalenceInterface
   {
       [ComImport]
       [Guid("8DA56996-A151-4136-B474-32784559F6DF")]
       public interface ISampleInterface
       {
           void GetUserInput();
           string UserInput { get; }
       }
   }
   ```

   ```vb
   Imports System.Runtime.InteropServices

   <ComImport()>
   <Guid("8DA56996-A151-4136-B474-32784559F6DF")>
   Public Interface ISampleInterface
       Sub GetUserInput()
       ReadOnly Property UserInput As String
   End Interface
   ```

1. **[ツール]** メニューの **[GUID の作成]** を選択し、 **[GUID の作成]** ダイアログ ボックスで **[レジストリ形式]** を選択します。 **[コピー]** を選択し、 **[終了]** を選択します。

1. コードの `Guid` 属性で、サンプルの GUID をコピーした GUID に置き換えて、中かっこ ( **{ }** ) を削除します。

1. **ソリューション エクスプローラー**で **[プロパティ]** フォルダーを展開し、*AssemblyInfo.cs* または *AssemblyInfo.vb* ファイルを選択します。 コード エディターで、次の属性をファイルに追加します。

   ```csharp
   [assembly: ImportedFromTypeLib("")]
   ```

   ```vb
   <Assembly: ImportedFromTypeLib("")>
   ```

1. **[ファイル]**  >  **[すべて保存]** を選択するか、**Ctrl**+**Shift**+**S** キーを押して、ファイルとプロジェクトを保存します。

1. **ソリューション エクスプローラー**で **TypeEquivalenceInterface** プロジェクトを右クリックし、 **[ビルド]** を選択します。 クラス ライブラリの DLL ファイルがコンパイルされ、指定したビルド出力パス (たとえば、*C:\TypeEquivalenceSample*) に保存されます。

## <a name="create-a-runtime-class"></a>ランタイム クラスを作成する

次に、型等価性ランタイム クラスを作成します。

1. Visual Studio で、 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** の順に選択します。

1. **[新しいプロジェクトの作成]** ダイアログ ボックスで、 **[Search for templates]\(テンプレートの検索\)** ボックスに「*クラス ライブラリ*」と入力します。 一覧から C# または Visual Basic の**クラス ライブラリ (.NET Framework)** テンプレートを選択し、 **[次へ]** を選択します。

1. **[新しいプロジェクトの構成]** ダイアログ ボックスの **[プロジェクト名]** に「*TypeEquivalenceRuntime*」と入力し、 **[作成]** を選択します。 新しいプロジェクトが作成されます。

1. **ソリューション エクスプローラー**で、*Class1.cs* ファイルまたは *Class1.vb* ファイルを右クリックし、 **[名前の変更]** を選択して、ファイル名を *Class1* から *SampleClass* に変更します。 クラスの名前も `SampleClass` 変更するかどうかを確認を求めるメッセージが表示されたら、 **[はい]** と応答します。 このクラスは、 `ISampleInterface` インターフェイスを実装します。

1. **ソリューション エクスプローラー**で **TypeEquivalenceInterface** プロジェクトを右クリックし、 **[プロパティ]** を選択します。

1. **[プロパティ]** 画面の左側のウィンドウで **[ビルド]** を選択し、 **[出力パス]** を TypeEquivalenceInterface プロジェクトで使用したのと同じ場所 (たとえば、*C:\TypeEquivalenceSample*) に設定します。

1. **[プロパティ]** 画面の左側のウィンドウで **[署名]** を選択し、 **[アセンブリに署名する]** チェック ボックスをオンにします。 **[厳密な名前のキー ファイルを選択してください]** のドロップダウンで、 **[新規作成]** を選択します。

1. **[厳密な名前キーの作成]** ダイアログで、 **[キー ファイル名]** に「*key.snk*」と入力します。 **[キーファイルをパスワードで保護する]** チェック ボックスをオフにし、 **[OK]** を選択します。

1. **ソリューション エクスプローラー**で、**TypeEquivalenceRuntime** プロジェクトを右クリックし、 **[追加]**  >  **[参照]** を選択します。

1. **[参照マネージャー]** ダイアログ ボックスで **[参照]** を選択し、出力パスのフォルダーを参照します。 *TypeEquivalenceInterface.dll* ファイルを選択し、 **[追加]** を選択して、 **[OK]** を選択します。

1. **ソリューション エクスプローラー**で **[参照]** フォルダーを展開し、**TypeEquivalenceInterface** 参照を選択します。 **[プロパティ]** ウィンドウで、まだ設定されていない場合は **[特定バージョン]** を **[False]** に設定します。

1. コード エディターで *SampleClass* クラス ファイルを開き、その内容を次のコードに置き換えて、`SampleClass` クラスを作成します。

   ```csharp
   using System;
   using TypeEquivalenceInterface;

   namespace TypeEquivalenceRuntime
   {
       public class SampleClass : ISampleInterface
       {
           private string p_UserInput;
           public string UserInput { get { return p_UserInput; } }

           public void GetUserInput()
           {
               Console.WriteLine("Please enter a value:");
               p_UserInput = Console.ReadLine();
           }
       }
   }
   ```

   ```vb
   Imports TypeEquivalenceInterface

   Public Class SampleClass
       Implements ISampleInterface

       Private p_UserInput As String
       Public ReadOnly Property UserInput() As String Implements ISampleInterface.UserInput
           Get
               Return p_UserInput
           End Get
       End Property

       Public Sub GetUserInput() Implements ISampleInterface.GetUserInput
           Console.WriteLine("Please enter a value:")
           p_UserInput = Console.ReadLine()
       End Sub
   End Class
   ```

1. **[ファイル]**  >  **[すべて保存]** を選択するか、**Ctrl**+**Shift**+**S** キーを押して、ファイルとプロジェクトを保存します。

1. **ソリューション エクスプローラー**で **TypeEquivalenceRuntime** プロジェクトを右クリックし、 **[ビルド]** を選択します。 クラス ライブラリの DLL ファイルがコンパイルされ、指定したビルド出力パスに保存されます。

## <a name="create-a-client-project"></a>クライアント プロジェクトを作成する

最後に、インターフェイス アセンブリを参照する型等価性クライアント プログラムを作成します。

1. Visual Studio で、 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** の順に選択します。

1. **[新しいプロジェクトの作成]** ダイアログ ボックスで、 **[Search for templates]\(テンプレートの検索\)** ボックスに「*コンソール*」と入力します。 一覧から C# または Visual Basic の**コンソール アプリ (.NET Framework)** テンプレートを選択し、 **[次へ]** を選択します。

1. **[新しいプロジェクトの構成]** ダイアログ ボックスの **[プロジェクト名]** に「*TypeEquivalenceClient*」と入力し、 **[作成]** を選択します。 新しいプロジェクトが作成されます。

1. **ソリューション エクスプローラー**で **TypeEquivalenceClient** プロジェクトを右クリックし、 **[プロパティ]** を選択します。

1. **[プロパティ]** 画面の左側のウィンドウで **[ビルド]** を選択し、 **[出力パス]** を TypeEquivalenceInterface プロジェクトで使用したのと同じ場所 (たとえば、*C:\TypeEquivalenceSample*) に設定します。

1. **ソリューション エクスプローラー**で **TypeEquivalenceClient** プロジェクトを右クリックし、 **[追加]**  >  **[参照]** を選択します。

1. **[参照マネージャー]** ダイアログで、**TypeEquivalenceInterface.dll** ファイルが一覧に既に表示されている場合は、それを選択します。 表示されていない場合は、 **[参照]** を選択し、出力パス フォルダーを参照して、(*TypeEquivalenceRuntime.dll* ではなく) *TypeEquivalenceInterface.dll* ファイルを選択し、 **[追加]** を選択します。 **[OK]** を選択します。

1. **ソリューション エクスプローラー**で **[参照]** フォルダーを展開し、**TypeEquivalenceInterface** 参照を選択します。 **[プロパティ]** ウィンドウで、 **[相互運用型の埋め込み]** を **[True]** に設定します。

1. コード エディターで *Program.cs* または *Module1.vb* ファイルを開き、その内容を次のコードに置き換えて、クライアント プログラムを作成します。

   ```csharp
   using System;
   using System.Reflection;
   using TypeEquivalenceInterface;

   namespace TypeEquivalenceClient
   {
       class Program
       {
           static void Main(string[] args)
           {
               Assembly sampleAssembly = Assembly.Load("TypeEquivalenceRuntime");
               ISampleInterface sampleClass =
                   (ISampleInterface)sampleAssembly.CreateInstance("TypeEquivalenceRuntime.SampleClass");
               sampleClass.GetUserInput();
               Console.WriteLine(sampleClass.UserInput);
               Console.WriteLine(sampleAssembly.GetName().Version.ToString());
               Console.ReadLine();
           }
       }
   }
   ```

   ```vb
   Imports System.Reflection
   Imports TypeEquivalenceInterface

   Module Module1

       Sub Main()
           Dim sampleAssembly = Assembly.Load("TypeEquivalenceRuntime")
           Dim sampleClass As ISampleInterface = CType( _
               sampleAssembly.CreateInstance("TypeEquivalenceRuntime.SampleClass"), ISampleInterface)
           sampleClass.GetUserInput()
           Console.WriteLine(sampleClass.UserInput)
           Console.WriteLine(sampleAssembly.GetName().Version)
           Console.ReadLine()
       End Sub

   End Module
   ```

1. **[ファイル]**  >  **[すべて保存]** を選択するか、**Ctrl**+**Shift**+**S** キーを押して、ファイルとプロジェクトを保存します。

1. **Ctrl**+**F5** キーを押し、プログラムをビルドして実行します。 コンソールの出力にアセンブリのバージョン **1.0.0.0** が返されることに注意してください。

## <a name="modify-the-interface"></a>インターフェイスを変更する

ここでは、インターフェイス アセンブリを変更して、そのバージョンを変更します。

1. Visual Studio で、 **[ファイル]**  >  **[開く]**  >  **[プロジェクト/ソリューション]** を選択し、**TypeEquivalenceInterface** プロジェクトを開きます。

1. **ソリューション エクスプローラー**で **TypeEquivalenceInterface** プロジェクトを右クリックし、 **[プロパティ]** を選択します。

1. **[プロパティ]** 画面の左側のウィンドウで **[アプリケーション]** を選択し、 **[アセンブリ情報]** を選択します。

1. **[アセンブリ情報]** ダイアログ ボックスで、 **[アセンブリのバージョン]** と **[ファイルのバージョン]** の値を「*2.0.0.0*」に変更して、 **[OK]** を選択します。

1. *SampleInterface.cs* ファイルまたは *SampleInterface.vb* ファイルを開き、次のコード行を `ISampleInterface` インターフェイスに追加します。

   ```csharp
   DateTime GetDate();
   ```

   ```vb
   Function GetDate() As Date
   ```

1. **[ファイル]**  >  **[すべて保存]** を選択するか、**Ctrl**+**Shift**+**S** キーを押して、ファイルとプロジェクトを保存します。

1. **ソリューション エクスプローラー**で **TypeEquivalenceInterface** プロジェクトを右クリックし、 **[ビルド]** を選択します。 クラス ライブラリの DLL ファイルの新しいバージョンがコンパイルされて、ビルド出力パスに保存されます。

## <a name="modify-the-runtime-class"></a>ランタイム クラスを変更する

また、ランタイム クラスを変更し、そのバージョンを更新します。

1. Visual Studio で、 **[ファイル]**  >  **[開く]**  >  **[プロジェクト/ソリューション]** を選択し、**TypeEquivalenceRuntime** プロジェクトを開きます。

1. **ソリューション エクスプローラー**で **TypeEquivalenceRuntime** プロジェクトを右クリックし、 **[プロパティ]** を選択します。

1. **[プロパティ]** 画面の左側のウィンドウで **[アプリケーション]** を選択し、 **[アセンブリ情報]** を選択します。

1. **[アセンブリ情報]** ダイアログ ボックスで、 **[アセンブリのバージョン]** と **[ファイルのバージョン]** の値を「*2.0.0.0*」に変更して、 **[OK]** を選択します。

1. *SampleClass.cs* ファイルまたは *SampleClass.vb* ファイルを開き、次のコードを `SampleClass` クラスに追加します。

   ```csharp
    public DateTime GetDate()
    {
        return DateTime.Now;
    }
   ```

   ```vb
   Public Function GetDate() As DateTime Implements ISampleInterface.GetDate
       Return Now
   End Function
   ```

1. **[ファイル]**  >  **[すべて保存]** を選択するか、**Ctrl**+**Shift**+**S** キーを押して、ファイルとプロジェクトを保存します。

1. **ソリューション エクスプローラー**で **TypeEquivalenceRuntime** プロジェクトを右クリックし、 **[ビルド]** を選択します。 クラス ライブラリの DLL ファイルの新しいバージョンがコンパイルされて、ビルド出力パスに保存されます。

## <a name="run-the-updated-client-program"></a>更新されたクライアント プログラムを実行する

ビルド出力フォルダーの場所に移動し、*TypeEquivalenceClient.exe* を実行します。 プログラムを再コンパイルしなくても、コンソール出力に `TypeEquivalenceRuntime` アセンブリの新しいバージョン *2.0.0.0* が反映されていることに注意してください。

## <a name="see-also"></a>関連項目

- [-link (C# コンパイラ オプション)](../../csharp/language-reference/compiler-options/link-compiler-option.md)
- [-link (Visual Basic)](../../visual-basic/reference/command-line-compiler/link.md)
- [C# プログラミング ガイド](../../csharp/programming-guide/index.md)
- [プログラミングの概念 (Visual Basic)](../../visual-basic/programming-guide/concepts/index.md)
- [.NET のアセンブリ](index.md)
