---
description: '詳細情報: 方法: WCF URL 予約を制限付きの予約に置き換える'
title: '方法: WCF URL 予約を制限付きの予約に置き換える'
ms.date: 03/30/2017
ms.assetid: 2754d223-79fc-4e2b-a6ce-989889f2abfa
ms.openlocfilehash: b3fec02e6bf041430dfb7082453b33c7f448bb28
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643424"
---
# <a name="how-to-replace-the-wcf-url-reservation-with-a-restricted-reservation"></a>方法: WCF URL 予約を制限付きの予約に置き換える

URL 予約を使用すると、特定の URL または URL セットからメッセージを受信するユーザーを制限できます。 予約は、URL テンプレート、アクセス制御リスト (ACL)、およびフラグのセットで構成されます。 URL テンプレートは、予約の対象となる URL を定義します。 URL テンプレートが処理される方法について詳しくは、「[受信要求のルーティング](/windows/win32/http/routing-incoming-requests)」を参照してください。 ACL は、指定された URL からメッセージを受信できるユーザーまたはユーザー グループを制御します。 フラグは、その予約で、ユーザーまたはグループに URL を直接リッスンする権限を与えるか、リッスンを他のプロセスに委任する権限を与えるかを指定します。  
  
 既定のオペレーティング システム構成の一部として、Windows Communication Foundation (WCF) はポート 80 に対してグローバルにアクセス可能な予約を作成します。これにより、すべてのユーザーは、双方向通信にデュアル HTTP バインディングを使用するアプリケーションを実行できます。 この予約の ACL はすべてのユーザー向けなので、管理者は URL または URL セットをリッスンする権限を明示的に許可または拒否することはできません。 このトピックでは、この予約を削除し、制限された ACL を使用する予約を再作成する方法について説明します。  
  
Windows Vista または Windows Server 2008 では、管理者特権でのコマンド プロンプトから「`netsh http show urlacl`」と入力することで、すべての HTTP URL 予約を表示できます。 次に WCF URL 予約の例を示します。

```output
Reserved URL : http://+:80/Temporary_Listen_Addresses/  
        User: \Everyone  
            Listen: Yes  
            Delegate: No  
            SDDL: D:(A;;GX;;;WD)  
```

 この予約には、WCF アプリケーションが双方向通信にデュアル HTTP バインディングを使用する際の URL テンプレートが含まれています。 この形式の URL は、デュアル HTTP バインディングを介して通信する際に、WCF サービスが WCF クライアントにメッセージを返信するために使用されます。 すべてのユーザーに対して、URL をリッスンする権限が与えられていますが、リッスンを他のプロセスに委任する権限は与えられていません。 また、ACL は SSDL (Security Descriptor Definition Language) で記述されています。 SSDL の詳細については、[SSDL](/windows/win32/secauthz/security-descriptor-definition-language) に関する記事を参照してください  
  
## <a name="to-delete-the-wcf-url-reservation"></a>WCF URL 予約を削除するには  
  
1. **[スタート]** ボタンをクリックし、 **[すべてのプログラム]** 、 **[アクセサリ]** の順にポイントします。次に、 **[コマンド プロンプト]** を右クリックし、表示されるコンテキスト メニューで **[管理者として実行]** をクリックします。 ユーザー アカウント制御 (UAC) ウィンドウで続行の許可を求められたら、 **[続行]** をクリックします。  
  
2. コマンド プロンプト ウィンドウに「`netsh http delete urlacl url=http://+:80/Temporary_Listen_Addresses/`」と入力します。  
  
3. 予約が正常に削除されると、次のメッセージが表示されます。 **URL 予約を正常に削除しました**  
  
## <a name="creating-a-new-security-group-and-new-restricted-url-reservation"></a>新しいセキュリティ グループおよび新しい制限付き URL 予約の作成  

 WCF URL 予約を制限付きの予約に置き換えるには、まず新しいセキュリティ グループを作成する必要があります。 この操作は、コマンド プロンプトを使用する方法か、コンピューターの管理コンソールを使用する方法で行うことができます。 行う必要があるのはいずれか一方のみです。  
  
### <a name="to-create-a-new-security-group-from-a-command-prompt"></a>コマンド プロンプトで新しいセキュリティ グループを作成するには  
  
1. **[スタート]** ボタンをクリックし、 **[すべてのプログラム]** 、 **[アクセサリ]** の順にポイントします。次に、 **[コマンド プロンプト]** を右クリックし、表示されるコンテキスト メニューで **[管理者として実行]** をクリックします。 ユーザー アカウント制御 (UAC) ウィンドウで続行の許可を求められたら、 **[続行]** をクリックします。  
  
2. コマンド プロンプトで、「`net localgroup "<security group name>" /comment:"<security group description>" /add`」と入力します。 **\<security group name>** は作成するセキュリティ グループの名前に、 **\<security group description>** はそのセキュリティ グループに適した説明に置き換えて入力してください。  
  
3. セキュリティ グループが正常に作成されると、次のメッセージが表示されます。 **コマンドは正常に終了しました。**  
  
### <a name="to-create-a-new-security-group-from-the-computer-management-console"></a>コンピューターの管理コンソールで新しいセキュリティ グループを作成するには  
  
1. **[スタート]** ボタンをクリックして **[コントロール パネル]** 、 **[管理ツール]** の順にポイントし、 **[コンピューターの管理]** をクリックしてコンピューターの管理コンソールを開きます。 ユーザー アカウント制御 (UAC) ウィンドウで続行の許可を求められたら、 **[続行]** をクリックします。  
  
2. **[システム ツール]** をクリックして、 **[ローカル ユーザーとグループ]** をクリックします。次に、 **[グループ]** フォルダーを右クリックし、表示されるコンテキスト メニューで **[新しいグループ]** をクリックします。 作成する新しいセキュリティ グループについて、 **[グループ名]** や **[説明]** などの情報を入力し、 **[作成]** をクリックしてセキュリティ グループを作成します。  
  
### <a name="to-create-the-restricted-url-reservation"></a>制限付き URL 予約を作成するには  
  
1. **[スタート]** ボタンをクリックし、 **[すべてのプログラム]** 、 **[アクセサリ]** の順にポイントします。次に、 **[コマンド プロンプト]** を右クリックし、表示されるコンテキスト メニューで **[管理者として実行]** をクリックします。 ユーザー アカウント制御 (UAC) ウィンドウで続行の許可を求められたら、 **[続行]** をクリックします。  
  
2. コマンド プロンプトで、「`netsh http add urlacl url=http://+:80/Temporary_Listen_Addresses/ user="<machine name>\<security group name>`」と入力します。 **\<machine name>** はグループを作成するコンピューター名に、 **\<security group name>** は上の手順で作成したセキュリティ グループ名に置き換えて入力してください。  
  
3. 予約が正常に作成されると、 "**URL 予約を正常に追加しました**" というメッセージが表示されます。
