Вы должны иметь телеграм бота, а именно "HTTP API" это ваш TOKEN и chat_id, если его у Вас нет, порядок создания:
Открываете телеграм
В поиске ищете @botFather
присоединяетесь
Вводите /newbot
Придумываете имя бота на латинском и должен заканчиваться на _bot
Например, haqq_marina_bot
botFather даст Вам HTTP API  вида
5611744630:AAFF9pC9674111D_oDTSteJQfS8m7_O0bcQ
и еще вам нужен chat_id
его можно получить водив на браузере https://api.telegram.org/bot<HTTP API>/getUpdates
ответбудет примерно таким:

{"ok":true,"result":[{"update_id":461136221,
"message":{"message_id":5,"from":{"id":1410371666,"is_bot":false,"first_name":"\u0410\u043d","last_name":"\u043c\u0438\u0441\u0441","username":"nadyatarabukina","language_code":"ru"},"chat":{"id":1410371666,"first_name":"\u0410\u043d","last_name":"\u043c\u0438\u0441\u0441","username":"nadyatarabukina","type":"private"},"date":1665454393,"text":"/start","entities":[{"offset":0,"length":6,"type":"bot_command"}]}},{"update_id":461136222,

находим '"from":{"id":1410371666,', ваш id_chat: 1410371666

Установка PANIC BOT

Проверяем, есть ли на сервере python
python3 --version
с дистрибутива ubuntu 20.04 python идет в коробке с ОС

Устанавливаем PIP и Redis Database Server
sudo apt-get install python3-pip redis-server -y

Устанавливаем Pipenv и включаем Redis
sudo pip3 install pipenv
sudo systemctl enable redis-server.service

cd
git clone https://github.com/tarabukinvanya739/tarpanic
cd tarpanic
git checkout master

обновляем pip и запускаем установку

сразу настроим конфиги вручную:
nano config/user_config_main.ini
Исправляем строчки
строка1 - любое имя вместо Name:
unique_alerter_identifier = Name 
строка6 - ставим TOKEN с телеграм бота:
bot_token = <TOKEN>
строка7 - ставим id_chat с телеграм бота
bot_chat_id = <id_chat>
строка32 - ставим TOKEN с телеграм бота:
bot_token = <TOKEN>
строка33 - ставим id_chat с телеграм бота
bot_chat_id = <id_chat>

сохраняемся и закрываем, CTRL+X,Y

nano config/user_config_nodes.ini
в строке 2 вводим свой moniker
в строке 3 вводим rpc своей ноды, например
node_rpc_url = 201.144.152.196:26657

pipenv update
pipenv run python run_setup.py

отвечаем 5 раз n, потом когда спрасит "Telegram commands are already set up. Do you wish to clear the current config? (Y/n)" отвечаем Y
потом n, n, n, n


