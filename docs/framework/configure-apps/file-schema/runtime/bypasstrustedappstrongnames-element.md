---
title: <bypassTrustedAppStrongNames> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- strong-name bypass feature
- bypassTrustedAppStrongNames element
- strong-named assemblies, loading into trusted application domains
- <bypassTrustedAppStrongNames> element
ms.assetid: 71b2ebf6-3843-41e2-ad52-ffa5cd083a40
ms.openlocfilehash: 96361a6742d1d2f76cb237344189d3277d7c8069
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73739083"
---
# <a name="bypasstrustedappstrongnames-element"></a>\<bypassTrustedAppStrongNames> 要素

完全に信頼された <xref:System.AppDomain>に読み込まれる完全信頼アセンブリの厳密な名前の検証をバイパスするかどうかを指定します。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<bypasstrustedappstrongnames> >**

## <a name="syntax"></a>構文

```xml
<bypassTrustedAppStrongNames
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`enabled`|必須の属性です。<br /><br /> 完全に信頼されたアセンブリの厳密な名前の検証を回避するバイパス機能が有効かどうかを指定します。 この機能が有効になっている場合、アセンブリの読み込み時に厳密な名前が正しいかどうかは検証されません。 既定値は、 `true`です。|

## <a name="enabled-attribute"></a>enabled 属性

|[値]|説明|
|-----------|-----------------|
|`true`|完全に信頼されたアセンブリの厳密な名前の署名は、アセンブリが完全に信頼された <xref:System.AppDomain>に読み込まれるときに検証されません。 既定値です。|
|`false`|完全に信頼されたアセンブリの厳密な名前の署名は、アセンブリが完全に信頼された <xref:System.AppDomain>に読み込まれるときに検証されます。 厳密な名前の署名は、署名が正しいかどうかのみを確認します。一致のために別の厳密な名前と比較されることはありません。|

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|

## <a name="remarks"></a>Remarks

厳密な名前のバイパス機能を使用すると、完全に信頼されたアセンブリに対する厳密な名前の署名の検証のオーバーヘッドを回避できます。

バイ パス機能は、厳密な名前で署名されていて、次の特性を持つアセンブリに適用されます。

- <xref:System.Security.Policy.StrongName> 証拠なしで完全に信頼されている (たとえば、`MyComputer` のゾーン証拠がある)。

- 完全に信頼された <xref:System.AppDomain> に読み込まれる。

- その <xref:System.AppDomain> の <xref:System.AppDomainSetup.ApplicationBase%2A> プロパティに基づいた場所から読み込まれる。

- 遅延署名されていない。

> [!NOTE]
> レジストリキーを使用して、コンピューター上のすべてのアプリケーションでバイパス機能が無効になっている場合、この構成ファイルの設定は無効です。 詳細については、「[方法: 厳密な名前のバイパス機能を無効](../../../../standard/assembly/disable-strong-name-bypass-feature.md)にする」を参照してください。

## <a name="example"></a>例

次の例は、完全に信頼されたアセンブリの厳密な名前の署名を検証する動作を指定する方法を示しています。

```xml
<configuration>
   <runtime>
      <bypassTrustedAppStrongNames enabled="false"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>関連項目

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [方法: 厳密な名前のバイパス機能を無効にする](../../../../standard/assembly/disable-strong-name-bypass-feature.md)
