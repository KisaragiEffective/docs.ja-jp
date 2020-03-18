---
title: '方法 : リフレクションのみのコンテキストにアセンブリを読み込む'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reflection, reflection-only loader context
- assemblies [.NET Framework], loading for reflection
- attributes [.NET Framework], retrieving
- assemblies [.NET Framework], reflection-only loader context
- reflection-only loader context
ms.assetid: 9818b660-52f5-423d-a9af-e75163aa7068
ms.openlocfilehash: cac6b3b3adf070ad6070e5c5941653f20dedd907
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130111"
---
# <a name="how-to-load-assemblies-into-the-reflection-only-context"></a>方法 : リフレクションのみのコンテキストにアセンブリを読み込む

リフレクションのみの読み込みコンテキストでは、他のプラットフォームや .NET Framework の他のバージョン用にコンパイルされたアセンブリを検査できます。 このコンテキストに読み込まれたコードは検査のみ可能で、実行できません。 つまり、コンストラクターを実行できないので、オブジェクトは作成できません。 また、コードを実行できないため、依存関係は自動的には読み込まれません。 依存関係を検査する必要がある場合は、依存関係を独自に読み込む必要があります。

## <a name="to-load-an-assembly-into-the-reflection-only-load-context"></a>リフレクションのみの読み込みのコンテキストにアセンブリを読み込むには

1. 表示名が指定されたアセンブリを読み込むには、<xref:System.Reflection.Assembly.ReflectionOnlyLoad%28System.String%29> メソッド オーバーロードを使用し、パスが指定されたアセンブリを読み込むには、<xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A> メソッドを使用します。 アセンブリがバイナリ イメージの場合は、<xref:System.Reflection.Assembly.ReflectionOnlyLoad%28System.Byte%5B%5D%29> メソッド オーバーロードを使用します。

    > [!NOTE]
    > リフレクションのみのコンテキストを使用して、実行コンテキストのバージョン以外の .NET Framework のバージョンから mscorlib.dll のバージョンを読み込むことはできません。

2. アセンブリに依存関係がある場合、<xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A> メソッドは依存関係を読み込みません。 依存関係を検査する必要がある場合は、依存関係を独自に読み込む必要があります。

3. アセンブリの <xref:System.Reflection.Assembly.ReflectionOnly%2A> プロパティを使用して、アセンブリをリフレクションのみのコンテキストに読み込むかどうかを決定します。

4. アセンブリまたはアセンブリ内の型に属性が適用されている場合は、<xref:System.Reflection.CustomAttributeData> クラスを使用してこれらの属性を検査し、リフレクションのみのコンテキストでコードが実行されないようにします。 <xref:System.Reflection.CustomAttributeData.GetCustomAttributes%2A?displayProperty=nameWithType> メソッドの適切なオーバーロードを使用して、アセンブリ、メンバー、モジュール、またはパラメーターに適用されている属性を表す <xref:System.Reflection.CustomAttributeData> オブジェクトを取得します。

    > [!NOTE]
    > アセンブリまたはそのコンテンツに適用されている属性は、そのアセンブリで定義されている場合もあれば、リフレクションのみのコンテキストに読み込まれた別のアセンブリで定義されている場合もあります。 属性がどこに定義されているかを事前に確認する方法はありません。

## <a name="example"></a>例

リフレクションのみのコンテキストに読み込まれたアセンブリに適用されている属性を検査する方法を以下のコード例に示します。

このコード例では、2 つのコンストラクターと 1 つのプロパティでカスタム属性を定義しています。 この属性は、アセンブリ、アセンブリで宣言された型、型のメソッド、メソッドのパラメーターに適用されます。 このコード例を実行すると、アセンブリがそれ自体をリフレクションのみのコンテキストに読み込み、アセンブリとそれに含まれている型およびメンバーに適用されているカスタム属性に関する情報を表示します。

> [!NOTE]
> コード例を簡素にするために、アセンブリがそれ自体を読み込んで検査します。 通常、同じアセンブリが実行コンテキストとリフレクションのみのコンテキストの両方に読み込まれることはありません。

[!code-cpp[CustomAttributeData#1](../../../samples/snippets/cpp/VS_Snippets_CLR/CustomAttributeData/CPP/source.cpp#1)]
[!code-csharp[CustomAttributeData#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CustomAttributeData/CS/source.cs#1)]
[!code-vb[CustomAttributeData#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CustomAttributeData/VB/source.vb#1)]

## <a name="see-also"></a>関連項目

- <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A>
- <xref:System.Reflection.Assembly.ReflectionOnly%2A>
- <xref:System.Reflection.CustomAttributeData>
