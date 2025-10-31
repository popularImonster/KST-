# Задания 1-9

# Задания 1-9

 Для начала я установил VirtualBox и положил туда образ Ubuntu Server 24.04.3<br><br><img width="934" height="451" alt="image" src="https://github.com/user-attachments/assets/b50b668c-036e-46e0-aab7-8e4bb0a0990c" /><br>
 
 
 Вошли в виртуальную машину:<br><br><img alt="image" src="https://github.com/user-attachments/assets/93e5a04a-8919-451b-be61-a37e4c3ad39e" /><br>


 Написали команду `sudo apt update`:<br><br><img alt="image" src="https://github.com/user-attachments/assets/636507d7-14f9-4acb-a670-fa27865de8d2" /><br>

 
 Для первого задания нам необходимо установить net-tools, дабы определить текущие сетевые настройки:<br><br><img width="974" height="555" alt="image" src="https://github.com/user-attachments/assets/c7b7f17b-a0c1-485c-aeb2-f6f2a6f415b2" /><br>

 
 Посмотрим настройки через `ifconfig`:<br><br><img width="974" height="479" alt="image" src="https://github.com/user-attachments/assets/e6b2e7d0-abee-4e48-a7bc-c75d3f185d35" /><br>

 
 Узнаем версию ОС через команду `cat /etc/os-release`:<br><br><img width="974" height="427" alt="image" src="https://github.com/user-attachments/assets/2c9c4d85-aa66-4e14-8f3a-1ce627bb8984" /><br>

Займемся 1 заданием. Необходимо настроить ip адрес, шлюз, dns и имя хоста.<br>

Изменим конфигурацию в файле 50-cloud-init.yaml:<br><br><img width="663" height="478" alt="image" src="https://github.com/user-attachments/assets/dc74be4b-4de0-4e6c-9082-7cf80d84d742" /><br><br>

Сохраним изменения через `sudo netplan apply`.<br>
Пропишем имя хоста через команду `sudo hostnamectl set-hostname ubuntu-server` и сразу проверим:<br><br><img width="974" height="124" alt="image" src="https://github.com/user-attachments/assets/a54676fb-37b5-4bac-b24c-7422c755af5e" /><br><br>

Перейдём к 2 заданию, где необходимо обновить систему до последнего обновления. Для этого нам помогут команды `sudo apt update` и после `sudo apt upgrade`:<br><br>
<img width="974" height="829" alt="image" src="https://github.com/user-attachments/assets/f9037d62-4fce-466e-ab45-c78a3a60b6f9" /><br><br>

После этого перезагрузимся через `sudo reboot`.<br>
Для администрирования сервера нам необходимы минимальные утилиты, а именно:<br>
Для работы с файлами:<br>
`lsd` - современная версия ls<br>
`tree` - просматривать структуру каталогов <br>
`mc` - файловый менеджер в терминале <br>
Редактировать файлы: <br>
`vim` – сложный и мощный текстовый редактор <br>
`nano` - простой редактор для быстрых изменений <br>
Для мониторинга системы:<br>
`htop` - просмотр процессов и загрузки системы<br>
`ncdu` - анализ использования дискового пространства<br> 
Для работы с сетью: <br>
`curl` - тестирование http-запросов<br>
`wget` - скачивание файлов<br>
`iproute2` (ip, ss) – для работы с сетью <br>
Для архивов:<br>
`tar` - создание и распаковка архивов <br>
`unzip/zip` - работа с zip-архивами <br>
Безопасность и базовые админ-инструменты <br>
`ufw` - базовая настройка брандмауэра<br>
`fail2ban` - защита SSH от перебора паролей.<br>
Когда определились с набором утилит сделаем команду <br>
`sudo apt update sudo apt install -y lsd tree mc vim nano htop ncdu curl wget iproute2 ping tar unzip zip ufw fail2ban`<br>
Получили набор данных утилит:<br><br>
<img width="974" height="381" alt="image" src="https://github.com/user-attachments/assets/9b8434d5-06f9-4a69-992e-c428bc14b30b" /><br><br>

Перейдём к 3 заданию. Нужно настроить часовой пояс.<br><br>
Проверим timedatectl status и увидим:<br><img width="747" height="261" alt="image" src="https://github.com/user-attachments/assets/8c75a83f-a3e3-4447-8ead-49782f571cf2" /><br><br>

Время неправильное, необходимо с помощью команды timedatectl list-timezones | grep Yeakaterinburg найти часовой пояс Екатеринбурга и поставить его командой `timedatectl set-timezone Asia/Yekaterinburg`.  <br>
Заодно проверим командой timedatectl:<br><br><img width="974" height="272" alt="image" src="https://github.com/user-attachments/assets/bf5c552b-f438-4e4e-aba9-6ecfd9596bd0" /><br><br>

Проверим синхронизацию по NTP командой `timedatectl show -p NTPSynchronized`:<br><br><img width="816" height="328" alt="image" src="https://github.com/user-attachments/assets/7d7c7203-b3a7-42f5-a5b8-fd07c62140e3" /><br><br>

Перейдём к 4 заданию:<br>
Установим необходимое ПО через команду `sudo apt install openssh-server` и проверим его работу через команду `sudo systenctl status ssh`:<br><br>
<img width="974" height="417" alt="image" src="https://github.com/user-attachments/assets/209ab6d1-e9bf-4d99-b338-5c3df52c106a" /><br><br>
После этого сделаем бэкап и запишем изменения в конфиг файл командой `sudo nano /etc/ssh/sshd_config`:<br><br>
1)	Отключаем вход под root, прописав PermitRootLogin no<br>
2)	Пропишем в конец AllowUsers vboxuser<br>
3)	Сделаем авторизацию по ключам, прописав PasswordAuthentication no<br>
4)	Пропишем Port 2222<br>
5)	600 для владельца доступ<br>
6)	Ограничим время авторизации до 1 минуты, прописав LoginGraceTime 1m<br>
7)	Пропишем LogLevel VERBOSE для подробных логов.<br><br>
<img width="974" height="915" alt="image" src="https://github.com/user-attachments/assets/c3195053-f1ec-4b31-97b2-51a85bf1b28d" /><br>
После данных изменений необходимо прописать `sudo systemctl restast ssh`.<br><br>

Перейдем к 5 заданию. Нам необходимо установить и настроить `fail2ban`. Установим его через `sudo apt install fail2ban -y`.<br>
Теперь можем сконфигурировать  новый файл по адресу `/etc/fail2ban/`. Менять какие-либо параметры напрямую в файле `jail.conf` не рекомендуется. Вместо этого необходимо создадим новый файл `jail.local`, разместив его в той же директории.<br>
Пропишем `sudo touch /etc/fail2ban/jail.local` для создания и `sudo nano /etc/fail2ban/jail.local` для редактирования файла. В самом файле опишем настройки, например такие:<br><br>
[DEFAULT]<br>
ignoreip = 95.111.123.21<br>
[sshd]  <br>
enabled  = true  <br>
findtime = 120  <br>
maxretry = 3  <br>
bantime = 43200  <br><br>
<img width="430" height="381" alt="image" src="https://github.com/user-attachments/assets/94c88999-7c84-4a84-aaff-6b70bd531e0c" /><br>
Сохраним и применим данные настройки через `sudo systemctl restart fail2ban.service`.<br>
Проверим применяются ли правила командами `sudo iptables -L` и `sudo fail2ban-client status`.<br>


Перейдем к заданию 6. <br>
Проверим статус nftables через команду sudo systemctl status nftables:<br>
<img width="974" height="176" alt="image" src="https://github.com/user-attachments/assets/63bf6fe9-a19d-4594-b534-a82b98015e94" /><br><br>

Переходим к настройке конфиг файла:<br><br>
1)	Разрешим loopback посрдеством команды `iif to accept`<br>
2)	 Пропишем команду для разрешения ответов на уже установленные соединения: `ct state established, related accept`<br>
3)	Пропишем команду `tcp dport 2222 accept` и `tcp dport 10051 accept` для разрешения SSH на порту 2222 и 10051 порт в последующем для Zabbix.<br>
4)	Разрешим ICMP командами `ip protocol icmp accept` и `ip6 nexthdr icmpv6 accept`<br>
5)	В ветке chain forward пропишем `type filter hook forward priority 0; policy drop`, чтобы все входящие подключения блокировались.<br>
6)	В ветке chain output пропишем `type filter hook forward priority 0; policy accept`, дабы исходящие не блокировались.<br>
В итоге получили такую конфигурацию:<br><br>
<img width="974" height="684" alt="image" src="https://github.com/user-attachments/assets/4143f065-f567-47e3-892c-45e31776559c" /><br><br>

Применим правила командой `sudo nft -f /etc/nftables.conf` и проверим правила командой `sudo nft list ruleset`:<br><br>
<img width="974" height="607" alt="image" src="https://github.com/user-attachments/assets/6b3c82cf-c2fb-4332-ba96-6f91e619a9b9" /><br><br>

Сохраним конфигурацию так, чтобы она восстанавливалась после перезагрузки командой `sudo systemctl enable nftables и start`.
Перейдём к заданию 7.
Установим Zabbix agent командой `wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu24.04_all.deb`<br>
Добавим команду `sudo dpkg -i zabbix-release_7.0-2+ubuntu24.04_all.deb` и `sudo apt update`. После установим агент командой `sudo apt install zabbix-agent -y`.<br>
Идём конфигурировать файл по пути `/etc/zabbix/zabbis-agent2.conf`, где прописали dns, hostname универсально и логи, которые могут понадобиться в будущем:<br><br>
<img width="974" height="1236" alt="image" src="https://github.com/user-attachments/assets/8dcf023b-2405-40b2-b311-cc813a69fd5a" /><br><br>


Чтобы добавить Zabbix в автозагрузку, нужно прописать `sudo systemctl enable zabbix-agent2`, заодно проверим его состояние командой `sudo systemctl status zabbix-agent2`:<br><br><img width="974" height="334" alt="image" src="https://github.com/user-attachments/assets/62174e03-3dca-4479-b189-d10e507459c0" /><br><br>

Перейдем к 8 заданию.<br>
У нас уже установлен cron, поэтому  создадим задачку через команду `sudo nano crontab -e`:<br><br>
<img width="925" height="322" alt="image" src="https://github.com/user-attachments/assets/3b18d00d-49b4-47f9-9892-22c153b37dfa" /><br><br>

Перейдем к 9 заданию.<br><br>
1.	Обновить систему и очистить кэш пакетов. (Это нужно для шаблона, он должен начинаться с последних стабильных обновлений безопасности и исправлений ошибок. Очистка кэша освободит место и удалит ненужные файлы)<br>
2.	Удалить пользовательские данные и логи. (Логи, временные файлы и история команд содержат данные, прошлой машины, в новой они не нужны)<br>
3.	Удалить специфичные для машины конфигурации( SSH ключи хоста, старые конфигурации и т.д.)<br>
4.	Убедиться, что все наши сервисы работают(на автозапуске)<br>
5.	Остановить сервер для создания снапшота.<br>
