---
title: DLL 読み込み時のエラーです。
ms.date: 07/20/2015
f1_keywords:
- vbrID48
ms.assetid: 4226cd1f-028c-477d-88a5-cb57f7e0cdc8
ms.openlocfilehash: 36452cc6ff03042939cd4066aef76129b5bb8f0a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74329552"
---
# <a name="error-in-loading-dll-visual-basic"></a>DLL 読み込み時のエラーです。(Visual Basic)
ダイナミックリンクライブラリ (DLL) は、`Declare` ステートメントの `Lib` 句で指定されたライブラリです。 このエラーには、次の原因が考えられます。  
  
- ファイルが DLL 実行可能ファイルではありません。  
  
- ファイルは Microsoft Windows DLL ではありません。  
  
- DLL が、存在しない別の DLL を参照しています。  
  
- DLL または参照先の DLL が、パスで指定されたディレクトリにありません。  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- ファイルがソーステキストファイルであり、DLL 実行可能ファイルでない場合は、コンパイルして DLL の実行可能な形式にリンクする必要があります。  
  
- ファイルが Microsoft Windows DLL でない場合は、Microsoft Windows と同等のものを取得します。  
  
- DLL が存在しない別の DLL を参照している場合は、参照されている DLL を取得し、使用できるようにします。  
  
- DLL または参照先の DLL がパスによって指定されたディレクトリにない場合は、DLL を参照先のディレクトリに移動します。  
  
## <a name="see-also"></a>参照

- [Declare Statement](../../../visual-basic/language-reference/statements/declare-statement.md)
