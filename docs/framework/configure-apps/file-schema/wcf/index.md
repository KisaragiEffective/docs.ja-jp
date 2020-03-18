---
title: WCF 構成スキーマ
ms.date: 03/30/2017
ms.assetid: c282aeb5-91f0-4522-8e2f-704c1ef3651f
ms.openlocfilehash: 866b0639f4391e1898bbe36e458df87e3c24bfff
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77448660"
---
# <a name="wcf-configuration-schema"></a>WCF 構成スキーマ
Windows Communication Foundation (WCF) 構成要素を使用すると、WCF サービスとクライアントアプリケーションを構成できます。 [ 構成エディター ツール (SvcConfigEditor.exe)](../../../wcf/configuration-editor-tool-svcconfigeditor-exe.md) を使用して、クライアントとサービスの構成ファイルを作成および変更できます。 構成ファイルは XML として書式設定されているので、テキスト エディターを使用して手動で編集する場合は、XML について理解している必要があります。 理解しないで編集すると、XML 要素タグや属性が見つからないなどの問題が発生する可能性があります。 問題の原因は、XML 要素タグと属性が大文字と小文字を区別することによります。  
  
 WCF 構成システムは、<xref:System.Configuration> 名前空間に基づいています。 したがって、<xref:System.Configuration> 名前空間によって提供される、構成ロック、暗号化、マージなどのすべての標準機能を使用して、アプリケーションとその構成のセキュリティを強化できます。 これらの概念の詳細については、次のトピックを参照してください。  
  
 [保護された構成を使用した構成情報の暗号化](https://docs.microsoft.com/previous-versions/aspnet/53tyfkaw(v=vs.100))  
  
 [構成設定のロック](https://docs.microsoft.com/previous-versions/aspnet/55th21y4(v=vs.100))  
  
 このセクションでは、各構成項目のすべての可能な値についてと各構成項目が他の WCF 構成要素とやり取りする方法について説明します。 次のマップは、WCF 構成スキーマを示しています。  
  
 ![WCF 構成スキーマを示す図。](./media/index/windows-communication-foundation-configuration-schema.gif)  
  
> [!CAUTION]
> アプリケーション構成ファイル (app.config) の WCF 構成セクションは、適切な Access Control リスト (ACL) を使用して保護し、潜在的なセキュリティ上の脅威を防ぐ必要があります。  たとえば、適切なユーザーだけが、アプリケーション バインドのセキュリティ設定、またはサービスの構成ファイルのサービス モデル セクションにアクセスまたは変更できるようにする必要があります。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [\<system.serviceModel>](system-servicemodel.md)  
 `ServiceModel` 要素について説明します。  
  
 [\<system.serviceModel.activation >](system-servicemodel-activation.md)  
 SMSvcHost.exe ツールを構成します。  
  
 [\<system.runtime.serialization >](system-runtime-serialization.md)  
 <xref:System.Runtime.Serialization.DataContractSerializer> などのシリアライザーの使用時にオプションを設定するための最上位の要素。  
  
## <a name="related-sections"></a>関連セクション  
 [Windows Communication Foundation アプリケーションの構成](../../../wcf/configuring-services.md)  
 WCF サービスとクライアントを構成する方法について説明します。
