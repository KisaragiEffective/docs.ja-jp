---
ms.openlocfilehash: 58d1c8cd3aff52703522391c14348bd81c108587
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74568123"
---
### <a name="custom-encoderfallbackbuffer-instances-cannot-fall-back-recursively"></a>カスタム EncoderFallbackBuffer インスタンスが再帰的にフォールバックしない

カスタム <xref:System.Text.EncoderFallbackBuffer> インスタンスが再帰的にフォールバックしません。 <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> を実装した場合、文字のシーケンスが変換先のエンコードに変換されるようにする必要があります。 それ以外の場合は、例外が発生します。

#### <a name="change-description"></a>変更の説明

文字をバイトに変換する操作中に、ランタイムによって不適切な形式または変換不能な UTF-16 のシーケンスが検出され、それらの文字は <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> メソッドに渡されます。 元の変換不能なデータの代わりに置き換える文字は、`Fallback` メソッドによって決定されますが、これらの文字は、<xref:System.Text.EncoderFallbackBuffer.GetNextChar%2A?displayProperty=nameWithType> をループで呼び出すことによってドレインされます。

それからランタイムはこれらの置換文字を、ターゲットのエンコードに変換しようとします。 この操作が成功する場合、ランタイムによって、元の入力文字列の中断した箇所からコード変換が継続されます。

.NET Core Preview 7 以前では、<xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> をカスタム実装することにより、変換先のエンコードに変換できなかった文字シーケンスが返されました。 置換文字がターゲットのエンコードにコード変換できない場合、<xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> メソッドによって新しい置換のシーケンスが返されることを期待し、ランタイムによって置換文字列が使用されて <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> メソッドが再度呼び出されます。 この処理は、結果的に、ランタイムによって正しい形式の、変換可能な代替が確認されるまで、または最大再帰数に到達するまで、継続します。

.NET Core 3.0 以降では、<xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> をカスタム実装した場合、変換先のエンコードに変換できる文字シーケンスが返される必要があります。 置換文字がターゲットのエンコードにコード変換できない場合、<xref:System.ArgumentException> がスローされます。 ランタイムは、それ以上 <xref:System.Text.EncoderFallbackBuffer> インスタンスに対し、再帰呼び出しを実行しなくなります。

この動作は、次の 3 つの条件がすべて満たされている場合にのみ行われます。

- ランタイムによって、不正な形式の UTF-16 シーケンスまたはターゲットのエンコードに変換できない UTF-16 シーケンスが検出される。
- カスタム <xref:System.Text.EncoderFallback> が指定された。
- カスタム <xref:System.Text.EncoderFallback> によって、新しい不正な形式の、または変換不能な URT-16 シーケンスへの置換を試行される。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="recommended-action"></a>推奨アクション

ほとんどの開発者は、何も措置を講じる必要はありません。

アプリケーションでカスタム <xref:System.Text.EncoderFallback> および <xref:System.Text.EncoderFallbackBuffer> クラスを使用している場合、<xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> メソッドがランタイムによって最初に呼び出されたときに、<xref:System.Text.EncoderFallbackBuffer.Fallback%2A> を実装することで、ターゲットのエンコードに直接変換できる正しい形式の UTF-16 で、データがフォールバック バッファーに入力されるようにします。

#### <a name="category"></a>カテゴリ

CoreFx

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType>
- <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType>

<!--

### Affected APIs

- `Overload:System.Text.EncoderFallbackBuffer.Fallback`
- `M:System.Text.EncoderFallbackBuffer.GetNextChar`

-->
