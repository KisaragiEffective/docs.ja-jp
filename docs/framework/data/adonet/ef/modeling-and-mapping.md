---
title: モデリングとマッピング
ms.date: 03/30/2017
ms.assetid: ec8a9515-3708-4cde-a688-4d8e6975f150
ms.openlocfilehash: e70411bb9c9797ee348aeef055c9af20ea092ae3
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77450597"
---
# <a name="modeling-and-mapping"></a>モデリングとマッピング
Entity Framework では、アプリケーションに最も適した方法で概念モデル、ストレージモデル、およびその2つの間のマッピングを定義できます。 Visual Studio の Entity Data Model ツールを使用すると、を作成できます。データベースまたはグラフィカルモデルからの[edmx ファイル](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cc982042(v=vs.100))。データベースまたはモデルが変更されたときに、そのファイルを更新します。  
  
 Entity Framework 4.1 以降では、Code First の開発を使用してモデルをプログラムで作成することもできます。 Code First の開発に対しては、2 つの異なるシナリオがあります。 どちらの場合でも、開発者は .NET Framework のクラス定義をコーディングしてモデルを定義し、データ注釈または Fluent API を使用してオプションで追加のマッピングまたは構成を指定します。  
  
 詳細については、「[モデルの作成](/ef/ef6/modeling/)」を参照してください。  
  
 .NET Framework に含まれている EDM ジェネレーターを使用することもできます。 EdmGen.exe は、既存のデータ ソースから .csdl ファイル、.ssdl ファイル、および .msl ファイルを生成します。 モデルとマッピングの内容を手動で作成することもできます。 詳細については、「 [EDM Generator (edmgen.exe)](edm-generator-edmgen-exe.md)」を参照してください。
