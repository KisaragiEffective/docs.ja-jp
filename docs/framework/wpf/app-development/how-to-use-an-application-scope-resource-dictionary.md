---
title: '方法 : アプリケーション スコープのリソース ディクショナリを使用する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dictionaries [WPF], resource
- resource dictionaries [WPF], application-scope
- application-scope resource dictionaries
ms.assetid: 53857682-bd2c-4f2c-8f25-1307d0b451a2
ms.openlocfilehash: 5bfb3ed0304598a5acf4b7682bf4a4169c5153d1
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459799"
---
# <a name="how-to-use-an-application-scope-resource-dictionary"></a>方法 : アプリケーション スコープのリソース ディクショナリを使用する
この例では、アプリケーション スコープのカスタム リソース ディクショナリを定義して、使用する方法を示します。  
  
## <a name="example"></a>例  
 <xref:System.Windows.Application> は、<xref:System.Windows.Application.Resources%2A>共有リソースのアプリケーションスコープストアを公開します。 既定では、<xref:System.Windows.Application.Resources%2A> プロパティは <xref:System.Windows.ResourceDictionary> の型のインスタンスで初期化されます。 <xref:System.Windows.Application.Resources%2A>を使用してアプリケーションスコープのプロパティを取得および設定するときに、このインスタンスを使用します。 詳細については、「[方法: アプリケーションスコープのリソースを取得して設定する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/aa348547(v=vs.100))」を参照してください。
  
 <xref:System.Windows.Application.Resources%2A>を使用して複数のリソースを設定する場合は、代わりにカスタムリソースディクショナリを使用してそれらのリソースを格納し、代わりに <xref:System.Windows.Application.Resources%2A> を設定することができます。 次に、XAML を使用してカスタムリソースディクショナリを宣言する方法を示します。
  
 [!code-xaml[HOWTOResourceDictionaries#1](~/samples/snippets/csharp/VS_Snippets_Wpf/HowToResourceDictionaries/CSharp/MyResourceDictionary.xaml#1)]  
  
 <xref:System.Windows.Application.Resources%2A> を使用してリソースディクショナリ全体をスワップすると、アプリケーションスコープのテーマをサポートできます。各テーマは、1つのリソースディクショナリでカプセル化されます。 <xref:System.Windows.ResourceDictionary> を設定する方法を次の例に示します。  
  
 [!code-xaml[HOWTOResourceDictionaries#2](~/samples/snippets/csharp/VS_Snippets_Wpf/HowToResourceDictionaries/CSharp/App.xaml#2)]  
  
 XAML で <xref:System.Windows.Application.Resources%2A> によって公開されているリソースディクショナリからアプリケーションスコープリソースを取得する方法を次に示します。  
  
 [!code-xaml[HOWTOResourceDictionaries#4](~/samples/snippets/csharp/VS_Snippets_Wpf/HowToResourceDictionaries/CSharp/MainWindow.xaml#4)]  
  
 コードでリソースも取得する方法を次に示します。  
  
 [!code-csharp[HOWTOResourceDictionaries#3](~/samples/snippets/csharp/VS_Snippets_Wpf/HowToResourceDictionaries/CSharp/MainWindow.xaml.cs#3)]
 [!code-vb[HOWTOResourceDictionaries#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HowToResourceDictionaries/VB/MainWindow.xaml.vb#3)]  
  
 <xref:System.Windows.Application.Resources%2A>を使用する場合は、2つの点を考慮する必要があります。 まず、ディクショナリ*キー*はオブジェクトであるため、プロパティ値を設定して取得するときは、まったく同じオブジェクトインスタンスを使用する必要があります。 (文字列を使用する場合、キーは大文字と小文字が区別されることに注意してください)。2つ目は、ディクショナリ*値*がオブジェクトであるため、プロパティ値を取得するときに、値を目的の型に変換する必要があります。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.ResourceDictionary>
- <xref:System.Windows.Application.Resources%2A>
- [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)
- [マージされたリソース ディクショナリ](../advanced/merged-resource-dictionaries.md)
