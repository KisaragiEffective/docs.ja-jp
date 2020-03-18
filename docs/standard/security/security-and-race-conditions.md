---
title: セキュリティと競合状態
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [.NET Framework], race conditions
- race conditions
- secure coding, race conditions
- code security, race conditions
ms.assetid: ea3edb80-b2e8-4e85-bfed-311b20cb59b6
ms.openlocfilehash: bc0d9f481fd212ede55bffde6cc20c3e080629e4
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159417"
---
# <a name="security-and-race-conditions"></a>セキュリティと競合状態
問題のもう1つの領域は、競合状態によってセキュリティホールが悪用される可能性があることです。 これにはいくつかの方法があります。 次のサブトピックでは、開発者が回避する必要のある主要な落とし穴をいくつか紹介します。  
  
## <a name="race-conditions-in-the-dispose-method"></a>Dispose メソッドの競合状態  
 クラスの**dispose**メソッド (詳細については、「[ガベージコレクション](../../../docs/standard/garbage-collection/index.md)」を参照) が同期されていない場合は、次の例に示すように、 **dispose**内のクリーンアップコードを複数回実行できる可能性があります。  
  
```vb  
Sub Dispose()  
    If Not (myObj Is Nothing) Then  
       Cleanup(myObj)  
       myObj = Nothing  
    End If  
End Sub  
```  
  
```csharp  
void Dispose()
{  
    if (myObj != null)
    {  
        Cleanup(myObj);  
        myObj = null;  
    }  
}  
```  
  
 この**Dispose**実装は同期されていないため、最初の1つのスレッドによって `Cleanup` が呼び出され、`_myObj` が**null**に設定される前に2番目のスレッドが呼び出される可能性があります。 これがセキュリティ上の問題であるかどうかは、`Cleanup` コードの実行時の動作によって異なります。 非同期の**Dispose**実装の主な問題は、ファイルなどのリソースハンドルを使用することです。 不適切な破棄によって、誤ったハンドルが使用されることがあります。これは、多くの場合、セキュリティの脆弱性につながります。  
  
## <a name="race-conditions-in-constructors"></a>コンストラクターの競合状態  
 アプリケーションによっては、クラスコンストラクターが完全に実行される前に、他のスレッドがクラスメンバーにアクセスできる場合があります。 すべてのクラスコンストラクターを確認して、このような場合にセキュリティの問題が発生していないことを確認し、必要に応じてスレッドを同期してください。  
  
## <a name="race-conditions-with-cached-objects"></a>キャッシュされたオブジェクトを使用した競合状態  
 次の例に示すように、セキュリティ情報をキャッシュしたり、コードアクセスセキュリティ[アサート](../../../docs/framework/misc/using-the-assert-method.md)操作を使用したりするコードは、クラスの他の部分が適切に同期されていない場合に、競合状態に対して脆弱になることがあります。  
  
```vb  
Sub SomeSecureFunction()  
    If SomeDemandPasses() Then  
        fCallersOk = True  
        DoOtherWork()  
        fCallersOk = False  
    End If  
End Sub  
  
Sub DoOtherWork()  
    If fCallersOK Then  
        DoSomethingTrusted()  
    Else  
        DemandSomething()  
        DoSomethingTrusted()  
    End If  
End Sub  
```  
  
```csharp  
void SomeSecureFunction()
{  
    if (SomeDemandPasses())
    {  
        fCallersOk = true;  
        DoOtherWork();  
        fCallersOk = false;  
    }  
}  
void DoOtherWork()
{  
    if (fCallersOK)
    {  
        DoSomethingTrusted();  
    }  
    else
    {  
        DemandSomething();  
        DoSomethingTrusted();  
    }  
}  
```  
  
 同じオブジェクトを持つ別のスレッドから呼び出すことができる `DoOtherWork` へのパスが他にある場合は、信頼されていない呼び出し元が要求を過去にスリップすることができます。  
  
 コードでセキュリティ情報がキャッシュされている場合は、この脆弱性を確認してください。  
  
## <a name="race-conditions-in-finalizers"></a>ファイナライザーでの競合状態  
 競合状態は、静的またはアンマネージリソースを参照するオブジェクトでも発生し、その後、そのファイナライザーで解放されます。 クラスのファイナライザーで操作されているリソースを複数のオブジェクトが共有している場合、そのオブジェクトは、そのリソースへのすべてのアクセスを同期する必要があります。  
  
## <a name="see-also"></a>参照

- [安全なコーディングのガイドライン](../../../docs/standard/security/secure-coding-guidelines.md)
