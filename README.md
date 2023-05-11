# Процесс деплоя

## Видео туториал

<a href="https://youtu.be/BvxrZ3WMrLE"><img style="max-width: 600px;" alt="image" src="https://user-images.githubusercontent.com/9513891/223895059-1ffa48c7-8801-4d7b-b9d3-15c857d03225.png"></a>

Thanks to [**lipeng0820**](https://www.youtube.com/@lipeng0820) for providing this video tutorial.

## Деплой вручную

### Шаг 1. Создайте Telegram-бота и получите токен

<img style="max-width: 600px;" alt="image" src="https://user-images.githubusercontent.com/9513891/222916992-b393178e-2c41-4a65-a962-96f776f652bd.png">

1. Открываем телеграм, находим бота BotFather и отправить ему команду `/start`.
2. Затем отправляем команду `/newbot` и говорим обту как будет называться ваш бот.
3. Имя бота обязательно должно заканчиваться на `_bot`.
4. BotFather сгенерирует токен. Скопируйте и сохраните этот токен в надежном месте. Этот токен является секретным ключом, который привязан к вашему боту. Не передавайте этот токен никому!
5. Позже, в настройках Cloudflare Workers, нам понадобится этот токен.

### Шаг 2. Регистрация учетной записи OpenAI и создание ключа API

<img style="max-width: 600px;" alt="image" src="https://user-images.githubusercontent.com/9513891/222917026-dd9bebcb-f4d4-4f8a-a836-5e89d220bbb9.png">

1. Открываем [OpenAI](https://platform.openai.com) и авторизируемся или регистрируем новый аккаунт.
2. Кликаем на аватарку в правом верхнем углу, чтобы перейти в [настройки](https://platform.openai.com/account/api-keys).
3. Переходим в пункт меню API Keys и создаем новый API Key.
4. Позже, в настройках Cloudflare Workers, нам понадобится этот ключ.

### Шаг 3. Деплой Worker-а

<img style="max-width: 600px;" alt="image" src="https://user-images.githubusercontent.com/9513891/222917036-fe70d0e9-3ddf-4c4a-9651-990bb84e4e92.png">

1. Открываем [Cloudflare Workers](https://dash.cloudflare.com/?to=/:account/workers) и регистрируем новый аккаунт.
2. Кликаем по пункту меню `Workers`.
3. Кликаем `Create a Service` в правом верхнем углу.
4. После создания Worker-а, вас перенаправит в него, кликните по кнопке `Quick Edit`, скопируйте этот [`код`](../../../dist/index.js) в редактор, и сохраните его.

### Шаг 4. Настройка переменных среды

<img style="max-width: 600px;" alt="image" src="https://user-images.githubusercontent.com/9513891/222916940-cc4ce79c-f531-4d73-a215-943cb394787a.png">

1. Открываем [Cloudflare Workers](https://dash.cloudflare.com/?to=/:account/workers).
2. Кликаем по пункту меню `Workers`, далее выбираем наш воркер.
3. В правом верхнем углу переходим в настройки `Setting -> Variables`.
4. В блоке `Environment Variables` нажимаем на синюю кнопку `Add variable` и начинаем добавлять переменные. Ключ это `variable name`, значение это `value`.
5. Ключ `API_KEY`: значение из 2-го шага `API Key`.
6. Ключ `TELEGRAM_AVAILABLE_TOKENS`: значение из 1-го шага токен.
7. Ключ `CHAT_WHITE_LIST`: Значение это ID-ки тех пользователей, которым бот может отвечать, пример `123456789,987654321`. Если вы не знаете свой ID, используйте команду `/new`, чтобы получить его в разговоре с созданным вами ботом.
8. Ключ `I_AM_A_GENEROUS_PERSON`: Не обязательная переменная.Если вы не понимаете, как получить ID, вы можете установить это значение в `true`, чтобы отключить функцию "белого списка" и разрешить доступ всем желающим.


### Шаг 5. Подключаем Базу данных
1. Перейдите в под категорию меню KV `Workers -> KV`.
2. Кликните `Create a Namespace` в правом верхнем углу. Назовите БД, например `Home-Workers-KV`.
3. Кликаем по пункту меню `Workers`, далее выбираем наш воркер.
4. В правом верхнем углу переходим в настройки `Setting -> Variables`.
5. Кликаем `Edit variables` в блоке `KV Namespace Bindings`.
6. Кликаем `Add variable`.
7. Ключ `DATABASE` и выбераем в селекте KV название только, что созданной БД.

### Шаг 6. Инициализация
1. Открываем [Cloudflare Workers](https://dash.cloudflare.com/?to=/:account/workers).
2. Кликаем по пункту меню `Workers`, далее выбираем наш воркер.
3. В блоке `Preview` кликаем по ссылке.
4. В открывшемся окне кликаем `You must >>>>> click here <<<<< to bind the webhook.`
5. Поздравляю, ваш чат бот настроен

### Шаг 7. Начните общаться в чате
<img style="max-width: 600px;" alt="image" src="https://user-images.githubusercontent.com/9513891/222917106-2bbc09ea-f018-489e-a7b9-317461348341.png">

1. Начните новый разговор с помощью команды `/new`. Если захотите сбросить контекст чата, также введите команду `/new`.
2. Modify user settings with the `/setenv KEY=VALUE` command, for example, `SETENV SYSTEM_INIT_MESSAGE=Starting now is Meow, and each sentence ends with Meow`.
3. Since all historical records are carried with each conversation, it is easy to reach the 4096 token limit, so clear the history by using the `/new` command when necessary.

## Автомтический деплой
1. Steps one, two, and three are for manual deployment.
2. Run `mv wrangler-example.toml wrangler.toml` and modify the corresponding configuration.
3. Run `npm install`.
4. Run `npm run deploy`.