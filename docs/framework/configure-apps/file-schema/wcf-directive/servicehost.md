---
title: '@ServiceHost'
ms.date: 03/30/2017
ms.assetid: 96ba6967-00f2-422f-9aa7-15de4d33ebf3
ms.openlocfilehash: fdd6d83836c4ef31a4d7c8e68cb0cc050ac6bea4
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76787799"
---
# <a name="servicehost"></a>\@ServiceHost

サービス ホストの生成に使用されるファクトリを、ホストされるサービスと、.svc ファイルで提供されるホスティング コードのアクセスとコンパイルに必要なその他のプログラミング部分に関連付けます。

## <a name="syntax"></a>構文

```xml
<% @ServiceHost
Service = "Service, ServiceNamespace"
Factory = "Factory, FactoryNamespace"
Debug = "Debug"
Language = "Language"
CodeBehind = "CodeBehind"
%>
```

## <a name="attributes"></a>属性

### <a name="service"></a>サービス

ホストされるサービスの CLR 型名。 これは、1 つ以上のサービス コンタクトを実装する型の修飾名にする必要があります。

### <a name="factory"></a>ファクトリ

サービス ホストのインスタンス化に使用されるサービス ホスト ファクトリの CLR 型名。 この属性は省略できます。 未指定の場合は、<xref:System.ServiceModel.Activation.ServiceHostFactory> のインスタンスを返す既定の <xref:System.ServiceModel.ServiceHost> が使用されます。

### <a name="debug"></a>デバッグ

デバッグシンボルを使用して Windows Communication Foundation (WCF) サービスをコンパイルする必要があるかどうかを示します。 WCF サービスをデバッグシンボルを使用してコンパイルする必要があるかどうかを `true` します。それ以外の場合は、`false`ます。

### <a name="language"></a>言語

ファイル (.svc) 内のすべてのインライン コードをコンパイルするときに使用する言語を指定します。 値は任意のを表すことができます。`C#`、`VB`、および `JS`を含む、NET でサポートされているC#言語。それぞれ、、Visual Basic、および JScript .net を参照します。 この属性は省略できます。

### <a name="codebehind"></a>CodeBehind

Xml web サービスを実装するクラスが同じファイル内に存在せず、アセンブリにコンパイルされずに *\bin*ディレクトリに配置されている場合に、xml web サービスを実装するソースファイルを指定します。

## <a name="remarks"></a>コメント

サービスをホストするために使用される <xref:System.ServiceModel.ServiceHost> は、Windows Communication Foundation (WCF) プログラミングモデル内の機能拡張ポイントです。 ファクトリ パターンは、ホスティング環境が直接インスタンス化できないポリモーフィック型の可能性があるため、<xref:System.ServiceModel.ServiceHost> のインスタンス化に使用されます。

既定の実装では <xref:System.ServiceModel.Activation.ServiceHostFactory> を使用して、<xref:System.ServiceModel.ServiceHost> のインスタンスを作成します。 ただし、ファクトリ実装の CLR 型名を `@ServiceHost` ディレクティブで指定することで、独自のファクトリ (派生ホストを返すもの) を指定できます。

既定のファクトリの代わりにカスタムサービスホストファクトリを使用するには、次のように `@ServiceHost` ディレクティブで型名を指定するだけです。

```xml
<% @ServiceHost Factory="DerivedFactory" Service="MyService" %>
```

ファクトリ実装はできるだけ軽量に保持してください。 多くのカスタム ロジックがある場合は、それらのロジックをファクトリ内部ではなくホスト内部に置くとコードがさらに再利用可能になります。

たとえば、`MyService`に対して AJAX 対応のエンドポイントを有効にするには、次の例に示すように、`@ServiceHost` ディレクティブで、既定の <xref:System.ServiceModel.Activation.ServiceHostFactory>ではなく、`Factory` 属性の値に <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> を指定します。

```xml
<% @ServiceHost
Service="MyService"
Language="C#"
Debug="true"
Factory="WebScriptServiceHostFactory"
%>
```

## <a name="see-also"></a>関連項目

- [カスタム サービス ホスト](../../../wcf/samples/custom-service-host.md)
