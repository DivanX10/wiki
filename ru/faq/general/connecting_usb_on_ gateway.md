# Подключаем USB накопитель для расширения памяти

[Стартовая страница WiKi](https://github.com/DivanX10/wiki#readme)

## Вступление

На шлюзе встроенный объем хранилища составляет 256 Мб и этого может быть не достаточно, но если подключить к usb разъему флэшку или SSD/HDD, тогда количество дополнительных программ будет ограничено только объемом usb накопителя. Для расширения памяти будем использовать монтирование usb накопителя. Имеется возможность подключить накопители, как с USB интерфейсом, так и различные преходники. Подтверждена работа с: USB/SD-microSD, USB/SSD, USB/HDD-Sata, USB/HDD-IDE и даже USB/SSD с интерфейсом PCI-e. 

> **В шлюзах Xiaomi (dgnwg05lm) и Aqara (zhwg11lm) (далее будет использоваться общее название - "Шлюз") установлены контроллеры USB v2. Это не значит, что нельзя подключать накопители USB3, но ждать от них высоких скоростей не стоит, ведь штатное железо не позволяет реализовать преимущества скорости USB3. Они просто будут работать в режиме USB2. Тем не менее, подключение USB Flash или HDD окупится новыми возможностями.**
> **Описание процесса составлено из простейших шагов. Предложены способы исключающие ошибку. Тем не менее, соблюдаю традицию и напоминаю, что всё, что Вы делаете, Вы делаете Сами, на Свой страх и риск, и делаете это по Собственному желанию, Без принуждения.**

## Цели (возможности и желания)
Шлюзы, которые рассмотрены в этой статье выполнены производителями в компактном корпусе. Это ограничивает выбор USB накопителей, которые мы хотим разместить внутри шлюза по габаритам.
На этом этапе необходимо опрелелиться:
*  **a)** Всё рассчитано и будем установливать компактное устройство внутрь шлюза. При этом не забывая о невеликом ресурсе компактных устройств, например USB Flash или microSD и их низкой скорости;
*  **b)** Решено использовать надёжный HDD/SSD и релизовать все преимущества накопителя с большим ресурсом и объёмом. Нужно только найти для материнской платы шлюза более вместительный корпус, в который установятся HDD с адаптером, в штатном корпусе будет установлена розетка USB или из корпуса будут выведены провода USB и подключен внешний накопитель. Вам решать, каким способом подключить внешний накопитель данных.
![usb connection and configuration 1-2](https://user-images.githubusercontent.com/64090632/143878197-46a834c0-ebc2-48e5-b9e9-a8338cc2ecb1.jpg)

> **Большинство дальнейших шагов будут общими. Но там, где встретятся отличия, в инструкции появятся сответствующие подпункты "a" и "b". Обратите на это внимание и следуйте согласно вышему выбору.**

## Подключение USB.
Подключаем внешний USB как показано на **Фото 2** Для включения режима OTG необходимо замкнуть USB-GND и USB-ID. Для питания вашего устройства необходимо на контакт USB+5v подать +5v. Их удобно взять с колодки питания контакт+5v **Фото 2** (требуется уточнение). 

![Фото 2](----------- Нужно вставить ссылку на фото usb-otg.png ---------------------)
![usb-otg](https://user-images.githubusercontent.com/64090632/143878257-3aea3718-0781-4bfb-9de5-338106b82e48.png)

![Фото 3](----------- Нужно вставить ссылку на фото power-RGB.png ---------------------)
![power-RGB](https://user-images.githubusercontent.com/64090632/143878306-96a662b8-4ac6-4bae-9edf-99ece75880a2.png)


## Подготовка файловой системы внешнего накопителя.
> **Файловую систему можно подготовить и в самой системе с уже подключенным накопителем, но рекомендую позаботиться об этом зарание и проделать это на компьютеле, в любой, удобной вам программе. Покажем на примере программы AOMEI Partition Assistant. Она бесплатна для некоммерческого использования и обладает достаточным функционалом**
* **1)** Вставляем в порт USB компьютера накопитель и запускаем программу AOMEI Partition Assistant.
* **2)** Убеждаемся, что наш диск появился в списке. Поочерёдно кликаем правой кнопкой мыши на всех его разделах и выбираем "Удаление раздела".
![Фото 4](----------- Нужно вставить ссылку на фото usb-0.png ---------------------)
![usb-0](https://user-images.githubusercontent.com/64090632/143878572-89f55642-0f9f-4e92-80dd-05cd63725f5d.png)

* **3)** Разбиваем чистый диск на необходимые разделы. Для этого кликаем ПКМ по неразмеченной области и выбираем "Создание раздела". В появившемся окне выбираем рузмер раздела. Буква диска нам не интересна, поэтому не трогаем. Файловую систему (EXT2 или EXT4) выбираем в соответствии с приведёнными ниже рекомендациями.
  * **a)** Если вы решили разместить накопитель внутри штатного корпуса, то вероятно это будет USB-Flash или USB-microSD. У них довольно низкий ресурс эксплуатации, поэтому рекомендую разместить раздел Swap ближе к концу диапазоня памяти.
   
    Последовательность такая:
     * Начинаем с создания раздела **Overlay**. Назначаем ему размер от 1 до 10Gb. Форматирование выбираем **EXT4**;
     * Если размер памяти накопителя позволяет, то создаём **раздел для хранения данных**.  Назначаем ему размер почти на всё оставшееся место, **оставив примерно 0,5Gb свободного места на диске**. Форматирование выбираем **EXT4**;
     * Должно остаться **примерно 0,5Gb**. Создаём на этом месте раздел для **Swap** и форматируем его в **EXT2**
![Фото 5](----------- Нужно вставить ссылку на фото usb-sd.png ---------------------)
![usb-sd](https://user-images.githubusercontent.com/64090632/143878760-fbb45958-a954-4c61-85e3-bfece5a9add1.png)

  *  **b)** Если выбрали в качестве USB накопителя HDD или SSH, то это позволит в полной мере реализовать возможности системы, а работа с устройством у которого большой ресурс эксплуатации и быстродействия продлит ваши спокойные дни. 
  
     Делаем так:
      * Начинаем с создания раздела **Swap**. Назначаем ему размер около 0,5Gb. Форматирование выбираем **EXT2**;
      * Вторым оздаём **Overlay**.  Назначаем ему место, **размером от 1 до 10Gb**. Форматирование выбираем **EXT4**;
      * На всё оставшееся место размещаем раздел для **Данных** и форматируем его в **EXT4**
      
![Фото 6](----------- Нужно вставить ссылку на фото usb-hdd.png ---------------------)
![usb-hdd](https://user-images.githubusercontent.com/64090632/143878827-de5ebaf4-7617-4016-b4fb-a07edad0717b.png)


## Устанавливаем необходимые пакеты
> **Часть пакетов уже установлена в актуальных версиях размещённых в https://openlumi.github.io/releases/, но всё-таки стоит выполнить эту команду целиком. Затем перезагрузить**
```
opkg update
opkg install block-mount kmod-usb-core kmod-usb-storage
```

## Настраиваем USB в системе.
В LuCI идём "**System**" - "**Mount Points**". На странице поочерёдно нажимаем "**Generate Config**", затем "**Mount attached devices**"
Внизу странице, в разделе **Mount Points** должны появиться разделы диска, который мы подготовили и подключили.
> **Если разделы диска не появились, то проверяем всё по уже выполненным пунктам, обращая особое внимание на качество и правильность пайки. Иногда стоит поменять местами D+ и D-. Проверяйте по pinout, не обращайте внимание на цвет проводов в вашем кабеле.**
Попробуйте принудительно перевести систему в режим **Host**
```
echo host > /sys/class/udc/ci_hdrc.0/device/role
```

> **Если разделы появились, то переходите к следующему пункту. Если разделы диска не появились, то ищите неисправность. Примите условие: - Всё должно работать.**
## Все разделы успешно определились в системе. Займёмся их функционалом.
**Подключение раздела "Swap"**

  **a)** Итак, у вас флешка и раздел Swap подготовлен в конце диска. 
  Проверьте соответствие номера раздела на вашем диске, при необходимости исправьте и выполние а терминале следующее:
```
mkswap /dev/sda3
swapon /dev/sda3
```
  **b)** Проверьте соответствие номера раздела на вашем диске, при необходимости исправьте и выполние а терминале следующее:
```
mkswap /dev/sda1
swapon /dev/sda1
```
В LuCI идём "**System**" - "**Mount Points**". На странице в низу появился раздел "**Swap**", в этом разделе нажимаем "**Add**" Появится окно **Mount Points - Swap Entry**. Здесь ставим галку "Enabled", не трогаем верхнее и среднее окна, а в нижнем выбираем раздел под Swap.
![Фото 7](----------- Нужно вставить ссылку на фото usb-3.png ---------------------)
![usb-3](https://user-images.githubusercontent.com/64090632/143879842-7cd9ed0c-b905-4f16-964f-5af53bfe6120.png)

Поочереди нажимаем "Save" "Save & Apply".
В LuCI, на вкладке "Status" - "Overview" должна появиться шкала свободного места "Swap free".
![Фото 8](----------- Нужно вставить ссылку на фото usb-4.png ---------------------)
![usb-4](https://user-images.githubusercontent.com/64090632/143879872-68384090-a47f-4aa8-9eaf-c7fad9d7ad67.png)

**Подключение раздела "Overlay"**
В LuCI идём "**System**" - "**Mount Points**". На странице ищем раздел "**Mount Points**", в этом разделе нажимаем "**Add**" Появится окно **Mount Points - Mount Entry**. Здесь ставим галку **"Enabled"**, не трогаем два верхних окна окна, в окне **Device"** выбираем раздел, приготовленный Вами под Overlay, а в нижнем окне выбираем **/overlay**. Проверяем и нажимаем "Save" и "Save & Apply".
**Подключение раздела для хранения данных**
В LuCI идём "**System**" - "**Mount Points**". На странице ищем раздел "**Mount Points**", в этом разделе, на пункте раздела, подготовленного под хранилище нажимаем "**Edit**" Появится окно **Mount Points - Mount Entry**. Здесь ставим галку "**Enabled"**, и в обоих окнах выбираем раздел для данных. Обычно он самый большой. Проверяем и нажимаем "Save" и "Save & Apply".
**Перезагружаем систему.**
В LuCI или в терминале смотрим и убеждаемся, что все разделы корректно определились.

> **Если разделы диска не подхватились, то повторите этот раздел инструкции**
## Далее используем ресурсы на своё усмотрение.

## Немного разгрузим оперативную память

Идём в **"System"** - **"Software"** - **"Configure opkg"** и строку **"lists_dir ext" приводим к следующему виду**:
```
/usr/lib/opkg/opkg-lists
```
Теперь opkg-lists будет писаться на hdd, немного освободится оперативка и списки будут сохраняться после перезанрузки.
Но, если давненько их не обновляли, перед установкой пакетов стоит сделать "opkg update".

В файле
```
/etc/init.d/homeassistant
```
в строку "procd_set_param command hass --config "
Пропишим следующее:
```
/etc/homeassistant --log-file /etc/homeassistant/home-assistant.log --log-rotate-days 3
```
В файле 
```
/etc/homeassistant/configuration.yaml
```
Сотрём или закомментируем строку:
```
#  db_url: 'sqlite:///:memory:'
```
А сохранение количество дней в базе данных можем теперь позволить больше:
```
  purge_keep_days: 7
```
**Оперативная память освобождена от БД и логов**

## Настройка записи логов для Zigbee2MQTT

Открываем файл `configuration.yaml` расположенный по пути `/etc/zigbee2mqtt/configuration.yaml`, находим строку `log_directory: /tmp/log/zigbee2mqtt/%TIMESTAMP%` и заменяем на `log_directory: /usb/log/zigbee2mqtt/%TIMESTAMP%`


Для примера выкладываю конфиг zigbee2mqtt `/etc/zigbee2mqtt/configuration.yaml`. Он может отличаться от вашего, но этот пример нужен для понимания, куда надо добавить строку `log_directory: /usb/log/zigbee2mqtt/%TIMESTAMP%` . Если по какой-то причине у вас нет строки `log_directory: /tmp/log/zigbee2mqtt/%TIMESTAMP%`, то найдите строку `log_level: info` и под этой строки добавьте `log_directory: /usb/log/zigbee2mqtt/%TIMESTAMP%`


```
# Home Assistant integration (MQTT discovery)
homeassistant: true

# allow new devices to join
permit_join: false

# MQTT settings
mqtt:
  # MQTT base topic for zigbee2mqtt MQTT messages
  base_topic: zigbee2mqtt
  # MQTT server URL
  server: 'mqtt://localhost'
  # MQTT server authentication, uncomment if required:
  # user: my_user
  # password: my_password

# Serial settings
serial:
  port: /dev/ttymxc1
  adapter: zigate
advanced:
  baudrate: 1000000
  log_level: info
  log_directory: /usb/log/zigbee2mqtt/%TIMESTAMP%
  # Optional: Log file name, can also contain timestamp, e.g.: zigbee2mqtt_%TIMESTAMP%.log (default: shown below)
  log_file: log.txt
  # Optional: Log rotation (default: shown below)
  log_rotation: true
  # Optional: Output location of the log (default: shown below), leave empty to supress logging (log_output: [])
  # possible options: 'console', 'file', 'syslog'
  log_output:
   # - console
    - file

devices: devices.yaml
groups: groups.yaml

frontend:
  port: 8090
experimental:
  new_api: true
```

## Источники
* [Чат Xiaomi_gw_Hack](https://t.me/xiaomi_gw_hack)
* [USB Хранилище](https://openwrt.org/ru/doc/howto/usb.storage)


*Автор статьи @Clear_Highway*







