---
ms.openlocfilehash: 38c774417fc94fa080bf2b82c04d575e9068cdcb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858400"
---
### <a name="reflection-objects-can-no-longer-be-passed-from-managed-code-to-out-of-process-dcom-clients"></a>リフレクション オブジェクトが、マネージド コードからアウト プロセス DCOM クライアントに渡されなくなった

|   |   |
|---|---|
|説明|リフレクション オブジェクトはマネージド コードからアウト プロセス DCOM クライアントに渡されなくなりました。 影響を受ける型は次のとおりです。<ul><li><xref:System.Reflection.Assembly?displayProperty=name></li><li><xref:System.Reflection.MemberInfo?displayProperty=name> (およびその派生型、<xref:System.Reflection.FieldInfo?displayProperty=name>、<xref:System.Reflection.MethodInfo?displayProperty=name>、<xref:System.Type?displayProperty=name>、<xref:System.Reflection.TypeInfo?displayProperty=name> など)</li><li><xref:System.Reflection.MethodBody?displayProperty=name></li><li><xref:System.Reflection.Module?displayProperty=name></li><li>[https://login.microsoftonline.com/consumers/](<xref:System.Reflection.ParameterInfo?displayProperty=name>)</li></ul>オブジェクトの <code>IMarshal</code> の呼び出しは <code>E_NOINTERFACE</code> を返します。|
|提案される解決策|非リフレクション オブジェクトで動作するように、マーシャリング コードを更新します。|
|スコープ|Minor|
|バージョン|4.6|
|[種類]|ランタイム|
