---
title: openGenericCERCall MDA
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), CER calls
- open generic CER calls
- constrained execution regions
- openGenericCERCall MDA
- CER calls
- managed debugging assistants (MDAs), CER calls
- generics [.NET Framework], open generic CER calls
ms.assetid: da3e4ff3-2e67-4668-9720-fa776c97407e
ms.openlocfilehash: de1735103314dfedbabe27623f579ce2c1e728af
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217275"
---
# <a name="opengenericcercall-mda"></a>openGenericCERCall MDA

`openGenericCERCall` マネージド デバッグ アシスタントは、ルート メソッドにジェネリック型変数を持つ制約された実行領域 (CER) グラフが JIT コンパイル時またはネイティブ イメージ生成時に処理されている場合に、少なくとも 1 つのジェネリック型変数がオブジェクト参照型であることを警告するためにアクティブ化されます。

## <a name="symptoms"></a>現象

スレッドが中止されたとき、またはアプリケーション ドメインがアンロードされたときに CER コードが実行されません。

## <a name="cause"></a>原因

JIT コンパイル時には、処理結果のコードが共有され、オブジェクト参照型変数がそれぞれ任意のオブジェクト参照型になる可能性があるため、オブジェクト参照型を含むインスタンス化は代理にすぎません。 このため、一部のランタイム リソースを前もって準備することが妨げられる場合があります。

特に、ジェネリック型変数を含むメソッドは、バックグラウンドでリソースを遅延割り当てする可能性があります。 このようなメソッドは、ジェネリック辞書エントリと呼ばれます。 たとえば、ステートメント `List<T> list = new List<T>();` `T` がジェネリック型の変数である場合、ランタイムは、実行時に正確なインスタンス化 (たとえば、`List<Object>, List<String>`など) を検索して作成する必要があります。 この操作は、メモリ不足など、開発者が制御できないさまざまな理由で失敗することがあります。

この MDA は、JIT コンパイル時にのみアクティブになり、正確なインスタンス化が存在するときにはアクティブになりません。

この MDA がアクティブになるとき、正しくないインスタンス化に対して CER が機能しないという症状が発生する可能性があります。 実際、MDA がアクティブになる状況下では、ランタイムは CER の実装を試みません。 そのため、開発者が CER の共有インスタンス化を使用している場合、目的の CER の領域内で発生した JIT コンパイル エラー、ジェネリック型の読み込みエラー、スレッドの中止などはキャッチされません。

## <a name="resolution"></a>解決策

CER が存在する可能性があるメソッドには、オブジェクト参照型であるジェネリック型変数を使用しないでください。

## <a name="effect-on-the-runtime"></a>ランタイムへの影響

この MDA は CLR に影響しません。

## <a name="output"></a>出力

この MDA からの出力の例を次に示します。
  
 ```output
 Method 'GenericMethodWithCer', which contains at least one constrained execution region, cannot be prepared automatically since it has one or more unbound generic type parameters.
 The caller must ensure this method is prepared explicitly at run time prior to execution. 
 method name="GenericMethodWithCer"
 declaringType name="OpenGenericCERCall"
 ```

## <a name="configuration"></a>構成

```xml
<mdaConfig>
  <assistants>
    <openGenericCERCall/>
  </assistants>
</mdaConfig>
```  

## <a name="example"></a>例

CER コードは実行されません。

```csharp
using System;
using System.Collections.Generic;
using System.Runtime.CompilerServices;

class Program
{
    static void Main(string[] args)
    {
        CallGenericMethods();
    }
    static void CallGenericMethods()
    {
        // This call is correct. The instantiation of the method
        // contains only nonreference types.
        MyClass.GenericMethodWithCer<int>();

        // This call is incorrect. A shared version of the method that
        // cannot be completely analyzed will be JIT-compiled. The 
        // MDA will be activated at JIT-compile time, not at run time.
        MyClass.GenericMethodWithCer<String>();
    }
}

class MyClass
{
    public static void GenericMethodWithCer<T>()
    {
        RuntimeHelpers.PrepareConstrainedRegions();
        try
        {

        }
        finally
        {
            // This is the start of the CER.
            Console.WriteLine("In finally block.");
        }
    }
}
```

## <a name="see-also"></a>参照

- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A>
- <xref:System.Runtime.ConstrainedExecution>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
