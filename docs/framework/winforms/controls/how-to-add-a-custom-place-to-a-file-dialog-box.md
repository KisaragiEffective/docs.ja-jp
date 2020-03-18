---
title: '方法 : よく使用する場所をファイル ダイアログ ボックスに追加する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Custom Place to dialog box
- adding Custom Place to dialog box
- CustomPlaces collection
ms.assetid: 63f6469b-59cd-40f6-9e61-8b5831856780
ms.openlocfilehash: 7172e484451cf932413920910ec9124b3388bd76
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976991"
---
# <a name="how-to-add-a-custom-place-to-a-file-dialog-box"></a>方法 : よく使用する場所をファイル ダイアログ ボックスに追加する
Windows Vista の既定の 開く および 保存 ダイアログボックスの左側には、**お気に入りリンク** というタイトルの領域があります。 この領域にはカスタム プレイスという名称が付いています。 <xref:System.Windows.Forms.OpenFileDialog> クラスと <xref:System.Windows.Forms.SaveFileDialog> クラスを使用すると、<xref:System.Windows.Forms.FileDialog.CustomPlaces%2A> コレクションにフォルダーを追加できます。  
  
> [!NOTE]
> カスタムプレースが <xref:System.Windows.Forms.OpenFileDialog> または <xref:System.Windows.Forms.SaveFileDialog>に表示されるようにするには、<xref:System.Windows.Forms.FileDialog.AutoUpgradeEnabled%2A> プロパティを `true` (既定値) に設定する必要があります。  
  
### <a name="to-add-a-custom-place-to-a-file-dialog-box"></a>ファイル ダイアログ ボックスにカスタム プレイスを追加するには  
  
- ダイアログボックスの <xref:System.Windows.Forms.FileDialog.CustomPlaces%2A> コレクションに、パス、既知のフォルダー GUID、または <xref:System.Windows.Forms.FileDialogCustomPlace> オブジェクトを追加します。  
  
     次のコード例は、パスを追加する方法を示しています。  
  
    ```vb  
    OpenFileDialog1.CustomPlaces.Add("C:\MyCustomPlace")  
    ```  
  
    ```csharp  
    openFileDialog1.CustomPlaces.Add("C:\\MyCustomPlace");  
    ```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.FileDialog>
- <xref:System.Windows.Forms.FileDialogCustomPlacesCollection.Add%2A?displayProperty=nameWithType>
- [ファイル ダイアログ ボックスのカスタム プレイス用既知のフォルダー GUID](known-folder-guids-for-file-dialog-custom-places.md)
