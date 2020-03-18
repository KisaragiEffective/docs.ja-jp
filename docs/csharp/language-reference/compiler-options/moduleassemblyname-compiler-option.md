---
title: -moduleassemblyname (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /moduleassemblyname
helpviewer_keywords:
- moduleassemblyname compiler option [C#]
- /moduleassemblyname compiler option [C#]
- .moduleassemblyname compiler option [C#]
ms.assetid: d464d9b9-f18d-423b-95e9-66c7878fd53a
ms.openlocfilehash: 1477eeb0f2e16e18cb86009739bc8e7d9dee2ac0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173719"
---
# <a name="-moduleassemblyname-c-compiler-option"></a>-moduleassemblyname (C# コンパイラ オプション)
.netmodule がアクセスできる非パブリック型のアセンブリを指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-moduleassemblyname:assembly_name  
```  
  
## <a name="arguments"></a>引数  
 `assembly_name`  
 .netmodule がアクセスできる非パブリック型のアセンブリの名前です。  
  
## <a name="remarks"></a>解説  
 **-moduleassemblyname** は、.netmodule をビルドする場合と、次の条件に当てはまる場合に使用する必要があります。  
  
- .netmodule で、既存のアセンブリ内の非パブリック型へのアクセスが必要な場合。  
  
- .netmodule をビルドするアセンブリの名前を把握していること。  
  
- 既存のアセンブリに、.netmodule をビルドするアセンブリへのフレンド アセンブリのアクセス権が付与されていること。  
  
 .netmodule のビルドの詳細については、「[-target:module (C# コンパイラ オプション)](./target-module-compiler-option.md)」を参照してください。  
  
 フレンド アセンブリの詳細については、「[フレンド アセンブリ](../../../standard/assembly/friend.md)」を参照してください。  
  
 このオプションは、開発環境からは利用できません。これはコマンドラインからコンパイルするときにのみ使用できます。  
  
 このコンパイラ オプションは Visual Studio では使用できず、プログラムで変更することはできません。  
  
## <a name="example"></a>例  
 このサンプルは、csman_an_assembly というアセンブリへのフレンド アセンブリのアクセス権を付与する、プライベート型のアセンブリをビルドします。  
  
```csharp  
// moduleassemblyname_1.cs  
// compile with: -target:library  
using System;  
using System.Runtime.CompilerServices;  
  
[assembly:InternalsVisibleTo ("csman_an_assembly")]  
  
class An_Internal_Class
{  
    public void Test()
    {
        Console.WriteLine("An_Internal_Class.Test called");
    }  
}  
```  
  
## <a name="example"></a>例  
 このサンプルは、アセンブリ moduleassemblyname_1.dll 内の非パブリック型にアクセスする .netmodule をビルドします。 この .netmodule が csman_an_assembly というアセンブリにビルドされることがわかっていれば、 **-moduleassemblyname** を指定して、csman_an_assembly へのフレンド アセンブリのアクセス権が付与されているアセンブリ内の非パブリック型へのアクセスを .netmodule に許可することができます。  
  
```csharp  
// moduleassemblyname_2.cs  
// compile with: -moduleassemblyname:csman_an_assembly -target:module -reference:moduleassemblyname_1.dll  
class B {  
    public void Test() {  
        An_Internal_Class x = new An_Internal_Class();  
        x.Test();  
    }  
}  
```  
  
## <a name="example"></a>例  
 このコード サンプルは、既にビルドされたアセンブリと .netmodule を参照する、アセンブリ csman_an_assembly をビルドします。  
  
```csharp  
// csman_an_assembly.cs  
// compile with: -addmodule:moduleassemblyname_2.netmodule -reference:moduleassemblyname_1.dll  
class A {  
    public static void Main() {  
        B bb = new B();  
        bb.Test();  
    }  
}  
```  
  
**An_Internal_Class.Test called**

## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
