---
description: '詳細情報: My による開発 (Visual Basic)'
title: My による開発
ms.date: 07/20/2015
f1_keywords:
- My.MyWpfExtension.Windows
helpviewer_keywords:
- My object
- My namespace
- My feature
- Visual Basic, programming in
ms.assetid: f1d04509-5e46-4551-9f9f-94334a121fca
ms.openlocfilehash: f19365471dab5fa30b565a9dd93c40a2f5795118
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99666564"
---
# <a name="development-with-my-visual-basic"></a>My による開発 (Visual Basic)

Visual Basic には、多彩な機能を提供する一方で生産性や使いやすさを向上させる、迅速なアプリケーション開発用の新しい機能が用意されています。 こうした機能の 1 つである `My` という機能は、情報へのアクセス、およびアプリケーションやそのランタイム環境に関連する既定のオブジェクト インスタンスを提供します。 この情報は、IntelliSense によって検出可能な形式で編成され、用途に応じて論理的に区別されます。  
  
 `My` の最上位メンバーは、オブジェクトとして公開されます。 各オブジェクトは、名前空間でも `Shared` メンバーがあるクラスでも同じように動作し、関連するメンバーのセットを公開します。  
  
 次の表は、最上位の `My` オブジェクトと各オブジェクトの相互関係を示しています。  
  
 ![My のオブジェクト モデルを示す図。](./media/index/my-object-model-relationships.gif)  
  
## <a name="in-this-section"></a>このセクションの内容  

 [My.Application、My.Computer、および My.User でのタスクの実行](performing-tasks-with-my-application-my-computer-and-my-user.md)  
 `My` の中心となる 3 つのオブジェクト (`My.Application`、`My.Computer`、および `My.User`) について説明します。これらのオブジェクトは、情報と機能へのアクセスを提供します。  
  
 [My.Forms と My.WebServices が提供する既定のオブジェクト インスタンス](default-object-instances-provided-by-my-forms-and-my-webservices.md)  
 `My.Forms` オブジェクトと `My.WebServices` オブジェクトについて説明します。これらのオブジェクトは、アプリケーションで使用されるフォーム、データ ソース、および XML Web サービスへのアクセスを提供します。  
  
 [My.Resources と My.Settings による Rapid Application Development](rapid-application-development-with-my-resources-and-my-settings.md)  
 `My.Resources` オブジェクトと `My.Settings` オブジェクトについて説明します。これらのオブジェクトは、アプリケーションのリソースと設定へのアクセスを提供します。  
  
 [Visual Basic アプリケーション モデルの概要](overview-of-the-visual-basic-application-model.md)  
 Visual Basic アプリケーションのスタートアップまたはシャットダウン モデルについて説明します。  
  
 [プロジェクトの種類に応じた My の機能](how-my-depends-on-project-type.md)  
 異なる種類のプロジェクトで使用できる `My` 機能の詳細を説明します。  
  
## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.ApplicationServices.User>
- [My.Forms オブジェクト](../../language-reference/objects/my-forms-object.md)
- [My.WebServices オブジェクト](../../language-reference/objects/my-webservices-object.md)
- [プロジェクトの種類に応じた My の機能](how-my-depends-on-project-type.md)
