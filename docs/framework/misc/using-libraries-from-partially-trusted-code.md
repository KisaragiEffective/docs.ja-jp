---
title: 部分信頼コードからのライブラリの使用
ms.date: 03/30/2017
helpviewer_keywords:
- security [.NET Framework], partially trusted code
- partially trusted code
- partial trust
- AllowPartiallyTrustedCallersAttribute attribute
- code access security, partially trusted code
- APTCA
ms.assetid: dd66cd4c-b087-415f-9c3e-94e3a1835f74
ms.openlocfilehash: 6836d6c7f67afba316125b57b2c3e64a59ac648f
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216991"
---
# <a name="using-libraries-from-partially-trusted-code"></a>部分信頼コードからのライブラリの使用
[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
> [!NOTE]
> このトピックでは、厳密な名前付きアセンブリの動作について説明し、[レベル 1](security-transparent-code-level-1.md)のアセンブリにのみ適用します。 [セキュリティ透過的なコード、](security-transparent-code-level-2.md) .NET Framework 4 以降のレベル2のアセンブリは、厳密な名前の影響を受けません。 セキュリティシステムへの変更の詳細については、「[セキュリティの変更](../security/security-changes.md)」を参照してください。  
  
 ホストまたはサンドボックスから不完全な信頼を受けているアプリケーションは、ライブラリの作成者が <xref:System.Security.AllowPartiallyTrustedCallersAttribute> 属性を使用して具体的に許可しない限り、共有マネージド ライブラリを呼び出せません。 そのため、アプリケーションの作成者は、ライブラリによっては部分的に信頼されたコンテキストから使用できないことを認識する必要があります。 既定では、部分信頼[サンドボックス](how-to-run-partially-trusted-code-in-a-sandbox.md)で実行され、完全に信頼されたアセンブリの一覧にないすべてのコードは、部分的に信頼されています。 部分的に信頼されたコンテキストからコードを実行する、または部分的に信頼されたコードによってコードが呼び出されることはないと考えられる場合は、このセクションの情報を考慮する必要はありません。 ただし、部分的に信頼されたコードと対話する必要があるコード、または部分的に信頼されたコンテキストから操作するコードを記述する場合は、次の要因を考慮する必要があります。  
  
- ライブラリは、複数のアプリケーションで共有するために、厳密な名前で署名する必要があります。 厳密な名前を使用すると、コードをグローバル アセンブリ キャッシュに配置したり、サンドボックス化する <xref:System.AppDomain> の完全信頼一覧に追加したりできます。また、コンシューマーは、実際にあなたが発信する特定のモバイル コードを確認することができます。  
  
- 既定では、厳密な名前の[レベル 1](security-transparent-code-level-1.md)の共有ライブラリは、完全な信頼に対して暗黙的な[LinkDemand](link-demands.md)を実行します。ライブラリの作成者は何もする必要はありません。  
  
- 呼び出し元に完全な信頼がないにもかかわらず、このようなライブラリを呼び出そうとする場合、ランタイムは <xref:System.Security.SecurityException> をスローします。これにより、呼び出し元はライブラリにリンクできなくなります。  
  
- 自動**LinkDemand**を無効にし、例外がスローされないようにするには、 **AllowPartiallyTrustedCallersAttribute**属性を共有ライブラリのアセンブリスコープに配置します。 この属性により、部分的に信頼されたマネージド コードからライブラリを呼び出すことができます。  
  
- それでも、この属性を持つライブラリへのアクセスが許可されている部分的に信頼されたコードは、<xref:System.AppDomain> が定義する追加の制約を受けます。  
  
- 部分的に信頼されたコードが**AllowPartiallyTrustedCallersAttribute**属性を持たないライブラリを呼び出す場合、プログラムによる方法はありません。  
  
 特定のアプリケーションに対してプライベートなライブラリは、厳密な名前または**AllowPartiallyTrustedCallersAttribute**属性を必要とせず、アプリケーション外部の悪意のあるコードから参照することはできません。 このようなコードは、部分的に信頼されたモバイル コードの意図的または意図的でない誤使用から保護されています。開発者は追加で何かをする必要はありません。  
  
 次の種類のコードについて、部分的に信頼されたコードによる使用を明示的に有効にすることを検討してください。  
  
- セキュリティの脆弱性を入念にテストしたコードと、「[安全なコーディングのガイドライン](../../standard/security/secure-coding-guidelines.md)」で説明されているガイドラインに準拠していること。  
  
- 部分的に信頼されたシナリオ用に特に記述された厳密な名前のコード ライブラリ。  
  
- インターネットからダウンロードしたコードによって呼び出される、厳密な名前で署名された (部分的に信頼された、または完全に信頼された) すべてのコンポーネント。  
  
> [!NOTE]
> .NET Framework クラスライブラリ内の一部のクラスには**AllowPartiallyTrustedCallersAttribute**属性がないため、部分的に信頼されたコードから呼び出すことはできません。  
  
## <a name="see-also"></a>参照

- [コード アクセス セキュリティ](code-access-security.md)
