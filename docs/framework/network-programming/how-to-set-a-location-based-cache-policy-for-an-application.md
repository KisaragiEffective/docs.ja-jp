---
title: '方法: アプリケーションの場所ベースのキャッシュ ポリシーを設定します。'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- explicitly defining cache behavior
- location-based cache policies
- local cache
- request cache policies
- cache [.NET Framework], location-based policies
ms.assetid: 683bb88e-3411-4f46-9686-3411b6ba511c
ms.openlocfilehash: 6fe569e781b005461ea41e3d6b90859666f9601a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79180777"
---
# <a name="how-to-set-a-location-based-cache-policy-for-an-application"></a>方法: アプリケーションの場所ベースのキャッシュ ポリシーを設定します。
場所ベースのキャッシュ ポリシーを使用すると、要求されたリソースの場所を基にしてアプリケーションでキャッシュの動作を明示的に定義することができます。 このトピックでは、キャッシュ ポリシーをプログラムで設定する方法を示します。 構成ファイルを使用してアプリケーションのポリシーを設定する方法については、「[\<requestCaching> 要素 (ネットワーク設定)](../configure-apps/file-schema/network/requestcaching-element-network-settings.md)」を参照してください。  
  
### <a name="to-set-a-location-based-cache-policy-for-an-application"></a>アプリケーションの場所ベースのキャッシュ ポリシーを設定するには  
  
1. <xref:System.Net.Cache.RequestCachePolicy> または <xref:System.Net.Cache.HttpRequestCachePolicy> オブジェクトを作成します。  
  
2. アプリケーション ドメインの既定値として、ポリシー オブジェクトを設定します。  
  
### <a name="to-set-a-policy-that-takes-requested-resources-from-a-cache"></a>要求されたリソースをキャッシュから取得するポリシーを設定するには  
  
- 可能な場合は要求されたリソースをキャッシュから取得するポリシーを作成します。それ以外の場合は、キャッシュのレベルを <xref:System.Net.Cache.HttpRequestCacheLevel.CacheIfAvailable> に設定して、サーバーに要求を送信します。 要求は、リモートのキャッシュを含めて、クライアントとサーバー間にある任意のキャッシュによって満たすことができます。  
  
    ```csharp  
    public static void UseCacheIfAvailable()  
    {  
        HttpRequestCachePolicy policy = new HttpRequestCachePolicy  
            (HttpRequestCacheLevel.CacheIfAvailable);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub UseCacheIfAvailable()  
        Dim policy As New HttpRequestCachePolicy _  
             (HttpRequestCacheLevel.CacheIfAvailable)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-prevents-any-cache-from-supplying-resources"></a>どのキャッシュもリソースを提供しないようにするポリシーを設定するには  
  
- どのキャッシュも要求されたリソースを提供しないようにするポリシーを作成するには、キャッシュ レベルを <xref:System.Net.Cache.HttpRequestCacheLevel.NoCacheNoStore> に設定します。 このポリシー レベルは、リソースが存在する場合にローカル キャッシュからリソースを削除し、さらにリモート キャッシュにもリソースを削除するように指示します。  
  
    ```csharp  
    public static void DoNotUseCache()  
    {  
    HttpRequestCachePolicy policy = new HttpRequestCachePolicy
            (HttpRequestCacheLevel.NoCacheNoStore);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub DoNotUseCache()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.NoCacheNoStore)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-returns-requested-resources-only-if-they-are-in-the-local-cache"></a>リソースがローカル キャッシュ内にある場合にのみ、要求されたリソースを返すポリシーを設定するには  
  
- リソースがローカル キャッシュ内にある場合にのみ、要求されたリソースを返すポリシーを作成するには、キャッシュ レベルを <xref:System.Net.Cache.HttpRequestCacheLevel.CacheOnly> に設定します。 要求されたリソースがキャッシュにない場合、<xref:System.Net.WebException> 例外がスローされます。  
  
    ```csharp  
    public static void OnlyUseCache()  
    {  
        HttpRequestCachePolicy policy = new HttpRequestCachePolicy
            (HttpRequestCacheLevel.CacheOnly);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub OnlyUseCache()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.CacheOnly)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-prevents-the-local-cache-from-supplying-resources"></a>ローカル キャッシュがリソースを提供しないようにするポリシーを設定するには  
  
- ローカル キャッシュが要求されたリソースを提供しないようにするポリシーを作成するには、キャッシュ レベルを <xref:System.Net.Cache.HttpRequestCacheLevel.Refresh> に設定します。 要求されたリソースが中間のキャッシュに存在し、正常に再検証された場合は、中間のキャッシュが要求されたリソースを提供できます。  
  
    ```csharp  
    public static void DoNotUseLocalCache()  
    {  
     HttpRequestCachePolicy policy = new HttpRequestCachePolicy
            (HttpRequestCacheLevel.Refresh);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub DoNotUseLocalCache()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.Refresh)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-prevents-any-cache-from-supplying-requested-resources"></a>どのキャッシュも要求されたリソースを提供しないようにするポリシーを設定するには  
  
- どのキャッシュも要求されたリソースを提供しないようにするポリシーを作成するには、キャッシュ レベルを <xref:System.Net.Cache.HttpRequestCacheLevel.Reload> に設定します。 サーバーによって返されるリソースは、キャッシュに格納できます。  
  
    ```csharp  
    public static void SendToServer()  
    {  
    HttpRequestCachePolicy policy = new HttpRequestCachePolicy
            (HttpRequestCacheLevel.Reload);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub SendToServer()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.Reload)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-allows-any-cache-to-supply-requested-resources-if-the-resource-on-the-server-is-not-newer-than-the-cached-copy"></a>サーバー上のリソースがキャッシュされたコピーよりも新しいバージョンではない場合に、要求されたリソースをキャッシュが提供することを許可するポリシーを設定するには  
  
- サーバー上のリソースがキャッシュされたコピーよりも新しいバージョンではない場合に、要求されたリソースをキャッシュが提供することを許可するポリシーを作成するには、キャッシュ レベルを <xref:System.Net.Cache.HttpRequestCacheLevel.Revalidate> に設定します。  
  
    ```csharp  
    public static void CheckServer()  
    {  
    HttpRequestCachePolicy policy = new HttpRequestCachePolicy  
             (HttpRequestCacheLevel.Revalidate);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub CheckServer()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.Revalidate)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
## <a name="see-also"></a>参照

- [ネットワーク アプリケーションのキャッシュ管理](cache-management-for-network-applications.md)
- [キャッシュ ポリシー](cache-policy.md)
- [場所ベースのキャッシュ ポリシー](location-based-cache-policies.md)
- [時間ベースのキャッシュ ポリシー](time-based-cache-policies.md)
- [\<requestCaching> 要素 (ネットワーク設定)](../configure-apps/file-schema/network/requestcaching-element-network-settings.md)
