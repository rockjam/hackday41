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


