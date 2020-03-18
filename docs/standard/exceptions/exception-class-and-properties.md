---
title: Exception クラスとプロパティ
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- exceptions, Exception class
- Exception class
ms.assetid: e2e1f8c4-e7b4-467d-9a66-13c90861221d
ms.openlocfilehash: df05150a5bdd5d24766be252f5cec9a436720d8c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708946"
---
# <a name="exception-class-and-properties"></a>Exception クラスとプロパティ

<xref:System.Exception> クラスは、例外の継承元となる基底クラスです。 たとえば、<xref:System.InvalidCastException> クラスの階層は次のようになります。

<xref:System.Object>\
&nbsp;&nbsp;<xref:System.Exception>\
&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.SystemException>\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.InvalidCastException>

<xref:System.Exception> クラスには、簡単に例外を理解することに役立つ次のプロパティがあります。

| プロパティ名 | [説明] |
| ------------- | ----------- |
| <xref:System.Exception.Data> | キーと値のペアの任意のデータを保持する <xref:System.Collections.IDictionary> です。 |
| <xref:System.Exception.HelpLink> | 例外の原因に関する詳細情報を提供するヘルプ ファイルには、URL (または URN) を保持できます。 |
| <xref:System.Exception.InnerException> | このプロパティを使用すると、例外処理中に一連の例外を作成して保持することができます。 既にキャッチされた例外を含む新しい例外を作成するのにも使用できます。 <xref:System.Exception.InnerException> プロパティの 2 つ目の例外によって、元の例外をキャプチャできます。これにより、2 つ目の例外を処理するコードが追加の情報を調べることができます。 たとえば、形式が正しくない引数を受け取るメソッドがあるとします。  コードは、引数の読み取りを試みますが、例外がスローされます。 メソッドは、例外をキャッチし、<xref:System.FormatException> をスローします。 例外がスローされた原因を判断するための呼び出し元の機能を向上させるには、ヘルパー ルーチンによってスローされた例外をキャッチし、発生したエラーの内容を示す例外をスローするメソッドが望ましい場合があります。 内部例外の参照を元の例外に設定できる、新しいより意味のある例外を作成できます。 この意味のある例外は、呼び出し元にスローすることができます。 この機能により、最初にスローされた例外で終了する一連のリンクされた例外を作成することができます。 |
| <xref:System.Exception.Message> | 例外の原因に関する詳細を提供します。
| <xref:System.Exception.Source> | エラーの原因となるアプリケーションまたはオブジェクトの名前を取得または設定します。 |
| <xref:System.Exception.StackTrace>| エラーが発生した場所を判断するために使用できるスタック トレースが含まれています。 デバッグ情報が使用できる場合には、スタック トレースにソース ファイル名とプログラム行番号が記述されます。 |

<xref:System.Exception> から継承するほとんどのクラスは、追加メンバーを実装したり、追加の機能を提供することはありません。これらは、<xref:System.Exception> から継承するだけです。 そのため、例外の最も重要な情報は、例外クラスの階層、例外の名前と、例外に含まれる情報で見つけることができます。

<xref:System.Exception> から派生したオブジェクトのみをスローし、キャッチすることをお勧めしますが、<xref:System.Object> クラスから派生したオブジェクトはすべて例外としてスローできます。 <xref:System.Exception> から派生していないオブジェクトのスローとキャッチは、すべての言語ではサポートされていないことに注意してください。
  
## <a name="see-also"></a>参照

- [例外](index.md)
