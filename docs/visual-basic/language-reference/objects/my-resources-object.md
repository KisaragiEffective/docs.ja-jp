---
title: My.Resources オブジェクト
ms.date: 07/20/2015
f1_keywords:
- My.Resources
- My.Resources.MyResources.ResourceManager
- My.Resources.MyResources.Culture
helpviewer_keywords:
- My.Resources object
ms.assetid: 34c3f2dc-7b87-432c-9d5f-17ea666bb266
ms.openlocfilehash: 7f5d81194123ad2151a494a3cb79aa1955e0fdad
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350327"
---
# <a name="myresources-object"></a>My.Resources オブジェクト
アプリケーションのリソースにアクセスするためのプロパティとクラスを提供します。  
  
## <a name="remarks"></a>コメント  
 `My.Resources` オブジェクトは、アプリケーションのリソースへのアクセスを提供し、アプリケーションのリソースを動的に取得できるようにします。 詳細については、「[アプリケーションリソースの管理 (.net)](/visualstudio/ide/managing-application-resources-dotnet)」を参照してください。  
  
 `My.Resources` オブジェクトは、グローバルリソースのみを公開します。 フォームに関連付けられたリソースファイルへのアクセスは提供されません。 フォームリソースには、フォームからアクセスする必要があります。  
  
 `My.Resources` オブジェクトから、アプリケーションのカルチャ固有のリソースファイルにアクセスできます。 既定では、`My.Resources` オブジェクトは、<xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.UICulture%2A> プロパティのカルチャに一致するリソースファイルからリソースを検索します。 ただし、この動作をオーバーライドして、リソースに使用する特定のカルチャを指定することができます。 詳細については、「[デスクトップ アプリケーションのリソース](../../../framework/resources/index.md)」を参照してください。  
  
## <a name="properties"></a>[プロパティ]  
 `My.Resources` オブジェクトのプロパティは、アプリケーションのリソースへの読み取り専用アクセスを提供します。 リソースを追加または削除するには、**プロジェクトデザイナー**を使用します。 **プロジェクトデザイナー**によって追加されたリソースにアクセスするには *、`My.Resources.`リソースを使用します*。  
  
 また、**ソリューションエクスプローラー**でプロジェクトを選択し、 **[プロジェクト]** メニューの **[新しい項目の追加]** または **[既存項目の追加]** をクリックして、リソースファイルを追加または削除することもできます。 この方法で追加されたリソースにアクセスするには、`My.Resources.`*Resourcefilename* *`.`を使用します*。  
  
 各リソースには名前、カテゴリ、および値があり、これらのリソースの設定によって、リソースにアクセスするためのプロパティが `My.Resources` オブジェクトにどのように表示されるかが決まります。 **プロジェクトデザイナー**に追加されたリソースの場合:  
  
- 名前によって、プロパティの名前が決まります。  
  
- リソースデータはプロパティの値です。  
  
- カテゴリによって、プロパティの種類が決まります。  
  
|カテゴリ|プロパティのデータ型|  
|---|---|  
|**文字列**|[String](../../../visual-basic/language-reference/data-types/string-data-type.md)|  
|**イメージ**|<xref:System.Drawing.Bitmap>|  
|**アイコン**|<xref:System.Drawing.Icon>|  
|**オーディオ**|<xref:System.IO.UnmanagedMemoryStream><br /><br /> <xref:System.IO.UnmanagedMemoryStream> クラスは <xref:System.IO.Stream> クラスから派生するので、<xref:Microsoft.VisualBasic.Devices.Audio.Play%2A> メソッドなどのストリームを受け取るメソッドで使用できます。|  
|**ファイル**|テキストファイルの[文字列](../../../visual-basic/language-reference/data-types/string-data-type.md)を -   します。<br />イメージファイルの <xref:System.Drawing.Bitmap> を -   します。<br />アイコンファイルの <xref:System.Drawing.Icon> を -   します。<br />サウンドファイルの <xref:System.IO.UnmanagedMemoryStream> を -   します。|  
|**その他**|デザイナーの**Type**列の情報によって決定されます。|  
  
## <a name="classes"></a>クラス  
 `My.Resources` オブジェクトは、各リソースファイルを共有プロパティを持つクラスとして公開します。 クラス名は、リソースファイルの名前と同じです。 前のセクションで説明したように、リソースファイル内のリソースはクラスのプロパティとして公開されます。  
  
## <a name="example"></a>例  
 この例では、フォームのタイトルを、アプリケーションリソースファイル内の `Form1Title` という名前の文字列リソースに設定します。 この例を使用するには、アプリケーションのリソースファイルに `Form1Title` という名前の文字列が必要です。  
  
 [!code-vb[VbVbalrMyResources#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#1)]  
  
## <a name="example"></a>例  
 この例では、フォームのアイコンを、アプリケーションのリソースファイルに格納されている `Form1Icon` という名前のアイコンに設定します。 この例を使用するには、アプリケーションのリソースファイルに `Form1Icon` という名前のアイコンが必要です。  
  
 [!code-vb[VbVbalrMyResources#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#2)]  
  
## <a name="example"></a>例  
 この例では、フォームの背景画像を、アプリケーションリソースファイル内の `Form1Background`という名前のイメージリソースに設定します。 この例を使用するには、アプリケーションのリソースファイルに `Form1Background` という名前のイメージリソースが必要です。  
  
 [!code-vb[VbVbalrMyResources#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#3)]  
  
## <a name="example"></a>例  
 この例では、アプリケーションのリソースファイルに `Form1Greeting` という名前のオーディオリソースとして格納されているサウンドを再生します。 この例を使用するには、アプリケーションのリソースファイルに `Form1Greeting` という名前のオーディオリソースが必要です。 `My.Computer.Audio.Play` メソッドは、Windows フォームアプリケーションに対してのみ使用できます。  
  
 [!code-vb[VbVbalrMyResources#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#4)]  
  
## <a name="example"></a>例  
 この例では、アプリケーションの文字列リソースのフランス語カルチャバージョンを取得します。 リソースには `Message`という名前が付けられます。 `My.Resources` オブジェクトが使用するカルチャを変更するために、この例では <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.ChangeUICulture%2A>を使用します。  
  
 この例を使用するには、アプリケーションのリソースファイルに `Message` という名前の文字列が必要です。また、アプリケーションには、そのリソースファイル Resources.fr のフランス語カルチャバージョンが必要です。 アプリケーションにフランス語カルチャバージョンのリソースファイルがない場合、`My.Resource` オブジェクトは、既定のカルチャリソースファイルからリソースを取得します。  
  
 [!code-vb[VbVbalrMyResources#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#10)]  
  
## <a name="see-also"></a>関連項目

- [アプリケーション リソースの管理 (.NET)](/visualstudio/ide/managing-application-resources-dotnet)
- [デスクトップ アプリケーションのリソース](../../../framework/resources/index.md)
