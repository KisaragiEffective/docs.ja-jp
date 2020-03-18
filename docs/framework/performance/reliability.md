---
title: 信頼性
ms.date: 03/30/2017
helpviewer_keywords:
- SQL Server [.NET Framework]
- managed code, reliability
- reliability [.NET Framework]
- writing reliable code
- code, reliability
ms.assetid: 294aa306-0afe-4cbe-b397-86ba9f1860f8
ms.openlocfilehash: 2d6601c4cbad32f768ff16301307083f35d986a0
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75715963"
---
# <a name="reliability"></a>信頼性
SQL Server などのサーバー環境で実行するコードは、非同期例外から保護することが重要です。 ここで説明するように、信頼性とは、SQL Server に限ったことではなく、.NET Framework バージョン 2.0 の環境で実行するホスト用に信頼性の高いコードを書くことです。 ただし、SQL Server はバージョン 2.0 の新しい信頼性機能を広範に使う初めてのサービスなので、例として使います。  
  
 SQL Server で実行されるコードは、他のサーバー環境より厳格な信頼性ガイドラインに対応する必要があります。 これは、リソース消費での SQL Server の安定した動作のためです。  <xref:System.OutOfMemoryException> および <xref:System.Threading.ThreadAbortException> 例外は、SQL Server 環境では珍しくありません。 一般に、これらのガイドラインでの主眼は、信頼性より、<xref:System.AppDomain> レベルのリサイクルが発生したときに完全に信頼されたマネージド コードが正常に失敗できるようにすることにあります。リサイクルは、サーバーが整合性と可用性を維持する主要な手段です。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [SQL Server プログラミングとホスト保護属性](sql-server-programming-and-host-protection-attributes.md)  
 SQL Server が <xref:System.Security.Permissions.HostProtectionAttribute> 属性を使ってマネージド コードの実行を制限する方法について説明します。  
  
 [信頼性に関するベスト プラクティス](reliability-best-practices.md)  
 信頼性の要件を満たすコードを記述するためのガイドラインを提供します。  
  
 [制約された実行領域](constrained-execution-regions.md)  
 制約された実行領域 (CER) の機能と動作について説明します。  
  
## <a name="reference"></a>参照先  
 <xref:System.Security.Permissions.HostProtectionAttribute>  
  
 <xref:System.Security.Permissions.HostProtectionResource>
