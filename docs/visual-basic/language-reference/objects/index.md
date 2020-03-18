---
title: オブジェクト
ms.date: 07/20/2015
helpviewer_keywords:
- objects [Visual Basic]
ms.assetid: 651c73e4-dca8-402b-9c6b-e3902b3a3f4b
ms.openlocfilehash: 2108e36956ada98e48e6ab05cec56dbf2a12b3dd
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74838261"
---
# <a name="objects-visual-basic"></a>オブジェクト (Visual Basic)
このトピックでは、Visual Basic ランタイム オブジェクトを示すトピックや、そのメンバー プロシージャ、プロパティ、およびイベントの表を含む、その他のトピックへのリンクを提供します。  
  
## <a name="visual-basic-run-time-objects"></a>Visual Basic ランタイム オブジェクト  
  
|||  
|---|---|  
|<xref:Microsoft.VisualBasic.Collection>|単一のオブジェクトとして関連するアイテムのグループを表示するための便利な方法を提供します。|  
|<xref:Microsoft.VisualBasic.Information.Err%2A>|ランタイム エラーに関する情報を格納します。|  
|`My.Application` オブジェクトは、次のクラスで構成されています。<br /><br /> <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase> は、すべてのプロジェクトで使用可能なメンバーを提供します。<br /><br /> <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> は、Windows フォーム アプリケーションで使用可能なメンバーを提供します。<br /><br /> <xref:Microsoft.VisualBasic.ApplicationServices.ConsoleApplicationBase> は、コンソール アプリケーションで使用可能なメンバーを提供します。|現在のアプリケーションまたは DLL にのみ関連付けられているデータを提供します。 システム レベル以外の情報は、`My.Application` に変更される可能性があります。<br /><br /> 一部のメンバーは、Windows フォームまたはコンソール アプリケーションでのみ使用できます。|  
|`My.Application.Info` (<xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Info%2A>)|バージョン番号、説明、読み込まれたアセンブリなど、アプリケーションに関する情報を取得するためにプロパティを提供します。|  
|`My.Application.Log` (<xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Log%2A>)|イベントと例外の情報をアプリケーションのログ リスナーに書き込むためのプロパティとメソッドを提供します。|  
|`My.Computer` (<xref:Microsoft.VisualBasic.Devices.Computer>)|オーディオ、時計、キーボード、ファイル システムなどのコンピューター コンポーネントを操作するためのプロパティを提供します。|  
|`My.Computer.Audio` (<xref:Microsoft.VisualBasic.Devices.Audio>)|サウンドを再生するためのメソッドを提供します。|  
|`My.Computer.Clipboard` (<xref:Microsoft.VisualBasic.Devices.Computer.Clipboard%2A>)|クリップボードを操作するためのメソッドを提供します。|  
|`My.Computer.Clock` (<xref:Microsoft.VisualBasic.Devices.Clock>)|システム クロックから現在の現地時刻と世界協定時刻 (グリニッジ標準時に相当します) にアクセスするためのプロパティを提供します。|  
|`My.Computer.FileSystem` (<xref:Microsoft.VisualBasic.FileIO.FileSystem>)|ドライブ、ファイル、ディレクトリを操作するためのプロパティとメソッドを提供します。|  
|`My.Computer.FileSystem.SpecialDirectories` (<xref:Microsoft.VisualBasic.FileIO.SpecialDirectories>)|一般的に参照されるディレクトリにアクセスするためのプロパティを提供します。|  
|`My.Computer.Info` (<xref:Microsoft.VisualBasic.Devices.ComputerInfo>)|コンピューターのメモリ、読み込まれたアセンブリ、名前、オペレーティング システムに関する情報を取得するためのプロパティを提供します。|  
|`My.Computer.Keyboard` (<xref:Microsoft.VisualBasic.Devices.Keyboard>)|現在どのキーが押されているかなど、キーボードの現在の状態にアクセスするためのプロパティを提供します。また、キーストロークを作業中のウィンドウに送るメソッドを提供します。|  
|`My.Computer.Mouse` (<xref:Microsoft.VisualBasic.Devices.Mouse>)|ローカル コンピューターにインストールされているマウスの形式と構成に関する情報を取得するためのプロパティを提供します。|  
|`My.Computer.Network` (<xref:Microsoft.VisualBasic.Devices.Network>)|コンピューターが接続されるネットワークと対話するためのプロパティ、イベント、およびメソッドを提供します。|  
|`My.Computer.Ports` (<xref:Microsoft.VisualBasic.Devices.Ports>)|コンピューターのシリアル ポートにアクセスするためのプロパティとメソッドを提供します。|  
|`My.Computer.Registry` (<xref:Microsoft.VisualBasic.MyServices.RegistryProxy>)|レジストリを操作するためのプロパティとメソッドを提供します。|  
|[My.Forms オブジェクト](../../../visual-basic/language-reference/objects/my-forms-object.md)|現在のプロジェクトで宣言されている各 Windows フォームのインスタンスにアクセスするためのプロパティを提供します。|  
|`My.Log` (<xref:Microsoft.VisualBasic.Logging.AspLog>)|イベントと例外の情報を Web アプリケーション用のアプリケーションのログ リスナーに書き込むためのプロパティとメソッドを提供します。|  
|[My.Request オブジェクト](../../../visual-basic/language-reference/objects/my-request-object.md)|要求されたページの <xref:System.Web.HttpRequest> オブジェクトを取得します。 `My.Request` オブジェクトには、現在の HTTP 要求に関する情報が含まれています。<br /><br /> `My.Request` オブジェクトは、ASP.NET アプリケーションでのみ使うことができます。|  
|[My.Resources オブジェクト](../../../visual-basic/language-reference/objects/my-resources-object.md)|アプリケーションのリソースにアクセスするためのプロパティとクラスを提供します。|  
|[My.Response オブジェクト](../../../visual-basic/language-reference/objects/my-response-object.md)|<xref:System.Web.HttpResponse> に関連付けられている <xref:System.Web.UI.Page> オブジェクトを取得します。 このオブジェクトでは、HTTP 応答データをクライアントに送信し、その応答に関する情報を含めることができます。<br /><br /> `My.Response` オブジェクトは、ASP.NET アプリケーションでのみ使うことができます。|  
|[My.Settings オブジェクト](../../../visual-basic/language-reference/objects/my-settings-object.md)|アプリケーションの設定にアクセスするためのプロパティとメソッドを提供します。|  
|`My.User` (<xref:Microsoft.VisualBasic.ApplicationServices.User>)|現在のユーザーに関する情報へのアクセスを提供します。|  
|[My.WebServices オブジェクト](../../../visual-basic/language-reference/objects/my-webservices-object.md)|現在のプロジェクトによって参照される各 Web サービスの単一のインスタンスを作成してアクセスするためのプロパティを提供します。|  
|<xref:Microsoft.VisualBasic.FileIO.TextFieldParser>|構造化テキスト ファイルの解析に使用するメソッドとプロパティを提供します。|  
  
## <a name="see-also"></a>参照

- [Visual Basic の言語リファレンス](../../../visual-basic/language-reference/index.md)
