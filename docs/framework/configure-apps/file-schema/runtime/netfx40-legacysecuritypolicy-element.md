---
title: <NetFx40_LegacySecurityPolicy> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- <NetFx40_LegacySecurityPolicy> element
- NetFx40_LegacySecurityPolicy element
ms.assetid: 07132b9c-4a72-4710-99d7-e702405e02d4
ms.openlocfilehash: d5192eb56bb8b640544bdc52a0bb9d8a5277efef
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73116253"
---
# <a name="netfx40_legacysecuritypolicy-element"></a>\<NetFx40_LegacySecurityPolicy > 要素

ランタイムがレガシ コード アクセス セキュリティ (CAS) ポリシーを使用するかどうかを指定します。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<NetFx40_LegacySecurityPolicy >**  

## <a name="syntax"></a>構文

```xml
<NetFx40_LegacySecurityPolicy
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`enabled`|必須の属性です。<br /><br /> ランタイムが従来の CAS ポリシーを使用するかどうかを指定します。|

## <a name="enabled-attribute"></a>enabled 属性

|[値]|説明|
|-----------|-----------------|
|`false`|ランタイムでは、従来の CAS ポリシーは使用されません。 既定値です。|
|`true`|ランタイムは、従来の CAS ポリシーを使用します。|

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|
|`runtime`|ランタイム初期化オプションに関する情報を含んでいます。|

## <a name="remarks"></a>Remarks

.NET Framework バージョン3.5 以前のバージョンでは、CAS ポリシーは常に有効です。 .NET Framework 4 では、CAS ポリシーを有効にする必要があります。

CAS ポリシーはバージョン固有です。 以前のバージョンの .NET Framework に存在するカスタム CAS ポリシーは、.NET Framework 4 で再指定する必要があります。

`<NetFx40_LegacySecurityPolicy>` 要素を .NET Framework 4 アセンブリに適用しても、[セキュリティ透過的なコード](../../../misc/security-transparent-code.md)には影響しません。透過性ルールも適用されます。

> [!IMPORTANT]
> `<NetFx40_LegacySecurityPolicy>` 要素を適用すると、[グローバルアセンブリキャッシュ](../../../app-domains/gac.md)にインストールされていない[ネイティブイメージジェネレーター (ngen.exe)](../../../tools/ngen-exe-native-image-generator.md)によって作成されたネイティブイメージアセンブリのパフォーマンスが大幅に低下する可能性があります。 パフォーマンスの低下は、属性が適用されたときにランタイムがネイティブイメージとしてアセンブリを読み込むことができないことが原因で発生し、その結果、ジャストインタイムアセンブリとして読み込まれます。

> [!NOTE]
> Visual Studio プロジェクトの [プロジェクトの設定] で、.NET Framework 4 より前のターゲット .NET Framework バージョンを指定した場合、そのバージョンに指定したカスタム CAS ポリシーも含めて、CAS ポリシーが有効になります。 ただし、新しい .NET Framework 4 種類とメンバーを使用することはできません。 [アプリケーション構成ファイル](../../index.md)のスタートアップ設定スキーマで[\<supportedruntime > 要素](../startup/supportedruntime-element.md)を使用して、以前のバージョンの .NET Framework を指定することもできます。

> [!NOTE]
> 構成ファイルの構文では、大文字と小文字が区別されます。 構文と例のセクションで提供されている構文を使用する必要があります。

## <a name="configuration-file"></a>構成ファイル

この要素は、アプリケーション構成ファイルでのみ使用できます。

## <a name="example"></a>例

次の例では、アプリケーションに対して従来の CAS ポリシーを有効にする方法を示します。

```xml
<configuration>
   <runtime>
      <NetFx40_LegacySecurityPolicy enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>関連項目

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
