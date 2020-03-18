---
title: Windows ストア アプリのための .NET Framework のリフレクション
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- reflection, Windows Store apps
- .NET for Windows Store apps, TypeInfo class
ms.assetid: 0d07090c-9b47-4ecc-81d1-29d539603c9b
ms.openlocfilehash: 42bcfd4a1adc66511a1183807c09e77d1448c754
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75715885"
---
# <a name="reflection-in-the-net-framework-for-windows-store-apps"></a>Windows ストア アプリのための .NET Framework のリフレクション

.NET Framework 4.5 以降、.NET Framework には、Windows 8.x ストアアプリで使用する一連のリフレクション型とメンバーが含まれています。 これらの型およびメンバーは、完全な .NET Framework だけでなく Windows ストア アプリ用 .NET でも使用できます。 このドキュメントでは、これらと .NET Framework 4 以前のバージョンでの対応するものとの主な相違点について説明します。  
  
 Windows 8.x ストアアプリを作成する場合は、Windows 8.x ストアアプリ用 .NET のリフレクション型とメンバーを使用する必要があります。 デスクトップ アプリを使用する場合もこれらの型およびメンバーを使用できますが、必須ではないため、両方のタイプのアプリに同じコードを使用できます。  
  
## <a name="typeinfo-and-assembly-loading"></a>TypeInfo およびアセンブリの読み込み  
 Windows 8.x ストアアプリ用 .NET では、<xref:System.Reflection.TypeInfo> クラスに、.NET Framework 4 <xref:System.Type> クラスの機能の一部が含まれています。 <xref:System.Type> オブジェクトは型定義への参照を表し、<xref:System.Reflection.TypeInfo> オブジェクトは型定義自体を表します。 これによって、参照するアセンブリをランタイムが必ずしも読み込まなくても、<xref:System.Type> オブジェクトを操作できるようになります。 関連付けられた <xref:System.Reflection.TypeInfo> オブジェクトを取得すると、アセンブリが強制的に読み込まれます。  
  
 <xref:System.Reflection.TypeInfo> には <xref:System.Type>で使用できる多くのメンバーが含まれており、Windows 8.x ストアアプリ用 .NET のリフレクションプロパティの多くは <xref:System.Reflection.TypeInfo> オブジェクトのコレクションを返します。 <xref:System.Reflection.TypeInfo> オブジェクトから <xref:System.Type> オブジェクトを取得するには、<xref:System.Reflection.IReflectableType.GetTypeInfo%2A> メソッドを使用します。  
  
## <a name="query-methods"></a>クエリ メソッド  
 Windows 8.x ストアアプリ用 .NET では、配列を返すメソッドではなく <xref:System.Collections.Generic.IEnumerable%601> コレクションを返すリフレクションプロパティを使用します。 リフレクション コンテキストは、大型のアセンブリまたは型用に、これらのコレクションの限定的な走査を実装できます。  
  
 リフレクション プロパティは、継承ツリーを走査する代わりに、特定オブジェクトの宣言されたメソッドのみを返します。 また、フィルター処理に <xref:System.Reflection.BindingFlags> パラメーターを使用しません。 代わりに、返されるコレクションで LINQ クエリを使用することにより、フィルター処理をユーザー コード内で発生させます。 (`typeof(Object)` の結果などにより) ランタイムから始まるリフレクション オブジェクトの場合、継承ツリーを走査するには <xref:System.Reflection.RuntimeReflectionExtensions> クラスのヘルパー メソッドを使用するのが最も適切です。 カスタマイズされたリフレクション コンテキストのオブジェクトのコンシューマーは、これらのメソッドを使用できず、独自に継承ツリーを走査する必要があります。  
  
## <a name="restrictions"></a>制約  
 Windows 8.x ストアアプリでは、一部の .NET Framework の種類とメンバーへのアクセスが制限されています。 たとえば、<xref:System.Reflection.MethodInfo> オブジェクトを使用して、Windows 8.x ストアアプリ用 .NET に含まれていない .NET Framework メソッドを呼び出すことはできません。 また、Windows 8.x ストアアプリのコンテキスト内で安全と見なされない特定の型およびメンバーは、<xref:System.Runtime.InteropServices.Marshal> および <xref:System.Runtime.InteropServices.WindowsRuntime.WindowsRuntimeMarshal> メンバーと同様にブロックされます。 この制限は、.NET Framework の型とメンバーにのみ適用されます。ユーザー コードやサードパーティ コードは通常どおり呼び出すことができます。  
  
## <a name="example"></a>使用例  
 この例では、Windows 8.x ストアアプリ用 .NET のリフレクション型とメンバーを使用して、継承されたメソッドとプロパティを含む <xref:System.Globalization.Calendar> 型のメソッドとプロパティを取得します。 このコードを実行するには、[リフレクション] という名前のプロジェクトに `textblock1` という名前の <xref:Windows.UI.Xaml.Controls.TextBlock?displayProperty=nameWithType> コントロールを含む Windows 8.x ストアページのコードファイルに貼り付けます。 別の名前のプロジェクトにこのコードを張り付ける場合は、名前空間の名前をプロジェクトに一致するように変更してください。  
  
 [!code-csharp[System.ReflectionWinStoreApp#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.reflectionwinstoreapp/cs/mainpage.xaml.cs#1)]
 [!code-vb[System.ReflectionWinStoreApp#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.reflectionwinstoreapp/vb/mainpage.xaml.vb#1)]  
  
## <a name="see-also"></a>関連項目

- [リフレクション](reflection.md)
