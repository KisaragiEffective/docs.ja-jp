---
title: -delaysign
ms.date: 03/10/2018
helpviewer_keywords:
- delaysign compiler option [Visual Basic]
- -delaysign compiler option [Visual Basic]
- -delaysign compiler option [Visual Basic]
ms.assetid: c76e61a4-1884-4252-9fb2-377f99caa690
ms.openlocfilehash: 3ee94df096b756be544964cfbbd405355e3f728f
ms.sourcegitcommit: 1f12db2d852d05bed8c53845f0b5a57a762979c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "72581266"
---
# <a name="-delaysign"></a>-delaysign

アセンブリに完全に署名するか、部分的に署名するかを指定します。

## <a name="syntax"></a>構文

```console
-delaysign[+ | -]
```

## <a name="arguments"></a>引数

`+` &#124; `-`  
省略可能です。 完全署名されたアセンブリを作成する場合は、`-delaysign-` を使用します。 公開キーをアセンブリに配置し、署名されたハッシュ用の領域を予約する場合は、`-delaysign+` を使用します。 既定値は、 `-delaysign-`です。

## <a name="remarks"></a>Remarks

`-delaysign` オプションは、 [-keyfile](../../../visual-basic/reference/command-line-compiler/keyfile.md) または [-keycontainer](../../../visual-basic/reference/command-line-compiler/keycontainer.md) と共に使用しなければ無効になります。

アセンブリに完全に署名するように指定すると、コンパイラはマニフェスト (アセンブリ メタデータ) を含むファイルをハッシュし、秘密キーでそのハッシュに署名します。 結果として得られるデジタル署名は、マニフェストを含むファイルに格納されます。 アセンブリが遅延署名されている場合、コンパイラは署名を計算して保存しませんが、後で署名を追加できるように、ファイルに領域を確保します。

たとえば、`-delaysign+` を使用すると、組織の開発者は、テスト担当者がグローバルアセンブリキャッシュに登録して使用できるアセンブリの署名されていないテストバージョンを配布できます。 アセンブリの作業が完了すると、組織の秘密キーを担当するユーザーがアセンブリに完全に署名できるようになります。 この compartmentalization は、すべての開発者がアセンブリを操作できるようにすると同時に、組織の秘密キーを漏えいから保護します。

アセンブリに署名する方法の詳細については、「[厳密な名前付きアセンブリの作成と使用](../../../standard/assembly/create-use-strong-named.md)」を参照してください。

### <a name="to-set--delaysign-in-the-visual-studio-integrated-development-environment"></a>Visual Studio 統合開発環境で-delaysign を設定するには

1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。

2. **[署名]** タブをクリックします。

3. **[遅延署名のみ]** ボックスに値を設定します。

## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-keyfile](../../../visual-basic/reference/command-line-compiler/keyfile.md)
- [-keycontainer](../../../visual-basic/reference/command-line-compiler/keycontainer.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
