---
title: プロパティのデザイン
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- member design guidelines, properties
- properties [.NET Framework], design guidelines
ms.assetid: 127cbc0c-cbed-48fd-9c89-7c5d4f98f163
ms.openlocfilehash: 8b6570b1b7c292729b78f2fe52f24f73319efe6c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743657"
---
# <a name="property-design"></a>プロパティのデザイン
プロパティは技術的にはメソッドとよく似ていますが、使用シナリオの観点からは非常に異なります。 スマートフィールドとして表示されます。 これらのメソッドには、フィールドの呼び出し構文とメソッドの柔軟性があります。

 呼び出し元がプロパティの値を変更できない場合は、get 専用プロパティを作成✔️ます。

 プロパティの型が変更可能な参照型である場合は、プロパティが get 専用であってもプロパティ値を変更できることに注意してください。

 ❌ には、set 専用のプロパティまたはプロパティを指定しないでください。 setter は、getter よりも広範なアクセシビリティを持っています。

 たとえば、public setter および protected getter を持つプロパティは使用しないでください。

 プロパティ getter を指定できない場合は、代わりにメソッドとして機能を実装します。 `Set` でメソッド名を開始し、プロパティとして指定した内容に従うことを検討してください。 たとえば、<xref:System.AppDomain> には、`CachePath`というセットのみのプロパティを使用するのではなく、`SetCachePath` と呼ばれるメソッドがあります。

 ✔️は、すべてのプロパティに対して適切な既定値を提供します。既定では、セキュリティホールや非常に非効率的なコードにはなりません。

 ✔️、オブジェクトが一時的に無効な状態になる場合でも、プロパティを任意の順序で設定できるようにします。

 同じオブジェクトの他のプロパティの値を指定すると、1つのプロパティの値が無効になる可能性がある点に対して、2つ以上のプロパティが相互に関連付けられることがよくあります。 このような場合は、相互に関連するプロパティがオブジェクトによって実際に使用されるまで、無効な状態に起因する例外を延期する必要があります。

 プロパティ setter が例外をスローした場合は、前の値を保持✔️。

 ❌ プロパティの getter から例外をスローしないようにします。

 プロパティの getter は単純な操作である必要があり、事前条件を指定することはできません。 Getter が例外をスローする場合は、メソッドとして再設計する必要があります。 この規則はインデクサーには適用されませんが、引数を検証した結果として例外が発生することに注意してください。

### <a name="indexed-property-design"></a>インデックス付きプロパティのデザイン
 インデックス付きプロパティは、パラメーターを持つことができる特殊なプロパティであり、配列のインデックス作成に似た特殊な構文を使用して呼び出すことができます。

 インデックス付きプロパティは、通常、インデクサーと呼ばれます。 インデクサーは、論理コレクション内の項目へのアクセスを提供する Api でのみ使用する必要があります。 たとえば、文字列は文字のコレクションであり、<xref:System.String?displayProperty=nameWithType> のインデクサーがその文字にアクセスするために追加されています。

 インデクサーを使用して、内部配列に格納されているデータにアクセスできるようにする✔️ます。

 ✔️項目のコレクションを表す型にインデクサーを指定することを検討してください。

 複数のパラメーターを使用してインデックス付きプロパティを使用する ❌ は避けてください。

 設計に複数のパラメーターが必要な場合は、プロパティが論理コレクションへのアクセサーを実際に表しているかどうかを再検討します。 そうでない場合は、代わりにメソッドを使用します。 `Get` または `Set`でメソッド名を開始することを検討してください。

 ❌、<xref:System.Int32?displayProperty=nameWithType>、<xref:System.Int64?displayProperty=nameWithType>、<xref:System.String?displayProperty=nameWithType>、<xref:System.Object?displayProperty=nameWithType>、または列挙型以外のパラメーターの型を持つインデクサーを使用しないようにします。

 設計に他の種類のパラメーターが必要な場合は、API が論理コレクションへのアクセサーを実際に表しているかどうかを厳密に再評価します。 そうでない場合は、メソッドを使用します。 `Get` または `Set`でメソッド名を開始することを検討してください。

 インデックス付きプロパティには名前 `Item` を使用✔️。明らかにわかりやすい名前が付いている場合 (`System.String`の <xref:System.String.Chars%2A> プロパティを参照している場合など)。

 でC#は、インデクサーは既定で Item という名前になります。 この名前をカスタマイズするには、<xref:System.Runtime.CompilerServices.IndexerNameAttribute> を使用できます。

 ❌ は、意味的に等価なインデクサーとメソッドの両方を提供しません。

 ❌、1つの型でオーバーロードされたインデクサーのファミリを複数指定することはできません。

 これは、 C#コンパイラによって強制されます。

 ❌ は、既定以外のインデックス付きプロパティを使用しないでください。

 これは、 C#コンパイラによって強制されます。

### <a name="property-change-notification-events"></a>プロパティ変更通知イベント
 場合によっては、プロパティ値の変更をユーザーに通知するイベントを提供すると便利です。 たとえば、`System.Windows.Forms.Control` は、`Text` プロパティの値が変更された後に `TextChanged` イベントを発生させます。

 高レベルの Api (通常はデザイナーコンポーネント) のプロパティ値が変更された場合に、変更通知イベントを発生させる✔️を検討してください。

 オブジェクトのプロパティが変更されたことをユーザーが知るための適切なシナリオがある場合、オブジェクトはプロパティの変更通知イベントを発生させる必要があります。

 ただし、基本型やコレクションなどの低レベルの Api に対して、このようなイベントを発生させるオーバーヘッドがあることはほとんどありません。 たとえば、<xref:System.Collections.Generic.List%601> は、新しい項目がリストに追加され、`Count` プロパティが変更されたときに、このようなイベントを発生させません。

 プロパティの値が外部の強制によって変更された場合に、変更通知イベントを発生させることを✔️します。

 プロパティ値が何らかの外部強制 (オブジェクトのメソッドを呼び出す以外の方法で) によって変更された場合、イベントの発生は、値が変更され、変更されたことを開発者に示します。 たとえば、テキストボックスコントロールの `Text` プロパティが適しています。 ユーザーが `TextBox`にテキストを入力すると、プロパティ値が自動的に変更されます。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [メンバーのデザインのガイドライン](../../../docs/standard/design-guidelines/member.md)
- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
