## Лабораторная работа 1. Базовая настройка коммутатора

### Схема подключения:

![schema](https://user-images.githubusercontent.com/18709313/109419041-1e1a4d80-79d4-11eb-95f8-b8b26630ad66.png)

### Таблица адресации:
Устройство | Интерфейс | IP-адрес | Маска подсети | Шлюз по умолчанию
------------ | ------------- | ------------- | ------------- | -------------
S1 | VLAN 99 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1
PC-A | NIC | 192.168.1.10 | 255.255.255.0 | 192.168.1.1

### Таблица VLAN:
VLAN | Интерфейс
------------ | -------------
99| S1 F0/6


### Задачи 
#### Часть 1. Проверка конфигурации коммутатора по умолчанию
#### Часть 2. Создание сети и настройка основных параметров устройства
    * Настройте базовые параметры коммутатора.
    * Настройте IP-адрес для ПК.   
#### Часть 3. Проверка сетевых подключений
    * Отобразите конфигурацию устройства.
    * Протестируйте сквозное соединение, отправив эхо-запрос.
    * Протестируйте возможности удаленного управления с помощью Telnet.    
#### Часть 4. Управление таблицей MAC-адресов
    * Запишите MAC-адрес узла.
    * Определите МАС-адреса, полученные коммутатором.
    * Перечислите параметры команды show mac address-table.
    * Назначьте статический MAC-адрес.

### Решение 
#### 1.2.b: Изучите текущий файл running configuration
  * *Сколько интерфейсов FastEthernet имеется на коммутаторе 2960?* - 24 интерфейса FastEthernet,
  * *Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?* - 2 интерфейса GigabitEthernet
  * *Каков диапазон значений, отображаемых в vty-линиях?* - В vty-линиях 0-4 и 5-15 диапазоны
#### 1.2.c: Изучите файл загрузочной конфигурации (startup configuration), который содержится в энергонезависимом ОЗУ (NVRAM). S1# show startup-config
  * *Почему появляется это сообщение?* - Запускается файл конфигурации
#### 1.2.d: Изучите характеристики SVI для VLAN 1
  * *Назначен ли IP-адрес сети VLAN 1?* - IP-адрес для сети VLAN1 не назначен
  * *Какой MAC-адрес имеет SVI? Возможны различные варианты ответов.* - MAC-адрес: 00:17:59:A7:51:80
  * *Данный интерфейс включен?* - Интерфейс Vlan1 выключен
#### 1.2.e: Изучите IP-свойства интерфейса SVI сети VLAN 1
  * *Какие выходные данные вы видите?* - Интерфейс Vlan1 выключен, свойства отсутствуют
#### 1.2.f: Подсоедините кабель Ethernet компьютера PC-A к порту 6 на коммутаторе и изучите IP-свойства интерфейса SVI сети VLAN 1. Дождитесь согласования параметров скорости и дуплекса между коммутатором и ПК
  * *Какие выходные данные вы видите?* - После подсоединение кабеля в терминале отобразилось сообщение "%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/6, changed state to up"
#### 1.2.g: Изучите сведения о версии ОС Cisco IOS на коммутаторе. S1#show version
  * *Под управлением какой версии ОС Cisco IOS работает коммутатор?* - Версия OC Cisco IOS - 15.0(2)SE4
  * *Как называется файл образа системы?* - Образ системы - C2960-LANBASEK9-M
  * *Какой базовый MAC-адрес назначен коммутатору?* - Базовый MAC-адрес: 00:17:59:A7:51:80
  * Полная информация о системе и оборудовании представлена по [ссылке](https://github.com/MarselSabirov/otus-network-engineer/blob/main/labs/lab1/S1_sys_info.txt)
#### 1.2.h: Изучите свойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A. Switch# show interface f0/6 
  * *Интерфейс включен или выключен?* - Интерфейс f0/6 включен
  * *Что нужно сделать, чтобы включить интерфейс?* - Для включения, необходимо перейти в режим глобальной конфигурации, ввести команду interface fastEthernet 0/6 для перехода в режим настроек данного интерфейса и ввести команду no shutdown для его включения
  * *Какой MAC-адрес у интерфейса?* - Информация об MAC-адресе интерфейса отсутствует
  * *Какие настройки скорости и дуплекса заданы в интерфейсе?* - Скорость 100Mb/s
#### 1.2.i: Изучите параметры сети VLAN по умолчанию на коммутаторе
  * *Активна ли сеть VLAN 1?* - Сеть VLAN1 неактивна
#### 1.2.j: Изучите флеш-память. S1# show flash. S1# dir flash
  * *Какое имя присвоено образу Cisco IOS?* - Содержимое флеш-памяти: 2960-lanbasek9-mz.150-2.SE4.bin. Имя образа предположительно "lanbasek9"

#### 2.1.g: Настройте базовые параметры коммутатора
  * *Для чего нужна команда login?* - Команда login нужна для включения функции проверки пароля

#### 3.1.b: Проверьте параметры административной VLAN 99. S1# show interface vlan 99 
  * *Какова полоса пропускания этого интерфейса?* - MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec, reliability 255/255, txload 1/255, rxload 1/255
  * *В каком состоянии находится VLAN 99?* - В состоянии UP
  * *В каком состоянии находится канальный протокол?* - В состоянии DOWN
#### 3.2: Протестируйте сквозное соединение, отправив эхо-запрос
  * *Из командной строки компьютера PC-A отправьте эхо-запрос на административный адрес интерфейса SVI коммутатора S1.* - Скриншот эхо-запроса с PC-A до S1 (192.168.1.2)
![ping_from_pc_to_switch (2)](https://user-images.githubusercontent.com/18709313/109425683-1ec1dc80-79f2-11eb-8395-1a3842252a1f.png)


#### 4.1: Запишите MAC-адрес узла
  * *В командной строке компьютера PC-A выполните команду ipconfig /all* - Физический адрес - 0003.E497.0643
#### 4.2: Отобразите МАС-адреса с помощью команды show mac address-table
  * *Сколько динамических адресов присутствует?* - Присутствует 1 адрес VLAN99
  * *Сколько МАС-адресов имеется в общей сложности?* - 1 MAC -адрес 0003.e497.0643
  * *Совпадает ли динамический MAC-адрес с МАС-адресом компьютера PC-A?* - Да, динамический MAC-адрес совпадает с MAC-адресом PC-A
#### 4.3.a: Отобразите параметры таблицы МАС-адресов. S1# show mac address-table ? 
  * *Сколько параметров доступно для команды show mac address-table?* - Отображается три команды: dynamic, interfaces, static
#### 4.3.b: Введите команду show mac address-table dynamic, чтобы отобразить только те МАС-адреса, которые были получены динамически. S1# show mac address-table dynamic 
  * *Сколько динамических адресов присутствует?* - Отображается 1 динамический адрес VLAN99
#### 4.3.c: Просмотрите запись MAC-адреса для компьютера PC-A. Формат MAC-адреса для команды: xxxx.xxxx.xxxx.
  * *S1# show mac address-table address <PC-A MAC here>* - Команды show mac address-table address 0003.e497.0643 и show mac address-table 0003.e497.0643 для отображения записи MAC-адреса не были выполнены успешны из-за сообщения об некорректном вводе команд

#### 4.4.b: Очистите таблицу MAC-адресов. S1# clear mac address-table dynamic. Убедитесь, что таблица МАС-адресов очищена. S1# show mac address-table
  * *Сколько статических MAC-адресов присутствует сейчас в таблице?* - В таблице отсутствуют статические MAC-адреса
  * *Сколько динамических адресов присутствует?* - В таблице отсутствуют динамические MAC-адреса
![Screenshot from 2021-02-28 19-51-31 (1)](https://user-images.githubusercontent.com/18709313/109426569-056f5f00-79f7-11eb-9e22-8454363fcde3.png)
#### 4.4.c: Снова изучите таблицу МАС-адресов.
  * *Сколько динамических адресов присутствует?* - После отправки эхо-запроса с консоли PC-A(ping 192.168.1.2), в таблице появился интерфейс vlan99
  * *Почему это значение изменилось с предыдущего раза?* - Связанно это с тем, что интерфейс vlan99 работет в режиме dynamic и оживает on-demand(по требованию)
![Screenshot from 2021-02-28 19-52-38 (1)](https://user-images.githubusercontent.com/18709313/109426573-0dc79a00-79f7-11eb-9e6c-cd8a825c0cb4.png)

#### 4.4.e: Выполните проверку записей в таблице MAC-адресов. S1# show mac address-table
  * *Сколько всего динамических адресов присутствует?* - Динамических 0 адресов
  * *Сколько статических адресов присутствует?* - Статических 1 адрес vlan99
![Screenshot from 2021-02-28 19-54-17 (1)](https://user-images.githubusercontent.com/18709313/109426578-17e99880-79f7-11eb-855e-ccf9429bbc13.png)

#### 4.4.g: Удалите запись статического МАС. S1(config)# no mac address-table static 0003.e497.0643 vlan 99 interface fastethernet 0/6  Убедитесь, что статический МАС-адрес был удален. S1# show mac address-table 
  * *Сколько всего статических MAC-адресов содержится в таблице?* - После удаления записи статического MAC-адреса, в таблица адресов пустая
![Screenshot from 2021-02-28 20-01-20 (1)](https://user-images.githubusercontent.com/18709313/109426684-8d556900-79f7-11eb-9f55-23c1e903840a.png)


###  Конфигурация коммутатора
```
Building configuration...

Current configuration : 1506 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
no ip domain-lookup
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
 switchport access vlan 99
 switchport mode access
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan99
 ip address 192.168.1.2 255.255.255.0
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:ACAD::2/64
!
ip default-gateway 192.168.1.1
!
banner motd ^C
Unauthorized access is strictly prohibited. ^C
!
!
!
line con 0
 password 7 0822455D0A16
 logging synchronous
!
line vty 0 4
 password 7 0822455D0A16
 login
line vty 5 15
 password 7 0822455D0A16
 login
!
!
!
!
end
```
