---
title: '方法: ユーザー設定のためのプロパティ グリッドを作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- My.Settings object [Visual Basic], creating property grids for user settings
- user settings [Visual Basic], creating property grids
- property grids [Visual Basic], creating for user settings
- property grids
ms.assetid: b0bc737e-50d1-43d1-a6df-268db6e6f91c
ms.openlocfilehash: bed4e8a2b50f0115c3b8d9d6abf427df5f216388
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74329611"
---
# <a name="how-to-create-property-grids-for-user-settings-in-visual-basic"></a>方法: Visual Basic でユーザー設定のためのプロパティ グリッドを作成する

<xref:System.Windows.Forms.PropertyGrid> コントロールに `My.Settings` オブジェクトのユーザー設定プロパティを設定すると、ユーザー設定のためのプロパティ グリッドを作成できます。  
  
> [!NOTE]
> この例を使用するには、アプリケーションのユーザー設定を構成することが必要です。 詳細については、[アプリケーションの設定の管理 (.NET)](/visualstudio/ide/managing-application-settings-dotnet) を参照してください。  
  
 `My.Settings` オブジェクトでは、各設定はプロパティとして公開されます。 プロパティ名はその設定の名前と同じで、プロパティの型は設定の型と同じです。 プロパティが読み取り専用かどうかは、設定の**スコープ**でわかります。つまり、**アプリケーション**スコープの設定のプロパティは読み取り専用であるのに対し、**ユーザー**スコープの設定のプロパティは読み取り/書き込みです。 詳細については、「[My.Settings オブジェクト](../../../../visual-basic/language-reference/objects/my-settings-object.md)」を参照してください。  
  
> [!NOTE]
> アプリケーション スコープの設定値を実行時に変更または保存することはできません。 アプリケーション スコープの設定は、アプリケーションを作成するときに**プロジェクト デザイナー**を使用するか、アプリケーションの構成ファイルを編集するかのいずれかの方法でしか変更できません。 詳細については、「[アプリケーションの設定の管理 (.NET)](/visualstudio/ide/managing-application-settings-dotnet)」を参照してください。  
  
 この例では、<xref:System.Windows.Forms.PropertyGrid> コントロールを使用して、`My.Settings` オブジェクトのユーザー設定プロパティにアクセスします。 既定では、<xref:System.Windows.Forms.PropertyGrid> には `My.Settings` オブジェクトのすべてのプロパティが表示されます。 一方、ユーザー設定プロパティは <xref:System.Configuration.UserScopedSettingAttribute> 属性を持ちます。 この例では、<xref:System.Windows.Forms.PropertyGrid> の <xref:System.Windows.Forms.PropertyGrid.BrowsableAttributes%2A> プロパティを <xref:System.Configuration.UserScopedSettingAttribute> に設定して、ユーザー設定プロパティのみを表示します。  
  
### <a name="to-add-a-user-setting-property-grid"></a>ユーザー設定のプロパティ グリッドを追加するには  
  
1. アプリケーションのデザイン サーフェイス (ここでは `Form1` とする) に、 **[ツールボックス]** から **[PropertyGrid]** コントロールを追加します。  
  
     プロパティ グリッド コントロールの既定の名前は `PropertyGrid1` です。  
  
2. `Form1` のデザイン サーフェイスをダブルクリックして、フォーム読み込みのイベント ハンドラーのコードを開きます。  
  
3. `My.Settings` オブジェクトをプロパティ グリッド用に選択されたオブジェクトとして設定します。  
  
     [!code-vb[VbVbalrMyResources#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#11)]  
  
4. ユーザー設定のみを表示するように、プロパティ グリッドを構成します。  
  
     [!code-vb[VbVbalrMyResources#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#12)]  
  
    > [!NOTE]
    > アプリケーション スコープの設定のみを表示するには、<xref:System.Configuration.UserScopedSettingAttribute> ではなく <xref:System.Configuration.ApplicationScopedSettingAttribute> 属性を使用します。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  

 アプリケーションがユーザー設定を保存するのは、アプリケーションの終了時です。 設定をすぐに保存するには、`My.Settings.Save` メソッドを呼び出します。 詳細については、[Visual Basic でユーザー設定を永続化する](../../../../visual-basic/developing-apps/programming/app-settings/how-to-persist-user-settings.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [My.Settings オブジェクト](../../../../visual-basic/language-reference/objects/my-settings-object.md)
- [方法: Visual Basic でアプリケーション設定を読み取る](../../../../visual-basic/developing-apps/programming/app-settings/how-to-read-application-settings.md)
- [方法: Visual Basic でユーザー設定を変更する](../../../../visual-basic/developing-apps/programming/app-settings/how-to-change-user-settings.md)
- [方法: Visual Basic でユーザー設定を永続化する](../../../../visual-basic/developing-apps/programming/app-settings/how-to-persist-user-settings.md)
- [アプリケーションの設定の管理 (.NET)](/visualstudio/ide/managing-application-settings-dotnet)
