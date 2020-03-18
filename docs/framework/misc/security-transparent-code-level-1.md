---
title: 透過的セキュリティコード、レベル1
ms.date: 03/30/2017
helpviewer_keywords:
- transparent
- SecurityTreatAsSafeAttribute
- SecurityTransparentAttribute
- SecurityCriticalAttribute
- security-transparent code
- security [.NET Framework], security-transparent code
ms.assetid: 5fd8f46d-3961-46a7-84af-2eb1f48e75cf
ms.openlocfilehash: 8f232a7724ad831818627cbfc2845ea808a3fcfd
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215806"
---
# <a name="security-transparent-code-level-1"></a>透過的セキュリティコード、レベル1
[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 透過性を使用すると、開発者は、部分的に信頼されるコードに機能を公開する .NET Framework ライブラリのセキュリティを強化できます。 レベル 1 の透過性は、.NET Framework Version 2.0 で導入され、主に Microsoft 内でのみ使用されていました。 .NET Framework 4 以降では、[レベル2の透過性](security-transparent-code-level-2.md)を使用できます。 ただし、以前のセキュリティ規則で実行する必要があるレガシコードを特定できるように、レベル1の透過性は保持されています。  
  
> [!IMPORTANT]
> レベル 1 の透過性は、互換性を確保するためにのみ指定してください。つまり、<xref:System.Security.AllowPartiallyTrustedCallersAttribute> を使用するか、透過性モデルを使用しない、.NET Framework 3.5 以前で開発されたコードに対してのみレベル 1 を指定してください。 たとえば、部分的に信頼された呼び出し元からの呼び出しを許可する .NET Framework 2.0 アセンブリ (APTCA) にはレベル 1 の透過性を使用します。 .NET Framework 4 用に開発されたコードでは、常にレベル2の透過性を使用します。  
  
 このトピックには、次のセクションが含まれます。  
  
- [レベル1の透過性モデル](#the_level_1_transparency_model)  
  
- [透過性属性](#transparency_attributes)  
  
- [セキュリティ透過性の例](#security_transparency_examples)  
  
<a name="the_level_1_transparency_model"></a>   
## <a name="the-level-1-transparency-model"></a>レベル 1 の透過性モデル  
 レベル 1 の透過性を使用するときは、セキュリティ透過的、セキュリティ セーフ クリティカル、およびセキュリティ クリティカルの各方式にコードを分離するセキュリティ モデルを使用します。  
  
 アセンブリ全体、アセンブリ内の一部のクラス、またはクラス内の一部のメソッドを、セキュリティ透過的なコードとしてマークできます。 セキュリティ透過的なコードは特権を昇格することはできません。 この制限により、次の 3 つの効果が生まれます。  
  
- セキュリティ透過的なコードは、<xref:System.Security.Permissions.SecurityAction.Assert> アクションを実行できません。  
  
- セキュリティ透過的なコードによって満たされるリンク確認要求は、フル アクセス要求になります。  
  
- セキュリティ透過的なコードで実行される必要のある安全でない (検証できない) コードは、<xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode> セキュリティ アクセス許可に対するフル アクセス要求を発生させます。  
  
 これらの規則は、共通言語ランタイム (CLR: Common Language Runtime) による実行中に適用されます。 セキュリティ透過的なコードは、呼び出すコードのすべてのセキュリティ要件を、呼び出し元に返します。 セキュリティ透過的なコードを流れる確認要求では、特権を昇格できません。 信頼性の低いアプリケーションによってセキュリティ透過的なコードが呼び出され、高い特権の確認要求が発生した場合、その確認要求は信頼性の低いコードに戻されて失敗します。 セキュリティ透過的なコードはアサート アクションを実行できないので、確認要求を停止できません。 同じセキュリティ透過的なコードが、完全に信頼されているコードから呼び出された場合、確認要求は成功します。  
  
 セキュリティ クリティカルは、セキュリティ透過的の反対です。 セキュリティ クリティカルなコードは完全に信頼して実行され、特権が必要な操作をすべて実行できます。 セキュリティ セーフ クリティカルなコードは、広範なセキュリティ監査を経た特権コードであり、部分的に信頼された呼び出し元がアクセスを許可されていないリソースを使用するのを許可しないことが確認されています。  
  
 透過性の適用は明示的に実行する必要があります。 通常、データ操作とロジックを処理するコードの大部分はセキュリティ透過的なコードとしてマークでき、特権の昇格を実行する少数のコードをセキュリティ クリティカルまたはセキュリティ セーフ クリティカルなコードとしてマークします。  
  
> [!IMPORTANT]
> レベル 1 の透過性は、アセンブリ スコープに限定されます。アセンブリ間には適用されません。 レベル 1 の透過性は、主に Microsoft 内でセキュリティ監査を目的として使用されていました。 レベル 1 アセンブリ内のセキュリティ クリティカルな型およびメンバーには、他のアセンブリのセキュリティ透過的なコードからアクセスできます。 重要なのは、すべてのレベル 1 のセキュリティ クリティカルな型およびメンバーで、完全な信頼のためにリンク確認要求を実行することです。 また、セキュリティ セーフ クリティカルな型およびメンバーでも、それらの型またはメンバーがアクセスする保護リソースに対するアクセス許可を呼び出し元が有していることを確認する必要があります。  
  
 以前のバージョンの .NET Framework との下位互換性を維持するため、透過性属性の注釈が付けられていないメンバーはすべてセキュリティ セーフ クリティカルと見なされます。 注釈が付けられていない型はすべて透過的と見なされます。 透過性を検証するためのスタティック分析規則はありません。 したがって、透過性エラーを実行時にデバッグすることが必要になる場合があります。  
  
<a name="transparency_attributes"></a>   
## <a name="transparency-attributes"></a>透過性属性  
 コードの透過性を指定するために使用する 3 つの属性の説明を、次の表に示します。  
  
|属性|説明|  
|---------------|-----------------|  
|<xref:System.Security.SecurityTransparentAttribute>|アセンブリ レベルでのみ使用できます。 アセンブリ内のすべての型とメンバーをセキュリティ透過的として指定します。 このアセンブリにセキュリティ クリティカルなコードを含めることはできません。|  
|<xref:System.Security.SecurityCriticalAttribute>|<xref:System.Security.SecurityCriticalAttribute.Scope%2A> プロパティを設定せずにアセンブリ レベルで使用すると、アセンブリ内のすべてのコードは既定でセキュリティ透過的と指定されますが、アセンブリはセキュリティ クリティカルなコードを含むこともできると指定されます。<br /><br /> クラス レベルで使用すると、クラスまたはメソッドはセキュリティ クリティカルになりますが、クラスのメンバーはセキュリティ クリティカルになりません。 すべてのメンバーをセキュリティ クリティカルにするには、<xref:System.Security.SecurityCriticalAttribute.Scope%2A> プロパティを <xref:System.Security.SecurityCriticalScope.Everything> に設定します。<br /><br /> メンバー レベルで使用した場合、この属性はそのメンバーのみに適用されます。<br /><br /> セキュリティ クリティカルなクラスまたはメンバーは、特権の昇格を実行できます。 **重要:** レベル1の透過性では、セキュリティクリティカルな型およびメンバーは、アセンブリの外部から呼び出されると、セキュリティセーフクリティカルとして扱われます。 承認のない特権の昇格を防ぐために、セキュリティ クリティカルな型およびメンバーを完全な信頼のためのリンク確認要求で保護する必要があります。|  
|<xref:System.Security.SecuritySafeCriticalAttribute>|アセンブリ内のセキュリティ透過的なコードからアクセスできるセキュリティ クリティカルなコードを指定します。 この属性を指定しない場合、セキュリティ透過的なコードは、同じアセンブリ内のプライベートまたは内部のセキュリティ クリティカルなメンバーにアクセスできません。 この属性を指定した場合、セキュリティ クリティカルなコードに影響が出て、予期しない特権の引き上げが可能になります。 セキュリティ セーフ クリティカルなコードについては、厳しいセキュリティ監査を実行する必要があります。 **注:** セキュリティセーフクリティカルな型およびメンバーは、呼び出し元が保護されたリソースにアクセスする権限を持っているかどうかを判断するために、呼び出し元のアクセス許可を検証する必要があります。|  
  
 <xref:System.Security.SecuritySafeCriticalAttribute> 属性を使用すると、セキュリティ透過的なコードが、同じアセンブリ内のセキュリティ クリティカルなメンバーにアクセスできるようになります。 1 つのアセンブリに含まれるセキュリティ透過的なコードとセキュリティ クリティカルなコードは、2 つのアセンブリに分離されていると見なせます。 セキュリティ透過的なコードは、セキュリティ クリティカルなコードのプライベートまたは内部のメンバーを参照できません。 さらに、セキュリティ クリティカルなコードは、通常、そのコードのパブリック インターフェイスへのアクセスについて監査を受けます。 プライベートまたは内部の状態にアセンブリの外部からアクセスすることはできず、状態は分離されていると想定できます。 <xref:System.Security.SecuritySafeCriticalAttribute> 属性を使用すると、透過的セキュリティ コードとセキュリティ クリティカルなコードの間で状態の分離を維持しながら、必要なときには分離をオーバーライドできるようになります。 セキュリティ クリティカルなコードのプライベートまたは内部のメンバーが <xref:System.Security.SecuritySafeCriticalAttribute> でマークされていない場合、セキュリティ透過的なコードはそのコードにアクセスできません。 <xref:System.Security.SecuritySafeCriticalAttribute> を適用する場合は、事前に、パブリックに公開された場合と同じようにしてメンバーを監査してください。  
  
### <a name="assembly-wide-annotation"></a>アセンブリ全体の注釈  
 アセンブリ レベルでセキュリティ属性を使用することの効果について次の表で説明します。  
  
|Assembly 属性|アセンブリの状態|  
|------------------------|--------------------|  
|部分的に信頼されたアセンブリで属性なし|すべての型およびメンバーは透過的です。|  
|完全に信頼されたアセンブリ (グローバル アセンブリ キャッシュに存在または `AppDomain` で完全な信頼として指定) で属性なし。|すべての型は透過的であり、すべてのメンバーはセキュリティ セーフ クリティカルです。|  
|`SecurityTransparent`|すべての型およびメンバーは透過的です。|  
|`SecurityCritical(SecurityCriticalScope.Everything)`|すべての型およびメンバーはセキュリティ クリティカルです。|  
|`SecurityCritical`|すべてのコードは既定で透過的になります。 ただし、個々の型やメンバーに他の属性を設定することもできます。|  
  
<a name="security_transparency_examples"></a>   
## <a name="security-transparency-examples"></a>透過的セキュリティの例  
 .NET Framework 2.0 の透過性規則 (レベル 1 の透過性) を使用するには、次のアセンブリ注釈を使用します。  
  
```csharp
[assembly: SecurityRules(SecurityRuleSet.Level1)]  
```  
  
 アセンブリ全体を透過的にして、アセンブリがクリティカルなコードを含まず、どのような方法でも特権を昇格しないことを示す必要がある場合は、次の属性を使用して、アセンブリに透過性を明示的に追加できます。  
  
```csharp  
[assembly: SecurityTransparent]  
```  
  
 同じアセンブリの中にクリティカルなコードと透過的なコードを混在させる場合は、次に示すように、最初に <xref:System.Security.SecurityCriticalAttribute> 属性をアセンブリにマークして、アセンブリがクリティカルなコードを含むことができることを示します。  
  
```csharp  
[assembly: SecurityCritical]  
```  
  
 セキュリティ クリティカルな操作を実行する場合は、次のコード例で示すように、別の <xref:System.Security.SecurityCriticalAttribute> 属性を使用して、クリティカルな操作を実行するコードを明示的にマークする必要があります。  
  
```csharp  
[assembly: SecurityCritical]  
public class A  
{  
    [SecurityCritical]  
    private void Critical()  
    {  
        // critical  
    }  
  
    public int SomeProperty  
    {  
        get {/* transparent */ }  
        set {/* transparent */ }  
    }  
}  
public class B  
{
    internal string SomeOtherProperty  
    {  
        get { /* transparent */ }  
        set { /* transparent */ }  
    }  
}  
```  
  
 上記のコードは、`Critical` メソッドを明示的にセキュリティ クリティカルとしてマークしている以外は、透過的です。 アセンブリ レベルで <xref:System.Security.SecurityCriticalAttribute> 属性が指定されていても、既定の設定は透過的です。  
  
## <a name="see-also"></a>参照

- [透過的セキュリティコード、レベル2](security-transparent-code-level-2.md)
- [セキュリティの変更](../security/security-changes.md)
