---
ms.openlocfilehash: abb89099c4c8a5d9c0e55ef8f357faf44e75b045
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858833"
---
### <a name="wpf-spell-checking-in-text-enabled-controls-will-not-work-in-windows-10-for-languages-not-in-the-oss-input-language-list"></a>OS の入力言語リストにない言語の場合、Windows 10 でテキスト対応コントロールの WPF スペル チェックが動作しなくなる

|   |   |
|---|---|
|説明|プラットフォームのスペル チェック機能は入力言語リストに存在する言語でしか使用できないため、 Windows 10 での実行時には、WPF テキスト対応コントロール向けのスペル チェック機能が動作しないことがあります。Windows 10 では、使用可能なキーボードのリストに言語を追加すると、対応するオンデマンド機能 (FOD) パッケージが自動的にダウンロードおよびインストールされ、スペル チェック機能が提供されます。 入力言語リストに言語を追加することで、スペル チェック機能がサポートされます。|
|提案される解決策|Windows 10 でスペルチェックを動作させるには、スペルチェック対象の言語またはテキストを入力言語として追加する必要があることに注意してください。|
|スコープ|エッジ|
|バージョン|4.6|
|[種類]|ランタイム|
