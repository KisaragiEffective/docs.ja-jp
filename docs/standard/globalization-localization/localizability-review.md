---
title: ローカライズ化の確認
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- world-ready applications, localizability
- application development [.NET Framework], localization
- localizability [.NET Framework]
- international applications [.NET Framework], localizability
- globalization [.NET Framework], localizability
- culture, localizability
- localization [.NET Framework], localizability
- global applications, localizability
- localizing resources
ms.assetid: 3aee2fbb-de47-4e37-8fe4-ddebb9719247
ms.openlocfilehash: b286bdd2c5d7b03a0a2b5f94478e252da6cd0ae2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73120864"
---
# <a name="localizability-review"></a>ローカライズ化の確認

ローカライズ化の確認は、国際対応アプリケーション開発の中間段階です。 グローバル化されたアプリケーションがローカライズの準備ができていることを確認し、特別な処理が必要なコードやユーザー インターフェイスの側面を特定します。 この手順は、ローカライズ プロセスでアプリケーションに機能上の欠陥が持ち込まれないようにするためにも役立ちます。 ローカライズ化の確認で生じた問題がすべて解決されたら、アプリケーションのローカライズの準備は完了です。 ローカライズ化の確認が徹底していれば、ローカライズ プロセス中にソース コードを変更する必要はありません。

ローカライズ化の確認は、次の 3 つの確認で構成されます。

- [グローバリゼーションの推奨事項は実装されているか](#global)

- [カルチャに依存した機能は正しく処理されているか](#culture)

- [各種言語データでアプリケーションはテストされているか](#test)

<a name="global"></a>
## <a name="implement-globalization-recommendations"></a>グローバリゼーションの推奨事項を実装する

ローカライズを考慮してアプリケーションの設計と開発を行い、[グローバリゼーション](../../../docs/standard/globalization-localization/globalization.md)に関する記事で説明されている推奨事項に従っている場合、ローカライズ化の確認は品質保証の点でほぼ合格です。 それ以外の場合は、この段階で[グローバリゼーション](../../../docs/standard/globalization-localization/globalization.md)の推奨事項を確認して実装し、ローカライズを妨げるソース コードのエラーを修正する必要があります。

<a name="culture"></a>
## <a name="handle-culture-sensitive-features"></a>カルチャに依存した機能を処理する

.NET では、カルチャによって大きく異なるさまざまな領域のプログラムによるサポートは提供されていません。 ほとんどの場合、次のような機能領域を処理するカスタム コードを作成する必要があります。

- アドレス

- 電話番号

- 用紙サイズ

- 長さ、重量、面積、体積、および温度に使用される測定単位

   .NET には測定単位の変換のサポートが組み込まれていませんが、次の例に示すように、<xref:System.Globalization.RegionInfo.IsMetric%2A?displayProperty=nameWithType> プロパティを使用して特定の国または地域でメートル法が使用されているかどうかを判断できます。

   [!code-csharp[Conceptual.Localizability#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.localizability/cs/ismetric1.cs#1)]
   [!code-vb[Conceptual.Localizability#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.localizability/vb/ismetric1.vb#1)]

<a name="test"></a>
## <a name="test-your-application"></a>アプリケーションのテスト

アプリケーションをローカライズする前に、各言語版のオペレーティング システム上で各種言語データを使用してアプリケーションをテストする必要があります。 この時点では、ほとんどのユーザー インターフェイスはローカライズされていませんが、次のような問題を検出できます。

- シリアル化されたデータが、オペレーティング システムのバージョン間で正しく逆シリアル化されない場合。

- 現在のカルチャの規則を反映していない数値データ。 たとえば、不適切なグループ区切り記号、小数点区切り記号、または通貨記号で数値が表示されることがあります。

- 現在のカルチャの規則を反映していない日時データ。 たとえば、月と日を表す数値が不適切な順序で表示される場合、日付の区切りが正しくない場合、タイム ゾーン情報が正しくない場合があります。

- アプリケーションの既定のカルチャを指定していないために見つからないリソース。

- 特定のカルチャでは通常とは異なる順序で表示される文字列。

- 予期しない結果が返される文字列の比較または等価比較。

アプリケーションの開発時にグローバリゼーションの推奨事項に従い、カルチャに依存した機能を正しく処理し、テスト中に発生したローカライズの問題を特定して対処した場合は、次の手順である[ローカライズ](../../../docs/standard/globalization-localization/localization.md)に進むことができます。

## <a name="see-also"></a>参照

- [グローバライズとローカライズ](../../../docs/standard/globalization-localization/index.md)
- [ローカリゼーション](../../../docs/standard/globalization-localization/localization.md)
- [グローバリゼーション](../../../docs/standard/globalization-localization/globalization.md)
- [デスクトップ アプリケーションのリソース](../../../docs/framework/resources/index.md)
