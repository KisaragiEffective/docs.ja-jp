---
title: DateTime XAML 構文
ms.date: 03/30/2017
helpviewer_keywords:
- DateTime XAML syntax [WPF], strings for
- DateTime XAML syntax [WPF], where used
- short date format [WPF], DateTime
- DateTime XAML syntax [WPF]
- DateTime XAML text [WPF]
- DateTime XAML syntax [WPF], format strings for
ms.assetid: 5901710a-609b-40c8-9d65-f0016cd9090b
ms.openlocfilehash: c9fb030e6f819b1ca463199a76acd32cb1865f33
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458937"
---
# <a name="datetime-xaml-syntax"></a>DateTime XAML 構文
<xref:System.Windows.Controls.Calendar> や <xref:System.Windows.Controls.DatePicker>などの一部のコントロールには、<xref:System.DateTime> 型を使用するプロパティがあります。 これらのコントロールに対する日付または時刻の初期値は、分離コードで実行時に指定するのが一般的です。ただし、日付または時刻の初期値を XAML で指定することもできます。 WPF XAML パーサーは、組み込みの XAML テキスト構文を使用して、<xref:System.DateTime> 値の解析を処理します。 このトピックでは、<xref:System.DateTime> XAML テキスト構文の詳細について説明します。  

<a name="where_datetime_xaml_syntax_is_used"></a>   
## <a name="when-to-use-datetime-xaml-syntax"></a>DateTime XAML 構文が使用される状況  
 日付は必ずしも XAML で設定する必要はありません。また、XAML で設定することが適切とは言えない場合もあります。 たとえば、<xref:System.DateTime.Now%2A?displayProperty=nameWithType> プロパティを使用して実行時に日付を初期化することや、ユーザー入力に基づいてコードビハインドで暦の日付の調整をすべて行うことができます。 ただし、日付を <xref:System.Windows.Controls.Calendar> にハードコーディングし、コントロールテンプレートで <xref:System.Windows.Controls.DatePicker> することが必要になる場合があります。 これらのシナリオでは、<xref:System.DateTime> XAML 構文を使用する必要があります。  
  
### <a name="datetime-xaml-syntax-is-a-native-behavior"></a>DateTime XAML 構文はネイティブの動作  
 <xref:System.DateTime> は、CLR の基本クラスライブラリで定義されているクラスです。 基本クラスライブラリが CLR の残りの部分とどのように関連しているかによって、<xref:System.ComponentModel.TypeConverterAttribute> をクラスに適用し、型コンバーターを使用して XAML から文字列を処理し、実行時のオブジェクトモデルの <xref:System.DateTime> に変換することはできません。 変換動作を提供する `DateTimeConverter` クラスは存在しません。このトピックで説明する変換動作は、WPF XAML パーサーのネイティブな動作です。  
  
<a name="format_strings_for_datetime_xaml_syntax"></a>   
## <a name="format-strings-for-datetime-xaml-syntax"></a>DateTime XAML 構文の書式指定文字列  
 書式指定文字列を使用して <xref:System.DateTime> の形式を指定できます。 書式指定文字列は、値を作成するときに使用できるテキスト構文を形式化します。 既存の WPF コントロールの <xref:System.DateTime> 値は、通常、時刻コンポーネントではなく <xref:System.DateTime> の日付コンポーネントのみを使用します。  
  
 XAML で <xref:System.DateTime> を指定する場合は、任意の書式指定文字列を同義に使用できます。  
  
 また、このトピックで特に示されていない形式および書式指定文字列も使用できます。 技術的には、WPF XAML パーサーによって指定および解析された <xref:System.DateTime> 値の XAML は、<xref:System.DateTime.Parse%2A?displayProperty=nameWithType>の内部呼び出しを使用するため、<xref:System.DateTime.Parse%2A?displayProperty=nameWithType> で受け入れられた任意の文字列を XAML 入力に使用できます。 詳細については、「<xref:System.DateTime.Parse%2A?displayProperty=nameWithType>」を参照してください。  
  
> [!IMPORTANT]
> DateTime XAML 構文では、ネイティブ変換の <xref:System.Globalization.CultureInfo> として常に `en-us` が使用されます。 XAML の属性レベルの型変換は、そのコンテキストなしで動作するため、XAML の <xref:System.Windows.FrameworkElement.Language%2A> 値または `xml:lang` 値の影響を受けません。 日や月を表示する順序などにはカルチャによる違いがあるため、ここで示した書式指定文字列は補完しないでください。 ここで示した書式指定文字列は、その他のカルチャ設定に関係なく、XAML を解析する場合に適しています。  
  
 以下のセクションでは、一般的な <xref:System.DateTime> 書式指定文字列のいくつかについて説明します。  
  
### <a name="short-date-pattern-d"></a>短い形式の日付パターン ("d")  
 XAML での <xref:System.DateTime> の短い日付形式を次に示します。  
  
 `M/d/YYYY`  
  
 これは、WPF コントロールでの一般的な使用法で、必要なすべての情報を指定するための最も簡単な形式です。タイム ゾーン オフセットと時刻要素を誤って指定した場合でも影響を受けないため、他の形式ではなく、この形式の使用をお勧めします。  
  
 たとえば、2010 年 6 月 1 日を指定するには、次の文字列を使用します。  
  
 `3/1/2010`  
  
 詳細については、「<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType>」を参照してください。  
  
### <a name="sortable-datetime-pattern-s"></a>並べ替えできる DateTime のパターン ("s")  
 XAML での並べ替え可能な <xref:System.DateTime> パターンを次に示します。  
  
 `yyyy'-'MM'-'dd'T'HH':'mm':'ss`  
  
 たとえば、2010 年 6 月 1 日を指定するには、次の文字列を使用します (時刻要素はすべて 0 として入力されます)。  
  
 `2010-06-01T000:00:00`  
  
### <a name="rfc1123-pattern-r"></a>RFC1123 パターン ("r")  
 RFC1123 パターンの利点は、カルチャに依存しないために、RFC1123 パターンが使用されている他の日付ジェネレーターから入力された文字列を使用できることです。 XAML の RFC1123 <xref:System.DateTime> パターンを次に示します。  
  
 `ddd, dd MMM yyyy HH':'mm':'ss 'UTC'`  
  
 たとえば、2010 年 6 月 1 日を指定するには、次の文字列を使用します (時刻要素はすべて 0 として入力されます)。  
  
 `Mon, 01 Jun 2010 00:00:00 UTC`  
  
### <a name="other-formats-and-patterns"></a>その他の形式とパターン  
 前述のように、XAML の <xref:System.DateTime> は、<xref:System.DateTime.Parse%2A?displayProperty=nameWithType>の入力として許容される任意の文字列として指定できます。 これには、他の形式化された形式 (<xref:System.Globalization.DateTimeFormatInfo.UniversalSortableDateTimePattern%2A>など) と、特定の <xref:System.Globalization.DateTimeFormatInfo> フォームとして形式化されていない形式も含まれます。 たとえば、フォーム `YYYY/mm/dd` は <xref:System.DateTime.Parse%2A?displayProperty=nameWithType>の入力として許容されます。 このトピックでは、使用可能な形式の一部を説明します。標準的な使用手順としては、短い形式の日付パターンをお勧めします。  
  
## <a name="see-also"></a>関連項目

- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
