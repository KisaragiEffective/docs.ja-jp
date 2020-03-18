---
title: '方法 : データ保護を使用する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DPAPI
- encryption [.NET Framework], data protection API
- data [.NET Framework], decryption
- ProtectedMemory class, about ProtectedMemory class
- ProtectedData class, about ProtectedData class
- cryptography [.NET Framework], data protection API
- data protection API [.NET Framework]
- decryption
- data [.NET Framework], encryption
ms.assetid: 606698b0-cb1a-42ca-beeb-0bea34205d20
ms.openlocfilehash: 0efd677f11189b28b8efc184c04b30a047ab942b
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75706033"
---
# <a name="how-to-use-data-protection"></a>方法 : データ保護を使用する
.NET Framework では、データ保護 API (DPAPI) へのアクセスを提供しています。DPAPI を使用すると、現在のユーザー アカウントまたはコンピューターからの情報を使用してデータを暗号化できます。  DPAPI を使用すると、暗号化キーを明示的に生成および格納するという困難な問題を軽減できます。  
  
 <xref:System.Security.Cryptography.ProtectedMemory> クラスを使用すると、メモリ内のバイト配列を暗号化できます。  この機能は、Microsoft Windows XP 以降のオペレーティング システムで使用できます。  現在のプロセスによって暗号化されるメモリは、現在のプロセスのみ、すべてのプロセス、または同じユーザーのコンテキストによって復号化されることを指定できます。  <xref:System.Security.Cryptography.ProtectedMemory> オプションの詳しい説明については、「<xref:System.Security.Cryptography.MemoryProtectionScope> 列挙型」を参照してください。  
  
 バイト配列のコピーを暗号化するには、<xref:System.Security.Cryptography.ProtectedData> クラスを使用します。 この機能は、Microsoft Windows 2000 以降のオペレーティング システムで使用できます。  現在のユーザー アカウントによって暗号化されたデータは、同じユーザー アカウントによってのみ復号化できることを指定できます。あるいは、現在のユーザー アカウントによって暗号化されたデータは、コンピューター上の任意のアカウントによって復号化できることを指定できます。  <xref:System.Security.Cryptography.ProtectedData> オプションの詳しい説明については、「<xref:System.Security.Cryptography.DataProtectionScope> 列挙型」を参照してください。  
  
### <a name="to-encrypt-in-memory-data-using-data-protection"></a>データの保護を使用してメモリ内のデータを暗号化するには  
  
1. 暗号化するバイト配列、エントロピ、およびメモリ保護スコープを渡す際に、静的な <xref:System.Security.Cryptography.ProtectedMemory.Protect%2A> メソッドを呼び出します。  
  
### <a name="to-decrypt-in-memory-data-using-data-protection"></a>データ保護を使用してメモリ内のデータを復号化するには  
  
1. 復号化するバイト配列およびメモリ保護スコープを渡す際に、静的な <xref:System.Security.Cryptography.ProtectedMemory.Unprotect%2A> メソッドを呼び出します。  
  
### <a name="to-encrypt-data-to-a-file-or-stream-using-data-protection"></a>データ保護を使用してファイルやストリームのデータを暗号化するには  
  
1. ランダム エントロピを作成します。  
  
2. 暗号化するバイト配列、エントロピ、およびデータ保護スコープを渡す際に、静的な <xref:System.Security.Cryptography.ProtectedData.Protect%2A> メソッドを呼び出します。  
  
3. ファイルまたはストリームに暗号化されたデータを書き込みます。  
  
### <a name="to-decrypt-data-from-a-file-or-stream-using-data-protection"></a>データ保護を使用してファイルやストリームからデータを復号化するには  
  
1. ファイルまたはストリームから暗号化されたデータを読み取ります。  
  
2. 復号化するバイト配列およびデータ保護スコープを渡す際に、静的な <xref:System.Security.Cryptography.ProtectedData.Unprotect%2A> メソッドを呼び出します。  
  
## <a name="example"></a>使用例  
 次のコード例は、暗号化と復号化の 2 つの形式を示しています。  最初に、コード例では、メモリ内のバイト配列を暗号化してから復号化します。  次に、コード例では、バイト配列のコピーを暗号化し、ファイルに保存して、そのファイルからデータを読み込み、その後データを復号化しています。  例では、元のデータ、暗号化されたデータ、および復号化されたデータが表示されます。  
  
 [!code-csharp[DPAPI-HowTO#1](../../../samples/snippets/csharp/VS_Snippets_CLR/DPAPI-HowTO/cs/sample.cs#1)]
 [!code-vb[DPAPI-HowTO#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DPAPI-HowTO/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル方法  
  
- `System.Security.dll` への参照を含めます。  
  
- <xref:System>、<xref:System.IO>、<xref:System.Security.Cryptography>、および <xref:System.Text> 名前空間を含めます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Security.Cryptography.ProtectedMemory>
- <xref:System.Security.Cryptography.ProtectedData>
