---
ms.openlocfilehash: 8db115a46df3fcea103e8fa6896542d0116aa256
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804661"
---
### <a name="vbnet-no-longer-supports-partial-namespace-qualification-for-systemwindows-apis"></a>VB.NET は、System.Windows Api の部分的な名前空間の修飾をサポートしなくなりました

|   |   |
|---|---|
|説明|.NET Framework 4.5.2 以降では、VB.NET プロジェクトは、部分的に修飾された名前空間で System.Windows API を指定できません。 たとえば、<code>Windows.Forms.DialogResult</code> の参照は失敗します。 代わりに、コードは、完全修飾名 (<xref:System.Windows.Forms.DialogResult>) を参照するか、特定の名前空間をインポートして、<xref:System.Windows.Forms.DialogResult?displayProperty=name> を参照する必要があります。|
|提案される解決策|<code>System.Windows</code> API を単純な名前で参照するか (および関連する名前空間をインポートする)、完全修飾名で参照するように、コードを更新する必要があります。|
|スコープ|Minor|
|バージョン|4.5.2|
|[種類]|再ターゲット中|
