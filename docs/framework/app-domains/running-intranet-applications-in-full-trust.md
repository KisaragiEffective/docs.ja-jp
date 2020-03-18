---
title: イントラネット アプリケーションの完全信頼での実行
ms.date: 03/30/2017
helpviewer_keywords:
- full trust, running intranet applications in
- intranet applications, running in full trust
- running intranet applications in full trust
ms.assetid: ee13c0a8-ab02-49f7-b8fb-9eab16c6c4f0
ms.openlocfilehash: c93f84dc53abbb86cbfc4ae36e9cdcbe0bd50273
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73119758"
---
# <a name="running-intranet-applications-in-full-trust"></a>イントラネット アプリケーションの完全信頼での実行

.NET Framework 3.5 Service Pack 1 (SP1) 以降、アプリケーションとそのライブラリ アセンブリをネットワーク共有から完全信頼アセンブリとして実行できます。 イントラネット上の共有から読み込まれたアセンブリに、<xref:System.Security.SecurityZone.MyComputer> ゾーンの証拠が自動的に追加されます。 この証拠は、コンピューター上に存在するアセンブリと同じ許可セット (通常は完全な信頼) をこれらのアセンブリに付与します。 この機能は、ClickOnce アプリケーションまたはホスト上で実行するように設計されたアプリケーションには適用されません。  
  
## <a name="rules-for-library-assemblies"></a>ライブラリ アセンブリのルール  

ネットワーク共有上の実行可能ファイルによって読み込まれるアセンブリには、次のルールが適用されます。  
  
- ライブラリ アセンブリは、実行可能アセンブリと同じフォルダーに存在する必要があります。 サブフォルダーに存在するアセンブリまたは異なるパスで参照されているアセンブリには、完全な信頼の許可セットは付与されません。  
  
- 実行可能ファイルがアセンブリの遅延読み込みを行う場合は、実行可能ファイルの開始に使ったものと同じパスを使う必要があります。 たとえば、共有 \\\\*network-computer*\\*share* がドライブ文字にマップされていて、そのパスから実行可能ファイルが実行される場合、ネットワーク パスを使って実行可能ファイルにより読み込まれるアセンブリには、完全な信頼は付与されません。 <xref:System.Security.SecurityZone.MyComputer> ゾーン内のアセンブリを遅延読み込みするには、実行可能ファイルはドライブ文字のパスを使う必要があります。  
  
## <a name="restoring-the-former-intranet-policy"></a>以前のイントラネット ポリシーの復元  

以前のバージョンの .NET Framework では、共有アセンブリに <xref:System.Security.SecurityZone.Intranet> ゾーンの証拠が付与されていました。 共有上のアセンブリに完全な信頼を付与するには、コード アクセス セキュリティ ポリシーを指定する必要がありました。  
  
この新しい動作は、イントラネットのアセンブリの既定値です。 コンピューター上のすべてのアプリケーションに適用されるレジストリ キーを設定することで、<xref:System.Security.SecurityZone.Intranet> の証拠を提供する以前の動作に戻すことができます。 この手順は、32 ビット コンピューターと 64 ビット コンピューターでは異なります。  
  
- 32 ビット コンピューターでは、システム レジストリの HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework キーの下にサブキーを作成します。 キーの名前は LegacyMyComputerZone、DWORD 値は 1 です。  
  
- 64 ビット コンピューターでは、システム レジストリの HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework キーの下にサブキーを作成します。 キーの名前は LegacyMyComputerZone、DWORD 値は 1 です。 HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\\.NETFramework キーの下に、同じサブキーを作成します。  
  
## <a name="see-also"></a>関連項目

- [アセンブリを使用したプログラミング](../../standard/assembly/program.md)
