---
ms.openlocfilehash: f18b96eaeec8a6427ffb7776327517989d0225d0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "72887793"
---
### <a name="aspnet-accessibility-improvements-in-net-framework-471"></a>.NET Framework 4.7.1 の ASP.NET アクセシビリティ機能の強化

|   |   |
|---|---|
|説明|.NET Framework 4.7.1 以降、ASP.NET のお客様のサポートを改善する目的で、ASP.NET Web コントロールによる Visual Studio のアクセシビリティ テクノロジの処理方法が向上しています。  この機能強化には次の変更内容が含まれています。<ul><li>[詳細の表示] ウィザードの [フィールドの追加] ダイアログや ListView ウィザードの [ListView の構成] ダイアログなど、コントロールで不足していた UI アクセシビリティ パターンを実装するための変更。</li><li>データ ページャー フィールド エディターなど、ハイ コントラスト モードでの表示を改善するための変更。</li><li>DataPager コントロールの [ページャーのフィールドを編集] ウィザードの [フィールド] ダイアログ、[ObjectContext の構成] ダイアログ、[データ ソースの構成] ウィザードの [Configure Data Selction]\(データの選択の構成\) ダイアログなど、キーボードの操作性を改善するための変更。</li></ul>|
|提案される解決策|**これらの変更を選択する方法と選択しない方法**<br>Visual Studio のデザイナーでこれらの変更を利用するには、.NET Framework 4.7.1 以降で実行する必要があります。 Web アプリケーションの場合、次のいずれかの手法をとることで、以上の変更によって機能が強化されます。<ul><li>Visual Studio 2017 15.3 以降をインストールする。このバージョンからは既定で、次の項目にある AppContext スイッチで新しいアクセシビリティ機能に対応しています。</li><li>下の例のように、devenv.exe.config ファイルの <code>Switch.UseLegacyAccessibilityFeatures</code> セクションに <code>&lt;runtime&gt;</code> AppContext スイッチを追加し、それを <code>false</code> に設定することで、以前のアクセシビリティ動作を無効にする。</li></ul><pre><code class="lang-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;&#13;&#10;&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;...&#13;&#10;&lt;!-- AppContextSwitchOverrides value attribute is in the form of &#39;key1=true/false;key2=true/false&#39;  --&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false&quot; /&gt;&#13;&#10;...&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>.NET Framework 4.7.1 以降を対象とするアプリケーションで以前のアクセシビリティ動作を残す場合、この AppContext スイッチを明示的に <code>true</code> に設定することで以前のアクセシビリティ機能を選択できます。|
|スコープ|Minor|
|バージョン|4.7.1|
|[種類]|再ターゲット中|
