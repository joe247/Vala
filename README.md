# Учебник Vala

### Предисловие

Это перевод основного учебника Vala от GNOME, какое то время я занимался им прямо на их платформе но потом решил перенести все на GitBook.  
Я также добавлял множество информации от себя, так что он не полностью соответствует оригиналу.

### Введение

#### Что такое Vala?

Vala — это новый язык программирования, который позволяет использовать современные способы программирования для написания приложений, которые будут основываться на библиотеках GNOME, частично GLib и GObject. С давних пор эта платформа является полноценной средой разработки, включая такие особенности как система динамических типов данных и вспомогательные инструменты управления памятью. До создания Vala, единственным способом написания программ для этой платформы было использование API языка C, что приводило к нежелательным последствиям, использованию высокоуровневых скриптовых языков, требующих наличие виртуальной машины, таких как Python, Mono C\#, или, как альтернативный вариант, использование библиотеки-оболочки для C++.

Технология Vala уникальна тем, что генерирует код на языке C, который может быть скомпилирован для запуска без необходимости установки дополнительных библиотек, не включенных в GNOME. 

{% hint style="info" %}
Это не значит что язык имеет какую-либо зависимость от платформы GNOME, единственное что необходимо для запуска Vala это библиотека [GLIb](https://ru.wikipedia.org/wiki/GLib) которая является абсолютно кроссплатформенной\(все DLL под Windows занимают 2 МБ, на Linux GLib поставляется из коробки в любом дистрибутиве\)   
Хотя имеется возможность писать без GLib, у Vala существуют альтернативные бэкенды самый развитый из которых: POSIX, с ним единственной зависимостью будет являться стандартная библиотека С [libc](https://ru.wikipedia.org/wiki/%D0%A1%D1%82%D0%B0%D0%BD%D0%B4%D0%B0%D1%80%D1%82%D0%BD%D0%B0%D1%8F_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA%D0%B0_%D1%8F%D0%B7%D1%8B%D0%BA%D0%B0_%D0%A1%D0%B8).
{% endhint %}



Производительность программ, написанных на Vala, будет сопоставима с производительностью программ, написанных непосредственно на C. И в то же время, они более просты в написании и сопровождении. Также Vala добавляет множество проверок для улучшения безопасности. Их можно отключить для увеличения производительности флагом --disable-asserts

![https://github.com/kostya/benchmarks](.gitbook/assets/image%20%282%29.png)

---

Программы, написанные на Vala, могут делать то же, что и программы, написанные на C. И хотя Vala предоставляет функции, не доступные C программистам, их можно реализовать и на C, но это трудоемкий и сложный процесс \(ООП, Дженерики, Лямбды, Миксины, Контроль памяти ...\)

### Vala разрабатывается слишком маленьким количеством людей?

Это не так, дело в том что Std Vala выступает GLib, а доп библиотеками GNOME стек.

Статья "[Vala не язык программирования](https://blogs.gnome.org/despinosa/2017/02/14/vala-is-not-a-programming-language/)" которую я пока не перевёл, вкратце в ней говорится о том что Vala состоит из большого количества вещей развиваемых по отдельности огромными сообществами. 

* [GLib](https://github.com/GNOME/glib) [git](https://github.com/GNOME/glib) [wiki](https://ru.wikipedia.org/wiki/GLib) [маны](https://app.gitbook.com/@gavr123456789/s/glib/)\(на русском и тоже на gitbook\)
* [GIO](https://github.com/GNOME/glib/tree/master/gio) -- часть GLib про ввод/вывод
* [GIR](https://github.com/GNOME/gobject-introspection) система установки прозрачного межъязыкового взаимодействия, [подробно](https://gi.readthedocs.io/en/latest/)
* Gobject -- объектная система типов с поддержкой интроспекции, рефлексии и контейнером динамических типов\(для создания привязок к Python например\), [wiki](https://ru.wikipedia.org/wiki/GObject), [habr](https://habr.com/ru/post/348204/)  
* GDA -- GNU Data Access - либа создающая универсальный API ко всем видам баз данных, [wiki](https://en.wikipedia.org/wiki/GNOME-DB), [маны](https://developer.gnome.org/libgda/stable/), [пример](https://wiki.gnome.org/Projects/Vala/GDA) с vala и SQLite 
* GTK огромный графический стек [wiki](https://ru.wikipedia.org/wiki/GTK), [git](https://gitlab.gnome.org/GNOME/gtk), скоро выйдет 4тая версия в которой очень много изменений самые главные из которых рендер интерфейса на Vulkan/GL, значительные улучшения поддержки платформы Windows. Буквально сегодня вышла новость об очередном релизе \([рус](http://www.opennet.ru/opennews/art.shtml?num=50648)\)
* GCC 

Разберу одну ситуацию: только что вышло мажорное обновление GCC 9.  В нем улучшили оптимизацию `switch`\(второй параграф [здесь](http://www.opennet.ru/opennews/art.shtml?num=50622)\(рус\)\) и эта оптимизация автоматически попала в vala тк кк она компилируется в С код. 

Из этого всего следует что сам компилятор vala это около 10%, и над Vala косвенно работают множество людей. 

#### Для кого предназначено это руководство?

Данное руководство не углубляется в подробности основ написания программ. Здесь лишь кратко объясняются принципы объектно-ориентированного программирования, подробно рассказывая об основных принципах Vala. Данное руководство окажется полезным, если вы уже имеете опыт программирования, хотя глубокие познания какого-либо языка не требуются.

Синтаксис Vala очень похож на C\#, но я буду стараться избегать описания функций с точки зрения сходства и отличия их от C\# или Java с целью сделать руководство более доступным и понятным.

Для понимания вам будет полезно знание языка C. Хотя это не требуется для понимания самого Vala, важно знать, что он основывается на C и зачастую взаимодействует с его библиотеками. Знание этого языка программирования безусловно облегчит углубленное понимание Vala.

#### Условные обозначения

Исходный код программ будет написан моноширинным шрифтом, команды в терминале будут начинаться с символа $. Всё остальное должно быть понятно. Я обычно программирую с подробными комментариями, иногда указывая на то, что уже очевидно. Я постараюсь объяснить случаи, когда что-либо можно опустить, но это не значит, что я советую вам это делать.

Рано или поздно я добавлю ссылки на документацию по Vala, но пока что это невозможно.

#### Первая программа

Простой вариант:

```bash
print("Hello World!\n");
```

Ага, вот настолько просто, запустить можно с помощью `$ vala filename`

{% hint style="warning" %}
`Если вам не нужно выделения памяти на куче то main можно не объявлять.`
{% endhint %}

ООП \(Java/C\# style\) вариант:

```java
class Demo.HelloWorld : GLib.Object {
    public static int main(string[] args) {
        stdout.printf("Привет, мир\n");
    return 0;
    }
}
```

Конечно же это "Привет, мир" \(Hello World\), написанный на Vala. Я надеюсь, что некоторые части кода вам уже понятны, но всё равно я объясню всё по порядку.

```java
class Demo.HelloWorld : GLib.Object {
```

Эта строка начинает определение класса. Классы в Vala в общих чертах похожи на классы в других языках. Класс - это тип объекта, экземпляры которого могут быть созданы, обладая одинаковыми свойствами. Реализацией классов занимается библиотека gobject, для общего использования это знать не обязательно.

Нужно отметить, что этот класс является подклассом GLib.Object. Это потому, что Vala также поддерживает другие типы классов, но в большинстве случаев это то, что вам нужно. Фактически, некоторые возможности языка Vala можно использовать только если ваш класс происходит от класса Object из библиотеки GLib.

Другие части этой строки демонстрируют пространства имен и полностью определённые имена, но они будут объяснены позже.

```java
public static int main(string[] args) {
```

Это является началом определения метода. Метод — это функция, связанная с классом, которая может быть вызвана объектом. Если метод является статическим, это означает, что он может быть вызван без привязки к какому-либо экземпляру этого типа. Название метода «main» и наличие подписи означает, что Vala примет его за точку входа в программу.

Метод main не обязательно должен быть объявлен внутри класса. Однако, если он объявлен внутри класса, то он обязательно должен быть статическим, не имеет значения публичным или приватным. Тип возвращаемого значения может быть целочисленным \(int\) или не иметь типа \(void\). Если типом возвращаемого методом значения является void, то программа будет завершаться с кодом выхода 0. Параметр с массивом строк принимает аргументы командной строки и не является обязательным.

```java
stdout.printf("Привет, мир\n");
```

stdout - это объект пространства имён GLib, доступ к которому Vala обеспечивает когда бы это ни понадобилось. Эта строка указывает Vala вызвать метод printf объекта stdout, с приветственной строкой в качестве аргумента. В Vala это обыкновенный синтаксис для вызова методов объектов или для доступа к данным объекта. \n - это экранированная последовательность для перехода на новую строку.

```java
return 0;
```

Функция return возвращает код завершения и прекращает выполнение метода main, который в свою очередь завершает выполнение программы. Возвращённое значение является кодом завершения программы.

Последние строки просто завершают определения метода и класса.

#### Компиляция и запуск

Если у вас установлен Vala, то все, что нужно для компиляции и запуска программы это:

```bash
valac hello.vala
./hello
```

{% hint style="info" %}
Так же можно использовать просто `vala` тогда произойдет компиляция и запуск одновременно, как если бы это был Python
{% endhint %}

valac - это компилятор Vala, создающий двоичный исполняемый файл. В результате двоичный файл будет иметь то же имя, что и файл исходного кода. Сразу после компиляции файл может быть запущен на компьютере. Возможно, вы уже догадались, каков результат его работы.



#### Установка

{% hint style="info" %}
Установка на Linux: пакет присутствует во всех дистрибутивах под названием `vala` или `valac`

Установка на Windows: [бинарная](http://valainstaller.sourceforge.net/), "[правильная](https://wiki.gnome.org/Projects/Vala/ValaOnWindows)", после установки через MSYS рекомендую добавить /bin MSYS'а в Path чтобы иметь доступ к `valac` из консоли

Установка на Mac: `$ brew install vala`

FreeBSD: `$ cd /usr/ports/lang/vala/ && make install clean`  
`$ pkg install vala`
{% endhint %}

