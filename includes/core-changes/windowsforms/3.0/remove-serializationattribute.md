---
ms.openlocfilehash: d48ced9d0201a33f9149aba155ddd3d8bc04c93f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74643851"
---
### <a name="serializableattribute-removed-from-some-windows-forms-types"></a>SerializableAttribute が一部の Windows フォーム型から削除された

<xref:System.SerializableAttribute> が、既知のバイナリ シリアル化シナリオを持たない一部の Windows フォームのクラスから削除されています。

#### <a name="change-description"></a>変更の説明

次の型は .NET Framework では <xref:System.SerializableAttribute> で修飾されていますが、その属性は .NET Core では削除されています。

- `System.InvariantComparer`
- <xref:System.ComponentModel.Design.ExceptionCollection?displayProperty=nameWithType>
- <xref:System.ComponentModel.Design.Serialization.CodeDomSerializerException?displayProperty=nameWithType>
- `System.ComponentModel.Design.Serialization.CodeDomComponentSerializationService.CodeDomSerializationStore`
- <xref:System.Drawing.Design.ToolboxItem?displayProperty=nameWithType>
- `System.Resources.ResXNullRef`
- `System.Resources.ResXDataNode`
- `System.Resources.ResXFileRef`
- <xref:System.Windows.Forms.Cursor?displayProperty=nameWithType>
- `System.Windows.Forms.NativeMethods.MSOCRINFOSTRUCT`
- `System.Windows.Forms.NativeMethods.MSG`

従来、このシリアル化メカニズムには、メンテナンスとセキュリティに関して重大な問題がありました。 型の `SerializableAttribute` を維持するということは、それらの型ではバージョン間でシリアル化の変更をテストする必要があることを意味し、場合にっよってはフレームワーク間でも必要です。 これにより、それらの型を進化させることが難しくなり、保守コストが高くなる可能性があります。 これらの型には既知のバイナリ シリアル化のシナリオがないため、属性を削除しても影響は最小限です。

詳細については、「[バイナリ シリアル化](~/docs/standard/serialization/binary-serialization.md)」を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0 Preview 9

#### <a name="recommended-action"></a>推奨アクション

これらの型に依存する可能性のあるすべてのコードを、シリアル化可能としてマークするように更新します。

#### <a name="category"></a>カテゴリ

Windows フォーム

#### <a name="affected-apis"></a>影響を受ける API

- なし

<!--

### Affected APIs

- Not detectable via API analysis

-->
