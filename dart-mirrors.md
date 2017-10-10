---
published: false
layout: post
tag: dart
original: http://mrale.ph/blog/2017/01/08/the-fear-of-dart-mirrors.html
title: The fear of dart:mirrors
author: "Vyacheslav Egorov"
translator: "Ирина Смышляева"

---
[dart:mirrors](https://api.dartlang.org/stable/1.21.1/dart-mirrors/dart-mirrors-library.html) должно быть самая непонятая, неправильно используемая и несправедливо забытая из библиотек ядра Dart'а. Она была в языке с самого начала, но по-прежнему окружена туманом неопределенности и отмечена как нестабильная в документации, несмотря на то, что API не менялось уже давно.

dart:mirrors редко оказывается во внимании, потому что это библиотека, которая позволяет метапрограммировать. Рядовому разработчику редко приходится иметь с этим дело напрямую - скорее, это касается авторов билиотек или фреймворков.

И на самом деле, в начале моего пути к этому посту стояло не dart:mirrors, а что-то совсем другое:

## Десериализация JSON

В декабре 2016 я залипал на канал Дарта в Slack (от него отказались в пользу [dart-lang Gitter](https://gitter.im/dart-lang/home)), и увидел что некто, назовем его Максимилиан, спрашивает о том, какие в Дарте есть способы распарсить JSON. Если вы пришли из JavaScript, такой вопрос может удивить - разве сделать это не так же просто, как `JSON.parse(...)` в JavaScript?

Да, так же просто: можно просто использовать `JSON.decode(...)` из встроенной библиотека dart:convert, но есть загвоздка. В JavaScript `JSON.parse` вернет объект, потому что в JS объекты - это бесформенные облака свойств и такой код совершенно нормальный:
```
let userData = JSON.parse(str);
console.log(`Got user ${userData.name} from ${userData.city}`);
```
Однако каждый объект Дарта имеет фиксированный класс, который полностью определяет его форму. `JSON.decode` может вернуть Map, но не объект  вашего пользовательского типа:
```
Map userData = JSON.decode(str);
console.log("Got user ${userData['name']} from ${userData['city']}");  // OK

class UserData {
  String name;
  String city;
}
```
UserData userData = JSON.decode(str);  // не OK
console.log("Got user ${userData.name} from ${userData.city}");

Распространенное решение этой проблемы заключается в написании хелпера для декодирования, умеющего создавать объекты из Maps:
```
class UserData {
  String name;
  String city;

  UserData(this.name, this.city);

  UserDate.fromJson(Map m)
      : this(m['name'], m['city']);
}
```
UserData userData = new UserDate.fromJson(JSON.decode(str));
console.log("Got user ${userData.name} from ${userData.city}");

Но написание такого шаблонного кода не лучший способ провести день. Поэтому в [pub](https://pub.dartlang.org/) можно найти множество пакетов, автоматизирующих это и... [барабанная дробь](https://www.youtube.com/watch?v=mzAfTmC3It0)... некоторые из них используют dart:mirrors.

### Что такое dart:mirrors?

Если вы не знакомы с языком, вероятно вы никогда не слышали слово mirror (зеркало) в контексте языков программирования. Это игра слов: зеркальное апи позволяет программе отражаться (думать о себе). Исторически это пошло от SELF, как и многие другие отличные VM технологии. Посмотрите [пост Gilad Bracha](https://gbracha.blogspot.ru/2010/03/through-looking-glass-darkly.html) и походите по ссылкам, если хотите узнать больше про зеркала и их роль в других системах.

Зеркала существуют, чтобы отвечать на рефлективные вопросы вроде *"Какой тип данного объекта?", "Какие поля/методы у него есть?", "Какой тип поля?", "Какой тип параметра метода?"* и выполнять рефлективные действия *"Получить значение этого поля из того объекта!"* или *"Вызвать этот метод в том объекте!"*.

В JavaScript объекты сами по себе содержат рефлективные возможности: чтобы динамически получить свойство по имени можно просто сделать obj[key], чтобы вызвать метод по имени с динамическим списком аргументов obj[name].apply(obj, args).

Дарт, в свою очередь, инкапсулирует эти возможности в библиотеке dart:mirrors:
```
import 'dart:mirrors' as mirrors;

    setField(obj, name, value) {
      // Забирает InstanceMirror, который позволяет получить доступ к стейту obj
      final mirror = mirrors.reflect(obj);
      // Устанавливает поле через зеркало
      mirror.setField(new Symbol(name), value);
    }
```
    На первый взгляд апи может показаться многословным, но на самом деле такие вещи как `Symbol(name)` написаны не просто так: они напоминают, что имена, данные классам и методам могуты быть изменены или искажены компилятором в попытке уменьшить размер сгенерированного кода. Пока просто запомните, мы к этому еще вернемся.

### Вернемся к десериализации JSON

Библиотеки для десериализации, использующие dart:mirrors, очень просто использовать: достаточно просто добавить аннотацию в класс. Ниже находится пример, как в аннотиции [dartson](https://pub.dartlang.org/packages/dartson) выглядит модель данных из вопроса Максимилиана:

(Для сохранения анонимности я переименовал все поля и классы, но сохранил структуру модели данных)

```
import 'package:dartson/dartson.dart';

@Entity()
class Data {
  String something0;
  String something1;
  List<Data1> sublist0;
  List<Data2> sublist1;
  List<Data3> sublist2;
}

@Entity()
class Data1 {
  String something0;
  String something1;
  String something2;
  String something3;
  String something4;
  String something5;
  String something6;
}

@Entity()
class Data2 {
  String something0;
  String something1;
}

@Entity()
class Data3 {
  String something0;
  String something1;
}

final DSON = new Dartson.JSON();

// Десериализация объекта Data из JSON-строки.
Data d = DSON.decode(string, new Data());
```

Кто-то может спросить: если все так просто и гладко, то в чем тогда вопрос?

Выходит так, что dartson ну очень медленный. Когда Максимилиан использует файл весом 9Мб, содержащий нужные для работы данные, вот что он видит:


  dart decode-benchmark.dart --with-dartson  
  loaded data.json: 9389696 bytes  
  DSON.decode(...) took: <span style="color:red"><b>2654ms</b></span>  
  $ d8 decode-benchmark.js  
  loaded data.json: 9389696 bytes  
  JSON.parse(...) took: <span style="color:green"><b>118ms</b></span>    

Эти цифры не выглядят здорово. Давайте сделаем еще немного измерений:

  $ dart decode-benchmark.dart --with-json
  loaded data.json: 9389696 bytes
  JSON.decode(...) took: <span style="color:orange"><b>236ms</b></span>

То есть, просто декодировать JSON стороку в неструктурированный лес (?!) Maps и Lists в десять раз быстрее, чем использовать dartson? Это обескураживает и виноваты в этом, определенно, dart:mirrors! По крайней мере, к такому консенсусу пришел слак канал.

Прежде чем мы закопаемся поглубже, я хотел бы отметить вот что: JSON parser у V8 написан на C++,
а у Дарта - на нем самом, и это потоковый анализатор. Если помнить об этом, разница между JSON.decode(...) Дарта and JSON.parse(...) V8 уже не кажется *настолько* плохой.

Еще интересно попробовать и сравнить другие языки, использующие reflections для десериализацииJSON: Максимилиан попробовал пакет encoding/json в Go:
```
import (
  "io/ioutil"
  "encoding/json"
  "fmt"
  "time"
)

type Data struct {
  Something0 string `json:"something0"`
  Something1 string `json:"something1"`
  Sublist0 []Data1 `json:"sublist0"`
  Sublist1 []Data2 `json:"sublist1"`
  Sublist2 []Data3 `json:"sublist2"`
}

var data Data
err := json.Unmarshal(data, &data)
```

  $ go run decode-benchmark.go
  loaded data.json: 9389696 bytes
  json.Unmarshal(...) took: <span style="color:orange"><b>279ms</b></span>


Получается, в Go десериализация JSON в структуру занимает примерно столько же времени, сколько у Дарта уходит на его десериализацию в unstructured Maps, даже если encoding/json использует для этого рефлекшены.

Вот диаграмма размаха времени парсинга 9МБ файла Максимилиана (по 30 раз)

![parse time diagram](http://mrale.ph/images/2017-01-08/plot-0.png)

dartson отсутствует на этой картике потому что он, по крайней мере, в 10 раз медленнее, чем что-то еще...

### Углубимся в производительность dartson'а

Один из самых простых способов исследования производительности Дарта - использовать Observatory:

```
$ dart --observe decode-benchmark.dart --dartson
Observatory listening on http://127.0.0.1:8181/
loaded data.json: 9389696 bytes
...
```

На странице профиля цпу мы найдем неутешительную картину:

![cpu profile](http://mrale.ph/images/2017-01-08/profile-0.png)

Увиденное говорит нам, что Дартсон тратит очень много времени на интерполяцию строк. Беглым взглядом на исходный код видно, что дарт делает много записей вроде этой:

```
void _fillObject(InstanceMirror objMirror, Map filler) {
  // ...
  _log.fine("Filled object completly: ${filler}");
}
```

или этой

```
Object _convertValue(TypeMirror valueType, Object value, String key) {
  // ...
  _log.finer('Convert "${key}": $value to ${symbolName}');
  // ...
}
```

Это очевидная трата времени, поскольку интерполяция происходит даже если логирование выключено. Таким образом, в первую очередь, чтобы улучшить производительность десериализации, нужно совсем убрать все это логирование:

$ dart decode-benchmark.dart --with-dartson
loaded data.json: 9389696 bytes
DSON.decode(...) took: <span style="color:orange"><b>1542ms</b></span>

Вуаля! Мы только что сделали десериализацию с дартсоном на 42% быстрее, изменив кое-что, не затрагивающее другие зеркала и JSON. Если снова посмотреть в профиль, то там стоновится интереснее:

![cpu profile without logging](http://mrale.ph/images/2017-01-08/profile-1.png)
