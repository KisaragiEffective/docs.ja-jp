---
ms.openlocfilehash: 19359422f79f8240676b0057c7391f6b06f961ee
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79147550"
---
### <a name="invalidasynchronousstateexception-moved-to-another-assembly"></a>InvalidAsynchronousStateException が別のアセンブリに移動された

<xref:System.ComponentModel.InvalidAsynchronousStateException> クラスは移動されました。

#### <a name="change-description"></a>変更の説明

.NET Core 2.2 およびそれ以前のバージョンでは、<xref:System.ComponentModel.InvalidAsynchronousStateException> クラスは *System.ComponentModel.TypeConverter* アセンブリにあります。

.NET Core 3.0 以降では、*System.ComponentModel.Primitives* アセンブリに含まれています。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="recommended-action"></a>推奨アクション

この変更によって影響を受けるのは、リフレクションを使用し、<xref:System.ComponentModel.InvalidAsynchronousStateException> などのメソッドや、型が特定のアセンブリにあることを想定している <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> のオーバーロードを呼び出すことによって、<xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> を読み込んでいるアプリケーションのみです。 その場合は、メソッドの呼び出しで参照されているアセンブリを、型の新しいアセンブリの場所を反映するように更新する必要があります。

#### <a name="category"></a>カテゴリ

CoreFx

#### <a name="affected-apis"></a>影響を受ける API

[なし] :

<!--

### Affected APIs

- Not detectable via API analysis

-->
