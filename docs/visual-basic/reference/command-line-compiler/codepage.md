---
title: -codepage
ms.date: 03/09/2018
helpviewer_keywords:
- -codepage compiler option [Visual Basic]
- codepage compiler option [Visual Basic]
- -codepage compiler option [Visual Basic]
ms.assetid: be36ec33-6800-4505-838c-4124564f5cc9
ms.openlocfilehash: a38fb4be9347b3372b4a459fce2e96b9e38c3a51
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343545"
---
# <a name="-codepage-visual-basic"></a>-codepage (Visual Basic)
コンパイルですべてのソース コード ファイルに使用するコード ページを指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-codepage:id  
```  
  
## <a name="arguments"></a>引数  
  
|用語|Definition|  
|---|---|  
|`id`|必須。 コンパイラは、`id` によって指定されたコードページを使用して、ソースファイルのエンコーディングを解釈します。|  
  
## <a name="remarks"></a>コメント  
 特定のエンコーディングを使用して保存されたソースコードをコンパイルするには、`-codepage` を使用して、使用するコードページを指定します。 `-codepage` オプションは、コンパイル時にすべてのソースコードファイルに適用されます。 詳細については、「[.NET での文字エンコード](../../../standard/base-types/character-encoding.md)」を参照してください。  
  
 現在の ANSI コードページ、Unicode、または UTF-8 を使用してソースコードファイルが保存されている場合、`-codepage` オプションは必要ありません。 Visual Studio では、ユーザーが **[エンコード]** ダイアログボックスで別のエンコードを指定しない限り、既定では、すべてのソースコードファイルが現在の ANSI コードページと共に保存されます。 Visual Studio では、 **[エンコード]** ダイアログボックスを使用して、別のコードページを使用して保存されたソースコードファイルを開きます。  
  
> [!NOTE]
> `-codepage` オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。  
  
## <a name="see-also"></a>参照

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
