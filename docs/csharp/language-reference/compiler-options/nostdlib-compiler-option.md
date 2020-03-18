---
title: -nostdlib (C# コンパイラ オプション)
ms.date: 12/20/2019
f1_keywords:
- /nostdlib
helpviewer_keywords:
- nostdlib compiler option [C#]
- -nostdlib compiler option [C#]
- /nostdlib compiler option [C#]
ms.assetid: ec197989-fa49-4725-a455-e06b551eb65f
ms.openlocfilehash: ad8a2b5fc87dd7beee86d96331cf3961315be533
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75345076"
---
# <a name="-nostdlib-c-compiler-options"></a>-nostdlib (C# コンパイラ オプション)

**-nostdlib** は、System 名前空間の全体を定義する mscorlib.dll がインポートされないようにします。

## <a name="syntax"></a>構文

```console
-nostdlib[+ | -]
```

## <a name="remarks"></a>解説

独自の System 名前空間およびオブジェクトを定義または作成する場合は、このオプションを使用します。

**/nostdlib** を指定しない場合、( **-nostdlib-** を指定したことと同じで) mscorlib.dll がプログラムにインポートされます。 **-nostdlib** を指定することは、 **-nostdlib+** を指定することと同じです。

### <a name="to-set-this-compiler-option-in-visual-studio"></a>このコンパイラ オプションを Visual Studio で使用するには

> [!NOTE]
> 次の手順は、Visual Studio 2015 (および以前のバージョン) のみに適用されます。 **Do not reference mscorlib.dll** ビルド プロパティは、新しいバージョンの Visual Studio には存在しません。

1. プロジェクトの **[プロパティ]** ページを開きます。

2. **[ビルド]** プロパティ ページをクリックします。

3. **[詳細設定]** ボタンをクリックします。

4. **[mscorlib.dll を参照しない]** プロパティを変更します。

### <a name="to-set-this-compiler-option-programmatically"></a>このコンパイラ オプションをコードから設定するには

このコンパイラ オプションをプログラムで設定する方法については、「 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.NoStdLib%2A>」をご覧ください。

## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
