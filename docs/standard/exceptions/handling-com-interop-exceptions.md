---
title: COM 相互運用の例外の処理
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- unmanaged code, exceptions
- exceptions, unmanaged code
- HRESULTs
- exceptions, COM interop
- COM interop, exceptions
ms.assetid: e6104aa8-8e5f-4069-b864-def85579c96c
ms.openlocfilehash: 17cd739ac40b43bdd4a93b83a4ab9d0d92400e2d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708933"
---
# <a name="handling-com-interop-exceptions"></a>COM 相互運用の例外の処理
マネージド コードとアンマネージド コードは、例外を処理するために一緒に操作できます。 マネージド コードでメソッドが例外をスローすると、共通言語ランタイムは、HRESULT を COM オブジェクトに渡すことができます。 アンマネージド コードでメソッドが失敗して、失敗を示す HRESULT を返した場合、ランタイムはマネージド コードでキャッチできる例外をスローします。  
  
 ランタイムは、HRESULT を COM 相互運用からの自動的にマップして、より詳細な例外を取得します。 たとえば、E_ACCESSDENIED は <xref:System.UnauthorizedAccessException> になり、E_OUTOFMEMORY は <xref:System.OutOfMemoryException> になるなどです。  
  
 HRESULT がカスタムの結果であるか、またはランタイムで不明な場合は、ランタイムがジェネリック <xref:System.Runtime.InteropServices.COMException> をクライアントに渡します。 **COMException** の **ErrorCode** プロパティには、HRESULT 値が含まれます。  
  
## <a name="working-with-ierrorinfo"></a>IErrorInfo の処理  
 エラーが COM からマネージド コードに渡された場合、ランタイムは例外オブジェクトにエラー情報を入れます。 IErrorInfo をサポートして HRESULT を返す COM オブジェクトは、この情報をマネージド コードの例外に提供します。 たとえば、ランタイムは、COM エラーからの説明を例外の <xref:System.Exception.Message%2A> プロパティにマップします。 HRESULT が追加のエラー情報を提供しない場合、ランタイムは例外のプロパティの多くに既定値を指定します。  
  
 アンマネージド コードでメソッドが失敗した場合、例外をマネージド コードのセグメントに渡すことができます。 「[HRESULT と例外](../../../docs/framework/interop/how-to-map-hresults-and-exceptions.md)」のトピックには、HRESULT がランタイム例外オブジェクトにマップする方法を示す表が含まれています。  

## <a name="see-also"></a>参照

- [例外](index.md)
