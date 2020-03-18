---
title: 大文字の使用規則
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- camel-case names [.NET Framework]
- class library design guidelines [.NET Framework], capitalization
- Pascal-case names [.NET Framework]
- case sensitivity, capitalization conventions
- names [.NET Framework], capitalization
ms.assetid: 4c4ea526-9203-486f-b72d-29d61c5b3c6d
ms.openlocfilehash: 8af4a15e1e5b34c38b14c6b547cf44801bbf13e6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741757"
---
# <a name="capitalization-conventions"></a>大文字の使用規則
この章のガイドラインでは、一貫して適用されている場合に、型、メンバー、およびパラメーターの識別子を読みやすくするための簡単な方法を説明します。

## <a name="capitalization-rules-for-identifiers"></a>識別子の大文字小文字の規則
 識別子内の単語を区別するには、識別子内の各単語の最初の文字を大文字にします。 語句を区別するためにアンダースコアを使用したり、識別子内の任意の場所で単語を区別したりしないでください。 識別子の使用方法によっては、次の2つの適切な方法で識別子を大文字にすることができます。

- パスワードの大文字と小文字の区別

- キャメル

 次の例に示すように、パラメーター名を除くすべての識別子に使用される、文字表記規則は、各単語の最初の文字 (長さが2文字を超える頭字語を含む) を大文字にします。

 `PropertyDescriptor`
 `HtmlTag`

 次の識別子に示されているように、2文字の頭字語では、両方の文字が大文字になります。

 `IOStream`

 次の例に示すように、キャメル規則は、パラメーター名に対してのみ使用され、最初の単語を除く各単語の最初の文字を大文字にします。 この例に示すように、camel 形式の識別子を開始する2文字の頭字語は両方とも小文字です。

 `propertyDescriptor`
 `ioStream`
 `htmlTag`

 複数の単語で構成されるすべてのパブリックメンバー、型、名前空間の名前に対しては、文字の大文字と小文字の区別を使用✔️ます。

 パラメーター名にはキャメルを使用します。✔️

 次の表では、さまざまな種類の識別子の大文字と小文字の規則について説明します。

|[Identifier]|大文字小文字の区別|例|
|----------------|------------|-------------|
|名前空間|Pascal|`namespace System.Security { ... }`|
|種類|Pascal|`public class StreamReader { ... }`|
|インターフェイス|Pascal|`public interface IEnumerable { ... }`|
|方法|Pascal|`public class Object {` <br />  `public virtual string ToString();` <br /> `}`|
|プロパティ|Pascal|`public class String {` <br />  `public int Length { get; }` <br /> `}`|
|Event|Pascal|`public class Process {` <br />  `public event EventHandler Exited;` <br /> `}`|
|フィールド|Pascal|`public class MessageQueue {` <br />  `public static readonly TimeSpan` <br /> `InfiniteTimeout;` <br /> `}` <br /> `public struct UInt32 {` <br />  `public const Min = 0;` <br /> `}`|
|列挙値|Pascal|`public enum FileMode {` <br />  `Append,` <br />  `...` <br /> `}`|
|パラメーター|Camel|`public class Convert {` <br />  `public static int ToInt32(string value);` <br /> `}`|

## <a name="capitalizing-compound-words-and-common-terms"></a>複合語と一般的な用語を大文字にする
 ほとんどの複合用語は、大文字と小文字を区別するために1つの単語として扱われます。

 ❌ は、いわゆる終了形式の複合単語内の各単語を大文字にしないでください。

 これらは、エンドポイントなどの1つの単語として記述された複合ワードです。 大文字と小文字のガイドラインのために、閉じた複合ワードを1つの単語として扱います。 現在のディクショナリを使用して、複合単語が閉じた形式で記述されているかどうかを判断します。

|Pascal|Camel|Not|
|------------|-----------|---------|
|`BitFlag`|`bitFlag`|`Bitflag`|
|`Callback`|`callback`|`CallBack`|
|`Canceled`|`canceled`|`Cancelled`|
|`DoNot`|`doNot`|`Don't`|
|`Email`|`email`|`EMail`|
|`Endpoint`|`endpoint`|`EndPoint`|
|`FileName`|`fileName`|`Filename`|
|`Gridline`|`gridline`|`GridLine`|
|`Hashtable`|`hashtable`|`HashTable`|
|`Id`|`id`|`ID`|
|`Indexes`|`indexes`|`Indices`|
|`LogOff`|`logOff`|`LogOut`|
|`LogOn`|`logOn`|`LogIn`|
|`Metadata`|`metadata`|`MetaData, metaData`|
|`Multipanel`|`multipanel`|`MultiPanel`|
|`Multiview`|`multiview`|`MultiView`|
|`Namespace`|`namespace`|`NameSpace`|
|`Ok`|`ok`|`OK`|
|`Pi`|`pi`|`PI`|
|`Placeholder`|`placeholder`|`PlaceHolder`|
|`SignIn`|`signIn`|`SignOn`|
|`SignOut`|`signOut`|`SignOff`|
|`UserName`|`userName`|`Username`|
|`WhiteSpace`|`whiteSpace`|`Whitespace`|
|`Writable`|`writable`|`Writeable`|

## <a name="case-sensitivity"></a>大文字小文字の区別
 CLR で実行できる言語は、大文字と小文字の区別をサポートするためには必要ありません。 お使いの言語でサポートされている場合でも、フレームワークにアクセスする可能性のある他の言語はありません。 したがって、外部からアクセスできるすべての Api では、同じコンテキスト内の2つの名前を区別するために大文字と小文字だけを使用することはできません。

 ❌ では、すべてのプログラミング言語で大文字と小文字が区別されるとは限りません。 しかし、そうではありません。 名前は大文字と小文字のみで異なることはできません。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [名前付けのガイドライン](../../../docs/standard/design-guidelines/naming-guidelines.md)
