---
title: '方法 : レジストリ キーから値を読み取る'
ms.date: 07/20/2015
helpviewer_keywords:
- registry keys [Visual Basic], determining if a value exists in
- My.Computer.Registry object, examples
- registry [Visual Basic], determining if values exist
- registry keys [Visual Basic], reading from
- registry [Visual Basic], reading
ms.assetid: 775d0a57-68c9-464e-8949-9a39bd29cc64
ms.openlocfilehash: 73c32aefe06a68bb42fcb5f4615da0927e57e892
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345617"
---
# <a name="how-to-read-a-value-from-a-registry-key-in-visual-basic"></a>方法 : Visual Basic で、レジストリ キーから値を読み取る

`GetValue` オブジェクトの `My.Computer.Registry` メソッドは、Windows レジストリ内の値を読み込むために使用できます。  
  
 キー (次の例では "Software\MyApp") が存在しない場合は、例外がスローされます。 `ValueName` (次の例では "Name") が存在しない場合は、`Nothing` が返されます。  
  
 `GetValue` メソッドは、特定のレジストリ キー内に値が存在するかどうかを確認するためにも使用できます。  
  
 コードが Web アプリケーションからレジストリを読み取る際、現在のユーザーは Web アプリケーションに実装されている認証と偽装によって決定されます。  
  
### <a name="to-read-a-value-from-a-registry-key"></a>レジストリ キーから値を読み取るには  
  
- `GetValue` メソッドを使用してパスと名前を指定し、レジストリ キーから値を読み取ります。 次の例は、`Name` から値 `HKEY_CURRENT_USER\Software\MyApp` を読み取り、その値をメッセージ ボックスに表示します。  
  
     [!code-vb[VbResourceTasks#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#4)]  
  
 このコード例は IntelliSense コード スニペットとしても利用できます。 コード スニペット ピッカーでは、これは **[Windows オペレーティング システム] > [レジストリ]** に配置されます。 詳細については、「 [Code Snippets](/visualstudio/ide/code-snippets)」を参照してください。  
  
### <a name="to-determine-whether-a-value-exists-in-a-registry-key"></a>レジストリ キー内値が存在するかどうかを確認するには  
  
- `GetValue` メソッドを使用して値を取得します。 次のコードは、値が存在するかどうかを確認し、存在しない場合にはメッセージを返します。  
  
     [!code-vb[VbResourceTasks#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#12)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  

 レジストリは、データの格納に使用される最上位レベル (またはルート) のキーを保持します。 たとえば、HKEY_LOCAL_MACHINE ルート キーは、すべてのユーザーによって使用される、マシン レベルの設定を格納するため使用されます。これに対し HKEY_CURRENT_USER は、個々のユーザーに固有のデータを格納するために使用されます。  
  
 次の条件を満たす場合は、例外が発生する可能性があります。  
  
- キーの名前が `Nothing` である場合 (<xref:System.ArgumentNullException>)。  
  
- レジストリ キーからの読み取り権限がユーザーにない場合 (<xref:System.Security.SecurityException>)。  
  
- キー名が 255 文字の制限を超えている場合 (<xref:System.ArgumentException>)。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  

 このプロセスを実行するには、アセンブリに対して <xref:System.Security.Permissions.RegistryPermission> クラスで特権レベルが許可されている必要があります。 部分的に信頼されたコンテキストで実行している場合、プロセスは、特権がないために例外をスローする可能性があります。 同様に、ユーザーには、設定に対する作成や書き込みを行うための適切な ACL が必要です。 たとえば、コード アクセス セキュリティのアクセス許可を持つローカル アプリケーションには、オペレーティング システムのアクセス許可がない可能性があります。 詳しくは、「[コード アクセス セキュリティの基礎](../../../../framework/misc/code-access-security-basics.md)」をご覧ください。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- <xref:Microsoft.Win32.RegistryHive>
- [レジストリからの読み取りとレジストリへの書き込み](../../../../visual-basic/developing-apps/programming/computer-resources/reading-from-and-writing-to-the-registry.md)
