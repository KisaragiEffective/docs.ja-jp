---
title: CLS 準拠でない例外をキャッチする方法
ms.date: 07/20/2015
helpviewer_keywords:
- exceptions [C#], non-CLS
ms.assetid: db4630b3-5240-471a-b3a7-c7ff6ab31e8d
ms.openlocfilehash: 635cf0a9142f56dea4b2722fbf3f3eda505d85ee
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75346276"
---
# <a name="how-to-catch-a-non-cls-exception"></a>CLS 準拠でない例外をキャッチする方法
C++/CLI をはじめとする一部の .NET 言語では、<xref:System.Exception> から派生していない例外をオブジェクトでスローすることができます。 このような例外は "*CLS 準拠でない例外*" や "*非例外*" と呼ばれています。 C# では、CLS 準拠でない例外をスローすることはできませんが、それらをキャッチすることはできます。次の 2 とおりの方法があります。  
  
- `catch (RuntimeWrappedException e)` ブロック内で。
  
     Visual C# アセンブリの既定の動作上、CLS 準拠でない例外はラップされた例外としてキャッチされます。 元の例外にアクセスする必要がある場合 (<xref:System.Runtime.CompilerServices.RuntimeWrappedException.WrappedException%2A?displayProperty=nameWithType> プロパティからアクセスできます)、このメソッドを利用します。 この方法で例外をキャッチする方法については、このトピックで後述します。  
  
- 他のすべての `catch` ブロックの後に置いた汎用的な catch ブロック (例外の型が指定されていない catch ブロック) 内で。
  
     この方法は、CLS 準拠でない例外への応答として何らかのアクション (ログ ファイルへの書き込みなど) を実行したい場合で、なおかつ例外情報にアクセスする必要がない場合に使用します。 既定では、すべての例外が共通言語ランタイムによってラップされます。 この動作を無効にするには、`[assembly: RuntimeCompatibilityAttribute(WrapNonExceptionThrows = false)]` というアセンブリ レベルの属性を (通常、AssemblyInfo.cs ファイル内の) コードに追加します。  
  
### <a name="to-catch-a-non-cls-exception"></a>CLS 準拠でない例外をキャッチするには  
  
`catch(RuntimeWrappedException e)` ブロック内で、<xref:System.Runtime.CompilerServices.RuntimeWrappedException.WrappedException%2A?displayProperty=nameWithType> プロパティから元の例外にアクセスします。  
  
## <a name="example"></a>例  
 次の例では、C++/CLI で記述されたクラス ライブラリからスローされた、CLS 準拠でない例外をキャッチする方法を示します。 この例で、C# クライアント コードは、スローされる例外の型が <xref:System.String?displayProperty=nameWithType> であることを事前に把握しています。 その型にコードからアクセスできる限り、<xref:System.Runtime.CompilerServices.RuntimeWrappedException.WrappedException%2A?displayProperty=nameWithType> プロパティは元の型にキャストできます。  
  
```csharp
// Class library written in C++/CLI.
var myClass = new ThrowNonCLS.Class1();

try
{
    // throws gcnew System::String(  
    // "I do not derive from System.Exception!");  
    myClass.TestThrow();
}
catch (RuntimeWrappedException e)
{
    String s = e.WrappedException as String;
    if (s != null)
    {
        Console.WriteLine(s);
    }
}
```  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.CompilerServices.RuntimeWrappedException>
- [例外と例外処理](./index.md)
