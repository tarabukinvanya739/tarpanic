<h4>Настройка телеграм бота</h4>
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

  <h4>Открытие rpc ноды</h4>

nano ~/.haqqd/config/config.toml
в разделе [rpc] (примерно 109 строка)
поменять laddr = "tcp://127.0.0.1:26657" на laddr = "tcp://0.0.0.0:26657"
И перезагружаем ноду
  
  <h4>Установка PANIC BOT</h4>

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
node_rpc_url = http://<ip>:26657

pipenv update
pipenv run python run_setup.py

отвечаем 5 раз n, потом когда спрасит "Telegram commands are already set up. Do you wish to clear the current config? (Y/n)" отвечаем Y
потом n, n, n, n

проверяем запуск:
/usr/local/bin/pipenv run python /$USER/tarpanic/run_alerter.py

должно выйти Node monitor (Moniker) started.

создаем сервисный файл для запуска в фоновом режиме:
  
<code>
  
    printf "[Unit]
    Description=P.A.N.I.C.
    After=network.target
    StartLimitIntervalSec=0

    [Service]
    Type=simple
    Restart=always
    User=$USER
    TimeoutStopSec=90s
    WorkingDirectory=/$USER/tarpanic
    ExecStart=/usr/local/bin/pipenv run python /$USER/tarpanic/run_alerter.py
  
    [Install]
    WantedBy=multi-user.target" > /etc/systemd/system/panic.service 
  
 </code>
  
Запускаем и смотрим логи:
  sudo systemctl daemon-reload
  sudo systemctl enable panic
  sudo systemctl start panic
  sudo journalctl -u panic -f
  
Здесь тоже должно выйти "Node monitor (Moniker) started"
