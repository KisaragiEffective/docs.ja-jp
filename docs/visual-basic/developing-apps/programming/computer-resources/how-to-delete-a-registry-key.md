---
title: '方法: レジストリ キーを削除する'
ms.date: 07/20/2015
f1_keywords:
- vb.DeleteSetting
helpviewer_keywords:
- GetSetting function [Visual Basic]
- registry [Visual Basic], deleting values
- GetAllSettings function
- registry keys [Visual Basic], deleting
- registry [Visual Basic], deleting keys
- examples [Visual Basic], registry
ms.assetid: ab9aca0e-42b0-4ff7-8ff9-845a4bfdf9f2
ms.openlocfilehash: f38301a3a717a35b98e55804d6435d046bbbbab4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345655"
---
# <a name="how-to-delete-a-registry-key-in-visual-basic"></a>方法: レジストリ キーを削除する (Visual Basic)

<xref:Microsoft.Win32.RegistryKey.DeleteSubKey%28System.String%29> メソッドと <xref:Microsoft.Win32.RegistryKey.DeleteSubKey%28System.String%2CSystem.Boolean%29> メソッドはレジストリ キーの削除に使用できます。  
  
## <a name="procedure"></a>プロシージャ  
  
#### <a name="to-delete-a-registry-key"></a>レジストリ キーを削除するには  
  
- `DeleteSubKey` メソッドを使用して、レジストリ キーを削除します。 この例では、CurrentUser ハイブの Software/TestApp キーを削除します。 このキーは、コード内で適切な文字列に変更したり、ユーザーが指定した情報を使用したりすることができます。  
  
     [!code-vb[VbResourceTasks#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#19)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  

 キーと値の組み合わせが存在しない場合、`DeleteSubKey` メソッドは空の文字列を返します。  
  
 次の条件を満たす場合は、例外が発生する可能性があります。  
  
- キーの名前が `Nothing` である場合 (<xref:System.ArgumentNullException>)。  
  
- レジストリ キーを作成するためのアクセス許可がユーザーにない場合 (<xref:System.Security.SecurityException>)。  
  
- キー名が 255 文字の制限を超えている場合 (<xref:System.ArgumentException>)。  
  
- レジストリ キーが読み取り専用の場合 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  

 必要なアクセス許可が実行時に与えられない (<xref:System.Security.Permissions.RegistryPermission>) 場合、またはユーザーが設定の作成や書き込みを行うための適切な (ACL によって決定された) アクセス権を持っていない場合、レジストリ呼び出しは失敗します。 たとえば、コード アクセス セキュリティのアクセス許可を持つローカル アプリケーションには、オペレーティング システムのアクセス許可がない可能性があります。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.Win32.RegistryKey.DeleteSubKey%2A>
- <xref:Microsoft.Win32.RegistryKey>
- [セキュリティとレジストリ](../../../../visual-basic/developing-apps/programming/computer-resources/security-and-the-registry.md)
- [レジストリからの読み取りとレジストリへの書き込み](../../../../visual-basic/developing-apps/programming/computer-resources/reading-from-and-writing-to-the-registry.md)
