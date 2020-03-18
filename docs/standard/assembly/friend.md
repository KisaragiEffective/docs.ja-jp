---
title: フレンド アセンブリ
ms.date: 08/20/2019
ms.assetid: b65ea7de-0801-477a-a39c-e914c2cc107c
dev_langs:
- csharp
- vb
ms.openlocfilehash: a74d4b74ead8492028a092e090f9281231802a87
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74348178"
---
# <a name="friend-assemblies"></a>フレンド アセンブリ

"*フレンド アセンブリ*" とは、別のアセンブリの [internal](../../csharp/language-reference/keywords/internal.md) (C#) または [Friend](../../visual-basic/language-reference/modifiers/friend.md) (Visual Basic) 型およびメンバーにアクセスできるアセンブリです。 フレンド アセンブリとして指定した場合、public として宣言されていないその型とメンバーに、他のアセンブリからアクセスできるようになります。 この方法は、特に次の状況で利便性を発揮します。

- 単体テスト中、テスト コードが別のアセンブリで実行されている状況で、C# では `internal`、または Visual Basic では `Friend` としてマークされる、そのテスト対象のアセンブリ内のメンバーにアクセスしなければならないとき。

- クラス ライブラリの開発中、そのライブラリへの追加機能が別のアセンブリにある状況で、C# では `internal`、または Visual Basic では `Friend` としてマークされる、既存アセンブリ内のメンバーにアクセスしなければならないとき。

## <a name="remarks"></a>Remarks

<xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性を利用し、特定のアセンブリの 1 つまたは複数のフレンド アセンブリを特定できます。 次の例では、*Assembly A* において <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性が使用され、フレンド アセンブリとしてアセンブリ *AssemblyB* が指定されています。 これにより、アセンブリ *AssemblyB* では、*Assembly A* の中で `internal` (C#) または `Friend` (Visual Basic) としてマークされているすべての型とメンバーにアクセスすることができます。

> [!NOTE]
> *Assembly A* のような別のアセンブリの internal 型または internal メンバーにアクセスする *AssemblyB* のようなアセンブリをコンパイルするときは、 **-out** コンパイラ オプションを使用して、出力ファイル ( *.exe* または *.dll*) の名前を明示的に指定する必要があります。 この指定は必ず行ってください。コンパイラが外部参照にバインドする時点ではまだ、ビルド中のアセンブリの名前が生成されていないためです。 詳細については、「[-out (C#)](../../csharp/language-reference/compiler-options/out-compiler-option.md)」または「[-out (Visual Basic)](../../visual-basic/reference/command-line-compiler/out.md)」を参照してください。

```csharp
using System.Runtime.CompilerServices;
using System;

[assembly: InternalsVisibleTo("AssemblyB")]

// The class is internal by default.
class FriendClass
{
    public void Test()
    {
        Console.WriteLine("Sample Class");
    }
}

// Public class that has an internal method.
public class ClassWithFriendMethod
{
    internal void Test()
    {
        Console.WriteLine("Sample Method");
    }

}
```

```vb
Imports System.Runtime.CompilerServices
<Assembly: InternalsVisibleTo("AssemblyB")>

' Friend class.
Friend Class FriendClass
    Public Sub Test()
        Console.WriteLine("Sample Class")
    End Sub
End Class

' Public class with a Friend method.
Public Class ClassWithFriendMethod
    Friend Sub Test()
        Console.WriteLine("Sample Method")
    End Sub
End Class
```

`internal` (C#) または `Friend` (Visual Basic) 型およびメンバーにアクセスできるのは、明示的にフレンドとして指定されたアセンブリだけです。 たとえば、*AssemblyB* が *Assembly A* のフレンドであり、*Assembly C* で *AssemblyB* が参照されている場合、*Assembly A* の `internal` (C#) または `Friend` (Visual Basic) 型に *Assembly C* からアクセスすることはできません。

コンパイラは、<xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性に渡されるフレンド アセンブリ名の基本検証を実行します。 *Assembly A* で *AssemblyB* がフレンド アセンブリとして宣言されている場合、検証規則は次のとおりです。

- *Assembly A* に厳密な名前が付けられている場合、*AssemblyB* にも厳密な名前が付けられている必要があります。 属性に渡されるフレンド アセンブリ名は、アセンブリ名と、*AssemblyB* の署名に使用される厳密な名前のキーの公開キーで構成されている必要があります。

     *AssemblyB* の厳密な名前を、フレンド アセンブリ名として <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性に渡すことはできません。 アセンブリのバージョン、カルチャ、アーキテクチャ、公開キー トークンは含めないでください。

- *Assembly A* が厳密な名前でない場合、フレンド アセンブリの名前は、アセンブリ名のみで構成されている必要があります。 詳細については、[署名のないフレンド アセンブリを作成する](create-unsigned-friend.md)」を参照してください。

- *AssemblyB* に厳密な名前が付けられている場合、プロジェクトの設定またはコマンド ラインの `/keyfile` コンパイラ オプションを使用して、*AssemblyB* に対して厳密な名前のキーを指定する必要があります。 詳細については、[署名されたフレンド アセンブリを作成する](create-signed-friend.md)」を参照してください。

 <xref:System.Security.Permissions.StrongNameIdentityPermission> クラスも型を共有する機能を提供しますが、次のような違いがあります。

- フレンド アセンブリはアセンブリ全体に適用されますが、<xref:System.Security.Permissions.StrongNameIdentityPermission> は個々の型に適用されます。

- *AssemblyB* との間で共有したい型が *Assembly A* に数百個存在する場合、そのすべてに対して <xref:System.Security.Permissions.StrongNameIdentityPermission> を追加する必要があります。 フレンド アセンブリを使用した場合、フレンド関係を 1 回宣言するだけで済みます。

- <xref:System.Security.Permissions.StrongNameIdentityPermission> を使用する場合、共有する型をパブリックとして宣言する必要があります。 フレンド アセンブリを使用する場合、共有する型は `internal` (C#) または `Friend` (Visual Basic) として宣言します。

アセンブリの `internal` (C#) または `Friend` (Visual Basic) 型およびメソッドに、モジュール ファイル ( *.netmodule* 拡張子の付いたファイル) からアクセスする方法については、[-moduleassemblyname (C#)](../../csharp/language-reference/compiler-options/moduleassemblyname-compiler-option.md) または [-moduleassemblyname (Visual Basic)](../../visual-basic/reference/command-line-compiler/moduleassemblyname.md) に関する記事を参照してください。

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>
- <xref:System.Security.Permissions.StrongNameIdentityPermission>
- [方法: 署名のないフレンド アセンブリを作成する](create-unsigned-friend.md)
- [方法: 署名されたフレンド アセンブリを作成する](create-signed-friend.md)
- [.NET のアセンブリ](index.md)
- [C# プログラミング ガイド](../../csharp/programming-guide/index.md)
- [プログラミングの概念 (Visual Basic)](../../visual-basic/programming-guide/concepts/index.md)
