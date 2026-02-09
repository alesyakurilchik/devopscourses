HW4.1
1. дату и время, часовой пояс. Проверьте hostname, при необходимости поменять. Установить в качестве сервера времени `0.pool.ntp.org 1.pool.ntp.org`. Проверить правильность настройки, пояснить почему Вы решили что все правильно.  

1.1. Выполняем команду “sudo timedatectl set-timezone Europe/Minsk”, чтобы настроить минское время. Корректность выполнения команды в последствии была проверена командой “timedatectl”.  
<img width="940" height="336" alt="image" src="https://github.com/user-attachments/assets/16f48321-e330-4103-ab97-0eeac6eae49a" />

1.2. Проверяем имя хоста командой “hostnamectl”.
<img width="695" height="376" alt="image" src="https://github.com/user-attachments/assets/19376837-99bf-4f77-9127-7eec0a3d92fc" />

1.3. Для установки сервера времени используем команду “sudo nano /etc/systemd/timesyncd.conf”. Далее в редакторе добавляем нужные строки.
<img width="763" height="200" alt="image" src="https://github.com/user-attachments/assets/7324a3d4-65ee-459d-b7c5-c0858639993e" />

После этого выполняем команду “sudo systemctl restart systemd-timesyncd”.Далее выполняем проверку корректности командой “timedatectl timesync-status”.
<img width="663" height="394" alt="image" src="https://github.com/user-attachments/assets/0e8b37b2-3747-4c66-bb91-ec8aa81b5f20" />

 2. Статический ip-address, маршрут по умолчанию и днс сервер при помощи `netplan`.

2.1. Для настройки статического ip-адреса мы для начала перешли в нужный каталог командой cd /etc/netplan/, далее зашли в редактор командой “sudo nano 50-cloud-init.yaml” и испраивили то, что нам нужно было (addresses; via). После внесения изменений выполнили команду “sudo netplan try”, чтобы проверить корректность и применить изменения.
<img width="235" height="275" alt="image" src="https://github.com/user-attachments/assets/94ccd2a5-e552-4f75-8e11-074c1ac41394" />
<img width="475" height="141" alt="image" src="https://github.com/user-attachments/assets/b60d400d-1a51-4e4b-b1f5-556da842594e" />
После этого проверили, выполнилась ли настройка.
<img width="940" height="87" alt="image" src="https://github.com/user-attachments/assets/f1462cf2-5ecf-48da-abe9-7a41689da40a" />

3. 2 пользователей в разных группах. Выдайте группе права на обновление системы через настройку `sudo visudo`. Для одного из пользователей настройте права для использования `systemctl status`. Настройте доступ по ssh для новых пользователей через ssh ключи. Доступ по ssh по логину и паролю запретите.

3.1. Создаём группы “group1” и “group2”, а также создаём 2 пользователей для этих групп: user1g1(group1) и user1g2(group2). И проверяем правильно ли были выполнены действия.
<img width="635" height="443" alt="image" src="https://github.com/user-attachments/assets/f3f4e3f3-cce2-4350-a821-cdc6608d69db" />

3.2. Далее даём группе group1 право обнавлять систему через sudoers. Используем команду “sudo visudo” и добавляем строку “%group1 ALL=(ALL) /usr/bin/apt update, /usr/bin/apt upgrade”, которая позволит group1 обновлять систему.
<img width="825" height="374" alt="image" src="https://github.com/user-attachments/assets/f3df1769-7646-4e6d-8785-95d502baec00" />

Далее поверяем корректность синтаксиса командой sudo visudo -c.
<img width="758" height="180" alt="image" src="https://github.com/user-attachments/assets/9d77c4c1-657a-4983-a346-2be39c2716f4" />

Далее проверяем права доступа. Заходим под пользователем user1g1 и проверяем возможно ли выполнить обновление.
<img width="940" height="318" alt="image" src="https://github.com/user-attachments/assets/8a092c21-d408-4253-a7ee-3ff554ea1b51" />

3.3. Разрешаем пользователю user1g2 право на systemctl status.  Добавляем правила в /etc/sudoers.d/ с помощью команды sudo nano /etc/sudoers.d/user1g2. 
<img width="940" height="100" alt="image" src="https://github.com/user-attachments/assets/e03081a6-1333-40ca-9e09-d0407d0f6aa0" />

Права на файл:
<img width="941" height="43" alt="image" src="https://github.com/user-attachments/assets/a7e40920-c226-4e1e-a89a-6f8cab8593bd" />

Выполняем проверку: 
<img width="533" height="179" alt="image" src="https://github.com/user-attachments/assets/01b88307-38d5-4e96-9863-c3351a9e6d0c" />
<img width="538" height="218" alt="image" src="https://github.com/user-attachments/assets/c228534f-d318-4c58-b3ad-bbb4ba30074e" />

3.4. Создаём папку shh для user1g1 c помощью команды sudo mkdir -p /home/user1g1/.ssh, потом создаём файл authorized_keys c помощью команды sudo touch /home/user1g1/.ssh/authorized_keys, а потом даём доступ user1g1 к этой папке sudo chown -R user1g1:user1g1 /home/user1g1/.ssh. А также выдаём права командами sudo chmod 700 /home/user1g2/.ssh; sudo chmod 600 /home/user1g2/.ssh/authorized_keys.
Далее мы вставили публичный ключ в папку authorized_keys.
<img width="941" height="35" alt="image" src="https://github.com/user-attachments/assets/16b09325-d20a-477c-a946-7c4330847c9c" />

Далее проверили подключение с нашего хоста.
<img width="390" height="298" alt="image" src="https://github.com/user-attachments/assets/e6cdb359-6145-40da-8bd8-346b589a8baa" />

Потом делаем то же самое для user1g2. 
<img width="940" height="142" alt="image" src="https://github.com/user-attachments/assets/c33135f7-cde1-4fa2-b304-b7c3c8ab9998" />
<img width="940" height="134" alt="image" src="https://github.com/user-attachments/assets/c002fe06-93ba-46ad-a764-fec80f01a6f4" />
<img width="479" height="444" alt="image" src="https://github.com/user-attachments/assets/0f3f8d03-88f5-41cc-b6bd-6d6b0b7bdb7c" />

3.5 Запрещаем пользователям user1g1, user1g2 вход через ssh по  паролю.
<img width="941" height="82" alt="image" src="https://github.com/user-attachments/assets/719a7835-5397-4cf7-87e5-31a8d218d2f1" />
<img width="940" height="274" alt="image" src="https://github.com/user-attachments/assets/f7a92de3-d3c9-4ee6-8a54-613eefb175b9" />

 4. Каталог для работы группы людей, настройте что бы права во внутренних каталогах наследовались от  группы директории, а не основную группу пользователя. В ней создайте файл под одним из пользоватей разрешите доступ к нему только для владельца.	
 
Создаём каталог с вложенной в неё папку командой sudo mkdir -p /srv/shared в корневой директории, потом указываем владельца и группу для этих директорий sudo chown user1g1:group1 /srv/shared. Далее присваиваем права доступа sudo chmod 2775 /srv/shared, потом проверяем.
<img width="940" height="74" alt="image" src="https://github.com/user-attachments/assets/3de8e20c-c670-4d58-b1ea-fcff75e9fe90" />

Далее задаём права доступа для группы для  всех вложенных каталогов и файлов командой sudo setfacl -d -m g::rwx /srv/shared, а также для остальных пользователей sudo setfacl -d -m o::- /srv/shared. 

Создаём папку touch test_file.txt и файл mkdir test_dir и проверяем какие права доступа есть у пользователя.
<img width="941" height="290" alt="image" src="https://github.com/user-attachments/assets/a65653f7-6d21-4fd4-87ca-3cf14065df96" />

Создаём папку touch private.txt и устанавливаем права доступа только владельцу chmod 600 private.txt. 
<img width="941" height="266" alt="image" src="https://github.com/user-attachments/assets/a11f5f6f-ae09-4314-a6be-13d85e768436" />

5. сетевой доступ к виртуальной машине только по 22 и 80. Попробуйте подключиться по ssh. Установить nginx `sudo apt install nginx` посмотрите что доступно из браузера вашей основной машины при разрешенном трафике и запрещенном. Найдите информацию почему мы разрешали именно 80 порт, какие еще зарезервированные порты вы знаете?

Настраиваем сетевой доступ по 22 порту(SSH) sudo ufw allow 22/tcp comment 'SSH' и по 80(TCP) sudo ufw allow 80/tcp comment 'HTTP'. А также запрещаем входящую информацию sudo ufw default allow outgoing и разрешаем исходящую sudo ufw default deny incoming.
<img width="941" height="405" alt="image" src="https://github.com/user-attachments/assets/11bd56e8-adbf-4b64-9ecf-bf67ddba6c81" />

Далее применяем и включаем firewall.
<img width="940" height="86" alt="image" src="https://github.com/user-attachments/assets/63ceee11-11f3-4ea2-8598-55bdb05963a6" />

Проверяем сетевой трафик sudo ufw status verbose
<img width="918" height="310" alt="image" src="https://github.com/user-attachments/assets/671a72f1-53e9-4b51-a905-6ad6b41933b1" />

Загружаем nginx командой sudo apt install nginx.
<img width="940" height="191" alt="image" src="https://github.com/user-attachments/assets/b8952633-2549-4819-baf6-6c9356cdfef3" />

Проверяем загружен и запущен ли ngnix командой sudo systemctl status nginx.
<img width="940" height="226" alt="image" src="https://github.com/user-attachments/assets/52ab09d9-3195-40aa-a2c8-253003838b77" />

Проверяем работоспособность nginx
<img width="940" height="386" alt="image" src="https://github.com/user-attachments/assets/f9213563-8f72-4f1e-ac7b-3521e7940371" />

 В браузере мы видим следующее:
<img width="559" height="240" alt="image" src="https://github.com/user-attachments/assets/3bf7a838-10f4-4d5f-9958-3e50d7c59e4c" />

Найдите информацию почему мы разрешали именно 80 порт, какие еще зарезервированные порты вы знаете?
Потому что nginx работает по http, а http идёт по 80 порту.
Часто используемые порты:
Номер порта	Протокол	Название сервиса
20	TCP	FTP - данные
21	TCP	FTP - контрольное соединение
22	TCP	SSH-сервер
23	TCP	Telnet-сервер
25	TCP	SMTP - отправка почты
53	TCP, UDP	DNS - сервер доменных имен
69	UDP	TFTP-сервер
80	TCP	HTTP - web-сервер
110	TCP	POP3 - прием почты
145	TCP	IMAP - прием почты
161	UDP	SNMP - протокол управления сетевыми устройствами
443	TCP	HTTPS - защищенный шифрованием протокол HTTP
587	TCP	Защищенный шифрованием протокол SMTP
993	TCP	Защищенный шифрованием протокол IMAP
995	TCP	Защищенный шифрованием протокол POP3
1500	TCP	Панель ISPManager
2083	TCP	Панель cPanel
3306	TCP	Сервер базы данных MySQL
8083	TCP	Панель Vesta
 
