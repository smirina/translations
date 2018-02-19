---
published: false
layout: post
tag:
original: http://mrale.ph/blog/2017/01/08/the-fear-of-dart-mirrors.html
title: Everything easy is hard again
author: "Vyacheslav Egorov"
translator: "Ирина Смышляева"

---

Все опять сложно.

С этим докладом я выступал на [Mirror Conf](https://mirrorconf.com/) в Португалии 12 октября 2017 года и [Awwwards Conference](https://conference.awwwards.com/berlin/) в Берлине 9 февраля 2018 года.

Прошлым летом на веб конференции я читал лекцию, а после которой случился очень интересный разговор со студенткой, изучающей цифровой дизайн. Забавно было сравнить, в какой точке профессионального пути мы находимся. У меня было 15 лет опыта проетирования для веба, у нее только год, но каким-то непостижимым образом мы находились в одной ситуации: нам нравилась работа,
но мы были ошеломлены и озадачены тем, с какой скоростью возрастает сложность всего этого. Что за чертовщина происходит? (Это, конечно, риторический вопрос).

Для нас обоих было облегчением взаимно признаться в разочарованиии и замешательстве, и я стал задумываться, стоит ли просто посмеяться над этой ситуацией или отнестись серьезно. Тогда никто из нас не знал ответа, но спустя некоторе время я понял, что нужно сделать и то, и другое. Сейчас я хотел бы продолжить этот разговор и рассказать, что я думаю обо все этой путанице и чего она нам стоит. (I’d like to extend that conversation today and attempt to capture my perspective on that confusion and what it costs us.)

Основной причиной моего замешательства был перерыв. Три года назад я перестал делать вебсайты для клиентов чтобы сфокусироваться на [Abstract](https://www.goabstract.com/), компании где я был сооснователем. Моя работа там закончилась в начале прошлого года, и спустя некоторое время я решил возобновить работу дизайн студии, которой я управлял раньше.

И вы уже поняли, да? Первыми проектами были вебсайты. Многое могло измениться за три года, так что я решил освежить знания о том, как сделать хороший сайт и... оооу...

Все очень сложно, не правда ли?

![whoa](https://frankchimero.com/writing/everything-easy-is-hard-again/images/whoa.png)

Сначала сложность демотивировала. Я даже не был уверен, что захочу заниматься сайтами после того, как увидел все эти методы рразработки. В конце концов я все-таки решиося взяться за проекты. Нутро подсказывало мне, что сложные воркфлоу и тулчейны ссовсем не обязательны для многих проектов. Эта мысль - вторая тема этого разговора: я хотел бы встать на защиту простого дизайна и реализации как лучшего варианта для веба и работающих там людей.

Но я забегаю вперед. Сначала стоит немного больше рассказать о том, кто я и откуда взялся.

![cake](https://frankchimero.com/writing/everything-easy-is-hard-again/images/cake.png)

Я управляю дизайн-бутиком, что является претенциозным способом сказать, что студия крошечная с большой буквы К. Студия занимается всем: книги, журналы, брендинг и, конечно, веб сайты. Этот год юбилейный. Студия празднует 15 лет работы, а лично я - 20 лет разработки веб сайтов. В такую дату становишься сентиментальным и вспоминаешь, как все начиналось.

Студия появилась в 2002 году в лице человека (меня) с ноутбуком и стопкой бумаги на столе в углу его квартиры.

Пятнадцать лет спустя студия - это все еще человек с ноутбуком и стопкой бумаги на столе в углу его квартиры.

Сложно осознать, как много изменилось вокруг студии за эти 15 лет. Тогда не было социальных медиа, какими мы знаем их сегодня - ни Фейсбука, ни Инстаграма, ни снепов; большинство сайтов, на которые вы заходите сегодня, тогда не существовали, и большинство сайтов, на которые мы посещали тогда, не существуют сейчас. Не было айфонов. Мы выходили в сеть, находили маршрут и распечатывали карту как неандертальцы. Мы стучали камнями, пытаясь сделать графический дизайн.

Сейчас все по-другому, но я все еще сижу за своим столом.

--

Сначала я был обеспокоен отсутствием видимого прогресса студии, но потом меня отпустило: что, если все под контролем? Зачем менять то, что работает? Я смог подойти к множеству проектов с разных точек зрения и счастлив сообщить, что справился с этим весьма неплохо! Опыт и практика действительно помогают.

Кроме вебсайтов. Они отличаются от всего остального, потому что я не чувствую, что делаю их намного лучше, чем 20 лет назад. Мои навыки и знания немного возрастали, потом что-то менялось, и половина того, что я знал, становилось мертвым грузом. Едва ли это происходило с хоть чем-то еще из того, чем я занимался.

Я задумываюсь, есть ли у меня двадцать лет опыта разработки сайтов, или это пять лет опыта, повторившиеся четыре раза. Если вы работали в индустрии технологий, пожалуйста, скажите мне, что это звучит знакомо и вам.

Давайте я приведу вам пример такого пятилетнего цикла.

![okcomputer](https://frankchimero.com/writing/everything-easy-is-hard-again/images/okcomputer.jpg)

Как я уже сказал, я сделал свой первый сайт двадцать лет назад. Я знаю это, потому что я был подростком, делавшим работу бога: транскрибировал слова OK Computer Radiohead. Был 1997, я учил HTML, и была одна проблема, сбивавшая меня с толку:


Как мне расположить два элемента друг за другом?


Двадцать лет спустя мы все еще работаем над ответом на этот базовый вопрос.

```
<table>
  <tr>
    <td>Hi</td>
    <td>Mom</td>
  </tr>
</table>
```

Тогда в 1997 мы использовали таблицы и разделители. Это было как верстка сайта на (электоронных?) таблицах из ада. ПОчему-то мне это казалось забавным. Вероятно, меня завораживала возможность собрать что-то в своей комнате, нажать на кнопку и увидеть это-что-то уже "там".

```
{ float: left; }
```

Спустя пять лет на вебсайтах начинают использовать флоаты в CSS, потому что таблицы - это не семантично. Справедливо! С тех пор я потратил около 200 часов на чтение про обтекание элементов. Я все еще не уверен, что до конца разобрался; я печатаю clear: both и возношу молитвы блоковой модели.

```
{ display: flex; }
```

I was saved by Flexbox after five years of guess work. It is my baby. I was trained as a print designer, and with flexbox, I can type 3 or 4 lines of CSS, and have two blocks of text line up at the baseline. Hallelujah. I only needed to wait a decade to get this.

Спустя пять лет пришли флексбоксы и стали спасением. Моя прелесть. Я учился на дизайнера типографии и, получив флексбоксы, я могу напечатать 3 или 4 строчки в CSS и увидеть два блока текста, выровненных по бейзлайну. Аллилуйя. Всего-то нужно было подождать десятилетие.

```
{ display: grid; }
```

И теперь, после флексбоксов, пришли CSS гриды: мощная новая фича, которая обещает сделать отзывчивый веб дизайн еще более запутанным. Шучу, конечно, потому что гриды - серьезное улучшение управления лейаутом веба. Но я побаиваюсь сесть и узнать о них больше, потому что каждый раз, когда я вижу диаграмму, объясняющую, как работают CSS гриды...

![tables](https://frankchimero.com/writing/everything-easy-is-hard-again/images/tables.gif)

Я вижу напоминание о табличной верстке, которой я занимался в 1997. Внутренний голос говорит мне, что мы застряли в цикле, и вот он снова повторяется. Мы завершили круг, по которому будем бежать вечно. следующий подход к лейауту будет через пять лет, и он, вероятно, будет напоминать флоаты, и незнание того, как справиться с обтеканием элементов укусит меня за задницу второй раз в моей карьере.

![understanding](https://frankchimero.com/writing/everything-easy-is-hard-again/images/understanding.gif)

There are similar examples of the cycle in other parts of how websites get designed and made. Nothing stays settled, so of course a person with one year of experience and one with fifteen years of experience can both be confused. Things are so often only understood by those who are well-positioned in the middle of the current wave of thought. If you’re before the sweet spot in the wave, your inexperience means you know nothing. If you are after, you will know lots of things that aren’t applicable to that particular way of doing things. I don’t bring this up to imply that the young are dumb or that the inexperienced are inept—of course they’re not. But remember: if you stick around in the industry long enough, you’ll get to feel all three situations.

Есть много похожих примеров циклов в других частях разработки и создания сайтов. Ничто не стоит на месте, так что конечно и тот, у кого год опыта, и тот, у кого пятнадцать, будут в недоумении. Вещи понятны только тем, кто находится на пике волны. Если вы ниже максимума - вы еще ничего не знаете. После - вы знаете очень много всего, но это неприменимо к текущей разработке. Я веду это не к тому, что вы неопытны, глупы или некомпетенты - конечно нет. Но помните: если вы в индустрии достаточно долго, вы будете находить себя во всех этих состояниях.

One argument says that continual change in methodology is rigorous and healthy. I agree. Keeping things in play helps us to more easily fix what’s wrong. It’d be terrible if nothing could ever change. But I also agree with the other argument: people only have so much patience. How many laps around the cycle can a person run? I’m on lap five now, and I can tell you that it is exhausting to engage with rehashed ideas from the past without feeling a tiny amount of prejudice against them.

Говорят, что постоянное изменение методологии - это правильный и здоровый признак. Я согласен. Движение помогает нам быстрее справляться со сложностями. Было бы ужасно, если бы ничего не менялось. Но я согласен и с другим: терпение людей ограничено. Сколько таких кругов может пройти человек? Я сейчас на пятом, и, скажу я вам, крайне утомительно возвращаться к идеям из прошлого без малейшего предубеждения к ним.

Methods that were once taboo are back on the table. For instance, last week I was reading a post about the benefits of not using stylesheets and instead having inline styles for everything. The post made a few compelling points, but this approach would have been crazy talk a few years ago.

Методы, бывшие когда-то табу, снова перед нами. Например, на прошлой неделе я читал пост о плюсах использования инлайновых стилей вместо стайлшитов. Там были здравые аргументы, но сам посыл был сумашествием еще несколько лет назад.

So much of how we build websites and software comes down to how we think. The churn of tools, methods, and abstractions also signify the replacement of ideology. A person must usually think in a way similar to the people who created the tools to successfully use them. It’s not as simple as putting down a screwdriver and picking up a wrench. A person needs to revise their whole frame of thinking; they must change their mind.

Многое в том, как мы создаем сайты, сводится к тому, как мы думаем. Смещение инструментов, методов и абстракций также означает и смену идеологии. Чтобы успешно их использовать инструменты, нужно думать в том же направлении, как и те, кто их создал. Это не так же просто, как положить отвертку и взять гаечный ключ. Придется пересмотреть свои взгляды в целом и посмотреть на вещи с другой точки зрения.

In one way, it is easier to be inexperienced: you don’t have to learn what is no longer relevant. Experience, on the other hand, creates two distinct struggles: the first is to identify and unlearn what is no longer necessary (that’s work, too). The second is to remain open-minded, patient, and willing to engage with what’s new, even if it resembles a new take on something you decided against a long time ago.

С одной стороны, проще быть новичком: не нужно изучать то, что больше неактуально. Опыт, с другой стороны, создает две 




















ю
