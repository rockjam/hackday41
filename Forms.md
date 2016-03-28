# Формы

### Итерация №1

* Посмотреть как формы сделаны в яндекс деньгах
* Понять какие элементы управления нужны нам
		- input(data)
		- button
		- slider(progess(0-100), showHandle)
		- label
		- list(elems:List[Element], selected: Option[Int]) //Element(id, value); selected - id-шник выбранного элемента
		- checkbox(checked: Boolean)
		- form(inputs: List[Input])
* сделать построчное накидывание элементов. каждый элемент будет занимать одну строку

Notes:
У всех элементов есть признак enabled.
У всех элементов есть признак name.
У любого элемента может быть label(строка). Если лейбла нет, то это пустая строка.

(text)input types: 
* numeric(0)
* text(1)		

###Итерация №2
* сериализация форм в json с помошью upickle
* валидация форм. подумать заранее(не сделано)

####Пример формы в  JSON
validate later
```
{
	"dataType": "form",
    "data": {
            "inputs": [
                {
                    "data": "",
                    "name": "account",
                    "inputType": 1,
                    "label": "",
                    "enabled": true,
                    "$type": "TextInput"
                },
                {
                    "data": "",
                    "name": "amount",
                    "inputType": 0,
                    "label": "Amount",
                    "enabled": true,
                    "$type": "TextInput"
                },
                {
                    "name": "pay",
                    "label": "Pay!",
                    "enabled": true,
                    "$type": "Button"
                }
            ],
            "name": "skype",
			"action": ""
    }
}
```

Notes:
```
sendJson(dataType = "form", JSON from string)//нужно сделать чтобы можно было передавать json прямо в виде строки
```

###Итерация №3(Пятница ночь - утро субботы)
* Нужно чтобы из ботов можно было создавать формочки(нужно бы запаблишить botkit)


Все input-ы которые мы реализовали в виде json-а

```
//all kinds of inputs are here
[
        // ordinary text input with input type: text
        {
            "$type": "TextInput",
            "enabled": true,
            "name": "account",
            //if label is empty - it is encoded as empty string
            "label": "",
            "inputType": 1,
            "data": ""
        },
        // button
        {
            "$type": "Button",
            "enabled": true,
            "name": "doIt",
            "label": "Do it!"
        },
        // slidebar with handle
        {
            "$type": "Slider",
            "enabled": true,
            "name": "songProgress",
            "label": "",
            "progress": 53,
            "showHandle": true
        },
        // simple label
        {
            "$type": "Label",
            "enabled": true,
            "name": "label1",
            "label": "This is label 1"
        },
        // list with elements
        {
            "$type": "ElementsList",
            "enabled": true,
            "name": "songsList",
            "label": "Here are songs",
            "elems": [
                {
                    "id": 0,
                    "value": "When the leeve breaks"
                },
                {
                    "id": 1,
                    "value": "Lost in a supermarket"
                },
                {
                    "id": 2,
                    "value": "Holod"
                }
            ],
            // selected value is -1 when there is no selected value
            "selected": -1
        },
        // checkbox
        {
            "$type": "Checkbox",
            "enabled": true,
            "name": "saveTemplate",
            "label": "Save to templates",
            "checked": true
        }
    ]
```

И аналогичный код создания этих элементов на scala
```
    List(
      TextInput(
        enabled = true,
        name = "account",
        label = "",
        inputType = Text.toInt,
        data = ""
      ),
      Button(
        enabled = true,
        name = "doIt",
        label = "Do it!"
      ),
      Slider(
        enabled = true,
        name = "songProgress",
        label = "",
        progress = 53,
        showHandle = true
      ),
      Label(
        enabled = true,
        name = "label1",
        label = "This is label 1"

      ),
      ElementsList(
        enabled = true,
        name = "songsList",
        label = "Here are songs",
        List(
          Element(0, "When the leeve breaks"),
          Element(1, "Lost in a supermarket"),
          Element(2, "Holod")
        ),
        // none are selected
        -1
      ),
      Checkbox(
        enabled = true,
        name = "saveTemplate",
        label = "Save to templates",
        checked = true
      )
    )
```

###Итерация №4(Суббота утро)
* Валидация формочек(отложено до лучших времен)
* Сабмит форм юзером(начал)

Про сабмит форм:

* нужно решить, как именно передавать форму.
	- для бота с плеером передаем по одному input-у в форме.
	- для yandex money будет форма с кучкой инпутов.
	- для hyper bot-а. Одна форма, множество параметров

Notes:

* нужно сделать метод в котлине для того чтобы формы создавать проще. можно также сделать обвязку для всех классов, чтобы можно было их создавать в духе
	 ```
	   Label(enabled = true, label = "hello",...)
	 ``` √
* нужно добавить у формы методы toString = write(this) и validate: `String Xor Unit`? √
* в будущем нужно будет делать UpdateContentChangedWeak 
* parse все таки должен возвращать Option √

About form validation:

* Формы в рамках одного бота имеют уникальное имя.



###Итерация №5(Суббота 15.35)

...


###План выступления на демофесте

Всем привет. Меня зовут Николай, а это Глеб. Мы разрабатываем мессенджер Actor
За 48 часов мы сделали ботов удобными для людей
Боты отличные помошники. Чаще всего мы общаемся с нимии текстом, но это бывает недобно.

Раньше мы делали так же. Например сделали такую оплату телефона через кошелек Яндекса:

--- тут показываем яндекс бота - старую версию. --
Я комментирую:
	- пишем боту что хотим положить денег на телефон - отправляем сообщение
	- вводим телефон - отправляем сообщение
	- пишем сколько хотим положить денег, отправляем сообщение
	- палим всем вам CVC код карты - отправляем сообщение.
	Ко всему этому важно не опечататься - а то начинай сначала 

Это неудобно, более того - это надоедает!
Но можно лучше! Мы считаем что нужен удобный и привычный способ передачи информации ботам. Такой к которому мы привыкли за последние 10 лет.

ФОРМЫ! 
Давайте оплатим телефон через кошелек Яндекс через нового бота:
	- Во первых вы легко узнаете фирменный стиль Яндекса. Нр самом деле это наша стилизация. Но выгядит классно!
	- пишем что хотим положить денег на телефон
	- заполняем телефон, ошибиться сложно, потому что нам доступна только цифровая клавиатура, пишем сколько платим, CVC код который сложно подглядеть
	- жмем "Оплатить"
Гораздо проще и приятнее - согласны?
Данные отображаются в чате, но при этом никаких секретных данных мы не видим

Представьте что есть бот напоминалка. Вы хотите чтобы он напомнил вам сходить в театр в 2 апреля в 6 вечера. Как вы будете вводить это текстом?
А вот как это будет выглядеть с формой:



Это просто, формы красиво выглядят, там где можно ввести текст мы вводили текст, где цифры - интерфейс не позволит ошибиться.


Боты это не только оплата телефона и мемы. Это всевозможные интеграции, домашняя автоматизация, а теперь и с приятным интерфейсом





ФОРМЫ - ВМЕСТО ТЫСЯЧИ СЛОВ
