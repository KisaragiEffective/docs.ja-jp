---
title: '方法: アプリケーション リソースを使用する'
ms.date: 03/30/2017
helpviewer_keywords:
- application resources [WPF]
- resources [WPF], application resources
ms.assetid: 507ea937-5191-406b-8797-0a3d9f94156d
ms.openlocfilehash: e4114466fa8016f8e31100d7a37038b0abfdccca
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460272"
---
# <a name="how-to-use-application-resources"></a>方法: アプリケーション リソースを使用する
この例では、アプリケーション リソースを使用する方法を示します。  
  
## <a name="example"></a>例  
 次の例は、アプリケーション定義ファイルを示しています。 アプリケーション定義ファイルでは、リソースセクション (<xref:System.Windows.Application.Resources%2A> プロパティの値) を定義します。 アプリケーション レベルで定義されているリソースには、そのアプリケーションの一部であるその他すべてのページからアクセスできます。 この例では、リソースは宣言済みのスタイルです。 コントロールテンプレートを含む完全なスタイルは長くなる可能性があるため、この例では、スタイルの <xref:System.Windows.Controls.ContentControl.ContentTemplate%2A> プロパティ setter 内で定義されているコントロールテンプレートを省略しています。  
  
 [!code-xaml[ResourcesApplication#PreTemplateResource](~/samples/snippets/csharp/VS_Snippets_Wpf/ResourcesApplication/CS/app.xaml#pretemplateresource)]  
[!code-xaml[ResourcesApplication#PostTemplateResource](~/samples/snippets/csharp/VS_Snippets_Wpf/ResourcesApplication/CS/app.xaml#posttemplateresource)]  
  
 次の例は、前の例で定義したアプリケーションレベルのリソースを参照する [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページを示しています。 リソースは、要求されたリソースの一意のリソースキーを指定する[StaticResource マークアップ拡張機能](staticresource-markup-extension.md)を使用して参照されます。 "GelButton" というキーを持つリソースが、現在のページで見つからないため、要求されているリソースのリソース ルックアップ スコープは、現在のページを越えて、定義されているアプリケーション レベルのリソースまで継続されます。  
  
 [!code-xaml[ResourcesApplication#ConsumingPage](~/samples/snippets/csharp/VS_Snippets_Wpf/ResourcesApplication/CS/page1.xaml#consumingpage)]  
  
## <a name="see-also"></a>関連項目

- [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)
- [アプリケーション管理の概要](../app-development/application-management-overview.md)
- [方法トピック](resources-how-to-topics.md)
