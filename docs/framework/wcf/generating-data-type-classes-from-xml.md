---
title: XML からのデータ型クラスの生成
ms.date: 03/30/2017
ms.assetid: e4e5e4e8-527f-44d1-92fa-8904a08784ea
ms.openlocfilehash: 977b12b5c61c196a4f033361d37785e4ed0af73a
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975851"
---
# <a name="generating-data-type-classes-from-xml"></a>XML からのデータ型クラスの生成
.NET Framework 4.5 には、XML からデータ型クラスを生成するための新しい機能が含まれています。 このトピックでは、.NET ブログ RSS フィードのデータ型を自動的に生成する方法について説明します。  
  
### <a name="obtaining-the-xml-from-the-net-blog-rss-feed"></a>.NET ブログの RSS フィードから XML を取得する  
  
1. Internet Explorer で、 [.Net ブログの RSS フィード](https://devblogs.microsoft.com/dotnet/feed/)に移動します。  
  
2. ページを右クリックし、 **[ソースの表示]** を選択します。  
  
3. Ctrl キーを押し**ながら A**キーを押してすべてのテキストを選択し、 **ctrl + C**キーを押してコピーすることで、フィードのテキストをコピーします。  
  
### <a name="creating-the-data-types"></a>データ型の作成  
  
1. プロキシが使用されるコード ファイルを開きます。 このファイルは、.NET Framework 4.5 プロジェクトの一部である必要があります。  
  
2. 既存のクラスの外部にあるファイルの場所にカーソルを置きます。  
  
3. **[編集]** 、 **[特殊な貼り付け]** 、 **[XML をクラスとして貼り付ける]** を選択します。  
  
4. `link`、`rss`、`rssChannel`、`rssChannelImage`、`rssChannelItem`、`rssChannelItemGuid` というクラスは、RSS フィード内の要素にアクセスするために必要なメンバーと共に作成されます。  
  
### <a name="using-the-generated-classes"></a>生成されたクラスの使用  
  
1. クラスが生成されると、他のクラスのコードなどで使用できます。 次のコード例では、`rssChannelImage` クラスの新しいインスタンスが返されます。  
  
    ```csharp
    var channelImage = new rssChannelImage()   
    {   
        title = "MyImage",   
        link = "http://www.contoso.com/images/channelImage.jpg",   
        url = "http://www.contoso.com/entries/myEntry.html"   
    };  
    ```
