---
title: シリアル化バインダーの使用
ms.date: 03/30/2017
ms.assetid: ab46c087-200c-45bf-9c95-5a6cda6e8b98
ms.openlocfilehash: bfce2a14c8757250c520919c8ff2a4d7048a9d5c
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74138655"
---
# <a name="usage-of-serialization-binder"></a>シリアル化バインダーの使用
このサンプルでは、<xref:System.Runtime.Serialization.SerializationBinder> を使用して、ジェネリック型のバージョンをシリアル化する際に変更する方法を示します。  
  
## <a name="demonstrates"></a>使用例  
 <xref:System.Runtime.Serialization.SerializationBinder>、 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>  
  
## <a name="discussion"></a>ディスカッション  
 このサンプルでは、異なるバージョンの .NET Framework を対象とする2つのエンティティが、バイナリフォーマッタとシリアル化バインダーを使用して通信できるようにする方法を示します。  
  
このサンプルは、.NET リモート処理を使用して開発されました。 これは、.NET Framework 4 を対象とするサーバーで構成されます。このサーバーは、ジェネリック型のコントラクトと2つの異なるクライアント (1 つは 2.0 .NET Framework、もう1つは .NET Framework 4) を実装します。  
  
 このサーバーは、シリアル化の際に型のバージョンを相応に変更できるようにするために、<xref:System.Runtime.Serialization.SerializationBinder> をバイナリ フォーマッタにアタッチします。これにより、両方のクライアントで、これらの型を適切に逆シリアル化できるようになります。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルを設定、ビルド、および実行するには  
  
1. クライアントを実行するには、ソリューション SBGenericsVTS を右クリックし、 **[プロパティ]** を選択します。  
  
2. **[共通プロパティ]** で、 **[スタートアッププロジェクト]** を選択し、 **[マルチスタートアッププロジェクト]** を選択します。  
  
3. **[サーバー]** 、 **[Client20]** 、 **[Client40]** の順に選択します。 これら3つのプロジェクトに対して **[開始]** アクションを選択し、残りの設定を **[なし]** のままにします。  
  
4. [ **OK]** をクリックし、F5 キーを押してサンプルを実行します。
