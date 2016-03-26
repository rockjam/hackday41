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
```
{
	"dataType": "form",
    "data": {
        "form": {
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
            "name": "skype"
        }
    }
}
```

Notes:
```
sendJson(dataType = "form", JSON from string)//нужно сделать чтобы можно было передавать json прямо в виде строки
```

###Итерация №3
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

###Итерация №4
* Валидация формочек
* Сабмит форм юзером

Про сабмит форм:

* нужно решить, как именно передавать форму.
	- для бота с плеером передаем по одному input-у в форме.
	- для yandex money будет форма с кучкой инпутов.
	- для hyper bot-а. Одна форма, множество параметров

Notes:

* нужно сделать метод в котлине для того чтобы формы создавать проще. можно также сделать обвязку для всех классов, чтобы можно было их создавать в духе
	 ```
	   Label(enabled = true, label = "hello",...)
	 ```
* нужно добавить у формы методы toString = write(this) и validate: `String Xor Unit`?
* в будущем нужно будет делать UpdateContentChangedWeak


About form validation:

* Формы в рамках одного бота имеют уникальное имя.
