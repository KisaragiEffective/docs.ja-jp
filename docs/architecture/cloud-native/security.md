---
title: セキュリティ
description: Azure 向けのクラウドネイティブ .NET アプリの設計 |保護
ms.date: 06/30/2019
ms.openlocfilehash: 848255de70038798417a558543d0b1ea8cff1e37
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "73840995"
---
# <a name="security"></a>セキュリティ

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

1日ではなく、会社がハッキングされたり、ユーザーのデータが失われたりすることについて、ニュースに何のストーリーも含まれていません。 他の国では、セキュリティを後で扱うことによって作成された問題を解決できません。 長年にわたり、企業は顧客データのセキュリティと、実際にはネットワーク全体を "優れた" ものとして扱ってきました。 Windows server は修正プログラム脆弱性に残されており、従来のバージョンの PHP は実行されたままで、MongoDB データベースは世界中にオープンになっています。

しかし、アプリケーションをビルドして展開するときに、セキュリティ上の考え方を維持するために、実際の結果になることはありません。 多くの企業では、 [Notpetya](https://www.wired.com/story/notpetya-cyberattack-ukraine-russia-code-crashed-the-world/)の2017の発生中にサーバーとデスクトップにパッチが適用されていない場合に発生する可能性があることを学習しました。 これらの攻撃のコストは数十億に簡単に到達しましたが、この1つの攻撃からの損失が100億米ドルであるという見積もりもあります。

政府機関も、インシデントのハッキングには対応していません。 Baltimore の町は[犯罪](https://www.vox.com/recode/2019/5/21/18634505/baltimore-ransom-robbinhood-mayor-jack-young-hackers)者によって ransom を保持していました。これにより、市民が請求を支払ったり、都市サービスを使用したりすることが不可能になります。

また、個人データに対して特定のデータ保護を義務付ける法律が強化されています。 ヨーロッパでは、GDPR は年を超えて有効になっています。さらに、カリフォルニア州は、2020年1月1日より有効になる CCDA と呼ばれる独自のバージョンを渡しました。 GDPR の下にある罰金は、企業をビジネスから除外するためにあれにすることができます。 Google は既に違反のために 5000万 Euro を fined していますが、これはバケットの中でも、潜在的な罰金と比較したものです。

つまり、セキュリティは重要なビジネスです。

>[!div class="step-by-step"]
>[前へ](identity-server.md)
>[次へ](azure-security.md)
