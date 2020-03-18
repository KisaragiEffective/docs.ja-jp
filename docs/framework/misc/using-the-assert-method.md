---
title: Assert メソッドの使用
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- granting permissions, overriding security checks
- code access security, overriding security checks
- overriding security checks
- security [.NET Framework], overriding security checks
- security [.NET Framework], assertions
- asserted permissions
- Assert method
- caller security checks
- permissions [.NET Framework], overriding security checks
- permissions [.NET Framework], assertions
ms.assetid: 1e40f4d3-fb7d-4f19-b334-b6076d469ea9
ms.openlocfilehash: 2bc46714a508990c5ae31b50e7d19a287da2c5c0
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215817"
---
# <a name="using-the-assert-method"></a>Assert メソッドの使用
[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 <xref:System.Security.CodeAccessPermission.Assert%2A> は、コードのアクセス許可のクラスおよび <xref:System.Security.PermissionSet>クラスで呼び出すことができるメソッドです。 **Assert**を使用すると、コード (および下流の呼び出し元) が、コードが実行するアクセス許可を持っているものの、その呼び出し元が実行するアクセス許可を持っていないアクションを実行できるようになります。 セキュリティ アサーションは、セキュリティ チェック時にランタイムが実行する正常なプロセスを変更します。 アクセス許可をアサートすると、アサートされたアクセス許可のためにコードの呼び出し元をチェックしないようセキュリティ システムに指示します。  
  
> [!CAUTION]
> アサーションはセキュリティ ホールを開き、セキュリティの制限事項を適用するランタイムのメカニズムを弱体化させる可能性があるため、慎重に使用してください。  
  
 アサーションは、ライブラリがアンマネージ コードを呼び出す状況、またはライブラリの使用目的と明らかに関係のないアクセス許可を必要とする呼び出しを行う状況に役立ちます。 たとえば、アンマネージコードを呼び出すすべてのマネージコードは、 **UnmanagedCode**フラグが指定された**SecurityPermission**を持つ必要があります。 ローカルのイントラネットからダウンロードするコードなど、ローカル コンピューターから発生していないコードには、既定でこのアクセス許可は付与されません。 そのため、ローカルのイントラネットからダウンロードするコードがアンマネージ コードを使用するライブラリを呼び出せるようにするには、ライブラリによってアサートされたアクセス許可がコードに指定されている必要があります。 さらに、ライブラリによっては呼び出し元からは見えない呼び出し、および特別なアクセス許可を必要とする呼び出しを行う場合があります。  
  
 また、コードが呼び出し元から完全に非表示にされる方法でリソースにアクセスする状況において、アサーションを使用することもできます。 たとえば、ライブラリがデータベースから情報を取得しますが、プロセス内でコンピューターのレジストリからも情報を読み取ると仮定してください。 ライブラリを使用している開発者は、ソースにアクセスできないため、コードを使用するためにコードが**RegistryPermission**を必要としていることを知る方法がありません。 この場合、コードの呼び出し元がレジストリへのアクセス許可を持つことを求めるのは妥当でないか不必要と判断した場合は、レジストリを読み取るアクセス許可をアサートすることができます。 このような状況では、 **RegistryPermission**のない呼び出し元がライブラリを使用できるように、ライブラリがアクセス許可をアサートするのが適切です。  
  
 アサーションがスタック ウォークに影響を与えるのは、アサートされたアクセス許可および下流の呼び出し元によって要求されるアクセス許可が同じ型である場合、かつ要求されたアクセス許可がアサートされているアクセス許可のサブセットである場合のみです。 たとえば、C:\ をアサートして C ドライブ上のすべての**ファイルを読み取る**場合、 **fileiopermission**が C:\Temp 内のファイルを読み取るために下流の要求が行われると、アサーションはスタックウォークに影響を与える可能性があります。ただし、 **FileIOPermission**から C ドライブに書き込む必要がある場合、アサーションは無効になります。  
  
 アサーションを実行する場合、コードにアサートしているアクセス許可と、アサーションを実行する権利を表す <xref:System.Security.Permissions.SecurityPermission> の両方が付与されている必要があります。 コードに付与されていないアクセス許可をアサートすることができますが、アサーションがセキュリティ チェックを成功させようとする前にセキュリティ チェックが失敗するため、アサーションが無意味になってしまいます。  
  
 次の図は、 **Assert**を使用した場合の動作を示しています。 次のステートメントについて、アセンブリ A、B、C、E、および F が true であり、P1 と P1A という 2 つのアクセス許可があると仮定してください。  
  
- P1A は C ドライブ上の .txt ファイルを読み取る権限を表します。  
  
- P1 は C ドライブ上のすべてのファイルを読み取る権限を表します。  
  
- P1A と P1 はどちらも**FileIOPermission**型であり、P1A は p1 のサブセットです。  
  
- アセンブリ E と F には P1A のアクセス許可が付与されています。  
  
- アセンブリ C には P1 のアクセス許可が付与されています。  
  
- アセンブリ A と B には P1 と P1A のアクセス許可のいずれも付与されていません。  
  
- メソッド A はアセンブリ A 内にあり、メソッド B はアセンブリ B 内にあり、以下同様です。  
  
 ![Assert メソッドアセンブリを示す図。](./media/using-the-assert-method/assert-method-assemblies.gif)    
  
 このシナリオでは、メソッド A が B を呼び出し、B は c を呼び出し、C は E を呼び出し、E は F を呼び出します。メソッド C は、c ドライブ (アクセス許可 P1) 上のファイルを読み取るためのアクセス許可をアサートし、メソッド E は C ドライブ (アクセス許可 P1A) で .txt ファイルを読み取るアクセス許可を要求します。 実行時に F の要求が発生した場合、F で始まるすべての呼び出し元のアクセス許可を確認するためにスタックウォークが実行されます。 P1A のアクセス許可が付与されているため、スタックウォークは C のアクセス許可を調べ、C のアサーションを検出します。 要求されたアクセス許可 (P1A) は、アサートされたアクセス許可 (P1) のサブセットであるため、スタック ウォークが停止し、セキュリティ チェックは自動的に成功します。 アセンブリ A と B に P1A のアクセス許可が付与されていないことは関係ありません。 P1 をアサートすることで、メソッド C は、呼び出し元がそのリソースへのアクセス許可を付与されていない場合でも呼び出し元が、P1 で保護されているリソースにアクセスできることを保証します。  
  
 クラス ライブラリをデザインし、クラスが保護されたリソースにアクセスする場合は、ほとんどの場合、呼び出し元のクラスが適切なアクセス許可を持つことを求めるセキュリティの要求を行う必要があります。 その後、ほとんどの呼び出し元にアクセス許可がないことがわかっている操作を実行し、呼び出し元がコードを呼び出すことを許可する必要がある場合は、コードが実行している操作を表すアクセス許可オブジェクトで**assert**メソッドを呼び出すことにより、アクセス許可をアサートできます。 この方法で**Assert**を使用すると、通常は、コードを呼び出すことができなかった呼び出し元を呼び出すことができます。 そのため、アクセス許可をアサートする場合、自分のコンポーネントが誤使用されるのを防ぐために、必ず事前に適切なセキュリティ チェックを実行する必要があります。  
  
 たとえば、信頼性の高いライブラリのクラスにファイルを削除するメソッドがあると仮定します。 メソッドは、アンマネージ Win32 関数を呼び出してファイルにアクセスします。 呼び出し元がコードの**Delete**メソッドを呼び出し、削除するファイルの名前を渡します。 c:\test.txt **Delete**メソッド内では、c:\test.txt への書き込みアクセスを表す <xref:System.Security.Permissions.FileIOPermission> オブジェクトがコードによって作成されます。 (ファイルを削除するには書き込みアクセスが必要です)。コードは、 **FileIOPermission**オブジェクトの**Demand**メソッドを呼び出すことによって、強制セキュリティチェックを呼び出します。 コール スタックのいずれかの呼び出し元にこのアクセス許可がない場合は、<xref:System.Security.SecurityException> がスローされます。 例外がスローされない場合、すべての呼び出し元に C:\Test.txt へのアクセス権があることがわかります。 ほとんどの呼び出し元にはアンマネージコードへのアクセス許可がないと考えられるので、コードはアンマネージコードを呼び出す権限を表す <xref:System.Security.Permissions.SecurityPermission> オブジェクトを作成し、オブジェクトの**Assert**メソッドを呼び出します。 最後に、コードは C:\Text.txt を削除するアンマネージ Win32 関数を呼び出し、呼び出し元にコントロールを返します。  
  
> [!CAUTION]
> そこで、アサートするためのアクセス許可によって保護されているリソースにアクセスするために自分のコードが他のコードによって使用される状況では、自分のコードでアサーションが使用されていないことを確認する必要があります。 たとえば、呼び出し元によってパラメーターとして指定された名前を持つファイルに書き込むコードでは、ファイルへの書き込みのための**FileIOPermission**をアサートしません。これは、コードがサードパーティによって悪用されるためです。  
  
 命令型セキュリティ構文を使用する場合、同じメソッド内の複数のアクセス許可で**Assert**メソッドを呼び出すと、セキュリティ例外がスローされます。 代わりに、 **PermissionSet**オブジェクトを作成し、呼び出す個々のアクセス許可を渡してから、 **PermissionSet**オブジェクトの**Assert**メソッドを呼び出します。 宣言セキュリティ構文を使用すると、 **Assert**メソッドを複数回呼び出すことができます。  
  
 次の例は、 **Assert**メソッドを使用してセキュリティチェックをオーバーライドする宣言型の構文を示しています。 **FileIOPermissionAttribute**構文は、<xref:System.Security.Permissions.SecurityAction> 列挙型と、アクセス許可が付与されるファイルまたはディレクトリの場所の2つの値を取ります。 呼び出し元にファイルへのアクセス許可があるかどうかがチェックされない場合でも、 **Assert**を呼び出すと、`C:\Log.txt` へのアクセスの要求が成功します。  
  
```vb  
Option Explicit  
Option Strict  
  
Imports System  
Imports System.IO  
Imports System.Security.Permissions  
  
Namespace LogUtil  
   Public Class Log  
      Public Sub New()  
  
      End Sub  
  
     <FileIOPermission(SecurityAction.Assert, All := "C:\Log.txt")> Public Sub   
      MakeLog()  
         Dim TextStream As New StreamWriter("C:\Log.txt")  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now) '  
         TextStream.Close()  
      End Sub  
   End Class  
End Namespace  
```  
  
```csharp  
namespace LogUtil  
{  
   using System;  
   using System.IO;  
   using System.Security.Permissions;  
  
   public class Log  
   {  
      public Log()  
      {      
      }     
      [FileIOPermission(SecurityAction.Assert, All = @"C:\Log.txt")]  
      public void MakeLog()  
      {     
         StreamWriter TextStream = new StreamWriter(@"C:\Log.txt");  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now);  
         TextStream.Close();  
      }  
   }  
}   
```  
  
 次のコードフラグメントは、 **Assert**メソッドを使用してセキュリティチェックをオーバーライドするための命令型の構文を示しています。 この例では、 **FileIOPermission**オブジェクトのインスタンスが宣言されています。 コンストラクターには、許可されているアクセスの種類を定義するための FileIOPermissionAccess が渡され、その後にファイルの場所を説明する文字列が続きます **。** **FileIOPermission**オブジェクトが定義されたら、その**Assert**メソッドを呼び出して、セキュリティチェックをオーバーライドするだけで済みます。  
  
```vb  
Option Explicit  
Option Strict  
Imports System  
Imports System.IO  
Imports System.Security.Permissions  
Namespace LogUtil  
   Public Class Log  
      Public Sub New()  
      End Sub 'New  
  
      Public Sub MakeLog()  
         Dim FilePermission As New FileIOPermission(FileIOPermissionAccess.AllAccess, "C:\Log.txt")  
         FilePermission.Assert()  
         Dim TextStream As New StreamWriter("C:\Log.txt")  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now)  
         TextStream.Close()  
      End Sub  
   End Class  
End Namespace  
```  
  
```csharp  
namespace LogUtil  
{  
   using System;  
   using System.IO;  
   using System.Security.Permissions;  
  
   public class Log  
   {  
      public Log()  
      {      
      }     
      public void MakeLog()  
      {  
         FileIOPermission FilePermission = new FileIOPermission(FileIOPermissionAccess.AllAccess,@"C:\Log.txt");   
         FilePermission.Assert();  
         StreamWriter TextStream = new StreamWriter(@"C:\Log.txt");  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now);  
         TextStream.Close();  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Security.PermissionSet>
- <xref:System.Security.Permissions.SecurityPermission>
- <xref:System.Security.Permissions.FileIOPermission>
- <xref:System.Security.Permissions.SecurityAction>
- [属性](../../standard/attributes/index.md)
- [コード アクセス セキュリティ](code-access-security.md)
