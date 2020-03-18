---
title: My.Settings オブジェクト
ms.date: 07/20/2015
f1_keywords:
- My.MySettingsProperty.Settings
- My.Settings
helpviewer_keywords:
- My.Settings object
ms.assetid: 41f30dc1-202a-4273-b9b7-5728941f996c
ms.openlocfilehash: 9560a51332ea596d4cf2228f1e07c158a0457ece
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350361"
---
# <a name="mysettings-object"></a>My.Settings オブジェクト
アプリケーションの設定にアクセスするためのプロパティとメソッドを提供します。  
  
## <a name="remarks"></a>コメント  
 `My.Settings` オブジェクトは、アプリケーションの設定へのアクセスを提供し、アプリケーションのプロパティ設定やその他の情報を動的に格納および取得できるようにします。 詳細については、[アプリケーションの設定の管理 (.NET)](/visualstudio/ide/managing-application-settings-dotnet) を参照してください。  
  
## <a name="properties"></a>プロパティ  
 `My.Settings` オブジェクトのプロパティを使用すると、アプリケーションの設定にアクセスできます。 設定を追加または削除するには、**設定デザイナー**を使用します。  
  
 各設定には**名前**、**種類**、**スコープ**、および**値**があり、これらの設定によって、各設定にアクセスするプロパティが `My.Settings` オブジェクトにどのように表示されるかが決まります。  
  
- **Name**プロパティの名前を指定します。  
  
- **Type**は、プロパティの型を決定します。  
  
- **スコープ**は、プロパティが読み取り専用かどうかを示します。 値が**アプリケーション**の場合、プロパティは読み取り専用です。値が**User**の場合、プロパティは読み取り/書き込み可能です。  
  
- **値**は、プロパティの既定値です。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|---|---|  
|`Reload`|最後に保存した値からユーザー設定を再読み込みします。|  
|`Save`|現在のユーザー設定を保存します。|  
  
 `My.Settings` オブジェクトは、<xref:System.Configuration.ApplicationSettingsBase> クラスから継承された高度なプロパティやメソッドも提供します。  
  
## <a name="tasks"></a>タスク  
 次の表に、`My.Settings` オブジェクトに関連するタスクの例を示します。  
  
|目的|参照先|  
|---|---|  
|アプリケーション設定の読み取り|[方法: Visual Basic でアプリケーション設定を読み取る](../../../visual-basic/developing-apps/programming/app-settings/how-to-read-application-settings.md)|  
|ユーザー設定を変更する|[方法: Visual Basic でユーザー設定を変更する](../../../visual-basic/developing-apps/programming/app-settings/how-to-change-user-settings.md)|  
|ユーザー設定を保持する|[方法: Visual Basic でユーザー設定を永続化する](../../../visual-basic/developing-apps/programming/app-settings/how-to-persist-user-settings.md)|  
|ユーザー設定のプロパティグリッドを作成する|[方法: Visual Basic でユーザー設定のためのプロパティ グリッドを作成する](../../../visual-basic/developing-apps/programming/app-settings/how-to-create-property-grids-for-user-settings.md)|  
  
## <a name="example"></a>例  
 次の例は、`Nickname` の設定値を表示します。  
  
 [!code-vb[VbVbalrMyResources#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#14)]  
  
 この例を実行するには、アプリケーションで `Nickname` 型の `String` を設定する必要があります。  
  
## <a name="see-also"></a>参照

- <xref:System.Configuration.ApplicationSettingsBase>
- [方法: Visual Basic でアプリケーション設定を読み取る](../../../visual-basic/developing-apps/programming/app-settings/how-to-read-application-settings.md)
- [方法: Visual Basic でユーザー設定を変更する](../../../visual-basic/developing-apps/programming/app-settings/how-to-change-user-settings.md)
- [方法: Visual Basic でユーザー設定を永続化する](../../../visual-basic/developing-apps/programming/app-settings/how-to-persist-user-settings.md)
- [方法: Visual Basic でユーザー設定のためのプロパティ グリッドを作成する](../../../visual-basic/developing-apps/programming/app-settings/how-to-create-property-grids-for-user-settings.md)
- [アプリケーションの設定の管理 (.NET)](/visualstudio/ide/managing-application-settings-dotnet)
