---
language: ru
title: C++. Конструкторы, оператор присваивания и деструктор
date: 2022-03-20
tag: cpp
---

> Я буду основываться на коде, приведённом в основной [статье](../classes) про классы.

В основной статье я показал как создать класс и конструктор для него:

```cpp
class Coffee
{
public:
	Coffee(const char *type, int temperature, int volume)
	{ 
		m_temperature = temperature;
		m_volume = volume;
		// Выделяем память под строку
		m_type = (char*)malloc(strlen(type));
		// Копируем type в m_type
		strcpy(m_type, type);
	}
    ...
```

### Разница между инициализацией и присваиванием

Прежде чем мы пойдём дальше, я хочу показать один принципиальный момент. Видишь ли ты разницу между заданием значений для переменной "a" и "b"?

```cpp
// Создание переменной (хранит неизвестное значение)
int a;
// Присваивание значения
a = 5;
// Инициализация переменной значением 5
int b = 5;
```

Для переменной "a" мы выполняем две операции: создание и присваивание. Для переменной "b" только одну -- создание переменной со значением 5.

> С примитивными типами (int, double и т.д.) это не даёт сильного прироста, но при инициализации объектов это реально даёт выигрыш в производительности.

Сейчас в классе у нас все переменные заполняются как переменная "a" -- давай поправим это, и будем их сразу инициализировать:

```cpp
class Coffee
{
public:
	Coffee(const char *type, int temperature, int volume) :
        m_temperature(temperature),
        m_volume(volume)
	{ 
		// Выделяем память под строку
		m_type = (char*)malloc(strlen(type));
		// Копируем type в m_type
		strcpy(m_type, type);
	}
    ...
```

Пишем двоеточие после сигнатуры конструктора, и перечисляем через запятую все конструкторы, которые хотим вызвать для наших полей (да, у обычных полей тоже есть что-то вроде конструктора).

m\_type так не проинициализируешь -- ему нужно выделять память, а потом копировать строку.

Старайся в любой возможности инициализировать переменные при их создании.

### Конструктор с параметрами

Так вот, конструктор, который я использовал, называется "конструктор с параметрами".

Чтобы создать объект через конструктор с параметрами, надо при создании объекта указать в параметрах необходимые параметры:

```cpp
Coffee params_coffee("latte", 50, 200);
```

Ладно, поехали дальше. Есть конструктор с параметрами -- наверно есть и без параметров?

### Конструктор без параметров

```cpp
class Coffee
{
public:
	Coffee() :
        m_temperature(70),
        m_volume(200)
	{ 
		// Выделяем память под строку
		m_type = (char*)malloc(strlen(type));
		// Копируем type в m_type
		strcpy(m_type, "latte");
	}
    ...
```

Из-за того, что в параметрах ничего не передаётся, в поля присваиваются произвольные значения.

Такой конструктор вызовется, если при создании объекта ничего не передавать:

```cpp
Coffee no_params_coffee;
```

### Множество конструкторов в одном классе

Одновременно в классе может быть определён один конструктор без параметров и множество конструкторов с параметрами:

```cpp
class Coffee
{
public:
	Coffee() {...} // #0
	Coffee(const char *type, int temperature, int volume) {...} // #1
	Coffee(const char *type) {...} // #2
	Coffee(int temperature, int volume) {...} // #3
    ...
```

Вот как они будут вызываться:

```cpp
Coffee c0;
Coffee c1("cappucino", 50, 150);
Coffee c2("cappucino");
Coffee c3(60, 150);
```

> Такое изобилие конструкторов работает благодаря [перегрузке функций](../intro#function).

Кстати, кроме инициализации полей, мы можем вызывать другие конструкторы:

```cpp
class Coffee
{
public:
    // Конструктор без параметров
	Coffee() :
        Coffee("latte", 70, 200)
	{}
    // Конструктор с параметрами
	Coffee(const char *type, int temperature, int volume) :
        m_temperature(temperature),
        m_volume(volume)
	{ 
		// Выделяем память под строку
		m_type = (char*)malloc(strlen(type));
		// Копируем type в m_type
		strcpy(m_type, type);
	}
    ...
```

Иногда это очень удобно, и позволяет избежать дублирования кода -- в этом примере я описал логику копирования строки только в одном месте.

### Конструктор с параметрами по умолчанию

Также, [из вводной статьи](../intro#function) можно вспомнить, что в C++ теперь можно указывать значения по умолчанию для параметров функций. Для методов это тоже работает:

```cpp
class Coffee
{
public:
	Coffee(const char *type="latte", int temperature=70, int volume=200) :
        m_temperature(temperature),
        m_volume(volume)
	{ 
		// Выделяем память под строку
		m_type = (char*)malloc(strlen(type));
		// Копируем type в m_type
		strcpy(m_type, type);
	}
    ...
```

С помощью одного конструктора с параметрами по умолчанию, можно покрыть сразу множество различных наборов параметров:

```cpp
// Вызов конструктора с параметрами - все параметры по умолчанию
Coffee params_coffee_0;
// Вызов конструктора с параметрами - все параметры, кроме type, по умолчанию
Coffee params_coffee_1("latte");
// Вызов конструктора с параметрами - volume по умолчанию
Coffee params_coffee_2("latte", 50);
// Вызов конструктора с параметрами - все параметры указаны
Coffee params_coffee_3("latte", 50, 200);
```

Я показал самые основные и полезные способы использования конструкторов.

Однако, на этом история про конструкторы не заканичивается -- давай занырнём поглубже.

# Методы генерируемые компилятором

Компилятор для каждого класса генерирует следующие методы:

* Конструктор без параметров -- его называют "конструктор по умолчанию"
* Конструктор копирования -- вызывается, если в параметрах указан другой объект
* Оператор присваивания -- вызывается, если был использован оператор "=" для существующего объекта
* Деструктор

Если ты определишь хоть какой-то конструктор (с параметрами или без), то конструктор по умолчанию не будет сгенерирован.

Конструктор копирования, оператор присваивания и деструктор, не будут генерироваться только в случае, если ты сам их определишь.

Чтобы мы знали врага в лицо, давай представим что в нашем классе Coffee вообще не определён конструктор -- в таком случае:

```cpp
// Создаю объект через конструктор по умолчанию
Coffee default_coffee;
// Создаю объект через конструктор копирования (передаю объект в параметры)
Coffee copy_coffee(default_coffee);
// Другой способ вызова конструктора копирования
Coffee another_copy_coffee = default_coffee;
// Вызов оператора присваивания (вызывается только для уже созданного объекта)
Coffee assign_coffee;
assign_coffee = params_coffee;
```

### Конструктор по умолчанию

Если бы я захотел определить свой конструктор, и чтобы он работал как конструктор по умолчанию, то он бы выглядел так:

```cpp
Coffee() :
        m_type(0)
        m_temperature(0),
        m_volume(nullptr)
{}
```

Если ты не определишь ни одного конструктора, то вот такой конструктор будет вызываться при создании объекта -- он занулит все поля и вызовет конструкторы по умолчанию для всех полей-объектов.

> На самом деле он вызовет для полей конструкторы по умолчанию через uniform initialization, вроде `m_type{}, m_temperature{}, m_volume{}`. Я расскажу что это такое в будущем, но можешь глянуть, если интересно.

Если у тебя в классе есть какие-то поля, которые требуют к себе особого обращения, то это не твой вариант, и надо писать свой конструктор. Например, в нашем случае, нам надо выделить память под строку.

### Конструктор копирования

По умолчанию у каждого класса существует конструктор копирования -- он полностью копирует все поля объекта в другой.

Такой способ копирования называется поверхностное копирование (shallow copy) -- увидев указатель, он не будет делать полную копию объекта, на который он указывает. Вместо этого, он только скопирует адрес, который лежит в указателе.

Вот пример вызова конструктора копирования для нашего класса:

```cpp
int main()
{
    Coffee c1("espresso", 90, 50); 
    // Constructed 50ml cup of hot espresso coffee.
    Coffee c2 = c1;
    c1.drink(15);
    // Drank 15ml of espresso.
    c2.drink(15);
    // Drank 15ml of espresso.
    return 0;
    // Destructed espresso.
    // Destructed ma.
    // free(): double free detected in tcache 2
    // Aborted (core dumped)
}
```

Воу, это что произошло!?

Смотри:

* Через конструктор с параметрами создался объект c1 и выделилась память под массив
* Через конструктор копирования создался объект c2 и в него скопировались все поля из c1 (даже указатель m\_type)
* При вызове деструктора с2 освобождается память под массив (локальные переменные удаляются в обратном порядке)
* При вызове деструктора c1 память под массив пытается высвободиться ещё раз, и это вызывает экстренное прерывание

Чтобы такой проблемы не было, надо создать конструктор копирования:

```cpp
Coffee(Coffee &other) :
        m_temperature(temperature),
        m_volume(volume)
{
    // Выделяем память под строку
    m_type = (char*)malloc(strlen(other.m_type));
    // Копируем m_type из other в m_type
    strcpy(m_type, other.m_type);
}
```

В этом случае каждый объект будет хранить свой собственный массив, и освободить два раза его уже не получится... или получится?

### Оператор присваивания

Тут та же история, что и с конструктором копирования -- оператор присваивания делает поверхностную копию объекта, и у нас в объекте оказывается ссылка на чужой массив:
```cpp
int main()
{
    Coffee c1("espresso", 90, 50); 
    // Constructed 50ml cup of hot espresso coffee.
    Coffee c2("latte", 60, 150); 
    // Constructed 150ml cup of warm latte coffee.
    c2 = c1;
    c1.drink(15);
    // Drank 15ml of espresso.
    c2.drink(15);
    // Drank 15ml of espresso.
    return 0;
    // Destructed espresso.
    // Destructed }\.
    // free(): double free detected in tcache 2
    // Aborted (core dumped)
}
```

Разница лишь в том, что оператор присваивания работает для уже созданного объекта, перезаписывая существующие значения.

Давайте заменим сгенерированный компилятором оператор присваивания, чтобы ничего не ломалось:

```cpp
Coffee& operator=(const Coffee& other)
{
    m_temperature = other.temperature;
    m_volume = other.volume;
    // Выделяем память под строку
    m_type = (char*)malloc(strlen(other.m_type));
    // Копируем m_type из other в m_type
    strcpy(m_type, other.m_type);
}
```

Всё, теперь точно ничего не сломается!

### Деструктор

Ну, тут всё просто -- если мы не определили своего деструктора, то деструктор по умолчанию просто вызовет деструкторы полей-объектов и всё.

Подчищать за нами память никто не будет -- для этого надо писать свой деструктор.

К счастью, ещё в основной статье я описал этот деструктор:

```cpp
~Coffee()
{
    // Освобождаем память под строку при удалении объекта
    std::cout << "Destructed " << m_type << "." << std::endl;
    free(m_type);
}
```

Как видишь, никаких занулений переменных я не делаю -- это ни к чему, всё равно объект вот-вот будет уничтожен. Главное подчистить за собой динамически выделенную память, чтобы не было утечек.

### = default; = delete;

Ну и напоследок, совсем небольшая ремарочка -- C++ даёт возможность явно указать хочешь ли ты, чтобы компилятор сгенерировал для тебя какой-либо метод по умолчанию.

Например, чтобы явно указать "сгенерируй конструктор и деструктор по умолчанию" надо написать:

```cpp
class Coffee
{
    Coffee() = default;
    ~Coffee() = default;
    ...
```

> Зачем это делать -- вопрос для обсуждения. Мне кажется, что это позволяет напомнить программисту, что эти методы существуют. Больше можно прочитать [здесь](https://www.fluentcpp.com/2019/04/23/the-rule-of-zero-zero-constructor-zero-calorie/).

Также, можно явно указать компилятору, что вы не хотите, чтобы он генерировал какой-либо метод.

Например, чтобы указать "не создавай конструктор копирования и оператор присваивания" надо написать:

```cpp
class Coffee
{
    Coffee(const Coffee& other) = delete;
    Coffee& operator(const Coffee& other) = delete;
    ...
```

Это позволяет защититься от копирования, если твой класс его не поддерживает.

# Заключение

Итого, мы узнали:

* Разницу между инициализацией и присваиванием -- стремимся к эффективности
* Конструктор с параметрами -- передаём при создании
* Конструктор без параметорв -- не передаём, но инициализируем в конструкторе
* Можественные конструкторы объекта -- делаем столько конструкторов, сколько нам надо
* Конструктор с параметрами по умолчанию -- покрываем множество комбинаций параметров
* Методы, генерируемые компилятором:
    * Конструктор по умолчанию -- зануляет поля
    * Конструктор копирования -- делает поверхностную копию другого объекта при создании
    * Оператор присваивания -- делает поверхностную копию другого объекта для готового объекта
    * Деструктор -- вызывает деструкторы у полей
* Если не хотим методы по умолчанию, то делаем свои или отменяем их генерацию через `= delete;`
* Можем явно указать, что используем метод по умолчанию через `= default;`

![I know kung fu](/assets/images/i-know-kung-fu.png)

Если не получилось за один-два прохода осознать эту статью, то лучше вернись к ней попозже, когда под рукой будет проект, на котором можно это посмотреть вживую.

Если у тебя получилось срастить это всё в голове, то можешь считать себе мастером над конструкторами.

Дальше будет статья про спецификаторы доступа -- про инкапсуляцию.
