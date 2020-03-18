---
title: インプロセスの side-by-side 実行
ms.date: 03/30/2017
helpviewer_keywords:
- in-process side-by-side execution
- side-by-side execution, in-process
ms.assetid: 18019342-a810-4986-8ec2-b933a17c2267
ms.openlocfilehash: 5ca2f03576946a23b3133bbe7532d46c4ad758ab
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79181660"
---
# <a name="in-process-side-by-side-execution"></a>インプロセスの side-by-side 実行
.NET Framework 4 以降では、インプロセスの side-by-side ホスティングを使用して、1 つのプロセスで複数のバージョンの共通言語ランタイム (CLR) を実行できます。 既定では、マネージド COM コンポーネントは、プロセスに読み込まれている .NET Framework のバージョンに関係なく、コンポーネントがビルドされた .NET Framework のバージョンで実行されます。  
  
## <a name="background"></a>背景  
 .NET Framework ではマネージド コード アプリケーションに対して常に side-by-side ホスティングが提供されてきましたが、.NET Framework 4 より前は、マネージド COM コンポーネントに対してこの機能は提供されていませんでした。 以前は、プロセスに読み込まれたマネージド COM コンポーネントは、既に読み込まれているランタイムのバージョン、またはインストールされている .NET Framework の最新バージョンで実行されました。 このバージョンが COM コンポーネントと互換性がない場合、コンポーネントは実行できませんでした。  
  
 .NET Framework 4 が備える side-by-side ホスティングの新しいアプローチでは、次のことが保証されます。  
  
- .NET Framework の新しいバージョンをインストールしても、既存のアプリケーションに影響はありません。  
  
- アプリケーションは、それがビルドされたバージョンの .NET Framework に対して実行されます。 明示的に指示しない限り、新しいバージョンの .NET Framework は使われません。 ただし、新しいバージョンの .NET Framework を使うようにアプリケーションを移行する方が簡単です。  
  
## <a name="effects-on-users-and-developers"></a>ユーザーと開発者への影響  
  
- **エンドユーザーとシステム管理者**。 これらのユーザーについては、単独で、またはアプリケーションと共に、ランタイムの新しいバージョンをインストールしても、コンピューターに影響がないことが、これまで以上に確実になっています。 既存のアプリケーションは引き続きこれまでと同じように動作します。  
  
- **アプリケーション開発者**。 Side-by-side ホスティングは、アプリケーション開発者に対する影響はほとんどありません。 既定では、アプリケーションはそれがビルドされた .NET Framework のバージョンで常に実行されます。これは変更されていません。 ただし、開発者はこの動作をオーバーライドし、新しいバージョンの .NET Framework で実行するようアプリケーションに指示できます ([シナリオ 2](#scenarios) をご覧ください)。  
  
- **ライブラリ開発者とコンシューマー**。 ライブラリ開発者が直面する互換性の問題は、side-by-side ホスティングでは解決されません。 アプリケーションによって (直接参照または <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> の呼び出しにより) 直接読み込まれるライブラリは、それが読み込まれる <xref:System.AppDomain> のランタイムを引き続き使います。 サポートする .NET Framework のすべてのバージョンについて、ライブラリをテストする必要があります。 アプリケーションが .NET Framework 4 ランタイムを使ってコンパイルされているものの、それより前のランタイムを使ってビルドされたライブラリが含まれる場合は、そのライブラリも .NET Framework 4 ランタイムを使います。 一方、以前のランタイムを使ってビルドされたアプリケーションと、.NET Framework 4 を使ってビルドされたライブラリがある場合は、アプリケーションも .NET Framework 4 を使うように強制する必要があります (「[シナリオ 3](#scenarios)」を参照してください)。  
  
- **マネージド COM コンポーネントの開発者**。 以前は、マネージド COM コンポーネントはコンピューターにインストールされている最新バージョンのランタイムを使って自動的に実行しました。 現在は、ビルドされたバージョンのランタイムに対して COM コンポーネントを実行できるようになりました。  
  
     次の表で示すように、.NET Framework バージョン 1.1 でビルドされたコンポーネントは、バージョン 4 のコンポーネントとであれば side-by-side で実行できますが、バージョン 2.0、3.0、3.5 のコンポーネントとは、これらのバージョンでは side-by-side ホスティングを使用できないために side-by-side では実行できません。  
  
    |.NET Framework のバージョン|1.1|2.0 - 3.5|4|  
    |----------------------------|---------|----------------|-------|  
    |1.1|利用不可|いいえ|はい|  
    |2.0 - 3.5|いいえ|利用不可|はい|  
    |4|はい|はい|利用不可|  
  
> [!NOTE]
> .NET Framework バージョン 3.0 および 3.5 はバージョン 2.0 に対して増分的にビルドされているため、side-by-side で実行する必要はありません。 これらは、本質的に同じバージョンです。  
  
<a name="scenarios"></a>
## <a name="common-side-by-side-hosting-scenarios"></a>side-by-side ホスティングの一般的なシナリオ  
  
- **シナリオ 1 :** 以前のバージョンの .NET Framework でビルドされた COM コンポーネントを使うネイティブ アプリケーション。  
  
     インストールされている .NET Framework のバージョン: .NET Framework 4、および COM コンポーネントによって使用される他のすべての .NET Framework のバージョン。  
  
     対処方法: このシナリオの場合は、何も行いません。 COM コンポーネントは、登録された .NET Framework のバージョンで実行されます。  
  
- **シナリオ 2**: .NET Framework 2.0 で実行することが好ましいが、バージョン 2.0 が存在しない場合は .NET Framework 4 で実行する、.NET Framework 2.0 SP1 でビルドされたマネージド アプリケーション。  
  
     インストールされている .NET Framework のバージョン: 以前のバージョンの .NET Framework および .NET Framework 4。  
  
     対処方法: アプリケーション ディレクトリ内の[アプリケーション構成ファイル](../configure-apps/index.md)で、次のように設定された [\<startup> 要素](../configure-apps/file-schema/startup/startup-element.md)と [\<supportedRuntime> 要素](../configure-apps/file-schema/startup/supportedruntime-element.md)を使用します。  
  
    ```xml  
    <configuration>  
      <startup >  
        <supportedRuntime version="v2.0.50727" />  
        <supportedRuntime version="v4.0" />  
      </startup>  
    </configuration>  
    ```  
  
- **シナリオ 3**: 以前のバージョンの .NET Framework でビルドされた COM コンポーネントを使う、.NET Framework 4 で実行したいネイティブ アプリケーション。  
  
     インストールされている .NET Framework のバージョン: .NET Framework 4。  
  
     対処方法: アプリケーション ディレクトリのアプリケーション構成ファイルで、`useLegacyV2RuntimeActivationPolicy` 属性と `true` に設定した `<startup>` 要素と、次のように設定された `<supportedRuntime>` 要素を使います。  
  
    ```xml  
    <configuration>  
      <startup useLegacyV2RuntimeActivationPolicy="true">  
        <supportedRuntime version="v4.0" />  
      </startup>  
    </configuration>  
    ```  
  
## <a name="example"></a>例  
 次の例では、コンポーネントがコンパイルされた .NET Framework のバージョンを使うことでマネージド COM コンポーネントを実行しているアンマネージド COM ホストを示します。  
  
 次の例を実行するには、.NET Framework 3.5 を使って次のマネージド COM コンポーネントをコンパイルして登録します。 コンポーネントを登録するには、 **[プロジェクト]** メニューの **[プロパティ]** をクリックし、 **[ビルド]** タブをクリックして、 **[COM の相互運用機能に登録]** チェック ボックスをオンにします。  
  
```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Runtime.InteropServices;  
  
namespace BasicComObject  
{  
    [ComVisible(true), Guid("9C99C4B5-CA54-4c58-8988-49B6811BA53B")]  
    public class MyObject : SimpleObjectModel.IPrintInfo  
    {  
        public MyObject()  
        {  
        }  
        public void PrintInfo()  
        {  
            Console.WriteLine("MyObject was activated in {0} runtime in:\n\tAppDomain {1}:{2}", System.Runtime.InteropServices.RuntimeEnvironment.GetSystemVersion(), AppDomain.CurrentDomain.Id, AppDomain.CurrentDomain.FriendlyName);  
        }  
    }  
}  
```  
  
 次のアンマネージ C++ アプリケーションをコンパイルすると、前の例で作成した COM オブジェクトがアクティブ化すされます。  
  
```cpp
#include "stdafx.h"  
#include <string>  
#include <iostream>  
#include <objbase.h>  
#include <string.h>  
#include <process.h>  
  
using namespace std;  
  
int _tmain(int argc, _TCHAR* argv[])  
{  
    char input;  
    CoInitialize(NULL) ;  
    CLSID clsid;  
    HRESULT hr;  
    HRESULT clsidhr = CLSIDFromString(L"{9C99C4B5-CA54-4c58-8988-49B6811BA53B}",&clsid);  
    hr = -1;  
    if (FAILED(clsidhr))  
    {  
        printf("Failed to construct CLSID from String\n");  
    }  
    UUID id = __uuidof(IUnknown);  
    IUnknown * pUnk = NULL;  
    hr = ::CoCreateInstance(clsid,NULL,CLSCTX_INPROC_SERVER,id,(void **) &pUnk);  
    if (FAILED(hr))  
    {  
        printf("Failed CoCreateInstance\n");  
    }else  
    {  
        pUnk->AddRef();  
        printf("Succeeded\n");  
    }  
  
    DISPID dispid;  
    IDispatch* pPrintInfo;  
    pUnk->QueryInterface(IID_IDispatch, (void**)&pPrintInfo);  
    OLECHAR FAR* szMethod[1];  
    szMethod[0]=OLESTR("PrintInfo");
    hr = pPrintInfo->GetIDsOfNames(IID_NULL,szMethod, 1, LOCALE_SYSTEM_DEFAULT, &dispid);  
    DISPPARAMS dispparams;  
    dispparams.cNamedArgs = 0;  
    dispparams.cArgs = 0;  
    VARIANTARG* pvarg = NULL;  
    EXCEPINFO * pexcepinfo = NULL;  
    WORD wFlags = DISPATCH_METHOD ;  
;  
    LPVARIANT pvRet = NULL;  
    UINT * pnArgErr = NULL;  
    hr = pPrintInfo->Invoke(dispid,IID_NULL, LOCALE_USER_DEFAULT, wFlags,  
        &dispparams, pvRet, pexcepinfo, pnArgErr);  
    printf("Press Enter to exit");  
    scanf_s("%c",&input);  
    CoUninitialize();  
    return 0;  
}  
```  
  
## <a name="see-also"></a>関連項目

- [\<startup> 要素](../configure-apps/file-schema/startup/startup-element.md)
- [\<supportedRuntime> 要素](../configure-apps/file-schema/startup/supportedruntime-element.md)
