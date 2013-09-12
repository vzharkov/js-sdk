API
---

Prostiezvonki
=============

### Конфигурация

При подключении скрипта создаётся глобальная переменная `prostiezvonki`. Для удобства к ней можно обращаться через псевдоним `pz`.

Для начала работы следует задать внутренний номер сотрудника:

```js
pz.setUserPhone(
    phone // внутренний номер телефона сотрудника
);
```

### Подключение к серверу

```js
pz.connect({
    host: 'ws://localhost:10150', // Адрес сервера
    client_id: 'password',        // Пароль
    client_type: 'jsapi'        // Тип приложения
});
```

> Максимальная длина пароля — 32 символа.

Функция возвращает объект со статусом выполнения и, при наличии ошибки, текстом этой ошибки:

```js
{
    result: 'ok'
}

{
    result: 'error',
    text: 'uh oh...'
}
```

### Разрыв подключения

```js
pz.disconnect();
```

### Проверка состояния подключения

Можно определить, активно ли подключение в данный момент:

```js
pz.isConnected();
```

либо подписаться на события подключения и отключения:

```js
pz.onConnect(
    callback // функция, которая будет вызвана при подключении к серверу
);

pz.onDisconnect(
    callback // функция, которая будет вызвана при отключении от сервера
);
```

### Исходящий звонок

```js
pz.call(
    phone // номер телефона, на который будет совершён звонок
);
```

### Перевод звонка

```js
pz.transfer(
    call_id, // идентификатор звонка, полученный от сервера
    phone    // номер телефона, на который нужно перевести звонок
);
```

### Получение событий

```js
pz.onEvent(
    callback // функция, которая будет вызвана при получении события от сервера
);
```

Первым аргументом функция принимает объект event:

```js
function (event) {
	alert('Индентификатор звонка — ' + event.callID)
}
```

Event
=====

### Методы

Для определения типа события можно использовать вспомогательные методы:

```js
event.isTransfer() // входящий запрос на переадресацию?
event.isIncoming() // входящий вызов на менеджера?
event.isHistory()  // завершенный вызов?
event.isOutcoming()  // исходящий вызов вызов?
event.isOutcomingAnswer()  // клиент поднял трубку?
event.isIncomingAnswer()  // менеджер поднял трубку?
```

### Свойства

Для всех типов:

* **event.type** - тип события.

	```
	1  - входящий запрос на переадресацию
	2  - входящий вызов на менеджера
    3  - завершенный вызов
    21 - завершенный вызов
    22 - завершенный вызов
	23 - завершенный вызов
	```

* **event.callID** - идентификатор звонка
* **event.from** - Номер звонящего абонента

Для входящего запроса на менеджера и завершённого вызова

* **event.to** - Номер вызываемого абонента

Для завершённого вызова

* **event.start** - Время начала вызова, в формате Timestamp
* **event.end** - Время окончания вызов, в формате Timestamp
* **event.duration** - Длина разговора, в секундах
* **event.direction** - Направление вызова: 0 – входящий, 1 – исходящий
* **event.record** - Ссылка на файл записи
