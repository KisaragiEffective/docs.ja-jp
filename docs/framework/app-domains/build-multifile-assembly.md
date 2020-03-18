---
title: '方法: マルチファイルアセンブリをビルドする'
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET Framework], multifile
- entry point for assembly
- compiling assemblies
- Al.exe
- command-line compilers
- assembly manifest, multifile assemblies
- assemblies [.NET Framework], compiling
- Assembly Linker
- code modules
- multifile assemblies
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 261c5583-8a76-412d-bda7-9b8ee3b131e5
ms.openlocfilehash: 0f8c6d57425657e321d80f9edffa20f27bc28770
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74429559"
---
# <a name="how-to-build-a-multifile-assembly"></a>方法: マルチファイルアセンブリをビルドする

この記事では、マルチファイル アセンブリを作成する方法を説明し、プロシージャの各手順を示すコードを提供します。

> [!NOTE]
> Visual Studio IDE for C# および Visual Basic は、シングルファイル アセンブリの作成でしか使用できません。 マルチファイル アセンブリを作成する場合は、コマンド ライン コンパイラまたは Visual C++ 付き Visual Studio を使用する必要があります。 マルチファイル アセンブリは .NET Framework でのみサポートされます。

## <a name="create-a-multifile-assembly"></a>マルチファイル アセンブリを作成する

1. アセンブリ内のほかのモジュールによって参照される名前空間を含むすべてのファイルをコンパイルして、コード モジュールを生成します。 コード モジュールの既定の拡張子は *.netmodule* です。

   たとえば、`Stringer` ファイルに `myStringer` と呼ばれるクラスを含む `Stringer` という名前空間があるとします。 `Stringer` クラスには、コンソールに 1 つの行を書き込む `StringerMethod` メソッドが含まれます。

   ```cpp
   // Assembly building example in the .NET Framework.
   using namespace System;

   namespace myStringer
   {
       public ref class Stringer
       {
       public:
           void StringerMethod()
           {
               System::Console::WriteLine("This is a line from StringerMethod.");
           }
       };
   }
   ```

   ```csharp
   // Assembly building example in the .NET Framework.
   using System;

   namespace myStringer
   {
       public class Stringer
       {
           public void StringerMethod()
           {
               System.Console.WriteLine("This is a line from StringerMethod.");
           }
       }
   }
   ```

   ```vb
   ' Assembly building example in the .NET Framework.
   Namespace myStringer
       Public Class Stringer
           Public Sub StringerMethod()
               System.Console.WriteLine("This is a line from StringerMethod.")
           End Sub
       End Class
   End Namespace
   ```

2. このコードをコンパイルするには、次のコマンドを使用します。

   ```cpp
   cl /clr:pure /LN Stringer.cpp
   ```

   ```csharp
   csc /t:module Stringer.cs
   ```

   ```vb
   vbc /t:module Stringer.vb
   ```

   *module* パラメーターと **/t:** コンパイラ オプションを指定すると、ファイルはアセンブリではなくモジュールとしてコンパイルされます。 コンパイラにより、アセンブリに追加できる *Stringer.netmodule* というモジュールが生成されます。

3. コード内で参照されるほかのモジュールを示すために必要なコンパイラ オプションを使用して、ほかのすべてのモジュールをコンパイルします。 この手順では、 **/addmodule** コンパイラ オプションを使用します。

   次の例では、*Client* というコード モジュールに、手順 1. で作成した `Main`Stringer.dll*モジュール内のメソッドを参照するエントリ ポイント* メソッドがあります。

   ```cpp
   #using "Stringer.netmodule"

   using namespace System;
   using namespace myStringer; //The namespace created in Stringer.netmodule.

   ref class MainClientApp
   {
       // Static method Main is the entry point method.
   public:
       static void Main()
       {
           Stringer^ myStringInstance = gcnew Stringer();
           Console::WriteLine("Client code executes");
           myStringInstance->StringerMethod();
       }
   };

   int main()
   {
       MainClientApp::Main();
   }
   ```

   ```csharp
   using System;
   using myStringer;

   class MainClientApp
   {
       // Static method Main is the entry point method.
       public static void Main()
       {
           Stringer myStringInstance = new Stringer();
           Console.WriteLine("Client code executes");
           myStringInstance.StringerMethod();
       }
   }
   ```

   ```vb
   Imports myStringer

   Class MainClientApp
       ' Static method Main is the entry point method.
       Public Shared Sub Main()
           Dim myStringInstance As New Stringer()
           Console.WriteLine("Client code executes")
           myStringInstance.StringerMethod()
       End Sub
   End Class
   ```

4. このコードをコンパイルするには、次のコマンドを使用します。

   ```cpp
   cl /clr:pure /FUStringer.netmodule /LN Client.cpp
   ```

   ```csharp
   csc /addmodule:Stringer.netmodule /t:module Client.cs
   ```

   ```vb
   vbc /addmodule:Stringer.netmodule /t:module Client.vb
   ```

   このモジュールは後の手順でアセンブリに追加するため、 **/t:module** オプションを指定します。 **Client** 内のコードが *Stringer.netmodule* 内のコードによって作成された名前空間を参照するため、 */addmodule* オプションを指定します。 コンパイラにより、*Stringer.netmodule* という別のモジュールへの参照を格納する、*Client.netmodule* というモジュールが生成されます。

   > [!NOTE]
   > C# コンパイラと Visual Basic コンパイラは、マルチファイル アセンブリを直接作成する場合、次の 2 種類の構文を使用します。
   >
   > 2 回のコンパイルで、2 ファイルのアセンブリを作成する。
   >
   >   ```cpp
   >   cl /clr:pure /LN Stringer.cpp
   >   cl /clr:pure Client.cpp /link /ASSEMBLYMODULE:Stringer.netmodule
   >   ```
   >
   >   ```csharp
   >   csc /t:module Stringer.cs
   >   csc Client.cs /addmodule:Stringer.netmodule
   >   ```
   >
   >   ```vb
   >   vbc /t:module Stringer.vb
   >   vbc Client.vb /addmodule:Stringer.netmodule
   >   ```
   >
   > 1 回のコンパイルで、2 ファイルのアセンブリを作成する。
   >
   >   ```cpp
   >   cl /clr:pure /LN Stringer.cpp
   >   cl /clr:pure Client.cpp /link /ASSEMBLYMODULE:Stringer.netmodule
   >   ```
   >
   >   ```csharp
   >   csc /out:Client.exe Client.cs /out:Stringer.netmodule Stringer.cs
   >   ```
   >
   >   ```vb
   >   vbc /out:Client.exe Client.vb /out:Stringer.netmodule Stringer.vb
   >   ```

5. [アセンブリ リンカー (Al.exe)](../tools/al-exe-assembly-linker.md) を使用して、アセンブリ マニフェストを格納する出力ファイルを作成します。 このファイルは、アセンブリの一部であるすべてのモジュールまたはリソースについての参照情報を格納します。

    コマンド プロンプトに次のコマンドを入力します。

    **al** \<*module name*> \<*module name*> … **/main:** \<*method name*>  **/out:** \<*file name*>  **/target:** \<*assembly file type*>

    このコマンドで、*module name* 引数はアセンブリに含める各モジュールの名前を指定します。 **/main:** オプションは、アセンブリのエントリ ポイントであるメソッド名を指定します。 **/out:** オプションは、アセンブリ メタデータを格納する出力ファイルの名前を指定します。 **/target:** オプションは、アセンブリがコンソール アプリケーション実行可能 ( *.exe*) ファイル、Windows 実行可能 ( *.win*) ファイル、またはライブラリ ( *.lib*) ファイルであることを指定します。

    *Al.exe* を使用して、*myAssembly.exe* というコンソール アプリケーション実行可能ファイルであるアセンブリを作成する例を次に示します。 このアプリケーションは、*Client.netmodule* および *Stringer.netmodule* という 2 つのモジュール、および *myAssembly.exe* というアセンブリ メタデータだけを格納する実行可能ファイルで構成されます。 アセンブリのエントリ ポイントは `Main` クラスの `MainClientApp` メソッドで、*Client.dll* 内に配置されています。

    ```cmd
    al Client.netmodule Stringer.netmodule /main:MainClientApp.Main /out:myAssembly.exe /target:exe
    ```

    [MSIL 逆アセンブラー (Ildasm.exe)](../tools/ildasm-exe-il-disassembler.md) を使用すると、アセンブリの内容を調査したり、ファイルがアセンブリであるかモジュールであるかを判断したりできます。

## <a name="see-also"></a>関連項目

- [アセンブリを作成する](../../standard/assembly/create.md)
- [方法: アセンブリの内容を表示する](../../standard/assembly/view-contents.md)
- [ランタイムがアセンブリを検索する方法](../deployment/how-the-runtime-locates-assemblies.md)
- [マルチファイル アセンブリ](multifile-assemblies.md)
