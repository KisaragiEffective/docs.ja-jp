---
title: 一般的な名前付け規則
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- names [.NET Framework], conflicts
- type names, conflicts
- language-specific type names
- names [.NET Framework], about naming guidelines
- names [.NET Framework], abbreviations
- abbreviation naming guidelines
- acronym naming guidelines
- Hungarian notation
- names [.NET Framework], type names
- names [.NET Framework], acronyms
ms.assetid: d3a77ea1-75d2-4969-a8c3-3e1e3e1aaedc
ms.openlocfilehash: f8576790a2be08c623807e236d156e0e7f93dd14
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741593"
---
# <a name="general-naming-conventions"></a>一般的な名前付け規則
ここでは、単語の選択に関連する一般的な名前付け規則、略語と頭字語の使用に関するガイドライン、および言語固有の名前の使用を回避するための推奨事項について説明します。

## <a name="word-choice"></a>単語の選択
 ✔️は、簡単に判読できる識別子名を選択します。

 たとえば、`HorizontalAlignment` という名前のプロパティは、`AlignmentHorizontal`よりも英語で読みやすくなります。

 ✔️簡潔にするために読みやすさを優先します。

 プロパティ名 `CanScrollHorizontally` は、`ScrollableX` (X 軸へのあいまいな参照) よりも優れています。

 ❌ アンダースコア、ハイフン、またはその他の英数字以外の文字を使用しないでください。

 ❌ は、ハンガリー表記法を使用しません。

 ❌、広く使用されているプログラミング言語のキーワードと競合する識別子を使用しないようにします。

 共通言語仕様（CLS）に準拠している言語は、ルール4に従って、その言語のキーワードを識別子名として使用する名前付けされた項目へのアクセスを許す機構を供給しなくてはなりません。 たとえば C# の場合、@ 記号をエスケープ メカニズムとして使用します。 しかしながら、エスケープ シーケンスを使う方法は、それを使わない方法よりもずっと難しいので、共通のキーワードを避けることをお勧めします。

## <a name="using-abbreviations-and-acronyms"></a>省略形と頭字語の使用
 ❌、識別子名の一部として省略形または短縮形を使用しないでください。

 たとえば、`GetWin`ではなく `GetWindow` を使用します。

 ❌ は、広く受け入れられていない頭字語や、必要な場合にのみ使用されます。

## <a name="avoiding-language-specific-names"></a>言語固有の名前の回避
 型名の言語固有のキーワードではなく、意味的に興味深い名前を使用する✔️ます。

 たとえば、`GetLength` は `GetInt`よりもわかりやすい名前です。

 通常は、言語固有の名前ではなく、汎用的な CLR 型名を使用します。これは、識別子が型以外のセマンティックの意味を持たない場合に、まれに発生します。✔️

 たとえば、<xref:System.Int64> に変換するメソッドは、`ToLong` ではなく `ToInt64`という名前にする必要があります ( C#<xref:System.Int64> は固有のエイリアス `long`の CLR 名であるため)。 次の表は、CLR 型名 (だけでなく C#、Visual Basic、および C++ の対応する型名) を使用していくつかの基本データ型を示します。

|C#|Visual Basic|C++|CLR|
|---------|------------------|-----------|---------|
|**sbyte**|**SByte**|**char**|**SByte**|
|**byte**|**Byte**|**unsigned char**|**Byte**|
|**short**|**Short**|**short**|**Int16**|
|**ushort**|**UInt16**|**unsigned short**|**UInt16**|
|**int**|**整数**|**int**|**Int32**|
|**uint**|**UInt32**|**unsigned int**|**UInt32**|
|**long**|**Long**|**__int64**|**Int64**|
|**ulong**|**UInt64**|**unsigned __int64**|**UInt64**|
|**float**|**Single**|**float**|**Single**|
|**double**|**Double**|**double**|**Double**|
|**bool**|**Boolean**|**bool**|**Boolean**|
|**char**|**Char**|**wchar_t**|**Char**|
|**string**|**String**|**String**|**String**|
|**object**|**Object**|**Object**|**Object**|

 型名を繰り返すのではなく、`value` や `item`などの共通名を使用する✔️、まれなケースでは、識別子にセマンティックの意味がなく、パラメーターの型が重要でない場合に、このような名前を使用します。

## <a name="naming-new-versions-of-existing-apis"></a>既存の Api の新しいバージョンの命名
 既存の API の新しいバージョンを作成するときに、古い API に似た名前を使用✔️ます。

 これにより、API の間の関係性を強調します。

 ✔️は、既存の API の新しいバージョンを示すために、プレフィックスではなくサフィックスを追加することをお勧めします。

 これは、ドキュメントを参照したり、IntelliSense を使用したりする際に発見しやすくなります。 ほとんどのブラウザーや IntelliSense は、アルファベット順に名前を並べるため、新しいバージョンの API は、古いバージョンの API の近くに編成されます。

 サフィックスまたはプレフィックスを追加するのではなく、新しいがわかりやすい識別子を使用する✔️ます。

 ✔️は、既存の API の新しいバージョンを示すために数字のサフィックスを使用します。特に、API の既存の名前が意味のある唯一の名前 (業界標準の場合) であり、意味のあるサフィックス (または名前の変更) を追加する場合は、適切な opti ではありません。代わっ.

 同じ API の以前のバージョンと区別するために、識別子に "Ex" (または同様の) サフィックスを使用しない ❌ ます。

 ✔️は、32ビット整数ではなく64ビット整数 (long 整数) で動作する Api のバージョンを導入するときに、"64" サフィックスを使用します。 既存の 32 ビット API が存在する場合にのみ、この方法を実行する必要があります。64 ビット バージョンのみの新しい API には使用しないでください。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [名前付けのガイドライン](../../../docs/standard/design-guidelines/naming-guidelines.md)
