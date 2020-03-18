---
title: <behaviorExtensions>
ms.date: 03/30/2017
ms.assetid: 59f2791a-c78f-40d7-aa80-0d9cd10135d9
ms.openlocfilehash: 39dc92d65a41d223ebd39aec3dc59871ad1fd101
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77448686"
---
# <a name="behaviorextensions"></a>\<の動作拡張機能 >
動作の拡張により、ユーザーはユーザー定義の動作要素を作成できます。 これらの要素は、標準の Windows Communication Foundation (WCF) 動作要素と共に使用できます。 `behaviorExtensions` セクションでは、構成で使用できるように要素を定義します。 次の例は、一般的な動作拡張を示します。  
  
```xml  
<system.serviceModel>
  <extensions>
    <behaviorExtensions>
      <add name="myBehavior"
           type="Microsoft.ServiceModel.Samples.MyBehaviorSection, MyBehavior,
                 Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </behaviorExtensions>
  </extensions>
</system.serviceModel>
```  
  
 構成機能を要素に追加するには、構成要素を記述して登録する必要があります。 詳細については、<xref:System.Configuration> を参照してください。  
  
 要素とその構成の型を定義したら、次の例に示すように拡張を使用できます。  
  
```xml  
<behaviors>
  <behavior configurationName="testChannelBehavior">
    <myBehavior />
    <channelSecurity cacheCookies="false"
                     detectReplays="false"
                     maxCachedNonces="9"
                     maxClockSkew="00:00:03"
                     maxCookieCachingTime="00:07:24"
                     replayWindow="00:07:22.2190000" />
  </behavior>
</behaviors>
```  
  
## <a name="security"></a>セキュリティ  
 `machine.config` ファイルと `app.config` ファイルに型を登録する場合は、完全修飾アセンブリ名の使用を強くお勧めします。 型が一意に定義されていない場合、CLR 型ローダーは次の場所を指定された順序で検索します。  
  
 型のアセンブリが判明している場合、ローダーは構成ファイルのリダイレクトの場所、GAC、構成情報を使用する現在のアセンブリ、およびアプリケーションの基本ディレクトリを検索します。 アセンブリが判明していない場合、ローダーは現在のアセンブリ、mscorlib、および `TypeResolve` イベント ハンドラーによって返される場所を検索します。 この CLR 検索順序は、Type Forwarding 機構や AppDomain.TypeResolve イベントなどのフックを使用して変更できます。  
  
 攻撃者が CLR 検索順序を悪用して、未承認のコードを実行する可能性があります。 完全修飾名 (厳密な名前) を使用することで型が一意に識別され、システムのセキュリティが強化されます。  
  
 詳細については、「[ランタイムがアセンブリを検索する方法](../../../deployment/how-the-runtime-locates-assemblies.md)」および「<xref:System.AppDomain.TypeResolve>」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.ServiceModel.Configuration.BehaviorExtensionElement>
- [動作を使用したランタイムの構成と拡張](../../../wcf/extending/configuring-and-extending-the-runtime-with-behaviors.md)
