Установка klipper на Elegoo Neptune 3 Pro c BIGTREETECH BTT PI V1.2 подключение через UART

Инструкция с картинками так же размещена https://telegra.ph/Ustanovka-klipper-na-Elegoo-Neptune-3-Pro-c-BIGTREETECH-BTT-PI-V12-podklyuchenie-cherez-UART-03-22

Два дня мытарств с разметкой и выпиливанием в передней панели отверстия для BTT PI, несколько раз всякими способами и по различным инструкциям попытки установить клиппер на нептун оказались ошибочными. Потом появился Константин из чата по нептунам и пояснил что большинство инструкций устарели, некоторые файлы в новых версиях называются по другому и прошивка от Feral для UART то же оказалась не рабочей. 
Вобщем я тут немного попытался упорядочить инфу на дату 03.2025 по процессу перепрошивки Elegoo neptune 3 Pro c Marlin на Klipper. Позже может опять что то измениться и эта инструкция будет неактуальной. 

https://www.printables.com/model/841472-elegoo-neptune-3-pro-klipper-upgrade-with-btt-pi-v - По этой ссылке берем инфу о комплектующих, способе подключения и размещения пишки в подвал принтера.

Необходимо 2 флеш карты. Одна пойдет в пишку где будет установлена ос (я использовал на 16гб), другая для прошивки принтера (использовал ту что шла в комплекте с принтером на 8гб). 
![1](https://github.com/user-attachments/assets/9c63940a-9bb4-4155-aa04-fc9ad6346b5e)
![2](https://github.com/user-attachments/assets/5aebe13f-13f6-452a-a35d-2e6f6249d4a7)

https://www.raspberrypi.com/software/ - Скачиваем эту программу для записи образа ОС на флешку.

https://github.com/bigtreetech/CB1/releases - скачиваем фаил ОС CB1_Debian12_Klipper_kernel6.6_20241219.img.xz
![3](https://github.com/user-attachments/assets/96388776-524f-44cb-af33-9cd2678ef8c3)


Запускаем программу и нажимаем выбрать ОС.
![4](https://github.com/user-attachments/assets/9321dcb4-1c01-4e2c-a1be-133285854535)

Выбираем use custom, находим наш дебиан, потом выбираем устройство на которое будем монтировать образ и нажимаем далее. Предложит сделать конфигурацию системы, можно попробовать, но я сделал все по дефолту.

![5](https://github.com/user-attachments/assets/81d54695-d595-4f0e-8077-059fa1dc217e)

Далее нужно внести небольшие изменения в некоторые файлы на флешке.
https://github.com/bigtreetech/cb1 - По этой ссылке берем инструкцию по конфигурированию ОС.
![6](https://github.com/user-attachments/assets/2a788767-c518-427b-b680-7fae58915822)

Для людей как и я не знающих что такое раскомментировать. Это значит вначале строки кода убрать #
Пока строки закомментированы, эти параметры не будут учитываться системой.
Некоторые строки в файле armbianEnv.txt по совету знатоков я закомментировал обратно:
![7](https://github.com/user-attachments/assets/e359848f-5d7f-41e0-9c79-ba85f25ff154)

Теперь мы уже должны видеть наш принтер в своей вай фай сети и знать его ip. Сделать это можете через веб интерфейс настройки роутера или как у меня родное от роутера приложение на телефоне для мониторинга.
Если же вы его так и не увидели проверяйте настройки в файле system.cfg
Именно там задаются настройки подключения к сети.
Крайний случай это подключение его по проводу к вашему роутеру и настройка через ssh по команде nmtui.
Через браузер заходим по ip адресу вашего принтера в панель управления.
Заходим в файл printer.cfg 
![8](https://github.com/user-attachments/assets/0c0ca1f0-773a-4e15-aae6-22e75381074a)

Полностью заменяем то что там написано на это: https://github.com/SerJ1us/Neptune3Pro/blob/main/printer.cfg
После замены справа, вверху будет кнопка сохранить и перезапустить.
Далее скачиваем прошивку принтера, для меня ее любезно собрал добрый товарищ из чата по нептунам https://t.me/ELEGOO_Neptune_3_and_4_series
https://github.com/SerJ1us/Neptune3Pro/blob/main/ZNP_ROBIN_NANO.bin

Пробовал брать прошивку 
https://github.com/TheFeralEngineer/Klipper-for-Elegoo-Neptune-series-3D-Printers/tree/main/Neptune%203%20Pro%20config/board%20firmware
Для соединения по UART она оказалась не рабочей. Напряжение на материнке принтера между контактами gnd-rx и gnd-tx не было. Потом опробовал прошить родным марлином, напряжение появилось. Далее Константин мне помог и собрал прошивку с которой напряжение появилось.

Итак, форматируем вторую флешку в FAT32, загружаем на нее прошивку и засовываем в мат.плату принтера. Родной дисплей на этом этапе уже можно отключить и убрать, информации он нам никакой не даст. 
Включаем принтер, ждем примерно 3 минуты и выключаем. 
Я на всякий случай засунул эту флешку в картридер и проверил произошла ли прошивка. После удачной прошивки расширение .bin должно измениться на .CUR
Далее вторая флешка для работы принтера нам не понадобится. А та что в пишке останется там навсегда.
После этих всех манипуляций вы должны через веб браузер лицезреть панель управления вашим принтером войдя по ip.
![9](https://github.com/user-attachments/assets/b216582d-34c2-4dbc-8d0c-27b27715d9de)

Если у кого то что то не получилось добро пожаловать в чат https://t.me/ELEGOO_Neptune_3_and_4_series 
Думаю что при адекватном запросе вы точно найдете там помощь.
Надеюсь мой опыт кому то пригодится и это вам поможет.





