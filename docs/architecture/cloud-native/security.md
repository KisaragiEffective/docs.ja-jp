---
title: セキュリティ
description: Azure 向けクラウド ネイティブ .NET アプリの設計 | セキュリティ
ms.date: 05/13/2020
ms.openlocfilehash: 9afbc2c960fdd16721e1d3aa7fd01d5c0c1fe2f9
ms.sourcegitcommit: 27db07ffb26f76912feefba7b884313547410db5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83613669"
---
# <a name="security"></a>セキュリティ

会社がハッキングされたり、ユーザーのデータが失われたというニュースを目にしない日はありません。 国でさえ、後から思い付いてセキュリティを扱うことによって発生する問題を免れることはできません。 長年にわたり、企業は顧客データのセキュリティと、実際、ネットワーク全体を、"あればいいもの" として扱ってきました。 Windows Server には修正プログラムが適用されておらず、従来のバージョンの PHP が実行されたままで、MongoDB データベースは世界中にオープンになっています。

しかし、アプリケーションをビルドして展開するときにセキュリティを考慮しないことによる、現実世界への影響が出始めています。 多くの企業では、2017 年に [NotPetya](https://www.wired.com/story/notpetya-cyberattack-ukraine-russia-code-crashed-the-world/) が大量感染したときにサーバーとデスクトップに修正プログラムを適用しなかった場合に発生する可能性があった苦労を学習しました。 これらの攻撃のコストは数十億に簡単に達しますが、この 1 つの攻撃からの損失が 100 億米ドルであるという見積もりもあります。

政府機関も、インシデントのハッキングから免れることはできません。 ボルチモア市は[犯罪者](https://www.vox.com/recode/2019/5/21/18634505/baltimore-ransom-robbinhood-mayor-jack-young-hackers)によるランサムウェアの対象になり、市民が請求を支払ったり、都市サービスを使用したりすることが不可能になりました。

また、個人データに対して特定のデータ保護を義務付ける法律が強化されています。 ヨーロッパでは 1 年以上前から GDPR が施行されており、カリフォルニア州では、2020 年 1 月 1 日より有効になる CCDA と呼ばれる独自のバージョンが通過しました。 GDPR での罰金は、企業が倒産しかねないほど過酷な場合があります。 Google は既に違反のために 5000 万ユーロを科せられましたが、可能性のある罰金と比較すればわずかなものです。

つまり、セキュリティは重要なビジネスです。

>[!div class="step-by-step"]
>[前へ](identity-server.md)
>[次へ](azure-security.md)
