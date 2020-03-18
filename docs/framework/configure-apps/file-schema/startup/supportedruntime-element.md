---
title: <supportedRuntime> configuration 要素-.NET
ms.date: 04/02/2019
ms.custom: updateeachrelease
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#supportedRuntime
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/startup/supportedRuntime
helpviewer_keywords:
- supportedRuntime element
- <supportedRuntime> element
ms.assetid: 1ae16e23-afbe-4de4-b413-bc457f37b69f
ms.openlocfilehash: 5e7fc5f81468ff7c4eba8145ee42a4c7cf8bc0b8
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71697520"
---
# <a name="supportedruntime-element"></a>\<supportedRuntime > 要素

アプリケーションがサポートする共通言語ランタイムバージョンと、オプションで .NET Framework バージョンを指定します。  

[\<configuration>](../configuration-element.md)  
&nbsp;&nbsp;[\<startup>](startup-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp; **\<supportedRuntime>**  

## <a name="syntax"></a>構文

```xml
<supportedRuntime version="runtime version" sku="sku id"/>
```

## <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|**version**|省略可能な属性です。<br /><br /> このアプリケーションがサポートする共通言語ランタイム (CLR: Common Language Runtime) のバージョンを指定する文字列値。 `version` 属性の有効な値については、「[ランタイムバージョン」の値](#version)に関するセクションを参照してください。 **注:** .NET Framework 3.5 では、"*runtime version*" の値は*major*という形式になります。*minor*。*ビルド*。 .NET Framework 4 以降では、メジャーバージョン番号とマイナーバージョン番号 ("v v4.0.30319" の代わりに "v4.0") のみが必要です。 短い文字列を使用することをお勧めします。|
|**sku**|省略可能な属性です。<br /><br /> 在庫管理単位 (SKU) を指定する文字列の値。SKU はこのアプリケーションがサポートする .NET Framework リリースを指定します。<br /><br /> .NET Framework 4.0 以降では、`sku` 属性の使用が推奨されます。  この属性が指定される場合は、アプリケーションが対象とする .NET Framework のバージョンを示します。<br /><br /> Sku 属性の有効な値については、「 [sku id」の値](#sku)に関するセクションを参照してください。|

## <a name="remarks"></a>コメント

**\<supportedRuntime >** 要素がアプリケーション構成ファイルに存在しない場合は、アプリケーションのビルドに使用されるランタイムのバージョンが使用されます。

**\<supportedruntime >** 要素は、ランタイムのバージョン1.1 以降を使用してビルドされたすべてのアプリケーションで使用する必要があります。 ランタイムのバージョン1.0 のみをサポートするようにビルドされたアプリケーションでは、 [\<requiredRuntime >](../startup/requiredruntime-element.md)要素を使用する必要があります。

> [!NOTE]
> [Corbindtoruntimebycfg](../../../unmanaged-api/hosting/corbindtoruntimebycfg-function.md)関数を使用して構成ファイルを指定する場合は、すべてのバージョンのランタイムに対して `<requiredRuntime>` 要素を使用する必要があります。 [Corbindtoruntimebycfg](../../../unmanaged-api/hosting/corbindtoruntimebycfg-function.md)を使用する場合、`<supportedRuntime>` 要素は無視されます。  
  
NET Framework 1.1 から 3.5 までのランタイムの複数のバージョンをサポートするアプリケーションでは、ランタイムの複数のバージョンをサポートする場合は、最初の要素で最も優先度の高いバージョンを指定し、最後の要素で最も優先度の低いバージョンを指定する必要があります。 .NET Framework 4.0 以降のバージョンをサポートするアプリの場合、`version` 属性は、.NET Framework 4 以降のバージョンに共通する CLR バージョンを示し、`sku` 属性は、アプリが対象とする1つの .NET Framework バージョンを示します。 

`sku` 属性を持つ **\<supportedRuntime >** 要素が構成ファイルに存在し、インストールされている .NET Framework のバージョンが指定したサポートされているバージョンより低い場合、アプリケーションの実行は失敗し、代わりにサポートされるバージョンのインストールを求めるメッセージが表示されます。 それ以外の場合は、インストールされている任意のバージョンでアプリケーションを実行しようとしますが、そのバージョンと完全に互換性がないと、予期しない動作をする可能性があります。 .NET Framework のバージョン間の互換性の違いについては、.NET Framework の「[アプリケーションの互換性](https://docs.microsoft.com/dotnet/framework/migration-guide/application-compatibility)」を参照してください。そのため、エラー診断を容易にするために、この要素をアプリケーション構成ファイルに含めることをお勧めします。 (Visual Studio によって自動的に生成された構成ファイルは、新しいプロジェクトを作成するときに自動的に生成されます)。
  
> [!NOTE]
> アプリケーションでレガシアクティブ化パスを使用する場合は、 [Corbindtoruntimeex 関数](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md)など、以前のバージョンではなく CLR のバージョン4をアクティブ化する場合、またはアプリケーションが .NET Framework 4 でビルドされていても、以前のバージョンの .NET Framework でビルドされた混在モードのアセンブリに依存している場合は、サポートされるランタイムの一覧で .NET Framework 4 を指定するだけでは不十分です また、構成ファイルの[\<スタートアップ > 要素](../startup/startup-element.md)で、`useLegacyV2RuntimeActivationPolicy` 属性を `true`に設定する必要があります。 ただし、この属性を `true` に設定すると、以前のバージョンの .NET Framework でビルドされたすべてのコンポーネントは、ビルドされたランタイムではなく、.NET Framework 4 を使用して実行されます。

アプリケーションは、そのアプリケーションを実行できる .NET Framework のすべてのバージョンでテストすることをお勧めします。

<a name="version"></a> 
## <a name="runtime-version-values"></a>"ランタイム バージョン" の値
`runtime` 属性は、特定のアプリケーションに必要な共通言語ランタイム (CLR) のバージョンを指定します。 すべての .NET Framework v4. x バージョンで `v4.0` CLR が指定されていることに注意してください。 次の表に、`version` 属性の*ランタイムバージョン*の値の有効な値を示します。

|.NET Framework のバージョン|`version` 属性|
|----------------------------|-------------------------|
|1.0|"v1.0.3705"|
|1.1|"v1.1.4322"|
|2.0|"v2.0.50727"|
|3.0|"v2.0.50727"|
|3.5|"v2.0.50727"|
|4.0-4.8|"v4.0"|

## <a name="sku"></a>"sku id" の値

`sku` 属性は、ターゲットフレームワークモニカー (TFM) を使用して、アプリが対象として実行する必要がある .NET Framework のバージョンを示します。 次の表に、`sku` 属性でサポートされている有効な値を示します。 .NET Framework 4 から始まります。

|.NET Framework のバージョン|`sku` 属性|
|----------------------------|---------------------|
|4.0|".NETFramework,Version=v4.0"|
|4.0、Client Profile|".NETFramework,Version=v4.0,Profile=Client"|
|4.0、プラットフォームの更新プログラム 1|".NETFramework、Version = v 4.0.1 "|
|4.0、Client Profile、更新プログラム 1|".NETFramework,Version=v4.0.1,Profile=Client"|
|4.0、プラットフォームの更新プログラム 2|".NETFramework、Version = v 4.0.2 "|
|4.0、Client Profile、更新プログラム 2|".NETFramework,Version=v4.0.2,Profile=Client"|
|4.0、プラットフォームの更新プログラム 3|".NETFramework,Version=v4.0.3"|
|4.0、Client Profile、更新プログラム 3|".NETFramework,Version=v4.0.3,Profile=Client"|
|4.5|".NETFramework,Version=v4.5"|
|4.5.1|".NETFramework,Version=v4.5.1"|
|4.5.2|".NETFramework,Version=v4.5.2"|
|4.6|".NETFramework,Version=v4.6"|
|4.6.1|".NETFramework,Version=v4.6.1"|
|4.6.2|".NETFramework,Version=v4.6.2"|
|4.7|".NETFramework,Version=v4.7"|
|4.7.1|".NETFramework,Version=v4.7.1"|
|4.7.2|".NETFramework、Version = v 4.7.2 "|
|4.8|".NETFramework,Version=v4.8"|

## <a name="example"></a>例

サポートされているランタイムのバージョンを構成ファイルで指定する例を次に示します。 構成ファイルは、アプリが .NET Framework 4.7 を対象としていることを示します。

```xml
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7" />
   </startup>
</configuration>
```

## <a name="configuration-file"></a>構成ファイル

この要素は、アプリケーション構成ファイルで使用できます。

## <a name="see-also"></a>参照

- [スタートアップ設定スキーマ](../startup/index.md)
- [構成ファイル スキーマ](../index.md)
- [インプロセスの side-by-side 実行](../../../deployment/in-process-side-by-side-execution.md)
