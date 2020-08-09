## Build MicroPython for ESP8266 and ESP32 in Docker

![bash-small](https://user-images.githubusercontent.com/13176091/54089754-070c6c00-4375-11e9-8495-d06e9d5f3fe3.png)

Cборка ```FIRMWARE``` для ESP32 и ESP8266 выполняется в Docker где используются идентичные скрипты ```make.sh```. Скрипты различаются только параметрами передаваемыми ```esptool```.

Для сборки Docker, необходимо:

#### Для ESP8266
* [MicroPython port to ESP8266](https://github.com/micropython/micropython/tree/master/ports/esp8266#micropython-port-to-esp8266)
* [SDK for ESP8266/ESP8285 chips](https://github.com/pfalcon/esp-open-sdk)

#### Для ESP32
* [MicroPython port to the ESP32](https://github.com/micropython/micropython/tree/master/ports/esp32#micropython-port-to-the-esp32)
* [ESP-IDF](https://github.com/espressif/esp-idf#developing-with-esp-idf) - используется ```v3.3```, так как ```v.4x``` еще ```beta```
* [Xtensa Toolchain для ESP32](https://docs.espressif.com/projects/esp-idf/en/latest/get-started/linux-setup.html)

Кроме указанных выше зависимостей необходимо установить:    
*Для работы с микроконтроллерами*  
* [esptool GitHub](https://github.com/espressif/esptool)  
  [esptool PyPI](https://pypi.org/project/esptool/)  
  ```bash
  pip3 install esptool
  ```
* [setuptools PyPI](https://pypi.org/project/setuptools/)  
  ```bash
  pip3 install setuptools
  ```
*Для работы монитора порта UART необходим установить*     
* [pySerial PyPI](https://pypi.org/project/pyserial/)  
  [pySerial GitHub](https://github.com/pyserial/pyserial)  
  [pySerial Documentation](https://pyserial.readthedocs.io/en/latest/shortintro.html)   
  ```bash
  pip3 install pyserial
  ```


#### Сборка Docker для ESP8266
```bash
git clone https://github.com/gwvsol/ESP8266-ESP32-Script-to-build-MicroPython.git

cd esp8266

docker build -t esp8266:sdk .
```
#### Сборка Docker для ESP32
```bash
git clone https://github.com/gwvsol/ESP8266-ESP32-Script-to-build-MicroPython.git

cd ESP8266-ESP32-Script-to-build-MicroPython/esp32

make build
```

#### Использование

```bash
docker run -it --name esp8266 --rm -v $(pwd):/var/fw esp8266:sdk

или

cd ESP8266-ESP32-Script-to-build-MicroPython/esp32

make start 
```

##### Работа со скриптом сборки

```bash
make.sh -h

############################################ HELP ###############################################
/usr/local/bin/make.sh -c  | Очистка SDK
/usr/local/bin/make.sh -m  | Cборка прошивки
/usr/local/bin/make.sh -cm | Очистка SDK и сборка прошивки
/usr/local/bin/make.sh -h  | Справка по работе со скриптом
#################################################################################################

```
#### Запись прошивки 
Для записи прошивки используется скрипт ```tools.sh```

```bash
./tools.sh -h

############################################ HELP ###############################################
./tools.sh -m   | Moнитор порта UART
./tools.sh -e   | Очистка ESP32
./tools.sh -w   | Только запись прошивки, необходимо передать имя прошивки
./tools.sh -ew  | Очистка ESP32 и запись новой прошивки, необходимо передать имя прошивки
./tools.sh -i   | Информация об ID ESP32
./tools.sh -if  | ID Flash ESP32
./tools.sh -mac | MAC адрес ESP32
./tools.sh -h   | Справка по работе со скриптом
#################################################################################################

```
Скрипт ```tools.sh``` необходимо разместить в директории проекта.   
Для работы монитора порта UART необходим установить ```pyserial```  

***