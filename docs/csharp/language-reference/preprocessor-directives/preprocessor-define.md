---
title: '#define - C# リファレンス'
ms.date: 06/30/2018
f1_keywords:
- '#define'
helpviewer_keywords:
- '#define directive [C#]'
ms.assetid: 23638b8f-779c-450e-b600-d55682de7d01
ms.openlocfilehash: c08d6f42c11184a4d14aa6712f9f0f8706a72cab
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173433"
---
# <a name="define-c-reference"></a>#define (C# リファレンス)
`#define` は、シンボルを定義するために使用します。 次の例に示すように、定義したシンボルを式として [#if](./preprocessor-if.md) ディレクティブに渡すと、式は `true` と評価されます。  

 ```csharp
 #define DEBUG
 ```
  
## <a name="remarks"></a>解説  
  
> [!NOTE]
> `#define` ディレクティブを使用して、通常 C および C++ で行うように定数値を宣言することはできません。 C# の定数は、クラスまたは構造体の静的メンバーとして定義することができます。 そのような定数がいくつかある場合は、それを保持するための "Constants" クラスを個別に作成することを検討してください。  
  
 シンボルを使用して、コンパイル条件を指定できます。 シンボルは、[#if](./preprocessor-if.md) または [#elif](./preprocessor-elif.md) で評価できます。 また、<xref:System.Diagnostics.ConditionalAttribute> を使用して、条件付きコンパイルを実行することもできます。  
  
 シンボルを定義することはできますが、シンボルに値は代入できません。 `#define` ディレクティブは、ファイル内で、プリプロセッサ ディレクティブではない他の命令よりも前に記述する必要があります。  
  
 シンボルは、[-define](../compiler-options/define-compiler-option.md) コンパイラ オプションでも定義できます。 [#undef](./preprocessor-undef.md) を使うと、シンボルを未定義状態にできます。  
  
 `-define` または `#define` で定義されたシンボルは、同じ名前の変数とは競合しません。 変数名をプリプロセッサ ディレクティブに渡すことはできません。シンボルはプリプロセッサ ディレクティブだけで評価されます。  
  
 `#define` で定義されたシンボルのスコープは、そのシンボルが定義されたファイル内だけです。  
  
 次の例に示すように、`#define` ディレクティブは、ファイルの先頭で指定する必要があります。  
  
```csharp  
#define DEBUG  
//#define TRACE  
#undef TRACE  
  
using System;  
  
public class TestDefine  
{  
    static void Main()  
    {  
#if (DEBUG)  
        Console.WriteLine("Debugging is enabled.");  
#endif  
  
#if (TRACE)  
     Console.WriteLine("Tracing is enabled.");  
#endif  
    }  
}  
// Output:  
// Debugging is enabled.  
```  
  
 シンボルの定義を解除する方法の例については、「[#undef](./preprocessor-undef.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# プリプロセッサ ディレクティブ](./index.md)
- [const](../keywords/const.md)
- [方法 : トレースとデバッグを指定して条件付きコンパイルを実行する](../../../framework/debug-trace-profile/how-to-compile-conditionally-with-trace-and-debug.md)
- [#undef](./preprocessor-undef.md)
- [#if](./preprocessor-if.md)
