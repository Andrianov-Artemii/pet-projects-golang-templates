# Как сделать своего telegram-бота на Golang

## Создание бота
Чтобы работать с ботом - нужно создать переменную в которую он будет записан
```golang
// Add API key
	bot, err := tgbotapi.NewBotAPI("<KEY>")
	// Errors panic
	if err != nil {
		log.Panic(err)
	} else {
		// Auto Log
		log.Printf("Authorized on account %s", bot.Self.FirstName)
	}
```

## Обновление
Чтобы принимать текст из чата - надо переодически запрашивать обновление от бота

```golang
	u := tgbotapi.NewUpdate(0)
	u.Timeout = 60
	updates, err := bot.GetUpdatesChan(u)
```

## Работа с обновлениями
Каждое обновление, которое нам приходит записываетя в переменную `updates`. Их там много и мы будем их перебирать. Для того, чтобы перебрать все обновления создадим цикл

```golang
	for update := range updates {
	    // Код с обнолениями
	}
```

Весь дальнейший код мы будем писать в межу фигурных скобок

### Проверка на пустоту
При обновлении сообщение может быть пустым. Поэтому мы должны проверить, что что-то получили, поэтому возьмем переменную `update.Message` и проверим, что она не равна `nil`

```golang
	if update.Message == nil { 
	    continue
	}
```

### Проверка на равенство команде
Однако сообщения не всегда будут пустыми. Чтобы проверить, что в сообщении что-то пришло - нужно проверить, что оно равно строке, где строка - сообщение, которое мы ожидаем получить

```golang
	if update.Message == "<MESSAGE>" { 
	    // Обработка сообщения
	}
```

## Отправка сообщений с текстом
Чтобы отправить просто текстовое сообщение - добавим данный код (создадим `msg` и заставим `bot` его отправить)
```golang
	msg := tgbotapi.NewMessage(update.Message.Chat.ID, "<MESSAGE>")
	bot.Send(msg)
}
```

## Отправка сообщений с текстом и картинкой
Чтобы отправить сообщение с картинкой - добавим данный код. При создании сообщения добавить путь к файлу, в котором лежит картинка и `Caption` (подпись)
```golang
    msg := tgbotapi.NewPhotoUpload(update.Message.Chat.ID, "PATH")
	msg.Caption = "<TEXT>"
	bot.Send(msg)
```

## Отправка сообщений с геолокацией
Чтобы отправить сообщение c геолокацией - создадим `msg` и добавим широту `long` и долготу `lat` (цифры с точками)
```golang
	location := tgbotapi.NewLocation(update.Message.Chat.ID, long, lat)
	bot.Send(location)
```

## Запуск бота
Чтобы запустить бота в `Terminal` прописываем команду
```sh
	go run main.go
```
