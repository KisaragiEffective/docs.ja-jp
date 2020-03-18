---
title: 数値演算関数の導出
ms.date: 07/20/2015
helpviewer_keywords:
- arithmetic operations, derived math functions
- cosecant function
- arcsecant function
- arccotangent function
- functions [Visual Basic], derived math functions
- inverse functions
- math functions, derived
- derived math functions
- cotangent function
- angles
- secant function
- trigonometric functions
- logarithms
- arccosecant function
- hyperbolic functions
- arcsine function
- degrees
- arccosine function
ms.assetid: 63e449d8-9444-44fb-8db1-6d9cf346e2aa
ms.openlocfilehash: 73cf56dd72f2baac0474d6f5c4e88228a1fe38cf
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349848"
---
# <a name="derived-math-functions-visual-basic"></a>数値演算関数の導出 (Visual Basic)
次の表は、<xref:System.Math?displayProperty=nameWithType> オブジェクトの組み込みの数学関数から派生できる非組み込みの数学関数を示しています。 組み込みの数学関数にアクセスするには、ファイルまたはプロジェクトに `Imports System.Math` を追加します。  
  
|関数|派生した同等のもの|  
|--------------|-------------------------|  
|正割 (秒 (x))|1 / Cos(x)|  
|余割 (Csc (x))|1/Sin (x)|  
|コタンジェント (Ctan (x))|1/Tan (x)|  
|逆サイン (アークサイン (x))|Atan (x/Sqrt (-x * x + 1))|  
|逆余弦 (Acos (x))|Atan (-x/Sqrt (-x * x + 1)) + 2 \* Atan (1)|  
|逆正割 (Asec (x))|2 * Atan (1) – Atan (Sign (x)/Sqrt (x \* x-1))|  
|逆余割 (Acsc (x))|Atan (Sign (x)/Sqrt (x * x – 1))|  
|逆コタンジェント (Acot (x))|2 * Atan (1)-Atan (x)|  
|ハイパーボリックサイン (Sinh (x))|(Exp (x) – Exp (-x))/2|  
|ハイパーボリックコサイン (Cosh (x))|(Exp (x) + Exp (-x))/2|  
|ハイパーボリックタンジェント (Tanh (x))|(Exp(x) – Exp(-x)) / (Exp(x) + Exp(-x))|  
|ハイパーボリックの正割 (x)|2 / (Exp(x) + Exp(-x))|  
|ハイパーボリック余割 (Csch (x))|2 / (Exp(x) – Exp(-x))|  
|ハイパーボリックコタンジェント (Coth (x))|(Exp(x) + Exp(-x)) / (Exp(x) – Exp(-x))|  
|逆双曲線正弦 (Asinh (x))|Log (x + Sqrt (x * x + 1))|  
|逆双曲線余弦 (Acosh (x))|Log (x + Sqrt (x * x-1))|  
|逆双曲線正接 (Atanh (x))|Log ((1 + x)/(1-x))/2|  
|逆双曲線正割 (AsecH (x))|Log ((Sqrt (-x * x + 1) + 1)/x)|  
|逆双曲線余割 (Acsch (x))|Log ((Sign (x) * Sqrt (x \* x + 1) + 1)/x|  
|逆双曲線コタンジェント (Acoth (x))|Log ((x + 1)/(x – 1))/2|  
  
## <a name="see-also"></a>参照

- [数値演算関数](../../../visual-basic/language-reference/functions/math-functions.md)
