---
author: admin
date: 2007-04-08T21:03:04-07:00
aliases:
- /2007/04/08/170
title: Как запустить отладчик при аварийном завершении приложения в Vista
slug: 170
tags:
- Отладка
---

По умолчанию служба "Problem reports and solutions" в Vista настроена так, что при аварийном завершении приложения у пользователя есть выбор из двух вариантов: посылать или не посылать отчет на сервер Microsoft. Это довольно логичный выбор в случае если за компьютером сидит "средний" пользователь, которого негоже пугать отладчиком. Однако отнимать у разработчика возможность загрузить любимый отладчик нехорошо. :-) Чтобы исправить ситуацию достаточно просто выключить Problem reporting.

<!--more-->Выберите Problem Reports and Solutions в Control Panel/System and Maintenance:

[![Problem reports and solutions.](/2007/04/problem_reports_and_solutions.thumbnail.png)](/2007/04/problem_reports_and_solutions.png)

Выберите Change settings в появившемся окне:

[![Settings.](/2007/04/problem_reports_and_solutions_settings.thumbnail.png)](/2007/04/problem_reports_and_solutions_settings.png)

Затем - Advanced settings:

[![Advanced settings.](/2007/04/problem_reports_and_solutions_advanced.thumbnail.png)](/2007/04/problem_reports_and_solutions_advanced.png)

И наконец выберите Off под "For my programs, problem reporting is": 

[![Problem reportings is off.](/2007/04/problem_reports_and_solutions_off.thumbnail.png)](/2007/04/problem_reports_and_solutions_off.png)

Теперь при аварийном завершении приложения система будет показывать вот такое окно:

![A debugger pops up on an application crash.](/2007/04/problem_reports_and_solutions_debug.png)

Готово.
