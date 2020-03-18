---
title: .NET で新しい文字列を作成する
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CopyTo method
- Join method
- Format method
- Concat method
- strings [.NET Framework], creating
- Insert method
ms.assetid: 06fdf123-2fac-4459-8904-eb48ab908a30
ms.openlocfilehash: ef65c50111d6ba91ab70d0b9c8cb90c606f9366c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73103827"
---
# <a name="creating-new-strings-in-net"></a>.NET で新しい文字列を作成する
.NET Framework では、単純な割り当てを使用した文字列の作成をサポートしています。また、多数の異なるパラメーターを使用した文字列の作成をサポートするために、クラス コンストラクターをオーバーロードします。 また、.NET Framework では、複数の文字列、文字列の配列、またはオブジェクトを組み合わせて新しい文字列オブジェクトを作成する、<xref:System.String?displayProperty=nameWithType> クラスのいくつかのメソッドも提供しています。  
  
## <a name="creating-strings-using-assignment"></a>割り当てを使用した文字列の作成  
 新しい <xref:System.String> オブジェクトを作成する最も簡単な方法としては、単に文字列リテラルを <xref:System.String> オブジェクトに割り当てます。  
  
## <a name="creating-strings-using-a-class-constructor"></a>クラス コンストラクターを使用した文字列の作成  
 <xref:System.String> クラス コンストラクターのオーバーロードを使用すると、文字配列から文字列を作成できます。 また、指定した回数だけ特定の文字を複製することで、新しい文字列を作成することもできます。  
  
## <a name="methods-that-return-strings"></a>文字列を返すメソッド  
 次の表は、新しい文字列オブジェクトを返すいくつかの便利なメソッドを示しています。  
  
|メソッド名|用途|  
|-----------------|---------|  
|<xref:System.String.Format%2A?displayProperty=nameWithType>|入力オブジェクトのセットから、書式設定された文字列をビルドします。|  
|<xref:System.String.Concat%2A?displayProperty=nameWithType>|2 つ以上の文字列から文字列をビルドします。|  
|<xref:System.String.Join%2A?displayProperty=nameWithType>|文字列の配列を組み合わせることで、新しい文字列をビルドします。|  
|<xref:System.String.Insert%2A?displayProperty=nameWithType>|既存の文字列の指定のインデックスに文字列を挿入することで、新しい文字列をビルドします。|  
|<xref:System.String.CopyTo%2A?displayProperty=nameWithType>|文字配列内の指定の位置に、文字列内の指定の文字をコピーします。|  
  
### <a name="format"></a>Format  
 **String.Format** メソッドを使用すると、書式設定された文字列を作成し、複数のオブジェクトを表す文字列を連結できます。 このメソッドは、渡されたすべてのオブジェクトを文字列に自動的に変換します。 たとえば、アプリケーションでユーザーに対して **Int32** 値と **DateTime** 値を表示する必要がある場合、**Format** メソッドを使用して、これらの値を表す文字列を簡単に作成できます。 このメソッドで使用される書式設定規則については、[複合書式指定](../../../docs/standard/base-types/composite-formatting.md)に関するセクションを参照してください。  
  
 次の例では、**Format** メソッドを使用して、整数型の変数を使用する文字列を作成します。  
  
 [!code-csharp[Strings.Creating#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#1)]
 [!code-vb[Strings.Creating#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#1)]  
  
 この例では、<xref:System.DateTime.Now%2A?displayProperty=nameWithType> は、現在のスレッドに関連付けられているカルチャで指定された方法で現在の日時を表示します。  
  
### <a name="concat"></a>Concat  
 **String.Concat** メソッドを使用すると、2 つ以上の既存のオブジェクトから新しい文字列オブジェクトを簡単に作成できます。 言語に依存せずに文字列を連結する方法を提供します。 このメソッドは、**System.Object** から派生したすべてのクラスを受け入れます。 次の例では、2 つの既存の文字列オブジェクトおよび区切り文字から文字列を作成します。  
  
 [!code-csharp[Strings.Creating#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#2)]
 [!code-vb[Strings.Creating#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#2)]  
  
### <a name="join"></a>結合  
 **String.Join** メソッドは、文字列の配列および区切り文字列から新しい文字列を作成します。 このメソッドは、複数の文字列を連結して、たとえばコンマで区切られたリストを作成する場合に便利です。  
  
 次の例では、スペースを使用して文字列の配列をバインドします。  
  
 [!code-csharp[Strings.Creating#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#3)]
 [!code-vb[Strings.Creating#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#3)]  
  
### <a name="insert"></a>挿入  
 **String.Insert** メソッドは、別の文字列内の指定の位置に文字列を挿入することで、新しい文字列を作成します。 このメソッドは、0 から始まるインデックスを使用します。 次の例では、`MyString` の 5 番目のインデックス位置に文字列を挿入し、この値を持つ新しい文字列を作成します。  
  
 [!code-csharp[Strings.Creating#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#4)]
 [!code-vb[Strings.Creating#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#4)]  
  
### <a name="copyto"></a>CopyTo  
 **String.CopyTo** メソッドは、文字配列に文字列の部分をコピーします。 文字列の開始インデックスと、コピーする文字数の両方を指定できます。 このメソッドは、ソース インデックス、文字配列、コピー先のインデックス、およびコピーする文字数を受け取ります。 すべてのインデックスは 0 から始まります。  
  
 次の例では、**CopyTo** メソッドを使用して、文字列オブジェクトから文字配列の最初のインデックス位置に、単語 "Hello" の文字をコピーします。  
  
 [!code-csharp[Strings.Creating#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#5)]
 [!code-vb[Strings.Creating#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#5)]  
  
## <a name="see-also"></a>参照

- [基本的な文字列操作](../../../docs/standard/base-types/basic-string-operations.md)
- [複合書式指定](../../../docs/standard/base-types/composite-formatting.md)
