---
description: '詳細については、「方法: 再ホストされたデザイナーで検証エラーを表示する」を参照してください。'
title: '方法: 再ホストされたデザイナーの検証エラーを表示する'
ms.date: 03/30/2017
ms.assetid: 5aa8fb53-8f75-433b-bc06-7c7d33583d5d
ms.openlocfilehash: 75ff45e6e9a81df96a9940ce21d1b160cab1000e
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99777737"
---
# <a name="how-to-display-validation-errors-in-a-rehosted-designer"></a>方法: 再ホストされたデザイナーの検証エラーを表示する

このトピックでは、再ホストされた Windows ワークフローデザイナーで検証エラーを取得して発行する方法について説明します。 再ホストされたデザイナー内のワークフローが有効であることを確認するために手順を示します。  
  
 このタスクは 2 つの部分で構成されています。 最初に、<xref:System.Activities.Presentation.Validation.IValidationErrorService> を実装します。  このインターフェイスには、1 つの重要なメソッド <xref:System.Activities.Presentation.Validation.IValidationErrorService.ShowValidationErrors%2A> を実装します。このメソッドは、エラーに関する情報を含む <xref:System.Activities.Presentation.Validation.ValidationErrorInfo> オブジェクトの一覧をデバッグ ログに渡します。  このインターフェイスを実装した後に、その実装のインスタンスを編集コンテキストに発行して、エラー情報を取得します。  
  
### <a name="implement-the-ivalidationerrorservice-interface"></a>IValidationErrorService インターフェイスの実装  
  
1. 以下に、検証エラーをデバッグ ログに書き込む単純な実装を表すコード例を示します。  
  
    ```csharp  
    using System.Activities.Presentation.Validation;  
    using System.Collections.Generic;  
    using System.Diagnostics;  
    using System.Linq;  
  
    namespace VariableFinderShell  
    {  
        class DebugValidationErrorService : IValidationErrorService  
        {  
            public void ShowValidationErrors(IList<ValidationErrorInfo> errors)  
            {  
                errors.ToList().ForEach(vei => Debug.WriteLine($"Error: {vei.Message}"));  
            }  
        }  
    }  
    ```  
  
### <a name="publishing-to-the-editing-context"></a>編集コンテキストへの発行  
  
1. 以下に、編集コンテキストに発行するコードを示します。  
  
    ```csharp  
    wd.Context.Services.Publish<IValidationErrorService>(new DebugValidationErrorService());  
    ```
