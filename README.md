# VPN
## Подготовка системы
### Для начала нам нужноз зайти на наш сервер и прокинуть наш ключ авторозизаци (RSA/ED25519) не только на нашего пользователя, но и пользователю root если не заходим через него.
### Для этого заходим под суперпользователя
```bash
sudo su
```
### Добавляем наш ключ
```bash
nano /root/.ssh/authorized_keys
```
### И выходим с нашего root пользователя командой `exit`

## Установка VPN
### Скачиваем и запускаем скрипт автоматической установки сервера IKEv2 предварительно предоставив доступ к скрипту
```bash
sudo wget https://raw.githubusercontent.com/neon0ff/VPN/main/setup.sh
```
```bash
sudo chmod u+x setup.sh
```
```bash
sudo ./setup.sh
```
### Во время выполненияя скрипта нас попросят ввести некоторые переменные
### Введите имя домена направленного на IP-адрес сервера
```bash
Hostname for VPN: yourdomain.com
```
### Имя пользователя VPN
```bash
VPN username: VPNuser
```
### Пароль пользователя
```bash
VPN password (no quotes, please): yourpassword
```
### Таймзону
```bash
Timezone (default: Europe/London): Europe/Moscow 
```
### Дальше скрипт запросит создать нового SSH-пользователя, этот шаг нельзя пропускать.
<sub> В дальнейшем мы не сможем пользоваться нашим пользователем и будем использовать созданного или root</sub>
### Когда он предложит использвать ключ авторизации скопированный с пользователя, соглашаемся

## Добавление новых VPN Клиентов
### Чтобы добавить нового пользователя в уже созданный сервер, отредактируем файл `/etc/ipsec.sectes.`
```bash
sudo nano /etc/ipsec.secrets
```
### В итоге мы жолны видеть что то вроде списка пользователей и их паролей
```bash
yourdomain.com : RSA "privkey.pem"
VPNuser1 : EAP "UserPass1"
VPNuser2 : EAP "UserPass2"
Alex : EAP "alexPassgg"
```
### После добавления пользователя выполним команду `sudo ipsec secrets` чтобы Strongswan перечитал конфиг.

### Исходник скрипта повзаимствован у [jawj](https://github.com/jawj/)
