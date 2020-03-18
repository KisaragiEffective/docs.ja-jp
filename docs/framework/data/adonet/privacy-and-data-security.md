---
title: プライバシーとデータ セキュリティ
ms.date: 03/30/2017
ms.assetid: 46fa5839-adf7-4c7c-bce3-71e941fa7de9
ms.openlocfilehash: a8d7ae2ece966b4649c9c988c123304fe3f1b4d9
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348133"
---
# <a name="privacy-and-data-security"></a>プライバシーとデータ セキュリティ
ADO.NET アプリケーションにおける機密情報の保護と管理は、基になる製品および作成に使用されたテクノロジに依存します。 ADO.NET そのものは、データを保護したり暗号化したりするためのサービスを提供しません。  
  
## <a name="cryptography-and-hash-codes"></a>暗号化とハッシュ コード  
 ADO.NET アプリケーションから .NET Framework <xref:System.Security.Cryptography> 名前空間のクラスを使用すると、認証されていない第三者によるデータの読み取りや変更を防止できます。 クラスには、アンマネージド Microsoft CryptoAPI 用のラッパーもあれば、マネージド実装もあります。 [暗号化サービス](../../../standard/security/cryptographic-services.md)のトピックでは、.NET Framework での暗号化の概要、やの実装方法、および特定の暗号化タスクを実行する方法について説明します。  
  
 暗号化ではデータを暗号化した後に復号化を行いますが、データのハッシュは不可逆性の処理です。 データのハッシュは、データが変更されていないことを確認することによって改ざんを防止する場合に役立ちます。ハッシュ アルゴリズムは、同一内容の入力文字列に対して、容易に比較できる同一内容の短い値を出力します。 ハッシュ[コードを使用してデータの整合性を確保](../../../standard/security/ensuring-data-integrity-with-hash-codes.md)するハッシュ値を生成および検証する方法について説明します。  
  
## <a name="encrypting-configuration-files"></a>構成ファイルの暗号化  
 アプリケーションのセキュリティを実現するうえで、データ ソースへのアクセスを保護することは、最も重要な目標の 1 つです。 接続文字列は、安全に保護されていないと、潜在的な脆弱性になります。 構成ファイルに保存された接続文字列は、.NET Framework が基本的な要素を定義するために使用する標準的な XML ファイルに格納されます。 保護構成機能を使用すると、構成ファイル内の機密情報を暗号化できます。 保護構成は、主に ASP.NET アプリケーションを想定して設計されたものですが、Windows アプリケーションの構成ファイル セクションを暗号化する目的でも使用できます。 詳細については、「[接続情報の保護](protecting-connection-information.md)」を参照してください。  
  
## <a name="securing-string-values-in-memory"></a>メモリ内の文字列値の保護  
 <xref:System.String> オブジェクトにパスワード、クレジット カード番号、個人データなどの機密情報が含まれていると、そのデータの使用後に、情報が漏洩するリスクが生じます。アプリケーションでは、文字列データをコンピューターのメモリから削除することはできません。  
  
 <xref:System.String> は不変であり、いったん作成された文字列値は変更できません。 文字列値を変更したように見えても、実際には <xref:System.String> オブジェクトの新しいインスタンスがメモリ内に作成されているだけです。データはテキスト形式で保存されます。 また、文字列のインスタンスがいつメモリから削除されるかを予測することも不可能です。 文字列型によって占有されたメモリは、.NET のガベージ コレクションによって非確定的に回収されます。 高い機密を要するデータに <xref:System.String> クラスや <xref:System.Text.StringBuilder> クラスを使用することは避けてください。  
  
 <xref:System.Security.SecureString> クラスには、Data Protection API (DPAPI) を使ってメモリ内のテキストを暗号化するためのメソッドが用意されています。 文字列は不要になった段階でメモリから削除されます。 文字列の内容を簡単に読み取ることのできる `ToString` メソッドは、<xref:System.Security.SecureString> には存在しません。 `SecureString` の新しいインスタンスを作成する際は、値を渡さずに初期化することも、<xref:System.Char> オブジェクトの配列へのポインターを渡して初期化することもできます。 その後、このクラスの各種のメソッドを使って文字列を操作できます。
  
## <a name="see-also"></a>関連項目

- [ADO.NET アプリケーションのセキュリティ保護](securing-ado-net-applications.md)
- [SQL Server のセキュリティ](./sql/sql-server-security.md)
- [ADO.NET の概要](ado-net-overview.md)
