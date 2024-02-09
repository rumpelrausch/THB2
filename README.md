# BTHome THB2, BTH01, TH-05
Custom firmware for Tuya [THB2](https://pvvx.github.io/THB2), [BTH01](https://pvvx.github.io/BTH01/), [TH05_V1.4](https://pvvx.github.io/TH-05). 

* Тестовый [PHY62x2BTHome.html](https://pvvx.github.io/THB2/web/PHY62x2BTHome.html)

Прошивка Boot-OTA для THB2 (файл bin\BOOT_THB2_v12.hex). 

Прошивка Boot-OTA для BTH01 (файл bin\BOOT_BTH01_v12.hex). 

Прошивка Boot-OTA для TH05_V1.4 (файл bin\BOOT_TH05_v12.hex). 

## Основные характеристики:

! _При настройках по умолчанию_ !

* Интервал BLE рекламы в формате BTHome v2 равен 5 секундам.
* Опрос датчика влажности и температуры производится  каждый второй интервал BLE рекламы - период 10 секунд.
* Измерение напряжения батареи происходит каждую минуту.
* Кнопка используется для быстрого подключения к старым BT-адаптерам. Нажатие кнопки переключает интервал BLE рекламы на более короткий период (1562.5 мс). Действие продолжится 60 секунд, затем интервал восстановится на установленный в настройках.
* Измеренное среднее потребление от источника в 3.3В при сканировании термометров THB2 и BTH01 в пассивном режиме составляет до 8 мкА. Для TH05_V1.4 среднее потребление около 23 мкА - [таков ток установленных компонентов](https://github.com/pvvx/THB2/issues/8#issuecomment-1908982171).
* Запись итории каждые 30 минут
* Интервал соединения с учетом Connect Latency - 900 мс
* Поддерживаемые сенсоры температуры и влажности: AHT30, CHT8305, CHT8215, CHT8310

## История версий:

| N | Описание |
|---|--- |
| 1.0 | Первая релизная версия |
| 1.1 | Добавлен триггер - вывод TX2 срабатывающий по установленным значениям температуры и/или влажности с гистерезисами. Передача состояния вывода RX2 при connect. Для термометров с экраном добавлен показ смайлика с "комфортом". Дополнены: изменение имени и MAC устройства. |
| 1.2 | Обработка и передача событий open/close со счетчиком с вывода маркированного "RX2" (для THB2 - "RX1"). |

## Прошивка:

Прошить устройство програмой Boot-OTA возможно через USB-COM адаптер с выходами на 3.3В:

1. Соединить GND, TX, RX, RTS–RESET, VCC (+3.3B).

| Адаптер | Устройство |
|---|---|
| GND | -Vbat |
| +3.3В | +Vbat |
| TX | RX1 |
| RX | TX1 |
| RTS | RESET |

Название контактов на устройстве смотреть в [THB2](https://pvvx.github.io/THB2), [BTH01](https://pvvx.github.io/BTH01/), [TH-05_V1.4](https://pvvx.github.io/TH-05). 

2. Запустить:
```
python3 rdwr_phy62x2.py -p COM11 -e -r wh BOOT_xxx_vxx.hex
```
3. Прошивка Boot-OTA завершена. Устройство работает.
4. Далее загружаем полную версию по OTA в [PHY62x2BTHome.html](https://pvvx.github.io/THB2/web/PHY62x2BTHome.html).

Дополнительно:

* Для предварительного стирания всей Flash используйте опцию `-a`.

* Для предварительного стирания рабочей области Flash используйте опцию `-e`.

## Сохранение оригинальной прошивки.

1. Соединить GND, TX, RX, RTS–RESET, VCC (+3.3B).
2. Запустить:
```
python3 rdwr_phy62x2.py -p COM11 -r rc 0x11000000 0x80000 ff_thb2.bin
```
3. Полученный файл ff_thb2.bin сохранить.

## Восстановление оригинальной прошивки.

1. Взять сохраненный файл ff_thb2.bin оригинальной прошивки.
2. Соединить GND, TX, RX, RTS–RESET, VCC (+3.3B).
3. Запустить:
```
python3 rdwr_phy62x2.py -p COM11 -b 1000000 -r we 0 ff_thb2.bin
```
Не все адаптеры USB-COM поддерживают 1Mbit. Тогда удалите опцию `-b 1000000` или выберите другой Baud rate.

4. Прошивка зашита. Устройство работает.


## Сборка прошивки.

Для сборки прошивки используется GNU Arm Embedded Toolchain.

Для работы в Eclipce используете импорт проекта и установите toolchain.path.

Дополнительная информация по чипам [PHY62xx](https://github.com/pvvx/PHY62x2).
