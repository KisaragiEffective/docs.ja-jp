---
title: -codepage (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /codepage
helpviewer_keywords:
- /codepage compiler option [C#]
- codepage compiler option [C#]
- -codepage compiler option [C#]
ms.assetid: 75942989-b69a-4308-90a0-840c73d2c478
ms.openlocfilehash: 3352e7fc446ace391540360a3b6b36d604ca5f13
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173758"
---
# <a name="-codepage-c-compiler-options"></a>-codepage (C# コンパイラ オプション)
このオプションでは、必要とするページが、システムで使用されている現在の既定のコードページでない場合に、コンパイル時に使用するコード ページを指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-codepage:id  
```  
  
## <a name="arguments"></a>引数  
 `id`  
 コンパイル時にすべてのソース コード ファイルで使うコード ページの ID です。  
  
## <a name="remarks"></a>解説  
 コンパイラでは、まずすべてのソース ファイルを UTF-8 として解釈しようと試みられます。 ソース コード ファイルが UTF-8 以外のエンコーディングで、7 ビット ASCII 文字以外の文字が使われている場合は、 **-codepage** オプションを使用して、使用する必要があるコード ページを指定できます。 **-codepage** は、コンパイル時にすべてのソース コード ファイルに適用されます。  

 使用しているシステムでサポートされているコード ページを確認する方法については、[GetCPInfo](/windows/desktop/api/winnls/nf-winnls-getcpinfo) に関するページをご覧ください。  
  
 このコンパイラ オプションは Visual Studio では使用できず、プログラムで変更することはできません。  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
