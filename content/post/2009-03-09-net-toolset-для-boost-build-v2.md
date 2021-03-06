---
author: admin
date: 2009-03-09T13:22:01-07:00
aliases:
- /2009/03/09/469
title: .NET toolset для Boost Build V2
slug: 469
tags:
- Boost.Build
---

Специально для любителей [Boost.Build V2](http://www.boost.org/doc/tools/build/index.html) – написал [toolset](/2009/03/dotnet.jam), добавляющий поддержку C# и VB.NET компиляторов из .NET Framework. Toolset автоматически распознает все установленные версии .NET Framework: 

```no-highlight
using dotnet : all ;
```

<!--more-->

Исполняемые файлы и библиотеки собирается как обычно с помощью правил “exe” и “lib”. Ссылки на системные библиотеки указываются через <find-shared-library>; путь к ним – с помощью <library-path>:

```no-highlight
lib carrots
    :
        carrots.cs
    ;

windir = [ modules.peek : windir ] ;

exe rabbit
    :
        rabbit.cs
        carrots
    :
        <library-path>"$(windir)/Microsoft.NET/Framework/v2.0.50727"
        <find-shared-library>System.dll
        <find-shared-library>System.Data.dll
    ;
```

Можно указать целевую платформу с помощью <architecture> и <address-model>. По умолчанию – anycpu.
