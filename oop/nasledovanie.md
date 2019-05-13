# Наследование

В Vala класс может наследовать от одного класса или вовсе не наследовать. На самом деле это обычно один класс, т.к. здесь нет неявного наследования как это в Java.

При наследовании вы создаете класс, объекты этого подкласса являются объектами и базового класса. Это значит что на них можно проводить те же действия, что и с объектами базового класса, поэтому если потребуется экземпляр базового класса, то экземпляр потомка можно к этому типу привести.

При создании класса у вас есть полный контроль над установкой доступа к методам и полям объекта. Следующий пример демонстрирует ряд таких возможностей:

```csharp
class SuperClass : GLib.Object {

    private int data;

    public SuperClass(int data) {
        this.data = data;
    }

    protected void protected_method() {
    }

    public static void public_static_method() {
    }
}

class SubClass : SuperClass {

    public SubClass() {
        base(10);
    }
}
```

`data` является полем для хранения данных в объектах класса `SuperClass`. В каждом экземпляре `SuperClass` будет такое поле, и он объявлен как private, поэтому он доступен внутри класса `SuperClass`.

`protected_method` является методом уровня объекта типа `SuperClass`. Вы можете его вызвать, только если создадите экземпляр класса `SuperClass` или его потомков, и только из кода внутри `SuperClass` или его потомков - последнее условие является результатом применения модификатора `protected`.

`public_static_method` имеет два модификатора. `static` значит, что данный метод можно вызвать и без наличия _экземпляра_ класса `SuperClass` или наследующих от него. Следовательно, метод не будет иметь доступа к ссылке во время выполнения. `public` значит, что метод можно вызывать из любого места, независимо как он относится к `SuperClass` или к наследующим от него.

С учетом этих определений, экземпляр подкласса \(`SubClass`\) будет содержать все три члена супер-класса \(`SuperClass`\), но не будет иметь доступа к открытым членам. Внешний код имеет доступ только к открытым методам.

С помощью `base` конструктор класса-наследника может по цепочке вызывать конструктор базового класса.
