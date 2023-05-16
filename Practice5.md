# Установка Wireguard сервера 

# Практическая работа будет выполнена с использованием скрипта:
```
https://pivpn.io  
```
## Конфигурация сервера
Для установки VPN сервера необходимо сначала обновить ядро 
```bash
sudo apt update
sudo apt install linux-headers-5.10.0-0.deb10.17-amd64
reboot
```
После перезапуска проверяем установленное ядро
```bash
uname -a
```
-  Версия ядра должна быть выше 5.6

После обновления и перезагрузки можно приступить к установке WireGuard сервера с помощью скрипта pivpn:
```bash
curl -L https://install.pivpn.io | bash
```

После установки WireGuard сервера необходимо добавить пользователя с помощью команды:
```bash
pivpn add
```
Будет создан конфигурационный файл wireguard клиента в папке `~/configs`. Этот файл можно передать на клиентский компьютер с помощью `Python сервера`.
В переменную `$PORT` нужно указать порт который проброшен через `linux-FW-2` на WAN интрефейс

``` bash
python3 -m http.server $PORT
```
## Конфигурация клиента

Для установки пакета wireguard необходимо сначала обновить ядро на клиенте

```bash
sudo apt update
sudo apt install linux-headers-5.10.0-0.deb10.17-amd64
reboot
```
После перезапуска проверяем установленное ядро
```bash
uname -a
```
-  Версия ядра должна быть выше 5.6
 
Далее необходимо добавить репозиторий в /etc/apt/source.list

` deb http://deb.debian.org/debian/ buster-backports main`

Далее небходимо обновить репозитории и установаить wireguard
```bash
sudo apt update -y && sudo apt install wireguard -y
```
после устновки wireguard необходимо скопировать конфиг в папку `/etc/wireguard`
```bash
mv config1.conf /etc/wireguard/wg0.conf
```

После чего активировать vpn соединение.
```bash 
sudo wg-quick up wg0
```
В результате выполнения команды выше может возникнуить ошибка `resovconf`,
если она возникла необходимо доустановаить пакет resolvconf и заново активироваить туннель.

```bash
sudo apt install resolvconf -y && sudo wg-quick up wg0

```
## Проверяем
$WIREGAURD_SERVER_INTERFACE_IP - IP адресс интерфейса wg0 и в обратную сторону
```bash 
ping $WIREGAURD_SERVER_INTERFACE_IP
```

В случае если тест пингом не пройден. Необходимо проверить настройку в конфигурации клиента Wireguard `Endpoint` она должна соотвествовать WAN интрерфейсу Linux-FW-2 


Успех!
