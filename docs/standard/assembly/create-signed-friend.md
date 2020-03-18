---
title: '方法: 署名されたフレンド アセンブリを作成する'
ms.date: 08/19/2019
ms.assetid: bab62063-61e6-453f-905f-77673df9534e
dev_langs:
- csharp
- vb
ms.openlocfilehash: 9912fa70014a8828e994cf528644aaa7cb351fea
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159495"
---
# <a name="how-to-create-signed-friend-assemblies"></a>方法: 署名されたフレンド アセンブリを作成する
この例では、厳密な名前を持つアセンブリと共にフレンド アセンブリを使用する方法を示します。 両方のアセンブリに厳密な名前が付けられている必要があります。 この例のアセンブリは両方とも同じキーを使用していますが、2 つのアセンブリそれぞれが別々のキーを使用することもできます。  
  
## <a name="create-a-signed-assembly-and-a-friend-assembly"></a>署名付きアセンブリとフレンド アセンブリを作成する  
  
1. コマンド プロンプトを開きます。  
  
2. 厳密な名前ツールで次のコマンド シーケンスを使用して、キー ファイルを生成し、公開鍵を表示します。 詳細については、「[Sn.exe (厳密名ツール)](../../framework/tools/sn-exe-strong-name-tool.md)」を参照してください。  
  
    1. この例で使用する厳密な名前キーを生成し、*FriendAssemblies.snk* ファイルに格納します。  
  
         `sn -k FriendAssemblies.snk`  
  
    2. *FriendAssemblies.snk* から公開鍵を抽出し、*FriendAssemblies.publickey* に追加します。  
  
         `sn -p FriendAssemblies.snk FriendAssemblies.publickey`  
  
    3. *FriendAssemblies.publickey* ファイルに格納されている公開鍵を表示します。  
  
         `sn -tp FriendAssemblies.publickey`  
  
3. 次のコードを含む、*friend_signed_A* という名前の C# または Visual Basic ファイルを作成します。 コードでは <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性を使用して、フレンド アセンブリとして *friend_signed_B* を宣言します。  

   厳密な名前ツールは、実行するごとに新しい公開鍵を生成します。 このため、次の例に示すコード内の公開鍵を、ここで生成した公開鍵に置き換える必要があります。  

   ```csharp  
   // friend_signed_A.cs  
   // Compile with:
   // csc /target:library /keyfile:FriendAssemblies.snk friend_signed_A.cs  
   using System.Runtime.CompilerServices;  

   [assembly: InternalsVisibleTo("friend_signed_B, PublicKey=0024000004800000940000000602000000240000525341310004000001000100e3aedce99b7e10823920206f8e46cd5558b4ec7345bd1a5b201ffe71660625dcb8f9a08687d881c8f65a0dcf042f81475d2e88f3e3e273c8311ee40f952db306c02fbfc5d8bc6ee1e924e6ec8fe8c01932e0648a0d3e5695134af3bb7fab370d3012d083fa6b83179dd3d031053f72fc1f7da8459140b0af5afc4d2804deccb6")]  
   class Class1  
   {  
       public void Test()  
       {  
           System.Console.WriteLine("Class1.Test");  
           System.Console.ReadLine();  
       }  
   }  
   ```  

   ```vb  
   ' friend_signed_A.vb  
   ' Compile with:
   ' Vbc -target:library -keyfile:FriendAssemblies.snk friend_signed_A.vb  
   Imports System.Runtime.CompilerServices  

   <Assembly: InternalsVisibleTo("friend_signed_B, PublicKey=0024000004800000940000000602000000240000525341310004000001000100e3aedce99b7e10823920206f8e46cd5558b4ec7345bd1a5b201ffe71660625dcb8f9a08687d881c8f65a0dcf042f81475d2e88f3e3e273c8311ee40f952db306c02fbfc5d8bc6ee1e924e6ec8fe8c01932e0648a0d3e5695134af3bb7fab370d3012d083fa6b83179dd3d031053f72fc1f7da8459140b0af5afc4d2804deccb6")>
   Public Class Class1  
       Public Sub Test()  
           System.Console.WriteLine("Class1.Test")  
           System.Console.ReadLine()  
       End Sub  
   End Class  
   ```  

4. 次のコマンドを使用して *friend_signed_A* をコンパイルして署名します。  

   ```csharp
   csc /target:library /keyfile:FriendAssemblies.snk friend_signed_A.cs  
   ```  

   ```vb
   Vbc -target:library -keyfile:FriendAssemblies.snk friend_signed_A.vb  
   ```  

5. 次のコードを含む、*friend_signed_B* という名前の C# または Visual Basic ファイルを作成します。 *friend_signed_A* で *friend_signed_B* がフレンド アセンブリとして指定されているため、*friend_signed_B* 内のコードは、*friend_signed_A* の `internal` (C#) または `Friend` (Visual Basic) 型とメンバーにアクセスできます。 このファイルには、次のコードが含まれています。  

   ```csharp  
   // friend_signed_B.cs  
   // Compile with:
   // csc /keyfile:FriendAssemblies.snk /r:friend_signed_A.dll /out:friend_signed_B.exe friend_signed_B.cs  
   public class Program  
   {  
       static void Main()  
       {  
           Class1 inst = new Class1();  
           inst.Test();  
       }  
   }  
   ```  

   ```vb  
   ' friend_signed_B.vb  
   ' Compile with:
   ' Vbc -keyfile:FriendAssemblies.snk -r:friend_signed_A.dll friend_signed_B.vb  
   Module Sample  
       Public Sub Main()  
           Dim inst As New Class1  
           inst.Test()  
       End Sub  
   End Module  
   ```  

6. 次のコマンドを使用して *friend_signed_B* をコンパイルして署名します。  

   ```csharp
   csc /keyfile:FriendAssemblies.snk /r:friend_signed_A.dll /out:friend_signed_B.exe friend_signed_B.cs  
   ```  

   ```vb
   vbc -keyfile:FriendAssemblies.snk -r:friend_signed_A.dll friend_signed_B.vb  
   ```  

   コンパイラによって生成されたアセンブリの名前は、<xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性に渡されたフレンド アセンブリ名と一致している必要があります。 `-out` コンパイラ オプションを使用して、出力アセンブリ ( *.exe* または *.dll*) の名前を明示的に指定する必要があります。 詳細については、「[-out (C# コンパイラ オプション)](../../csharp/language-reference/compiler-options/out-compiler-option.md)」または「[-out (Visual Basic)](../../visual-basic/reference/command-line-compiler/out.md)」を参照してください。  

7. *friend_signed_B.exe* ファイルを実行します。  

   そのプログラムでは、**Class1.Test** という文字列が出力されます。  
  
## <a name="net-security"></a>.NET セキュリティ  
 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性と <xref:System.Security.Permissions.StrongNameIdentityPermission> クラスには類似点があります。 主な違いは、<xref:System.Security.Permissions.StrongNameIdentityPermission> ではセキュリティ アクセス許可を要求することで特定のコード セクションを実行できますが、<xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性では `internal` (C#) または `Friend` (Visual Basic) 型とメンバーの参照可能範囲が制御されることです。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>
- [.NET のアセンブリ](index.md)
- [フレンド アセンブリ](friend.md)
- [方法: 署名のないフレンド アセンブリを作成する](create-unsigned-friend.md)
- [-keyfile (C#)](../../csharp/language-reference/compiler-options/keyfile-compiler-option.md)
- [-keyfile (Visual Basic)](../../visual-basic/reference/command-line-compiler/keyfile.md)
- [Sn.exe (厳密名ツール)](../../framework/tools/sn-exe-strong-name-tool.md)
- [厳密な名前付きアセンブリの作成と使用](create-use-strong-named.md)
- [C# プログラミング ガイド](../../csharp/programming-guide/index.md)
- [プログラミングの概念 (Visual Basic)](../../visual-basic/programming-guide/concepts/index.md)
