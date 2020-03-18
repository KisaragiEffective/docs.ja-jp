---
title: <type1>'<typename>' は、インターフェイス '<membername>' に対して '<interfacename>' を実装しなければなりません。
ms.date: 07/20/2015
f1_keywords:
- vbc30154
- bc30154
helpviewer_keywords:
- BC30154
ms.assetid: 259afdfa-3608-4760-adcb-88ec0da5020d
ms.openlocfilehash: a824b66eaad964049ced5cae5eb2cc370d00ba7f
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71696891"
---
# <a name="type1typename-must-implement-membername-for-interface-interfacename"></a>\<type1 > '\<typename > ' は、インターフェイス '\<interfacename > ' に '\<membername > ' を実装しなければなりません
'\<typename > ' は、インターフェイス '\<interfacename > ' に対して '\<membername > ' を実装しなければなりません。 実装するプロパティには、' ReadOnly '/' WriteOnly ' 指定子が一致しなければなりません。  
  
 インターフェイスを実装するクラスまたは構造体が要求しますが、インターフェイスで定義されたプロシージャ、プロパティ、またはイベントを実装しません。 インターフェイスのすべてのメンバーを実装する必要があります。  
  
 **エラー ID:** BC30154  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
1. インターフェイスで定義されているものと同じ名前およびシグネチャを持つメンバーを宣言します。 少なくとも `End Function`、`End Sub`、または `End Property` ステートメントを含めるようにしてください。  
  
2. `Function`、`Sub`、`Property`、または `Event` ステートメントの末尾に `Implements` 句を追加します。 例 :  
  
    ```vb  
    Public Event ItHappened() Implements IBaseInterface.ItHappened  
    ```  
  
3. プロパティを実装するときは、インターフェイス定義と同じ方法で `ReadOnly` または `WriteOnly` が使用されていることを確認してください。  
  
4. プロパティを実装する場合は、必要に応じて `Get` および `Set` プロシージャを宣言します。  
  
## <a name="see-also"></a>参照

- [Implements ステートメント](../../../visual-basic/language-reference/statements/implements-statement.md)
- [インターフェイス](../../../visual-basic/programming-guide/language-features/interfaces/index.md)
