---
ms.openlocfilehash: 31e7f84a787d255a474f4c2b1fa3068903dbed52
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901579"
---
### <a name="http-headernames-constants-changed-to-static-readonly"></a>HTTP:HeaderNames の定数を静的読み取り専用に変更

ASP.NET Core 3.0 Preview 5 以降、<xref:Microsoft.Net.Http.Headers.HeaderNames?displayProperty=fullName> のフィールドが `const` から `static readonly` に変更されました。

ディスカッションについては、[dotnet/aspnetcore#9514](https://github.com/dotnet/aspnetcore/issues/9514) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

これらのフィールドは以前は `const` でした。

#### <a name="new-behavior"></a>新しい動作

これらのフィールドは `static readonly` になりました。

#### <a name="reason-for-change"></a>変更理由

変更:

* アセンブリ境界を越えて値が埋め込まれるのを防ぎ、必要に応じて値を修正できるようにします。
* 参照の等価性チェックの高速化を可能にします。

#### <a name="recommended-action"></a>推奨アクション

3\.0 に対して再コンパイルします。 これらのフィールドを次の方法で使用しているソース コードはそれを実行できなくなりました。

* 属性引数として使用
* `switch` ステートメントで `case` として使用
* 別の `const` を定義するときに使用

この破壊的変更を回避するには、ヘッダー名の自己定義型の定数または文字列リテラルの使用に切り替えます。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.Net.Http.Headers.HeaderNames?displayProperty=fullName>

<!-- 

#### Affected APIs

`T:Microsoft.Net.Http.Headers.HeaderNames`

-->
