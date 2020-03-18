---
title: Storeadm.exe (分離ストレージ ツール)
ms.date: 03/30/2017
helpviewer_keywords:
- Storeadm.exe
- listing stores for current user
- Isolated Storage tool
- stores, current user
- removing stores
ms.assetid: b81202b8-d91d-4b23-9c53-4a112f74a44a
ms.openlocfilehash: 46e846eaf92835fb2a9130b85ed20749934ca5a1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75715714"
---
# <a name="storeadmexe-isolated-storage-tool"></a>Storeadm.exe (分離ストレージ ツール)
分離ストレージ ツールは、現在のユーザーに関するすべての既存ストアの一覧表示または削除を行います。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
storeadm [/list][/machine][/remove][/roaming][/quiet]  
```  
  
## <a name="parameters"></a>パラメーター  
  
|オプション|[説明]|  
|------------|-----------------|  
|**/h** **[elp]**|このツールのコマンド構文とオプションを表示します。|  
|**/list**|現在のユーザーに関するすべての既存ストアを表示します。 このユーザーによって実行された、すべてのアプリケーションまたはアセンブリに関するストアなどが表示されます。|  
|**/machine**|コンピューター ストアを選択します。 このオプションを **/list** または **/remove** オプションと一緒に使用すると、それらのアクションをマシン ストアに適用する必要があることを指定できます。<br /><br /> .NET Framework 2.0 で新たに追加されました。|  
|**/quiet**|クワイエット モードを指定します。このモードでは、情報の出力が中止され、エラー メッセージだけが表示されます。|  
|**/remove**|現在のユーザーに関するすべての既存ストアを削除します。|  
|**/roaming**|ローミング ストアを選択します。 このオプションを **/list** または **/remove** オプションと一緒に使用すると、それらのアクションをローミング ストアに適用する必要があることを指定できます。|  
|**/?**|このツールのコマンド構文とオプションを表示します。|  
  
## <a name="remarks"></a>解説  
 オプションを指定せずにコマンド ラインから Storeadm.exe を実行すると、Storeadm.exe に関する構文とオプションが表示されます。  
  
 一般に、 **/list** オプションと **/remove** オプションは同時に使用されますが、2 つ以上のオプションを指定した場合、それらのオプションはコマンド ラインに表示されている順序で実行されます。  
  
 アプリケーションは、ユーザー ストアまたはコンピューター ストアのいずれかを選択して保存できます。  
  
- ローカル ストアは、該当するユーザーに関するデータのローミングが有効な場合でも、ローミングされないことが保証されている場所 (Windows 2000 以降) に存在します。  
  
- ローミング ストアは、ローミングできる場所に存在しますが、ローミングできるのは、Windows NT の管理機能を通じて、該当するユーザーに対してローミングが有効化されている場合に限られます。  
  
- コンピューター ストアは、コンピューターのすべてのユーザーに共通で、コンピューターの共通ディレクトリに格納されます。  
  
    > [!NOTE]
    > コンピューター ストアは .NET Framework Version 2.0 で新たに追加されました。  
  
 ユーザーに対してローミングが実際に有効になっているかどうかは、Storeadm.exe の管理に影響を与えません。 オプションを指定せずに Storeadm.exe を実行した場合、すべてのアクションがローカル ストアに適用されます。 **/roaming** オプションを指定した場合は、すべてのアクションが、ローミングできるストアに適用されます。 **/machine** オプションを指定してこのツールを実行すると、すべてのアクションがコンピューター ストアに適用されます。  
  
## <a name="see-also"></a>参照

- [ツール](index.md)
- [分離ストレージ](../../standard/io/isolated-storage.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
