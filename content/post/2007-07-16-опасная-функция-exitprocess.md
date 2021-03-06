---
author: admin
date: 2007-07-15T22:56:34-07:00
aliases:
- /2007/07/15/210
title: Опасная функция ExitProcess
slug: 210
tags:
- Программирование
- Windows
---

Как вы думаете, какая их двух функций опаснее: [ExitProcess](http://msdn2.microsoft.com/en-us/library/ms682658.aspx) или [TerminateProcess](http://msdn2.microsoft.com/en-us/library/ms686714.aspx)? Ответ, конечно, зависит от определения того, что считать более безопасным. Однако если задать этот вопрос нескольким людям, большинство автоматически укажет на TerminateProcess. Почему? Потому что TerminateProcess в отличие от ExitProcess не делает попыток освободить занятые ресурсы. К примеру, программа не сможет записать несохранённые данные на диск, тем самым нарушив их целостность.

<!--more-->В реальности функция ExitProcess ни чем не безопаснее, чем TerminateProcess, а может и ещё хуже, так как внушает чувство ложной безопасности. Рассмотрим алгоритм работы ExitProcess. Помимо закрытия всех открытых описателей (handles) объектов и установки состояния объектов завершаемого процесса и принадлежащих ему потоков в состояние «signaled», ExitProcess делает две важные вещи:

  * Завершает все потоки процесса, кроме потока, вызвавшего ExitProcess;

  * Оповещает все загруженные DLL, вызывая DllMain c кодом DLL_PROCESS_DETACH.

В случае если один из потоков выполнял код, защищённый критической секцией, в момент своего завершения, то критическая секция может остаться захваченной навсегда. Если один из обработчиков DLL_PROCESS_DETACH попытается захватить эту секцию, то он будет заблокирован на ней навсегда. (Это пример из [статьи в MSDN](http://msdn2.microsoft.com/en-us/library/ms682658.aspx), посвященной ExitProcess).

Подобная ситуация возникает чаще, чем может показаться. Фактически, любые глобальные данные (всяческие списки, таблицы и т.п.), используемые из нескольких потоков, защищаются критическими секциями или чем-то подобным. Даже если вы уверены, что ваше приложение не использует критические секции во время завершения программы, остаются системные функции, которые также могут быть источником этой проблемы.

И наконец в общем случае вы не можете быть уверены какие DLL загружены в ваш процесс, так как:

  1. Набор библиотек может отличаться на разных версиях операционной системы;

  2. Набор библиотек может отличаться в зависимости он версий сторонних библиотек, которые использует ваше приложение;

  3. Другие приложения могут внедрять свой код в ваш процесс, например с помощью [SetWindowsHookEx](http://msdn2.microsoft.com/en-us/library/ms644990.aspx) или [CreateRemoteThread](http://msdn2.microsoft.com/en-us/library/ms682437.aspx). В этом случае, кстати, вероятность того, что внедряемая DLL синхронизирует доступ к глобальным данным с помощью критических секций стремится к 100%.

Т.е. это означает, что вызывая ExitProcess, вы выполняете заранее неизвестный код, обладающий неизвестным поведением. 

Единственный способ сделать вызов ExitProcess безопасным – подготовиться к вызову этой функции должным образом: корректно завершить все потоки и вызвать ExitProcess из единственного оставшегося потока. Если в процессе подготовки что-то пошло не так, то имеет смысл вызвать TerminateProcess вместо ExitProcess, так как хуже все равно не будет – состояние программы уже не определено. Следует также помнить, что CRT функции exit, _exit и прочие, а также выход из функции main в конце концов приводят в вызову ExitProcess. Т.е. они подвержены тем же самым проблемам, что и ExitProcess. 
