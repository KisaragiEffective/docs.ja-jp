---
title: -errorreport
ms.date: 08/14/2018
helpviewer_keywords:
- -errorreport compiler option [Visual Basic]
- /errorreport compiler option [Visual Basic]
- errorreport compiler option [Visual Basic]
ms.assetid: a7fe83a2-a6d8-460c-8dad-79a8f433f501
ms.openlocfilehash: a9741f7a8283f8603e02dae5abea151c6ee5d75e
ms.sourcegitcommit: 559259da2738a7b33a46c0130e51d336091c2097
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72775666"
---
# <a name="-errorreport"></a>-errorreport

Visual Basic コンパイラが内部コンパイラエラーを報告する方法を指定します。

## <a name="syntax"></a>構文

```console
-errorreport:{ prompt | queue | send | none }
```

## <a name="remarks"></a>Remarks

このオプションは、Microsoft の Visual Basic チームに Visual Basic 内部コンパイラエラー (ICE) を報告する便利な方法を提供します。 既定では、コンパイラは Microsoft に情報を送信しません。 ただし、内部コンパイラエラーが発生した場合は、このオプションを使用すると、エラーを Microsoft に報告できます。 この情報は、Microsoft のエンジニアが原因を特定するのに役立ちます。また、Visual Basic の次のリリースの向上に役立つ場合があります。

ユーザーがレポートを送信できるかどうかは、コンピューターとユーザーのポリシーのアクセス許可によって異なります。

次の表は、`-errorreport` オプションの効果をまとめたものです。

|オプション|動作|
|---|---|
|`prompt`|内部コンパイラエラーが発生した場合は、コンパイラによって収集された正確なデータを表示するためのダイアログボックスが表示されます。 エラー報告に機密情報があるかどうかを確認し、Microsoft に送信するかどうかを決定することができます。 送信する場合、コンピューターとユーザーのポリシー設定によって許可されている場合、コンパイラはデータを Microsoft に送信します。|
|`queue`|エラー レポートを待ち行列に入れます。 管理者特権でログインすると、最後にログインした後に発生したすべてのエラーを報告することができます (エラーの報告を3日ごとに複数回送信するように求めるメッセージは表示されません)。 これは、`-errorreport` オプションが指定されていない場合の既定の動作です。|
|`send`|内部コンパイラエラーが発生し、コンピューターとユーザーのポリシー設定で許可されている場合、コンパイラはデータを Microsoft に送信します。<br /><br /> オプション `-errorreport:send`、 [Windows エラー報告](/windows/desktop/wer/windows-error-reporting)システム設定によってレポートが有効になっている場合に、自動的にエラー情報を Microsoft に送信しようとします。 |
|`none`|内部コンパイラエラーが発生した場合、そのエラーは収集されず、マイクロソフトに送信されません。|

コンパイラは、エラー発生時にスタックを含むデータを送信します。通常は、ソースコードが含まれています。 [-Bugreport](../../../visual-basic/reference/command-line-compiler/bugreport.md)オプションと共に `-errorreport` を使用すると、ソースファイル全体が送信されます。

このオプションは、Microsoft のエンジニアがより簡単にエラーを再現できるようにするため、 [-bugreport](../../../visual-basic/reference/command-line-compiler/bugreport.md)オプションと共に使用することをお勧めします。

> [!NOTE]
> `-errorreport` オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。

## <a name="example"></a>例

次のコードは、`T2.vb` のコンパイルを試みます。コンパイラが内部コンパイラエラーを検出した場合は、Microsoft にエラーレポートを送信するように求めるメッセージが表示されます。

```console
vbc -errorreport:prompt t2.vb
```

## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [-bugreport](../../../visual-basic/reference/command-line-compiler/bugreport.md)
