---
title: ローカリゼーション属性とコメント
ms.date: 03/30/2017
helpviewer_keywords:
- localization [WPF], attributes
- localization [WPF], comments
ms.assetid: ead2d9ac-b709-4ec1-a924-39927a29d02f
ms.openlocfilehash: 4f9c2700d8163988b7ea1e75bec1427778cf571c
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72004894"
---
# <a name="localization-attributes-and-comments"></a>ローカリゼーション属性とコメント
ローカリゼーションコメント [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は、開発者がローカライズに関する規則やヒントを提供するために、XAML ソースコード内のプロパティです。 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] ローカリゼーション コメントには、ローカライズ可否属性と自由形式のローカリゼーション コメントの 2 つの情報が含まれます。 ローカライズ可否属性は、ローカライズするリソースを示すために [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ローカリゼーション API によって使用されます。 自由形式のコメントは、アプリケーションの作成者が含めたい任意の情報です。  

<a name="Localizer_Comments_"></a>   
## <a name="localization-comments"></a>ローカリゼーション コメント  
 マークアップアプリケーションの作成者が、テキストの長さ、フォントファミリ、またはフォントサイズの制約など、XAML の特定の要素に関する要件を持つ場合は、XAML コード内のコメントを使用して、この情報をローカライザーに渡すことができます。 ソース コードにコメントを追加する手順は次のとおりです。  
  
1. アプリケーション開発者は、XAML ソースコードにローカリゼーションコメントを追加します。  
  
2. ビルド プロセス中に、.proj ファイル内でアセンブリ内に自由形式のローカリゼーション コメントを残すかどうか、コメントの一部を取り除くか、またはすべてのコメントを取り除くかのいずれかを指定できます。 取り除かれたコメントは、別のファイルに配置されます。 次のような `LocalizationDirectivesToLocFile` タグを使用して、オプションを指定します。  
  
     `<LocalizationDirectivesToLocFile>` *value* `</LocalizationDirectivesToLocFile>`  
  
3. 割り当てることのできる値は次のとおりです。  
  
    - **None** - コメントと属性の両方がアセンブリ内に残り、別のファイルは生成されません。  
  
    - **CommentsOnly** - アセンブリからコメントだけを取り除き、別の LocFile に配置します。  
  
    - **All** - コメントと属性の両方をアセンブリから取り除き、両方とも別の 1 つの LocFile に配置します。  
  
4. ローカライズ可能なリソースが BAML から抽出された場合、ローカライズ可能な属性は BAML ローカライズ API によって尊重されます。  
  
5. 自由形式のコメントのみを含むローカリゼーション コメント ファイルは、後でローカライズ プロセスに組み込まれます。  
  
 次の例は、XAML ファイルにローカリゼーションコメントを追加する方法を示しています。  
  
 `<TextBlock x:Id = "text01"`  
  
 `FontFamily = "Microsoft Sans Serif"`  
  
 `FontSize = "12"`  
  
 `Localization.Attributes = "$Content (Unmodifiable Readable Text)`  
  
 `FontFamily (Unmodifiable Readable)"`  
  
 `Localization.Comments = "$Content (Trademark)`  
  
 `FontSize (Trademark font size)" >`  
  
 `Microsoft`  
  
 `</TextBlock>`  
  
 前のサンプル、Localization.Attributes セクションには、ローカリゼーション属性と Localization.Comments セクションの自由形式のコメントが含まれています。 次の表に、属性とコメントおよびローカライザーにとってのそれらの意味を示します。  
  
|ローカリゼーション属性|意味|  
|-----------------------------|-------------|  
|$Content (Unmodifiable Readable Text)|TextBlock 要素のコンテンツは変更できません。 ローカライザーは、"Microsoft" という単語を変更できません。 コンテンツは、ローカライザーに表示可能 (読み取り可能) です。 コンテンツのカテゴリはテキストです。|  
|FontFamily (Unmodifiable Readable)|TextBlock 要素のフォント ファミリ プロパティを変更することはできませんが、ローカライザーに表示することはできます。|  
  
|自由形式コメントのローカライズ|意味|  
|--------------------------------------|-------------|  
|$Content (Trademark)|アプリケーションの作成者が、TextBlock 要素内のコンテンツが商標であることをローカライザーに伝えます。|  
|FontSize (Trademark font size)|アプリケーションの作成者が、フォント サイズ プロパティが標準の商標のサイズに適切に従う必要があることを示します。|  
  
### <a name="localizability-attributes"></a>ローカライズ可否属性  
 Localization.Attributes 内の情報には、ターゲット値の名前と関連付けられているローカライズ可否の値のペアのリストが含まれています。 ターゲット名には、プロパティ名または特殊な $Content 名を指定できます。 プロパティ名を指定した場合は、ターゲット値はプロパティの値となります。 $Content を指定した場合は、ターゲット値は要素のコンテンツとなります。  
  
 次の 3 種類の属性があります。  
  
- **カテゴリ**。 これは、ローカライザー ツールで値を変更可能にするかどうかを指定します。 「<xref:System.Windows.LocalizabilityAttribute.Category%2A>」を参照してください。  
  
- **読みやすさ**。 これは、ローカライザー ツールで値を読み取る (および表示する) かどうかを指定します。 「<xref:System.Windows.LocalizabilityAttribute.Readability%2A>」を参照してください。  
  
- **変更可能性**。 これは、ローカライザー ツールで値の変更を許可するかどうかを指定します。 「<xref:System.Windows.LocalizabilityAttribute.Modifiability%2A>」を参照してください。  
  
 これらの属性は、スペースで区切られた任意の順序で指定できます。 重複する属性が指定された場合、最後の属性にオーバーライドされます。 たとえば、Localization.Attributes = "Unmodifiable Modifiable" は、これが最後の値であるため、変更可能性を Modifiable に設定します。  
  
 変更可能性と読みやすさはその名のとおりです。 カテゴリ属性は、テキストを変換するときに、ローカライザーを支援する定義済みのカテゴリを提供します。 Text、Label、Title などのカテゴリは、テキストの変換方法についてローカライザーに情報を提供します。 None、Inherit、Ignore、NeverLocalize の特殊なカテゴリもあります。  
  
 次の表に、特殊なカテゴリの意味を示します。  
  
|カテゴリ|意味|  
|--------------|-------------|  
|なし|ターゲット値に定義済みのカテゴリがありません。|  
|[継承する]|ターゲット値は、その親からそのカテゴリを継承します。|  
|無視|ローカライズ プロセスでは、ターゲット値は無視されます。 Ignore は現在の値にのみ影響します。 子ノードには影響しません。|  
|NeverLocalize|現在の値をローカライズすることはできません。 このカテゴリは、要素の子に継承されます。|  
  
<a name="Localization_Comments"></a>   
## <a name="localization-comments"></a>ローカリゼーション コメント  
 Localization.Comments には、ターゲットの値に関する自由形式の文字列が含まれます。 アプリケーション開発者は、アプリケーションのテキストを変換する方法についてのヒントをローカライザーに提供するための情報を追加できます。 コメントの形式は、"()" で囲まれた任意の文字列で指定できます。 文字をエスケープするには、'\\' を使用します。  
  
## <a name="see-also"></a>参照

- [WPF のグローバリゼーション](globalization-for-wpf.md)
- [自動レイアウトを使用してボタンを作成する](how-to-use-automatic-layout-to-create-a-button.md)
- [自動レイアウト用のグリッドを使用する](how-to-use-a-grid-for-automatic-layout.md)
- [アプリケーションをローカライズする](how-to-localize-an-application.md)
