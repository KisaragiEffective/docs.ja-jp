---
ms.openlocfilehash: 6fafb689af5d50b31b19f5d1fe7090a6c256ca45
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67802550"
---
### <a name="wpf-printing-stack-update"></a>WPF での印刷スタックの更新

|   |   |
|---|---|
|説明|<xref:System.Printing.PrintQueue?displayProperty=name> を使う WPF の印刷 API は、非推奨になった XPS 印刷 API ではなく Windows ドキュメント印刷パッケージ API を呼び出すようになりました。 この変更はサービス性を考慮して行われたもので、ユーザーも開発者も、動作または API の使用の変化を目にすることはありません。 Windows 10 Creators Update で実行すると、新しい印刷スタックは既定で有効になります。 以前のバージョンの Windows では、以前の印刷スタックが引き続き同じように動作します。|
|提案される解決策|Windows 10 Creators Update で以前のスタックを使用するには、<code>UseXpsOMPrinting</code> レジストリ キーの <code>HKEY_CURRENT_USER\Software\Microsoft\.NETFramework\Windows Presentation Foundation\Printing</code> REG_DWORD 値を <code>1</code> に設定します。|
|スコープ|エッジ|
|バージョン|4.7|
|[種類]|ランタイム|
