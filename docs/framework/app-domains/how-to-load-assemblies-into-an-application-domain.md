---
title: '方法 : アプリケーション ドメインにアセンブリを読み込む'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- application domains, loading assemblies
- loading assemblies
ms.assetid: 1432aa2d-bd83-4346-bf3b-a1b7920e2aa9
ms.openlocfilehash: c560e2c5858de67442ccbcd18c8f92b142cc178d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73119894"
---
# <a name="how-to-load-assemblies-into-an-application-domain"></a>方法 : アプリケーション ドメインにアセンブリを読み込む
アプリケーション ドメインにアセンブリを読み込むには、いくつかの方法があります。 推奨されているのは、<xref:System.Reflection.Assembly?displayProperty=nameWithType> クラスの `static` (Visual Basic では `Shared`) <xref:System.Reflection.Assembly.Load%2A> メソッドを使用する方法です。 それ以外には、以下の方法でアセンブリを読み込むことができます。  
  
- <xref:System.Reflection.Assembly> クラスの <xref:System.Reflection.Assembly.LoadFrom%2A> メソッドは、ファイルの場所が指定されたアセンブリを読み込みます。 このメソッドでアセンブリを読み込む場合は、別の読み込みコンテキストが使用されます。  
  
- <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A> メソッドと <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A> メソッドは、リフレクション専用コンテキストにアセンブリを読み込みます。 このコンテキストに読み込まれたアセンブリは検査できますが、実行することはできません。その結果、他のプラットフォームをターゲットにしているアセンブリを検査できます。 「[方法: リフレクションのみのコンテキストにアセンブリを読み込む](../reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)」を参照してください。  
  
> [!NOTE]
> リフレクション専用コンテキストは、.NET Framework Version 2.0 で新たに追加されました。  
  
- <xref:System.AppDomain> クラスの <xref:System.AppDomain.CreateInstance%2A> や <xref:System.AppDomain.CreateInstanceAndUnwrap%2A> などのメソッドは、アプリケーション ドメインにアセンブリを読み込むことができます。  
  
- <xref:System.Type> クラスの <xref:System.Type.GetType%2A> メソッドは、アセンブリを読み込むことができます。  
  
- <xref:System.AppDomain?displayProperty=nameWithType> クラスの <xref:System.AppDomain.Load%2A> メソッドは、アセンブリを読み込むことができますが、主に COM の相互運用性のために使用されます。 このメソッドの呼び出し元であるアプリケーション ドメイン以外のアプリケーション ドメインにアセンブリを読み込む場合は、このメソッドを使用しないでください。  
  
> [!NOTE]
> .NET Framework Version 2.0 以降では、現在読み込まれているランタイムよりもバージョン番号が新しい .NET Framework のバージョンでコンパイルされたアセンブリは、ランタイムに読み込まれないようになりました。 これはメジャー コンポーネントとマイナー コンポーネントの組み合わせたバージョン番号に適用されます。  
  
 読み込まれたアセンブリの Just-In-Time (JIT) コンパイル コードがアプリケーション ドメイン間で共有される方法を指定できます。 詳細については、「[アプリケーション ドメインとアセンブリ](application-domains.md#application-domains-and-assemblies)」を参照してください。  
  
## <a name="example"></a>例  
 次に示すコードは、"example.exe" または "example.dll" という名前のアセンブリを現在のアプリケーション ドメインに読み込み、アセンブリから `Example` という型を取得します。さらに、その型に使用する `MethodA` というパラメーターを持たないメソッドを取得して、そのメソッドを実行します。 読み込まれたアセンブリから情報を取得する方法の詳細については、「[型の動的な読み込みおよび使用](../reflection-and-codedom/dynamically-loading-and-using-types.md)」を参照してください。  
  
 [!code-cpp[System.AppDomain.Load#2](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.appdomain.load/cpp/source2.cpp#2)]
 [!code-csharp[System.AppDomain.Load#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.load/cs/source2.cs#2)]
 [!code-vb[System.AppDomain.Load#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.load/vb/source2.vb#2)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A>
- [アプリケーション ドメインを使用したプログラミング](application-domains.md#programming-with-application-domains)
- [リフレクション](../reflection-and-codedom/reflection.md)
- [アプリケーション ドメインの使用](use.md)
- [方法: リフレクションのみのコンテキストにアセンブリを読み込む](../reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)
- [アプリケーション ドメインとアセンブリ](application-domains.md#application-domains-and-assemblies)
