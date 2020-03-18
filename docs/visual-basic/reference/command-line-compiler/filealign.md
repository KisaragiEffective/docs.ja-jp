---
title: -filealign
ms.date: 03/10/2018
helpviewer_keywords:
- sections compiler option [Visual Basic]
- alignment compiler option [Visual Basic]
- -filealign compiler option [Visual Basic]
- section alignment
- /filealign compiler option [Visual Basic]
- filealign compiler option [Visual Basic]
ms.assetid: cc61ec3d-ad38-4b28-9659-099d73cad099
ms.openlocfilehash: fef2652f591e713140c651a9cb0df1ea9e6236c8
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005588"
---
# <a name="-filealign"></a>-filealign
出力ファイルでセクションをアラインするサイズを指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-filealign:number  
```  
  
## <a name="arguments"></a>引数  
 `number`  
 必須。 出力ファイル内のセクションの配置を指定する値。 有効値は 512、1024、2048、4096、および 8192 です。 これらの値はバイト単位です。  
  
## <a name="remarks"></a>コメント  
 @No__t-0 オプションを使用して、出力ファイル内のセクションの配置を指定できます。 セクションは、コードまたはデータのいずれかを含む移植可能な実行可能 (PE) ファイル内の連続したメモリのブロックです。 @No__t-0 オプションを使用すると、非標準の配置でアプリケーションをコンパイルできます。ほとんどの開発者は、このオプションを使用する必要はありません。  
  
 各セクションは、@no__t 0 の値の倍数である境界上にアラインされます。 固定の既定値はありません。 @No__t-0 が指定されていない場合、コンパイラはコンパイル時に既定値を選択します。  
  
 セクションのサイズを指定することにより、出力ファイルのサイズを変更できます。 セクションのサイズ変更は、比較的小さなデバイスで実行されるプログラムに対して有効な場合があります。  
  
> [!NOTE]
> @No__t-0 オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
