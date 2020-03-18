---
title: nonComVisibleBaseClass MDA
ms.date: 03/30/2017
helpviewer_keywords:
- visible classes
- managed debugging assistants (MDAs), COM visible classes
- nonComVisibleBaseClass MDA
- COM visible classes
- QueryInterface call failures
- MDAs (managed debugging assistants), COM visible classes
ms.assetid: 9ec1af27-604b-477e-9ee2-e833eb10d3ce
ms.openlocfilehash: b46d5c6ffbf12efbae113a95bbfccd5742ec9ec9
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217298"
---
# <a name="noncomvisiblebaseclass-mda"></a>nonComVisibleBaseClass MDA
`nonComVisibleBaseClass` マネージド デバッグ アシスタント (MDA: Managed Debugging Assistant) は、COM 参照可能ではない基本クラスから派生した COM 参照可能マネージド クラスの COM 呼び出し可能ラッパー (CCW: COM Callable Wrapper) で、ネイティブ コードまたはアンマネージド コードによって `QueryInterface` 呼び出しがなされるとアクティブになります。  `QueryInterface` 呼び出しによって MDA がアクティブになるのは、COM 参照可能マネージド クラスのクラス インターフェイスまたは既定の `IDispatch` が呼び出しによって要求された場合のみです。  `QueryInterface` 呼び出しが、<xref:System.Runtime.InteropServices.ClassInterfaceAttribute> 属性が適用され、COM 参照可能クラスによって明示的に実装された明示的なインターフェイスに対する呼び出しである場合、この MDA はアクティブになりません。  
  
## <a name="symptoms"></a>現象  
 ネイティブ コードからなされた `QueryInterface` 呼び出しが COR_E_INVALIDOPERATION HRESULT により失敗します。  HRESULT は、この MDA をアクティブにする `QueryInterface` 呼び出しをランタイムが許可しないことによって発生することがあります。  
  
## <a name="cause"></a>原因  
 ランタイムは、COM 参照可能ではないクラスから派生した COM 参照可能クラスのクラス インターフェイスまたは既定の `QueryInterface` インターフェイスに対する `IDispatch` 呼び出しを許可できません。これは、バージョン管理に関係する問題が発生する可能性があるからです。  たとえば、COM 参照可能でない基本クラスにパブリック メンバーを追加した場合、基本クラス メンバーを含む派生クラスの vtable がこの変更によって変更されるため、派生クラスを使用する既存の COM クライアントが破損する可能性があります。  COM に公開されている明示的なインターフェイスの場合は、インターフェイスの基本メンバーが vtable に含まれないため、この問題は発生しません。  
  
## <a name="resolution"></a>解決策  
 クラス インターフェイスを公開しないでください。 明示的なインターフェイスを定義し、それに <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> 属性を適用します。  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 この MDA は CLR に影響しません。  
  
## <a name="output"></a>出力  
 非 COM 参照可能クラス `QueryInterface` から派生した COM 参照可能クラス `Derived` で `Base` 呼び出しを実行した場合のメッセージ例を次に示します。  
  
```output
A QueryInterface call was made requesting the class interface of COM   
visible managed class 'Derived'. However since this class derives from   
non COM visible class 'Base', the QueryInterface call will fail. This   
is done to prevent the non COM visible base class from being   
constrained by the COM versioning rules.   
```  
  
## <a name="configuration"></a>構成  
  
```xml  
<mdaConfig>  
  <assistants>  
    <nonComVisibleBaseClass />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
- [相互運用マーシャリング](../interop/interop-marshaling.md)
