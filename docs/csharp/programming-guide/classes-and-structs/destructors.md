---
title: ファイナライザー - C# プログラミング ガイド
ms.date: 10/08/2018
helpviewer_keywords:
- ~ [C#], in finalizers
- C# language, finalizers
- finalizers [C#]
ms.assetid: 1ae6e46d-a4b1-4a49-abe5-b97f53d9e049
ms.openlocfilehash: c8ad738baa3ff76cf9ae8367f2fd2a1fb44a79d6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170300"
---
# <a name="finalizers-c-programming-guide"></a>ファイナライザー (C# プログラミング ガイド)
ガベージ コレクターによってクラス インスタンスが収集されている場合は、ファイナライザー (**デストラクター**とも呼ばれます) を使用して、最終的に必要なすべてのクリーンアップが実行されます。  
  
## <a name="remarks"></a>解説  
  
- ファイナライザーは、構造体には定義できません。 クラスでだけ使用します。  
  
- クラスで使用できるファイナライザーは 1 つだけです。  
  
- ファイナライザーを継承またはオーバーロードすることはできません。  
  
- ファイナライザーを呼び出すことはできません。 デストラクターは自動的に起動されます。  
  
- ファイナライザーは修飾子を取らず、パラメーターはありません。  
  
 たとえば、次はクラス `Car` に対するファイナライザーの宣言です。
  
 [!code-csharp[csProgGuideObjects#86](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#86)]  

ファイナライザーは、式本体の定義として実行することもできます。次に例を示します。

[!code-csharp[expression-bodied-finalizer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-destructor.cs#1)]  
  
 ファイナライザーは、オブジェクトの基底クラスで <xref:System.Object.Finalize%2A> を暗黙的に呼び出します。 そのため、ファイナライザーの呼び出しは、暗黙的に次のコードに解釈されます。  
  
```csharp  
protected override void Finalize()  
{  
    try  
    {  
        // Cleanup statements...  
    }  
    finally  
    {  
        base.Finalize();  
    }  
}  
```  
  
 つまり、派生が最も多いクラスから派生が最も少ないクラスまで、継承チェーンのすべてのインスタンスに対して、`Finalize` メソッドが再帰的に呼び出されます。  
  
> [!NOTE]
> 空のファイナライザーは使用しないでください。 ファイナライザーがクラスに存在するときは、エントリが `Finalize` キューで作成されます。 ファイナライザーを呼び出すと、ガベージ コレクターが呼び出され、このキューを処理します。 ファイナライザーが空の場合、パフォーマンスを不必要に低下させるだけです。  
  
 ファイナライザーがいつ呼び出されるかはガベージ コレクターによって決定されるため、プログラマは制御できません。 ガベージ コレクターは、アプリケーションが使用していないオブジェクトをチェックします。 終了処理が可能なオブジェクトと考えられる場合、ファイナライザー (存在する場合) を呼び出し、オブジェクトの格納に使用されているメモリを解放します。

 (.NET Core アプリケーションではなく) .NET Framework アプリケーションでは、プログラムが存在する場合、ファイナライザーも呼び出されます。
  
 <xref:System.GC.Collect%2A> を呼び出すことによって、ガベージ コレクションを強制的に行うことができます。ただし、パフォーマンスに問題が発生する可能性があるため、通常はこの処理を避けます。  
  
## <a name="using-finalizers-to-release-resources"></a>ファイナライザーを使ったリソースの解放  
 一般に C# では、ガベージ コレクションでランタイムにターゲットを設定しない言語で開発する場合ほど、メモリ管理を必要としません。 .NET Framework のガベージ コレクターが、オブジェクトに対するメモリの割り当てと解放を暗黙的に管理するからです。 ただし、ウィンドウ、ファイル、ネットワーク接続などのアンマネージ リソースをアプリケーションでカプセル化するとき、ファイナライザーを使ってこれらのリソースを解放する必要があります。 終了処理が可能なオブジェクトの場合、ガベージ コレクターはそのオブジェクトの `Finalize` メソッドを実行します。  
  
## <a name="explicit-release-of-resources"></a>リソースの明示的な解放  
 アプリケーションで高額な外部リソースを使用している場合、ガベージ コレクターがオブジェクトを解放する前にリソースを明示的に解放する手段を用意することが推奨されます。 この処理を行うには、オブジェクトに対して必要なクリーンアップを実行する `Dispose` メソッドを <xref:System.IDisposable> インターフェイスから実装します。 これによって、アプリケーションのパフォーマンスを大幅に向上させることができます。 このようにリソースを明示的に制御する場合でも、ファイナライザーは、`Dispose` メソッドの呼び出しが失敗したときにリソースをクリーンアップするための安全装置になります。  
  
 リソースのクリーンアップの詳細については、次のトピックを参照してください。  
  
- [アンマネージ リソースのクリーンアップ](../../../standard/garbage-collection/unmanaged.md)  
  
- [Dispose メソッドの実装](../../../standard/garbage-collection/implementing-dispose.md)  
  
- [using ステートメント](../../language-reference/keywords/using-statement.md)  
  
## <a name="example"></a>例  
 次の例では、継承チェーンを形成する 3 つのクラスを作成します。 `First` が基底クラスであり、`Second` は `First` から派生し、`Third` は `Second` から派生します。 3 つのクラスのいずれにもファイナライザーがあります。 `Main` では、派生が最も多いクラスのインスタンスが作成されます。 プログラムを実行すると、3 つのクラスのファイナライザーが派生が最も多いクラスから派生が最も少ないクラスの順に自動的に呼び出されます。  
  
 [!code-csharp[csProgGuideObjects#85](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#85)]  
  
## <a name="c-language-specification"></a>C# 言語仕様  

詳細については、「[C# 言語仕様](~/_csharplang/spec/classes.md#destructors)」の [デストラクタ―](/dotnet/csharp/language-reference/language-specification/introduction)に関するセクションを参照してください。
  
## <a name="see-also"></a>参照

- <xref:System.IDisposable>
- [C# プログラミングガイド](../index.md)
- [コンストラクター](./constructors.md)
- [ガベージ コレクション](../../../standard/garbage-collection/index.md)
