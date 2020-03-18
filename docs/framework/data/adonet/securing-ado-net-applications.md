---
title: アプリケーションの保護
ms.date: 03/30/2017
ms.assetid: 005a1d43-6ee5-471e-ad98-1d30a44d49d5
ms.openlocfilehash: c1bdf4329665e4d29a47c26fb7dba8eb41c1cc3a
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980029"
---
# <a name="securing-adonet-applications"></a>ADO.NET アプリケーションのセキュリティ保護
ユーザー入力の検証を怠るなど、コーディング時に陥りやすい基本的なミスを防ぐだけでは、安全な ADO.NET アプリケーションを作成することはできません。 データにアクセスするアプリケーションには、機密データの取得、操作、破壊など、攻撃者に攻略される可能性がある障害点が多数あります。 そのため、アプリケーションの設計フェーズで行う脅威のモデリングのプロセスから、アプリケーションの最終的な配置と継続的な保守に至るまで、セキュリティのすべての側面を理解することが重要です。  
  
 .NET Framework には、データベース アプリケーションのセキュリティを確保し、管理するのに有用なクラス、サービス、およびツールが多数用意されています。 共通言語ランタイム (CLR) によって、コードを実行するためのタイプ セーフな環境が実現され、それと同時に、コード アクセス セキュリティ (CAS) によって、マネージド コードのアクセス許可はより制限されています。 攻撃者によってもたらされる可能性のある損害は、安全なデータ アクセスのためのコーディング方法に従うことによって最小限に抑えることができます。  
  
 データベースなどのアンマネージ リソースを扱う場合、安全なコードを作成したとしても、自ら招いたセキュリティ ホールを防ぐことはできません。 SQL Server を含め、ほとんどのサーバー データベースには、正しく実装することによってセキュリティを強化できる独自のセキュリティ システムがあります。 しかし、データ ソースがいかに堅牢なセキュリティ システムを備えていたとしても、それが適切に構成されていなければ攻撃を防ぐことはできません。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [セキュリティの概要](security-overview.md)  
 安全な ADO.NET アプリケーションを設計するための推奨事項について説明します。  
  
 [安全なデータ アクセス](secure-data-access.md)  
 セキュリティで保護されたデータ ソースのデータを使用する方法について説明します。  
  
 [安全なクライアント アプリケーション](secure-client-applications.md)  
 クライアント アプリケーションのセキュリティに関する考慮事項について説明します。  
  
 [コード アクセス セキュリティと ADO.NET](code-access-security.md)  
 CAS を使用して ADO.NET コードを保護する方法について説明します。 部分信頼の使用方法についても説明します。  
  
 [プライバシーとデータ セキュリティ](privacy-and-data-security.md)  
 ADO.NET アプリケーションの暗号化オプションについて説明します。  
  
## <a name="related-sections"></a>関連セクション  
 [SQL Server のセキュリティ](./sql/sql-server-security.md)  
 開発者の観点から SQL Server のセキュリティ機能について説明します。  
  
 [セキュリティの考慮事項](./ef/security-considerations.md)  
 Entity Framework アプリケーションのセキュリティについて説明します。  
  
 [Security](../../../standard/security/index.md)  
 .NET のセキュリティをあらゆる観点から説明したトピックへのリンクが含まれています。  
  
 [セキュリティ ツール](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/7w3fd0wb(v=vs.90))  
 セキュリティ保護およびセキュリティ ポリシーの管理に使用する .NET Framework ツールについて説明します。  
  
 [セキュリティで保護されたアプリケーションを作成するためのリソース](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ms165101(v=vs.100))  
 安全なアプリケーションを作成するためのトピックへのリンク集です。  
  
 [セキュリティ参考文献](/visualstudio/ide/securing-applications)  
 オンラインまたは出版物として提供されている外部リソースへのリンク集です。  
  
## <a name="see-also"></a>関連項目

- [ADO.NET](index.md)
- [ADO.NET の概要](ado-net-overview.md)
