---
title: リンク確認要求
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [.NET Framework], demands
- demanded permissions
- permissions [.NET Framework], demands
- granting permissions, demands
- code access security, demands
- user demands for permission
- caller security checks
- link demands
ms.assetid: a33fd5f9-2de9-4653-a4f0-d9df25082c4d
ms.openlocfilehash: 31fbd938acb457a4ea803375d18cb1be11d8b287
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217169"
---
# <a name="link-demands"></a>リンク確認要求
[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 リンク確認要求では、Just-In-Time コンパイル時にセキュリティ チェックが実行され、コードの直前の呼び出し元アセンブリだけがチェックされます。 リンクは、コードを関数ポインターの参照やメソッドの呼び出しなどの型の参照にバインドするときに発生します。 呼び出し元のアセンブリが、コードにリンクするための十分な権限を持たない場合は、リンクは許可されず、コードが読み込まれて実行されると、ランタイム例外がスローされます。 リンク確認要求は、コードから継承するクラスでオーバーライドできます。  
  
 この種類の要求では、完全なスタック ウォークが実行されないことと、コードがおびき寄せによる攻撃を受ける可能性があることに注意してください。 たとえば、アセンブリ A のメソッドがリンク確認要求によって保護されている場合、アセンブリ B の直接の呼び出し元は、アセンブリ B のアクセス許可に基づいて評価されます。 ただし、アセンブリ B のメソッドを使用してアセンブリ A のメソッドを間接的に呼び出す場合、リンク確認要求は、アセンブリ C のメソッドを評価しません。リンク確認要求で指定されるのは、直接呼び出し元のアセンブリ内の直接の呼び出し元がコードにリンクするために必要なアクセス許可のみです。 すべての呼び出し元がコードを実行するために必要なアクセス許可は指定しません。  
  
 <xref:System.Security.CodeAccessPermission.Assert%2A>、<xref:System.Security.CodeAccessPermission.Deny%2A>、および <xref:System.Security.CodeAccessPermission.PermitOnly%2A> の各スタック ウォーク修飾子は、リンク確認要求の評価に影響を与えません。  リンク確認要求は、スタック ウォークを実行しないため、スタック ウォーク修飾子はリンク確認要求に影響を与えません。  
  
 リンク確認要求によって保護されているメソッドが[リフレクション](../reflection-and-codedom/reflection.md)によってアクセスされる場合、リンク確認要求は、リフレクションによってアクセスされるコードの直前の呼び出し元をチェックします。 これは、メソッドの探索と、リフレクションを使用して行うメソッド呼び出しの両方に該当します。 たとえば、コードがリフレクションを使用して、リンク確認要求で保護されているメソッドを表す <xref:System.Reflection.MethodInfo> オブジェクトを返し、その**MethodInfo**オブジェクトを、オブジェクトを使用して元のメソッドを呼び出す他のコードに渡すとします。 この場合、リンク確認要求のチェックは2回行われます。1回は**MethodInfo**オブジェクトを返すコードで、もう1つは、それを呼び出すコードの場合です。  
  
> [!NOTE]
> 静的クラスのコンストラクターで実行されるリンク確認要求では、静的コンストラクターは保護されません。これは、静的コンストラクターがアプリケーションのコードの実行パスの外側で、システムによって呼び出されるためです。 その結果、リンク確認要求がクラス全体に適用される場合、静的コンストラクターへのアクセスは保護できませんが、クラスの残りの部分は保護されます。  
  
 次のコード フラグメントでは、`ReadData` メソッドにリンクするコードはすべて `CustomPermission` のアクセス許可を持つ必要があることを宣言によって指定しています。 このアクセス許可は架空のカスタム許可であり、.NET Framework には実在しません。 要求は、`CustomPermissionAttribute`に**SecurityAction**フラグを渡すことによって行われます。  
  
```vb  
<CustomPermissionAttribute(SecurityAction.LinkDemand)> _  
Public Shared Function ReadData() As String  
    ' Access a custom resource.  
End Function    
```  
  
```csharp  
[CustomPermissionAttribute(SecurityAction.LinkDemand)]  
public static string ReadData()  
{  
    // Access a custom resource.  
}  
```  
  
## <a name="see-also"></a>参照

- [属性](../../standard/attributes/index.md)
- [コード アクセス セキュリティ](code-access-security.md)
