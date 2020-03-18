---
title: '方法: アセンブリの完全修飾名を検索する'
ms.date: 08/20/2019
helpviewer_keywords:
- names [.NET Framework], fully qualified type names
- names [.NET Framework], assemblies
- assemblies [.NET Framework], names
ms.assetid: 009dae23-e1f6-4a64-9a9a-32e4c34802b0
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 49ebaeabee7a346fb84f09e5a9e34590d1ea9811
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74348198"
---
# <a name="how-to-find-an-assemblys-fully-qualified-name"></a>方法: アセンブリの完全修飾名を検索する

グローバル アセンブリ キャッシュ内の .NET Framework アセンブリの完全修飾名を検出するには、グローバル アセンブリ キャッシュ ツール ([Gacutil.exe](../../framework/tools/gacutil-exe-gac-tool.md)) を使用します。 「[方法:グローバル アセンブリ キャッシュの内容を表示する](../../framework/app-domains/how-to-view-the-contents-of-the-gac.md)」を参照してください。

.NET Core アセンブリの場合、およびグローバル アセンブリ キャッシュにない .NET Framework アセンブリの場合、完全修飾アセンブリ名をいくつかの方法で取得できます。

- コードを使用しコンソールや変数に情報を出力したり、[Ildasm.exe (IL 逆アセンブラー)](../../framework/tools/ildasm-exe-il-disassembler.md) を使用して完全修飾名を含むアセンブリのメタデータを調べたりできます。

- アセンブリがアプリケーションによって既に読み込まれている場合、<xref:System.Reflection.Assembly.FullName%2A?displayProperty=nameWithType> プロパティの値を取得して、完全修飾名を取得できます。 アセンブリに定義されている <xref:System.Type> の <xref:System.Type.Assembly> プロパティを使用して、<xref:System.Reflection.Assembly> オブジェクトへの参照を取得できます。 具体的な例を次に示します。

- アセンブリのファイル システム パスがわかっている場合は、`static` (C#) または `Shared` (Visual Basic) <xref:System.Reflection.AssemblyName.GetAssemblyName%2A?displayProperty=nameWithType> メソッドを呼び出して、完全修飾アセンブリ名を取得できます。 単純な例を次に示します。

  ```csharp
  using System;
  using System.Reflection;

  public class Example
  {
     public static void Main()
     {
        Console.WriteLine(AssemblyName.GetAssemblyName(@".\UtilityLibrary.dll"));
     }
  }
  // The example displays output like the following:
  //   UtilityLibrary, Version=1.1.0.0, Culture=neutral, PublicKeyToken=null
  ```

  ```vb
  Imports System.Reflection

  Public Module Example
     Public Sub Main
        Console.WriteLine(AssemblyName.GetAssemblyName(".\UtilityLibrary.dll"))
     End Sub
  End Module
  ' The example displays output like the following:
  '   UtilityLibrary, Version=1.1.0.0, Culture=neutral, PublicKeyToken=null
  ```

- [Ildasm.exe (IL 逆アセンブラー)](../../framework/tools/ildasm-exe-il-disassembler.md) を使用して、アセンブリのメタデータを使用できます。これには完全修飾名が含まれています。

バージョン、カルチャ、アセンブリ名などのアセンブリ属性の設定の詳細については、「[アセンブリ属性の設定](set-attributes.md)」を参照してください。 アセンブリに厳密な名前を指定する方法の詳細については、「[厳密な名前付きアセンブリの作成と使用](create-use-strong-named.md)」を参照してください。

## <a name="example"></a>例

指定したクラスを含むアセンブリの完全修飾名をコンソールに表示する例を次に示します。 <xref:System.Type.Assembly?displayProperty=nameWithType> プロパティを使用して、そのアセンブリで定義されている型からアセンブリへの参照を取得します。

```cpp
#using <System.dll>
#using <System.Data.dll>

using namespace System;
using namespace System::Reflection;

ref class asmname
{
public:
    static void Main()
    {
        Type^ t = System::Data::DataSet::typeid;
        String^ s = t->Assembly->FullName->ToString();
        Console::WriteLine("The fully qualified assembly name " +
            "containing the specified class is {0}.", s);
    }
};

int main()
{
    asmname::Main();
}
```

```csharp
using System;
using System.Reflection;

class asmname
{
    public static void Main()
    {
        Type t = typeof(System.Data.DataSet);
        string s = t.Assembly.FullName.ToString();
        Console.WriteLine("The fully qualified assembly name " +
            "containing the specified class is {0}.", s);
    }
}
```

```vb
Imports System.Reflection

Class asmname
    Public Shared Sub Main()
        Dim t As Type = GetType(System.Data.DataSet)
        Dim s As String = t.Assembly.FullName.ToString()
        Console.WriteLine("The fully qualified assembly name " +
            "containing the specified class is {0}.", s)
    End Sub
End Class
```

## <a name="see-also"></a>関連項目

- [アセンブリ名](names.md)
- [アセンブリを作成する](create.md)
- [厳密な名前付きアセンブリの作成と使用](create-use-strong-named.md)
- [グローバル アセンブリ キャッシュ](../../framework/app-domains/gac.md)
- [ランタイムがアセンブリを検索する方法](../../framework/deployment/how-the-runtime-locates-assemblies.md)
