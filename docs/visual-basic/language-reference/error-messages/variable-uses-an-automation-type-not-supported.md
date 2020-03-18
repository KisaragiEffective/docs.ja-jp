---
title: サポートされていないオートメーション型を変数で使用しています
ms.date: 07/20/2015
f1_keywords:
- vbrID458
ms.assetid: bde4f4da-493b-452c-b6e4-1d370edba4cd
ms.openlocfilehash: 944c0c63cd0d7ae7f9ff770fd123231464af1eaf
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344828"
---
# <a name="variable-uses-an-automation-type-not-supported-in-visual-basic"></a>Visual Basic でサポートされていないオートメーションが変数で使用されています。

Visual Basic でサポートされていないデータ型を持つタイプライブラリまたはオブジェクトライブラリで定義された変数を使用しようとしました。

## <a name="to-correct-this-error"></a>このエラーを解決するには

- Visual Basic によって認識される型の変数を使用します。

     または

- `FileGet` または `FileGetObject`の使用中にこのエラーが発生した場合は、使用しようとしているファイルが `FilePut` または `FilePutObject`で書き込まれていることを確認してください。

## <a name="see-also"></a>参照

- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
