---
title: ハッシュ コードによるデータの整合性の保証
description: .NET でハッシュ コードを使用し、データの整合性を確保する方法について説明します。 ハッシュ値は、データを一意に識別する固定長の数値です。
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- generating hash
- verifying hash codes
- cryptography [.NET], hash
- data integrity
- checking hash codes
- encryption [.NET], hash
- hash
ms.assetid: 33660f33-b70f-4dca-8c87-ab35cfc2961a
ms.openlocfilehash: 7f5e1d54efa3a5ccf28f2e0863a9bc9ccb80f894
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689746"
---
# <a name="ensuring-data-integrity-with-hash-codes"></a>ハッシュ コードによるデータの整合性の保証

ハッシュ値は、データを一意に識別する固定長の数値です。 ハッシュ値は、大量のデータを非常に小さな数値として表現するため、デジタル署名と一緒に使用されます。 大きな値で署名するよりも、ハッシュ値を使用すれば効率的に署名できます。 ハッシュ値は、安全でないチャネルを介して送信されたデータの整合性を検証するためにも役立ちます。 受信したデータのハッシュ値を送信したデータのハッシュ値と比較すると、データが変更されたかどうかを判断できます。  
  
このトピックでは、<xref:System.Security.Cryptography> 名前空間のクラスを使用してハッシュ コードの生成と検証を実行する方法について説明します。  
  
## <a name="generating-a-hash"></a>ハッシュの生成

 マネージド ハッシュ クラスでは、バイト配列またはマネージド ストリーム オブジェクトのいずれかをハッシュできます。 SHA1 ハッシュ アルゴリズムを使用して文字列のハッシュ値を作成する例を次に示します。 この例では、<xref:System.Text.UnicodeEncoding> クラスを使用して文字列をバイト配列に変換し、<xref:System.Security.Cryptography.SHA256> クラスを使用してバイト配列をハッシュします。 ハッシュ値はコンソールに表示されます。  

 [!code-csharp[GeneratingAHash#1](../../../samples/snippets/csharp/VS_Snippets_CLR/generatingahash/cs/program.cs#1)]
 [!code-vb[GeneratingAHash#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/generatingahash/vb/program.vb#1)]  
  
 上記のコードは、次の文字列をコンソールに表示します。  
  
 `185 203 236 22 3 228 27 130 87 23 244 15 87 88 14 43 37 61 106 224 81 172 224 211 104 85 194 197 194 25 120 217`  
  
## <a name="verifying-a-hash"></a>ハッシュの検証

 データをハッシュ値と比較すると、整合性を判断できます。 通常、データは特定のタイミングでハッシュされ、ハッシュ値はなんらかの方法で保護されます。 後でデータをもう一度ハッシュし、保護されている値と比較できます。 ハッシュ値が一致する場合、データは変更されていません。 値が一致しない場合は、データが破損しています。 このシステムを機能させるには、保護されたハッシュを暗号化するか、信頼されていないあらゆる対象に対して内密を保つ必要があります。  
  
 文字列の前のハッシュ値を新しいハッシュ値と比較する例を次に示します。 この例では、ループ処理でハッシュ値の各バイトをすべて比較します。  
  
 [!code-csharp[VerifyingAHash#1](../../../samples/snippets/csharp/VS_Snippets_CLR/verifyingahash/cs/program.cs#1)]
 [!code-vb[VerifyingAHash#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/verifyingahash/vb/program.vb#1)]  
  
 2 つのハッシュ値が一致する場合は、上記のコードによってコンソールに次のように表示されます。  
  
```console  
The hash codes match.  
```  
  
 一致しない場合は、次のように表示されます。  
  
```console  
The hash codes do not match.  
```  
  
## <a name="see-also"></a>関連項目

- [暗号モデル](cryptography-model.md)
- [Cryptographic Services](cryptographic-services.md)
- [クロスプラットフォーム暗号化](cross-platform-cryptography.md)
- [ASP.NET Core のデータ保護](/aspnet/core/security/data-protection/introduction)
