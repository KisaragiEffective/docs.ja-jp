---
title: Checked と Unchecked - C# リファレンス
ms.date: 05/15/2018
helpviewer_keywords:
- operators [C#], checked and unchecked
- exceptions [C#], overflow checking
- checked statement [C#]
- overflow checking [C#]
- unchecked statement [C#]
- statements [C#], checked and unchecked
ms.assetid: a84bc877-2c7f-4396-8735-1ce97c42f35e
ms.openlocfilehash: 8ee4c481a30dce30029fbe8cc26f4798b523a7ed
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173641"
---
# <a name="checked-and-unchecked-c-reference"></a>Checked と Unchecked (C# リファレンス)
C# のステートメントは、checked または unchecked のいずれかのコンテキストで実行できます。 checked コンテキストでは、算術オーバーフローにより例外が発生します。 unchecked コンテキストでは、算術オーバーフローが無視され、結果の格納先の型に収まらない上位ビットが破棄されて、結果が切り詰められます。  
  
- [checked](checked.md): checked コンテキストを指定します。  
  
- [unchecked](unchecked.md): unchecked コンテキストを指定します。  
  
 オーバーフロー チェックにより、次の操作が影響を受けます。  
  
- 整数型で次の定義済み演算子を使用する式:  
  
     `++``--` (単項) `-``+``-``*``/`  
  
- 整数型間か、`float` または `double` から整数型へのの明示的な数値変換。  
  
 `checked` と `unchecked` のいずれも指定されない場合、非定数式 (実行時に評価される式) の既定のコンテキストが [-checked](../compiler-options/checked-compiler-option.md) コンパイラ オプションの値によって定義されます。 既定では、そのオプションの値の設定が解除され、算術演算が unchecked コンテキストで実行されます。

 定数式 (コンパイル時に完全に評価される式) の場合、既定のコンテキストは常に確認されます。 定数式が unchecked コンテキストに明示的に配置されていない限り、式のコンパイル時の評価中に発生するオーバーフローによってコンパイル時エラーが発生します。
  
## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [ステートメントのキーワード](statement-keywords.md)
