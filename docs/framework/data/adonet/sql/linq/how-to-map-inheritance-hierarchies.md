---
title: '方法 : 継承階層を割り当てる'
ms.date: 03/30/2017
ms.assetid: b27c779b-9355-4dc7-b95f-7dfd504b6e48
dev_langs:
- csharp
- vb
ms.openlocfilehash: 737cb8743d8fd9c93cd46ebf50fba3fe554a35f2
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634665"
---
# <a name="how-to-map-inheritance-hierarchies"></a>方法 : 継承階層を割り当てる
LINQ で継承マッピングを実装するには、次の手順に従って、継承階層のルートクラスで属性および属性プロパティを指定する必要があります。 Visual Studio を使用する開発者は、オブジェクトリレーショナルデザイナーを使用して継承階層をマップできます。 「[方法: O/R デザイナーを使用して継承を構成する](/visualstudio/data-tools/how-to-configure-inheritance-by-using-the-o-r-designer)」を参照してください。  
  
> [!NOTE]
> サブクラスには特別な属性やプロパティは必要ありません。 特に、サブクラスに <xref:System.Data.Linq.Mapping.TableAttribute> 属性がない点に注意してください。  
  
### <a name="to-map-an-inheritance-hierarchy"></a>継承階層を割り当てるには  
  
1. ルート クラスに <xref:System.Data.Linq.Mapping.TableAttribute> 属性を追加します。  
  
2. ルート クラスに対し、階層構造の各クラスについて <xref:System.Data.Linq.Mapping.InheritanceMappingAttribute> 属性を追加します。  
  
3. 各 <xref:System.Data.Linq.Mapping.InheritanceMappingAttribute> 属性に対し、<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.Code%2A> プロパティを定義します。  
  
     このプロパティは、データベース テーブルの <xref:System.Data.Linq.Mapping.ColumnAttribute.IsDiscriminator%2A> 列に含まれる値を保持し、このデータ行が属するクラスまたはサブクラスを示します。  
  
4. 各 <xref:System.Data.Linq.Mapping.InheritanceMappingAttribute> 属性に対し、<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.Type%2A> プロパティも追加します。  
  
     このプロパティは、キー値が示すクラスまたはサブクラスを表す値を保持します。  
  
5. 1 つの <xref:System.Data.Linq.Mapping.InheritanceMappingAttribute> 属性にのみ、<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.IsDefault%2A> プロパティを追加します。  
  
     このプロパティは、データベーステーブルの識別子の値が継承マッピングのどの <xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.Code%2A> 値とも一致しない場合に、*フォールバック*マッピングを指定するために機能します。  
  
6. <xref:System.Data.Linq.Mapping.ColumnAttribute.IsDiscriminator%2A> 属性に <xref:System.Data.Linq.Mapping.ColumnAttribute> プロパティを追加します。  
  
     このプロパティは、<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.Code%2A> 値を保持する列であることを示します。  
  
## <a name="example"></a>使用例  
  
> [!NOTE]
> Visual Studio を使用している場合は、オブジェクトリレーショナルデザイナーを使用して継承を構成できます。 「[方法: O/R デザイナーを使用して継承を構成する](/visualstudio/data-tools/how-to-configure-inheritance-by-using-the-o-r-designer)」を参照してください。  
  
 次のコード例では、`Vehicle` がルートクラスとして定義されており、前述の手順が LINQ の階層を記述するように実装されています。  
  
 [!code-csharp[DLinqCustomize#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCustomize/cs/Program.cs#4)]
 [!code-vb[DLinqCustomize#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCustomize/vb/Module1.vb#4)]  
  
## <a name="see-also"></a>関連項目

- [継承のサポート](inheritance-support.md)
- [方法 : コード エディターを使用してエンティティ クラスをカスタマイズする](how-to-customize-entity-classes-by-using-the-code-editor.md)
