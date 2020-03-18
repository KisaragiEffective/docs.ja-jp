---
title: UI オートメーションのセキュリティの概要
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, security model
- security model, UI Automation
ms.assetid: 1d853695-973c-48ae-b382-4132ae702805
ms.openlocfilehash: 70d24c3dcc531abcec6d4dce75b5f0b31757e0c0
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448776"
---
# <a name="ui-automation-security-overview"></a>UI オートメーションのセキュリティの概要

> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。

この概要では、Windows Vista の [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] のセキュリティモデルについて説明します。

<a name="User_Account_Control"></a>

## <a name="user-account-control"></a>ユーザー アカウント制御

セキュリティは Windows Vista の主な焦点であり、イノベーションの中では、より高い特権を必要とするアプリケーションやサービスの実行がブロックされることなく、ユーザーが標準 (管理者以外の) ユーザーとして実行できるようにする機能があります。

Windows Vista では、ほとんどのアプリケーションに標準トークンまたは管理トークンが付属しています。 アプリケーションが管理アプリケーションとして識別できない場合は、既定で標準のアプリケーションとして起動されます。 管理対象として識別されたアプリケーションを起動できるようになる前に、Windows Vista では、昇格されたアプリケーションの実行に同意するようにユーザーに求めます。 ユーザーがローカル管理者グループのメンバーである場合でも、同意を求めるメッセージは既定で表示されます。これは、管理者の資格情報を必要とするアプリケーションまたはシステム コンポーネントが実行の許可を要求するまで、管理者は標準ユーザーとして実行するためです。

<a name="Tasks_Requiring_Higher_Privileges"></a>

## <a name="tasks-requiring-higher-privileges"></a>より高い特権が必要なタスク

ユーザーが管理者特権を必要とするタスクを実行しようとすると、Windows Vista では、ユーザーに続行の同意を求めるダイアログボックスが表示されます。 このダイアログ ボックスは、悪意のあるソフトウェアがユーザー入力をシミュレートできないように、プロセス間通信から保護されます。 同様に、デスクトップのログオン画面は、通常は他のプロセスからはアクセスできません。

UI オートメーション クライアントは、他のプロセスと通信する必要があります。プロセスによっては、より高い特権レベルで実行している可能性があります。 クライアントにも、通常は他のプロセスによって表示できないシステム ダイアログ ボックスへのアクセスが必要になる可能性があります。 そのため、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] クライアントはシステムによって信頼されている必要があり、特別な特権で実行する必要があります。

より高い権限レベルで実行されているアプリケーションと通信する信頼を得るためには、アプリケーションに署名する必要があります。

<a name="Manifest_Files"></a>

## <a name="manifest-files"></a>マニフェスト ファイル

保護されたシステム UI にアクセスするには、次のように、`requestedExecutionLevel` タグに `uiAccess` 属性を含むマニフェストファイルを使用してアプリケーションをビルドする必要があります。

```xml
<trustInfo xmlns="urn:schemas-microsoft-com:asm.v3">
  <security>
    <requestedPrivileges>
      <requestedExecutionLevel
        level="highestAvailable"
        uiAccess="true" />
    </requestedPrivileges>
  </security>
</trustInfo>
```

このコードの `level` 属性の値は一例にすぎません。

既定では、`uiAccess` は "false" です。つまり、属性を省略した場合、またはアセンブリのマニフェストが存在しない場合、アプリケーションは保護された UI にアクセスできなくなります。
