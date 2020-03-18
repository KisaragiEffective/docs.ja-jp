---
title: <NetFx40_PInvokeStackResilience> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- <NetFx40_PInvokeStackResilience> element
- NetFx40_PInvokeStackResilience element
ms.assetid: 39fb1588-72a4-4479-af74-0605233b68bd
ms.openlocfilehash: 86f50aafe0b21d5080288e09ac7118ca1e4c939a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73116158"
---
# <a name="netfx40_pinvokestackresilience-element"></a>\<NetFx40_PInvokeStackResilience > 要素

ランタイムが実行時の不適切なプラットフォーム呼び出し宣言を自動的に修正するかどうかを指定します。これにより、マネージド コードとアンマネージド コード間の遷移が遅くなります。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<NetFx40_PInvokeStackResilience >**  

## <a name="syntax"></a>構文

```xml
<NetFx40_PInvokeStackResilience  enabled="1|0"/>
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`enabled`|必須の属性です。<br /><br /> ランタイムが無効なプラットフォーム呼び出し宣言を検出し、実行時に32ビットプラットフォームで自動的にスタックを修正するかどうかを指定します。|

## <a name="enabled-attribute"></a>enabled 属性

|[値]|説明|
|-----------|-----------------|
|`0`|ランタイムは、.NET Framework 4 で導入された高速な相互運用マーシャリングアーキテクチャを使用します。これは、不適切なプラットフォーム呼び出し宣言を検出して修正するものではありません。 既定値です。|
|`1`|ランタイムは、不適切なプラットフォーム呼び出し宣言を検出して修正する低速の遷移を使用します。|

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|
|`runtime`|ランタイム初期化オプションに関する情報を含んでいます。|

## <a name="remarks"></a>Remarks

この要素を使用すると、不適切なプラットフォーム呼び出し宣言に対して実行時の回復性を高めるために、高速な相互運用マーシャリングを行うことができます。

.NET Framework 4 以降では、合理化された相互運用マーシャリングアーキテクチャにより、マネージコードからアンマネージコードへの遷移で大幅なパフォーマンスが向上しています。 以前のバージョンの .NET Framework では、マーシャリング層が32ビットプラットフォームで正しくないプラットフォーム呼び出し宣言を検出し、自動的にスタックを修正しました。 新しいマーシャリングアーキテクチャでは、この手順は不要です。 その結果、遷移は非常に高速になりますが、不適切なプラットフォーム呼び出しの宣言によってプログラムエラーが発生する可能性があります。

開発中に不適切な宣言を簡単に検出できるように、Visual Studio のデバッグエクスペリエンスが改善されました。 デバッガーがアタッチされた状態でアプリケーションを実行すると、 [pInvokeStackImbalance](../../../debug-trace-profile/pinvokestackimbalance-mda.md) managed デバッグアシスタント (MDA) によって、不適切なプラットフォーム呼び出し宣言が通知されます。

再コンパイルできないコンポーネントがアプリケーションで使用されていて、不適切なプラットフォーム呼び出し宣言がある場合は、`NetFx40_PInvokeStackResilience` 要素を使用できます。 この要素をアプリケーション構成ファイルに追加すると `enabled="1"` が、以前のバージョンの .NET Framework の動作と互換性モードになり、移行速度が低下します。 以前のバージョンの .NET Framework に対してコンパイルされたアセンブリは、自動的にこの互換モードに設定され、この要素は必要ありません。

## <a name="configuration-file"></a>構成ファイル

この要素は、アプリケーション構成ファイルでのみ使用できます。

## <a name="example"></a>例

次の例では、マネージコードとアンマネージコードの間の遷移が低速であるため、アプリケーションの不適切なプラットフォーム呼び出し宣言に対して高度な復元を選択する方法を示します。

```xml
<configuration>
   <runtime>
      <NetFx40_PInvokeStackResilience enabled="1"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>関連項目

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [pInvokeStackImbalance](../../../debug-trace-profile/pinvokestackimbalance-mda.md)
