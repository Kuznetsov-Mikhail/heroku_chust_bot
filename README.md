Репозиторий содержит два приложения:
* телеграм бот для выполнения переноса стиля
* скрипт для выполнения переноса стиля вручную

# Telegram бот для выполнения переноса стиля (@chustransfer_bot) 
**Профиль бота в telegram: @chustransfer_bot**

## Взаимодействие с ботом
Бот способен обрабатывать команды:
1. **/start** - отображение приветствия, справка;
1. **/help** - отображение справки;
1. **/statistics** - отображение количества обработанных изображений от всех пользователей;

Для управления ботом следует следовать инструкциям в сообщениях.
Для выполнения доступны действия:
1. **Выбрать фото** - в ответ бот предложит прислать ему фото.
1. **Обработать фото** - предварительно необходимо задать фото, в ответ бот предложит выбрать стиль.
И после выбора стиля выполнит перенос. Стиль можно выбрать как стандартный, так и собственный
прислав боту соответствующее изображение. После обработки фото можно оставить отзыв о работе бота,
который будет передан разработчику.
1. **Посмотреть стандартные стили** - в ответ бот вышлет сообщения с изображениями стандартных стилей.
1. **Посмотреть случайный пример** - в ответ бот вышлет сообщения с примером переноса стиля.
  

## Запуск
Для запуска бота необходимо запустить на исполнение файл 'main.py'.

Входных параметров у скрипта нет, но для корректной работы неоходимо задать
несколько переменных окружения:
* TOKEN - токен для подключения к боту;
* PORT - порт на котором будут приниматься запросы от telegram;
* WEBHOOK_HOST - хост, который будет зарегистрирован как 
['webhook'](https://groosha.gitbook.io/telegram-bot-lessons/chapter4) для бота.
* PROC_IMSIZE (опционально) - размер изображений, которые будут обрабатываться,
чем он больше, тем дольше выполняется обработка (значение по умолчанию 256).
* OUT_IMSIZE (опционально) - размер выходных изображений. 
Обработанные изображения масштабируются к этому размеру (значение по умолчанию 512).
* FEEDBACK_CHAT_ID (опционально) - ID telegram чата, в который будут перенаправляться отзывы.
Если не указывать, отзывы не будут перенаправляться никуда.
* WAITING_TIME (опционально) - примерное время обработки одного изображения, используется для
предупреждения пользователя о времени ожидания, минуты (значение по умолчанию 5 минут).
* EXAMPLES_URL (опционально) - url zip архива с примерами ([значение по умолчанию]('https://drive.google.com/u/0/uc?id=1hkvqijfwePh0DcMSrP8cBdGPPLqP9yvM&export=download')).

## Особенности реализации
Взаимодействие с telegram происходит с использованием [aiogram](https://docs.aiogram.dev/en/latest/).
В основу разработки лёг репозиторий [deploy-your-bot-everywhere](https://github.com/deploy-your-bot-everywhere).
Взаимодействие происходит по сценарию ['webhook'](https://groosha.gitbook.io/telegram-bot-lessons/chapter4).

Перенос стиля выполняется по 
[классическому алгоритму](https://pytorch.org/tutorials/advanced/neural_style_tutorial.html)
с использованием библиотеки [pytorch](https://pytorch.org/).

Бот разработан с учётом того, что он будет запущен на платформе [heroku](https://dashboard.heroku.com/apps)
в бесплатном режиме.

# Скрипт для выполнения переноса стиля вручную

Скрипт написан для проверки работоспособности математической составляющей бота.

Для запуска необходимо запустить на исполнение файл 'core_test.py'.

Входных параметров у скрипта нет. Изображения стиля и содержания указываются
непосредственно в скрипте.

Примеры изображений стилей и содержаний для тестирования расположены в директориях
images и styles.

Для переноса стиля используется 
[классический алгоритм](https://pytorch.org/tutorials/advanced/neural_style_tutorial.html)
и библиотека [pytorch](https://pytorch.org/).

# Пояснения к содержимому репозитория
* examples - директория с примерами работы алгоритма.
    Должна содержать тройки файлов изображений **content***.\*, **style***.\*, **result***.\*, 
    где вместо * ставится номер примера и расширение. Изображения сожержат примеры содержания, стиля и результата.
    При первом запуске директория заполняется из автоматически скачиваемого архива.
* images - директория с примерами изображений содержания;
* styles - директория со стандартными изображениями стилей;
* tmp - директория для хранения временных файлов, чистится при каждом запуске;
* core.py - реализация методов и классов для выполнения переноса стиля;
* core_test.py - скрипт для выполнения переноса стиля вручную (вызывает core.py);
* main.py - скрипт с реализацией бота;
* Procfile - файл со строкой запуска для heroku;
* README.md - краткое описание проекта;
* requirements.txt - список зависимостей для heroku;
* runtime.txt - указание интерпретатора для heroku;
* utils.py - вспомогательные функции для работы бота.