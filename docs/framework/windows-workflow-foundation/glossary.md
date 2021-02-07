---
description: 詳細については、.NET Framework 4.5 の Windows Workflow Foundation 用語集を参照してください。
title: .NET Framework 4.5 の Windows Workflow Foundation 用語集
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Workflow Foundation [WF], glossary
- WF [WF], glossary
ms.assetid: ab682b2f-3779-45ca-b831-b7c03d7dbb3a
ms.openlocfilehash: 5339b3236a901e51a196aae7597aa2764bbd1c61
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99742265"
---
# <a name="windows-workflow-foundation-glossary-for-net-framework-45"></a>.NET Framework 4.5 の Windows Workflow Foundation 用語集

Windows Workflow Foundation のドキュメントで使用される用語については、次の表を参照してください。

## <a name="terms"></a>用語

|期間|定義|
|----------|----------------|
|activity|Windows Workflow Foundation のプログラム動作の単位です。 それぞれのアクティビティをまとめて、より複雑なアクティビティを構成できます。|
|アクティビティ操作|ワークフローおよびアクティビティの実行のコールバックを公開するために使用されるデータ構造です。|
|引数|アクティビティに出入りするデータ フローを定義します。 各引数には、指定された方向 (in、out、または in/out) があります。これらは、アクティビティの入力、出力、入出力パラメーターを表します。|
|ブックマーク (bookmark)|アクティビティの一時停止と再開まで待機することを可能にするポイントです。|
|補正|前に完了した作業の効果を取り消したり補正したりするように設計されたアクションのグループです。|
|相関|ワークフローまたはサービス インスタンスにメッセージをルーティングするためのメカニズムです。|
|expression|1 つまたは複数の引数を取得し、その引数の操作を実行し、単一の値を返すコンストラクターです。 式は、アクティビティが使用可能であれば、どこでも使用できます。|
|フローチャート|方向矢印と一緒にリンクされた記号でプログラム コンポーネントを表す既知のモデリング パラダイムです。  .NET Framework 4 では、Flowchart アクティビティを使用してワークフローをフローチャートとしてモデル化できます。|
|長時間のプロセス|すぐに制御が返されず、システムの再起動をまたぐ場合もあるプログラム実行の単位です。|
|永続化|メモリからアンロードしたり、システム障害から回復できるように、永続メディアにワークフローまたはサービスの状態を保存することです。|
|ステート マシン|イベント ドリブン状態遷移と一緒にリンクされた個々の状態でプログラム コンポーネントを表す既知のモデリング パラダイムです。  ワークフローは、StateMachine アクティビティを使用して、ステート マシンとしてモデル化できます。|
|サブスタンス|共通の識別子の下の関連ブックマークのグループを表し、特定のブックマーク再開が有効か、有効になる可能性があるかどうかがランタイムによって決定します。|
|型コンバーター|CLR 型は、CLR 型のインスタンスと他の型のインスタンスを変換する 1 つまたは複数の System.ComponentModel.TypeConverter 派生型に関連付けることができます。 型コンバーターは、System.componentmodel TypeConverterAttribute 属性を使用して CLR 型に関連付けられています。  TypeConverterAttribute は、CLR 型またはプロパティで直接指定できます。 プロパティで指定された型コンバーターは、プロパティの CLR 型で指定された型コンバーターよりも常に優先されます。|
|可変|保存して、後からアクセスする必要がある一部のデータのストレージを表します。|
|workflow|ホスト プロセスにより呼び出される単一のアクティビティまたはアクティビティのツリーです。|
|XAML|eXtensible Application Markup Language|
