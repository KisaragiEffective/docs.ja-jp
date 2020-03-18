---
title: '方法: 署名のないフレンド アセンブリを作成する'
ms.date: 08/19/2019
ms.assetid: 78cbc4f0-b021-4141-a4ff-eb4edbd814ca
dev_langs:
- csharp
- vb
ms.openlocfilehash: f8fec064507553b8208083578165965de2303a33
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74352436"
---
# <a name="how-to-create-unsigned-friend-assemblies"></a>方法: 署名のないフレンド アセンブリを作成する

この例では、署名のないアセンブリと共にフレンド アセンブリを使用する方法を示します。

## <a name="create-an-assembly-and-a-friend-assembly"></a>署名のないアセンブリとフレンド アセンブリを作成する

1. コマンド プロンプトを開きます。

2. 次のコードを含む、*friend_unsigned_A* という名前の C# または Visual Basic ファイルを作成します。 コードでは <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性を使用して、フレンド アセンブリとして *friend_unsigned_B* を宣言します。

   ```csharp
   // friend_unsigned_A.cs
   // Compile with:
   // csc /target:library friend_unsigned_A.cs
   using System.Runtime.CompilerServices;
   using System;

   [assembly: InternalsVisibleTo("friend_unsigned_B")]

   // Type is internal by default.
   class Class1
   {
       public void Test()
       {
           Console.WriteLine("Class1.Test");
       }
   }

   // Public type with internal member.
   public class Class2
   {
       internal void Test()
       {
           Console.WriteLine("Class2.Test");
       }
   }
   ```

   ```vb
   ' friend_unsigned_A.vb
   ' Compile with:
   ' vbc -target:library friend_unsigned_A.vb
   Imports System.Runtime.CompilerServices

   <Assembly: InternalsVisibleTo("friend_unsigned_B")>

   ' Friend type.
   Friend Class Class1
       Public Sub Test()
           Console.WriteLine("Class1.Test")
       End Sub
   End Class

   ' Public type with Friend member.
   Public Class Class2
       Friend Sub Test()
           Console.WriteLine("Class2.Test")
       End Sub
   End Class
   ```

3. 次のコマンドを使用して *friend_unsigned_A* をコンパイルして署名します。

   ```csharp
   csc /target:library friend_unsigned_A.cs
   ```

   ```vb
   vbc -target:library friend_unsigned_A.vb
   ```

4. 次のコードを含む、*friend_unsigned_B* という名前の C# または Visual Basic ファイルを作成します。 *friend_unsigned_A* が *friend_unsigned_B* をフレンド アセンブリとして指定しているため、*friend_unsigned_B* 内のコードは、*friend_unsigned_A* の `internal` (C#) または `Friend` (Visual Basic) 型とメンバーにアクセスできます。

   ```csharp
   // friend_unsigned_B.cs
   // Compile with:
   // csc /r:friend_unsigned_A.dll /out:friend_unsigned_B.exe friend_unsigned_B.cs
   public class Program
   {
       static void Main()
       {
           // Access an internal type.
           Class1 inst1 = new Class1();
           inst1.Test();

           Class2 inst2 = new Class2();
           // Access an internal member of a public type.
           inst2.Test();

           System.Console.ReadLine();
       }
   }
   ```

   ```vb
   ' friend_unsigned_B.vb
   ' Compile with:
   ' vbc -r:friend_unsigned_A.dll friend_unsigned_B.vb
   Module Module1
       Sub Main()
           ' Access a Friend type.
           Dim inst1 As New Class1()
           inst1.Test()

           Dim inst2 As New Class2()
           ' Access a Friend member of a public type.
           inst2.Test()

           System.Console.ReadLine()
       End Sub
   End Module
   ```

5. 次のコマンドを使用して *friend_unsigned_B* をコンパイルします。

   ```csharp
   csc /r:friend_unsigned_A.dll /out:friend_unsigned_B.exe friend_unsigned_B.cs
   ```

   ```vb
   vbc -r:friend_unsigned_A.dll friend_unsigned_B.vb
   ```

   コンパイラによって生成されるアセンブリの名前は、<xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性に渡されるフレンド アセンブリ名と一致している必要があります。 `-out` コンパイラ オプションを使用して、出力アセンブリ ( *.exe* または *.dll*) の名前を明示的に指定する必要があります。 詳細については、「[-out (C# コンパイラ オプション)](../../csharp/language-reference/compiler-options/out-compiler-option.md)」または「[-out (Visual Basic)](../../visual-basic/reference/command-line-compiler/out.md)」を参照してください。

6. *friend_unsigned_B.exe* ファイルを実行します。

   このプログラムで 2 つの文字列が出力されます。**Class1.Test** と **Class2.Test**です。

## <a name="net-security"></a>.NET セキュリティ

<xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性と <xref:System.Security.Permissions.StrongNameIdentityPermission> クラスには類似点があります。 主な違いは、<xref:System.Security.Permissions.StrongNameIdentityPermission> はセキュリティ アクセス許可を要求することで特定のコード セクションを実行できますが、<xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性では `internal` または `Friend` (Visual Basic) 型とメンバーの参照可能範囲を制御することです。

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>
- [.NET のアセンブリ](index.md)
- [フレンド アセンブリ](friend.md)
- [方法: 署名されたフレンド アセンブリを作成する](create-signed-friend.md)
- [C# プログラミング ガイド](../../csharp/programming-guide/index.md)
- [プログラミングの概念 (Visual Basic)](../../visual-basic/programming-guide/concepts/index.md)
