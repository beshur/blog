---
author: admin
date: 2007-02-21T01:24:25-07:00
aliases:
- /2007/02/21/151
title: Рецензирование кода (code review)
slug: 151
tags:
- Инструменты
- Программирование
---

Рецензирование кода (перевод подсмотрел у [Лебедева](http://alexlebedev.com/blog/walking-on-the-rake-2/)) – это на мой взгляд одна из полезнейших и при этом наиболее легко внедряемых практик разработки надёжного кода. Основная идея рецензирования заключается в систематической (пере)проверке кода с целью найти ошибки, допущенные при его написании. И поскольку рецензирование кода относится к ранним этапам разработки, найденные ошибки «ценнее», чем, скажем, ошибки, найденные при формальном тестировании.
Я не буду останавливаться на подробном описании процедуры рецензирования. В Интернете можно найти массу материалов по теме. Вот, например, страница из [Википедии](http://en.wikipedia.org/wiki/Code_review). Я же просто хочу поделиться своими наблюдениями.

<!--more-->Итак, рецензирование позволяет находить ошибки раньше. Однако помимо собственно нахождения ошибок, рецензирование способствует улучшению общего качества кода: полезные практики, применяемые одним разработчиком, перенимаются другими членами команды. Далее, разработчики больше узнают о коде (и логике, реализуемой этим кодом) из других частей проекта, которые, в противном случае, они бы вообще не увидели. В результате, переход из одной части проекта в другую проходит менее болезненно, что опять же положительно влияет на производительность работы и качество кода. 

Ещё одно положительное качество – внедрение рецензирования открывает дорогу другим техникам разработки надёжного кода. Скажем, написание юнит-тестов традиционно откладывается «на потом» и в конце концов тест так и остаётся ненаписанным. Рецензент же всегда может попенять автора кода за отсутствие юнит-тестов.

Не стоит впрочем думать, что рецензирование может решить все проблемы с качеством кода. По большому счёту, оно эффективно только вместе с другими техниками: использование юнит-тестов, ежедневная автоматическая сборка проекта, написание тестов до написания кода, рефакторинг и т.д.

Результативность рецензирования зависит от характера изменений в коде. Просмотр нового кода, как правило, даёт худший результат, чем просмотр исправления ошибки в старом коде. Точно также, чем больше объём кода рецензируется за раз, тем менее эффективно рецензирование. В результате, иногда имеет смысл вместо просмотра кода использовать другие методы, например написать побольше юнит-тестов для нового кода. 

Следует отметить, что эффективность рецензирования в значительной степени зависит от того, насколько удобно организован процесс. Например, я сталкивался с двумя вариантами. В первом случае, автор кода и рецензент вместе садились за монитор и просматривали изменения. При этом автор комментировал ход своей мысли, а рецензент внимал. В ещё более худшем варианте, для просмотра созывался митинг из нескольких человек. С моей точки зрения эффективность этих процедур стремится к нулю. Мало того, что каждый такой митинг отнимал много времени, так качество комментариев было весьма посредственным. Хотя, возможно, у кого-то именно такой вариант работает лучше всего.

Во втором случае, автор посылал diff изменений рецензенту, который посылал свои комментарии, просмотрев код в удобное ему время. В этом случае рецензент мог полностью сосредоточится на логике кода. Результативность в этом случае была гораздо выше. Редкий код оставался без комментариев.

Организовывая рецензирование, позаботьтесь об удобных инструментах, которыми будут пользоваться авторы и рецензенты. Хорошая визуальная программа просмотра diff-ов, позволяющая видеть изменения на фоне оригинального кода, - необходимейший инструмент в этом деле. В идеале, используемый инструментарий должен интегрироваться с системой контроля версий и bug tracking системой, но это совсем не обязательно.
