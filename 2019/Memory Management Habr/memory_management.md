Привет, Хабр! В этой статье я попытаюсь рассказать, что из себя представляет менеджмент памяти в игровых движках и в приложения в целом. Это не исчерпывающее руководство или мануал, а просто некоторый обзор проблем и подходов для их решения.

![](http://nerohelp.info/uploads/posts/2015-11/1448569243_2.jpg)
Невозможно все держать в памяти. Но если не успел подгрузить, то получишь мыло

<habracut/>

# С места в карьер

Так уж повелось, что крупные ААА игровые проекты разрабатываются преимущественно на движках, написанных с помощью C++. Одна из особенность (многочисленных) этого языка заключается в необходимости ручного управления памятью. Java/C# и тд. могут похвастать сборкой мусора (GC) - возможностью создавать объекты и при этом не освобождать использованную память руками. Это в некоторой степени упрощает и ускоряет разработку, но добавляет неприятную особенность. Периодически срабатывающий сборщик мусора, который может убить весь soft-real time и добавить неприятные зависания в игру.

Да, в проектах типа minecraft и тд. работа GC может быть и незаметна, так как они в целом не требовательны к ресурсам вычислителя, однако такие игры как Red Dead Redemption 2, God of War, GTA 5 работают "едва ли" не на пике производительности и требуют не только много ресурсов, но и так же грамотное их распределение. 

# Проблемы с памятью

Существует несколько проблем, возникающих при работе с ОЗУ компьютера. Все они, так или иначе, вызваны не только особенностями ОС и программного слоя, но и архитектурой железа, на котором все это добро работает. 

* **Количество памяти**. К сожалению, память ограничена физически. На PlayStation 4 это 8 GiB GDDR5, 3.5 GiB из которых операционная система резервирует для своих нужд. Виртуальная память и подкачка страниц особо не помогут, так как swapping страниц на диск - операция весьма медленная (в рамках фиксированных 30 кадров в секунду). 

* **Фрагментация**. Неприятный эффект, который появляется в процессе множественных аллокации кусочков памяти различного размера. 

* **Кэш процессора**. 

* **Malloc/free**. Операции по выделению/освобождению не происходят мгновенно. На современных ОС, если мы говорим про Windows/Linux/MacOS они реализованы хорошо и работаю быстро. Однако, потенциально это весьма трудоемкие операции. Кроме блокировки доступа в конкурентной среде, они требуют отображение страниц, поиск подходящего кусочка памяти (First Fit, Best fit, и тд.) и склеивание использованных при освобождений. 
