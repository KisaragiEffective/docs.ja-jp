---
title: '方法 : 階層 XML データでマスター詳細パターンを使用する'
ms.date: 03/30/2017
helpviewer_keywords:
- data binding [WPF], Master-Detail data paradigm
- Master-Detail data paradigm
ms.assetid: eb8dbdd8-5871-42bb-a16b-04e655fea677
ms.openlocfilehash: 0fe42d57fcaf4acee09340fdb3f5df73d7291566
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73733418"
---
# <a name="how-to-use-the-master-detail-pattern-with-hierarchical-xml-data"></a>方法 : 階層 XML データでマスター詳細パターンを使用する
この例では、XML データを使用してマスター詳細シナリオを実装する方法を示します。  
  
## <a name="example"></a>例  
 この例は、「[階層データでマスター詳細パターンを使用する](how-to-use-the-master-detail-pattern-with-hierarchical-data.md)」で説明されている例の XML データバージョンです。 この例では、データは `League.xml`ファイルからのものです。 3番目の <xref:System.Windows.Controls.ListBox> コントロールが、<xref:System.Windows.Controls.Primitives.Selector.SelectedValue%2A> プロパティにバインドすることによって、2番目の <xref:System.Windows.Controls.ListBox> の選択の変化を追跡する方法に注意してください。  
  
 [!code-xaml[MasterDetailXml#HowTo1](~/samples/snippets/csharp/VS_Snippets_Wpf/MasterDetailXml/CS/Window1.xaml#howto1)]  
[!code-xaml[MasterDetailXml#HowTo2](~/samples/snippets/csharp/VS_Snippets_Wpf/MasterDetailXml/CS/Window1.xaml#howto2)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.HierarchicalDataTemplate>
- [方法トピック](data-binding-how-to-topics.md)
