---
title: カスタム属性へのアクセス
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- custom attributes, accessibility
- attributes [.NET Framework], accessing
- reflection, custom attributes
ms.assetid: 1d8e3398-00d8-47d5-a084-214f9859d3d7
ms.openlocfilehash: a5651e9dc8cf40e737dd523ec5d29e876a9c0765
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130298"
---
# <a name="accessing-custom-attributes"></a>カスタム属性へのアクセス
属性がプログラム要素に関連付けられると、リフレクションを使用して、その存在と値をクエリすることができます。 .NET Framework バージョン 1.0 および 1.1 では、カスタム属性は実行コンテキストで検証されます。 .NET Framework バージョン 2.0 では、新しい読み込みコンテキスト (リフレクションのみのコンテキスト) が提供されます。これを使用して、実行のために読み込むことができないコードを検証できます。  
  
## <a name="the-reflection-only-context"></a>リフレクションのみのコンテキスト  
 リフレクションのみのコンテキストに読み込まれたコードは、実行することができません。 つまり、コンストラクターを実行する必要があるため、カスタム属性のインスタンスは作成できないということです。 リフレクションのみのコンテキストにカスタム属性を読み込み、検証するには、<xref:System.Reflection.CustomAttributeData> クラスを使用します。 静的な <xref:System.Reflection.CustomAttributeData.GetCustomAttributes%2A?displayProperty=nameWithType> メソッドの適切なオーバーロードを使用して、このクラスのインスタンスを取得できます。 「[方法: リフレクションのみのコンテキストにアセンブリを読み込む](how-to-load-assemblies-into-the-reflection-only-context.md)」を参照してください。  
  
## <a name="the-execution-context"></a>実行コンテキスト  
 実行コンテキストの属性をクエリする主なリフレクション メソッドは、<xref:System.Reflection.MemberInfo.GetCustomAttributes%2A?displayProperty=nameWithType> と <xref:System.Attribute.GetCustomAttributes%2A?displayProperty=nameWithType> です。  
  
 カスタム属性のアクセシビリティは、添付されているアセンブリに関してチェックされます。 これは、カスタム属性が添付されているアセンブリの型のメソッドが、カスタム属性のコンストラクターを呼び出すことができるかどうかをチェックするのと同じです。  
  
 <xref:System.Reflection.Assembly.GetCustomAttributes%28System.Boolean%29?displayProperty=nameWithType> などのメソッドは、型引数の可視性とアクセシビリティをチェックします。 ユーザー定義型を含むアセンブリのコードのみが、**GetCustomAttributes** を使用する型のカスタム属性を取得することができます。  
  
 次の C# の例は、一般的なカスタム属性の設計パターンです。 これは、ランタイムのカスタム属性のリフレクション モデルを示しています。  
  
```csharp
System.DLL  
public class DescriptionAttribute : Attribute  
{  
}  
  
System.Web.DLL  
internal class MyDescriptionAttribute : DescriptionAttribute  
{  
}  
  
public class LocalizationExtenderProvider  
{  
    [MyDescriptionAttribute(...)]  
    public CultureInfo GetLanguage(...)  
    {  
    }  
}  
```  
  
 このランタイムで **GetLanguage** メソッドに添付されている公開のカスタム属性の型 <xref:System.ComponentModel.DescriptionAttribute> のカスタム属性を取得しようとしている場合、次のアクションが実行されます。  
  
1. ランタイムは、**Type.GetCustomAttributes**(Type *type*) の型引数 **DescriptionAttribute** が公開であることをチェックするため、表示およびアクセスすることができます。  
  
2. ランタイムは、**DescriptionAttribute** から派生するユーザー定義型 **MyDescriptionAttribute** が **System.Web.DLL** アセンブリ内で表示およびアクセスできることをチェックします。これは、メソッド **GetLanguage**() に添付されている場所です。  
  
3. ランタイムは、**MyDescriptionAttribute** のコンストラクターが、**System.Web.DLL** アセンブリ内で表示およびアクセスできることをチェックします。  
  
4. ランタイムは、カスタム属性のパラメーターを使って **MyDescriptionAttribute** のコンストラクターを呼び出し、呼び出し元に新しいオブジェクトを返します。  
  
 カスタム属性のリフレクション モデルは、型が定義されているアセンブリの外にあるユーザー定義型のインスタンスをリークする可能性があります。 これは、**RuntimeMethodInfo** オブジェクトの配列を返す <xref:System.Type.GetMethods%2A?displayProperty=nameWithType> など、ユーザー定義型のインスタンスを返すランタイム システム ライブラリのメンバーと同様です。 クライアントがユーザー定義のカスタム属性の型に関する情報を検出できないようにするには、型のメンバーを非公開に定義します。  
  
 次の例では、カスタム属性にアクセスできるリフレクションを使用する基本的な方法を示します。  
  
 [!code-cpp[CustomAttributeData#2](../../../samples/snippets/cpp/VS_Snippets_CLR/CustomAttributeData/CPP/source2.cpp#2)]
 [!code-csharp[CustomAttributeData#2](../../../samples/snippets/csharp/VS_Snippets_CLR/CustomAttributeData/CS/source2.cs#2)]
 [!code-vb[CustomAttributeData#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CustomAttributeData/VB/source2.vb#2)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Reflection.MemberInfo.GetCustomAttributes%2A?displayProperty=nameWithType>
- <xref:System.Attribute.GetCustomAttributes%2A?displayProperty=nameWithType>
- [型情報の表示](viewing-type-information.md)
- [リフレクションに関するセキュリティ上の考慮事項](security-considerations-for-reflection.md)
