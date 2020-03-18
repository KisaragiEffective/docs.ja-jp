---
title: WriteOnly
ms.date: 07/20/2015
f1_keywords:
- WriteOnly
- vb.WriteOnly
helpviewer_keywords:
- write-only properties
- WriteOnly keyword [Visual Basic]
- sensitive data, protecting
- properties [Visual Basic], write-only
- sensitive data
ms.assetid: 488d2899-b09f-4cee-92f0-6f9f9fc4f944
ms.openlocfilehash: 847617ea6534089857a759fbea3bb16a3a5a36a1
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344189"
---
# <a name="writeonly-visual-basic"></a>WriteOnly (Visual Basic)
プロパティを書き込むことができても読み取ることができないことを指定します。  
  
## <a name="remarks"></a>コメント  
  
## <a name="rules"></a>ルール  
 **宣言コンテキスト。** `WriteOnly` は、モジュール レベルでのみ使用できます。 つまり、`WriteOnly` プロパティの宣言コンテキストは、クラス、構造体、またはモジュールである必要があり、ソースファイル、名前空間、またはプロシージャにすることはできません。  
  
 プロパティは、変数ではなく `WriteOnly`として宣言できます。  
  
## <a name="when-to-use-writeonly"></a>WriteOnly を使用する場合  
 場合によっては、コンシューマー側のコードで値を設定できても、それが何であるかを検出できないようにする必要があります。 たとえば、ソーシャル登録番号やパスワードなどの機密データは、設定されていないコンポーネントによるアクセスから保護する必要があります。 このような場合は、`WriteOnly` プロパティを使用して値を設定できます。  
  
> [!IMPORTANT]
> `WriteOnly` プロパティを定義して使用する場合は、次の追加の保護対策を検討してください。  
  
- **オーバーライド.** プロパティがクラスのメンバーである場合は、既定の[NotOverridable](../../../visual-basic/language-reference/modifiers/notoverridable.md)にすることを許可し、`Overridable` または `MustOverride`を宣言しません。 これにより、派生クラスがオーバーライドを通じて望ましくないアクセスを行うのを防ぐことができます。  
  
- **アクセスレベル。** プロパティの機微なデータを 1 つ以上の変数に保持している場合は、他のコードがアクセスできないように [Private](../../../visual-basic/language-reference/modifiers/private.md)変数を宣言します。
  
- **暗号化。** すべての機密データをプレーンテキストではなく暗号化された形式で保存します。 悪意のあるコードが何らかの方法でメモリ領域にアクセスできた場合は、データを使用するのが難しくなります。 暗号化は、機密データをシリアル化する必要がある場合にも役立ちます。  
  
- **リセット。** プロパティを定義するクラス、構造体、またはモジュールが終了しているときに、機微なデータを既定値またはその他の意味のない値にリセットします。 これにより、メモリの領域が一般アクセス用に解放されるときに、追加の保護が提供されます。  
  
- **永続性。** 機密データを回避できる場合は、ディスクなどに保存しないでください。 また、機微なデータをクリップボードに書き込まないでください。
  
 このコンテキストでは、`WriteOnly` 修飾子を使用できます。  
  
 [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)  
  
## <a name="see-also"></a>参照

- [ReadOnly](../../../visual-basic/language-reference/modifiers/readonly.md)
- [Private](../../../visual-basic/language-reference/modifiers/private.md)
- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
