---
title: <startup> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/startup
- http://schemas.microsoft.com/.NetConfiguration/v2.0#startup
helpviewer_keywords:
- container tags, <startup> element
- <startup> element
- startup element
ms.assetid: 536acfd8-f827-452f-838a-e14fa3b87621
ms.openlocfilehash: 634d9c5248c33619abec50d441d95c111febdcbf
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71699412"
---
# <a name="startup-element"></a>\<startup> 要素

共通言語ランタイムのスタートアップ情報を指定します。

[ **\<configuration>** ](../configuration-element.md)  
&nbsp;&nbsp; **\<startup>**  

## <a name="syntax"></a>構文

```xml
<startup useLegacyV2RuntimeActivationPolicy="true|false" > 
</startup>
```

## <a name="attributes-and-elements"></a>属性と要素

 次のセクションでは、属性、子要素、親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`useLegacyV2RuntimeActivationPolicy`|省略可能な属性です。<br /><br /> .NET Framework 2.0 ランタイムアクティブ化ポリシーを有効にするか、.NET Framework 4 のアクティブ化ポリシーを使用するかを指定します。|

## <a name="uselegacyv2runtimeactivationpolicy-attribute"></a>useLegacyV2RuntimeActivationPolicy 属性

|値|説明|
|-----------|-----------------|
|`true`|選択したランタイムの .NET Framework 2.0 ランタイムアクティブ化ポリシーを有効にします。これは、CLR バージョン2.0 ではなく、レガシランタイムアクティブ化手法 ( [Corbindtoruntimeex 関数](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md)など) を構成ファイルから選択されたランタイムにバインドするためです。 したがって、CLR バージョン4以降が構成ファイルから選択されている場合、以前のバージョンの .NET Framework で作成された混合モードアセンブリは、選択した CLR バージョンで読み込まれます。 この値を設定すると、CLR バージョン1.1 または CLR バージョン2.0 を同じプロセスに読み込むことができなくなり、インプロセスサイドバイサイド機能が実質的に無効になります。|
|`false`|.NET Framework 4 以降の既定のアクティブ化ポリシーを使用します。これは、従来のランタイムアクティブ化手法で CLR バージョン1.1 または2.0 をプロセスに読み込むことができるようにするためです。 この値を設定すると、.NET Framework 4 以降でビルドされた場合を除き、混合モードのアセンブリが .NET Framework 4 以降に読み込まれなくなります。 この値が既定値です。|

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[\<requiredRuntime>](requiredruntime-element.md)|バージョン 1.0 の共通言語ランタイムのみがアプリケーションでサポートされることを指定します。 ランタイムバージョン1.1 以降でビルドされたアプリケーションでは、 **\<supportedruntime >** 要素を使用する必要があります。|
|[\<supportedRuntime>](supportedruntime-element.md)|アプリケーションでサポートされる共通言語ランタイムのバージョンを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|

## <a name="remarks"></a>コメント

 **\<supportedruntime >** 要素は、ランタイムのバージョン1.1 以降を使用してビルドされたすべてのアプリケーションで使用する必要があります。 ランタイムのバージョン1.0 のみをサポートするようにビルドされたアプリケーションでは、 **\<requiredRuntime >** 要素を使用する必要があります。

 Microsoft Internet Explorer でホストされているアプリケーションのスタートアップコードは、 **\<スタートアップ >** 要素とその子要素を無視します。

## <a name="the-uselegacyv2runtimeactivationpolicy-attribute"></a>UseLegacyV2RuntimeActivationPolicy 属性

 この属性は、アプリケーションが[Corbindtoruntimeex 関数](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md)などの従来のアクティブ化パスを使用していて、以前のバージョンではなく CLR のバージョン4をアクティブ化する必要がある場合、またはアプリケーションが .NET Framework 4 でビルドされていて、以前のバージョンの .NET Framework でビルドされた混合モードのアセンブリに依存している場合 これらのシナリオでは、属性を `true`に設定します。

> [!NOTE]
> 属性を `true` に設定すると、CLR バージョン1.1 または CLR バージョン2.0 が同じプロセスに読み込むことができなくなります。これにより、インプロセスサイドバイサイド機能が実質的に無効になります (「 [COM 相互運用機能の Side-by-side 実行](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/8t8td04t(v=vs.100))」を参照してください)。

## <a name="example"></a>例

 次の例は、構成ファイルでランタイムバージョンを指定する方法を示しています。

```xml
<!-- When used with version 1.0 of the .NET Framework runtime -->
<configuration>
   <startup>
      <requiredRuntime version="v1.0.3705" safemode="true"/>
   </startup>
</configuration>
<!-- When used with version 1.1 (or later) of the runtime -->
<configuration>
   <startup>
      <supportedRuntime version="v1.1.4322"/>
      <supportedRuntime version="v1.0.3705"/>
   </startup>
</configuration>
```

## <a name="see-also"></a>参照

- [スタートアップ設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [方法: .NET Framework 4 以降のバージョンをサポートするようにアプリを構成する](../../../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
- [COM 相互運用のサイドバイサイド実行](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/8t8td04t(v=vs.100))
- [インプロセスの side-by-side 実行](../../../deployment/in-process-side-by-side-execution.md)
