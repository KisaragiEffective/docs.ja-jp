---
title: アンセーフ コードとポインター - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- security [C#], type safety
- C# language, unsafe code
- type safety [C#]
- unsafe keyword [C#]
- unsafe code [C#]
- C# language, pointers
- pointers [C#], about pointers
ms.assetid: b0fcca10-a92d-4f2a-835b-b0ccae6739ee
ms.openlocfilehash: 013af4e55c8fc396bbc92058d7fb454484f3263e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711832"
---
# <a name="unsafe-code-and-pointers-c-programming-guide"></a>アンセーフ コードとポインター (C# プログラミング ガイド)

タイプ セーフとセキュリティを維持するために、既定では C# はポインター演算をサポートしません。 ただし、[unsafe](../../language-reference/keywords/unsafe.md) キーワードを使用すれば、ポインターを使用できる unsafe コンテキストを定義できます。 ポインターについて詳しくは、[ポインター型](pointer-types.md)に関するページをご覧ください。  
  
> [!NOTE]
> 共通言語ランタイム (CLR) では、アンセーフ コードは検査できないコードと呼ばれます。 C# のアンセーフ コードは、必ずしも危険ではありません。ただ、CLR で安全性を検査できないコードであるというだけです。 そのため CLR は、完全に信頼できるアセンブリ内にある場合にのみ、アンセーフ コードを実行します。 アンセーフ コードを使用する場合は、セキュリティ上のリスクやポインター エラーが発生しないように注意してください。  
  
## <a name="unsafe-code-overview"></a>アンセーフ コードの概要

アンセーフ コードには次の特徴があります。

- メソッド、型、およびコード ブロックをアンセーフとして定義できます。

- アンセーフ コードでアプリケーションのパフォーマンスが向上することがあります。これは、配列のバインド チェックが削除されるためです。

- アンセーフ コードは、ポインターを必要とするネイティブ関数を呼び出すときに必要です。

- アンセーフ コードを使用すると、セキュリティと安定性の面でリスクが生じます。

- unsafe ブロックを含むコードは、[-unsafe](../../language-reference/compiler-options/unsafe-compiler-option.md) コンパイラ オプションを使ってコンパイルする必要があります。
  
## <a name="related-sections"></a>関連項目

詳細については次を参照してください:

- [ポインター型](pointer-types.md)

- [固定サイズ バッファー](fixed-size-buffers.md)

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/unsafe-code.md)の[アンセーフ コード](~/_csharplang/spec/introduction.md)に関するトピックを参照してください。
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [unsafe](../../language-reference/keywords/unsafe.md)
