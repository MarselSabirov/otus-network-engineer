## Лабораторная работа 2. Просмотр таблицы MAC-адресов коммутатора 

### Топология сети:

![Топология_2](https://user-images.githubusercontent.com/18709313/110961697-1f972e80-8361-11eb-9123-793bd2e881d3.png)

### Таблица адресации:
Устройство | Интерфейс | IP-адрес | Маска подсети
------------ | ------------- | ------------- | -------------
S1 | VLAN 1 | 192.168.1.11 | 255.255.255.0 
S2 | VLAN 1 | 192.168.1.12 | 255.255.255.0
PC-A | NIC | 192.168.1.1 | 255.255.255.0
PC-B | NIC | 192.168.1.2 | 255.255.255.0

### Таблица VLAN:
VLAN | Интерфейс
------------ | -------------
1 | S1 F0/6
1 | S2 F0/18

### Задачи 
#### Часть 1. Создание и настройка сети
#### Часть 2. Изучение таблицы МАС-адресов коммутатора

### Решение
### Часть 1. Создание и настройка сети
#### Шаг 1. Подключите сеть в соответствии с топологией.
#### Шаг 2. Настройте узлы ПК.
#### Шаг 3. Выполните инициализацию и перезагрузку коммутаторов.
  * *enable*
  * *show flash*
  * *erase startup-config*
  * *reload*
  * *enable*
  * *configure terminal*
  * *no ip domain-lookup*
  * *host S1*
  * *service password-encryption*
  * *enable secret class*
  * *banner motd #*
     *Unauthorized access is strictly prohibited. #* 
  * *line con 0*
    * *logging synchronous*
    * *password cisco*
    * *login*
  * *exit*
  * *line vty 0 4*
    * *password cisco*
    * *login*
  
#### Шаг 4. Настройте базовые параметры каждого коммутатора. 
  *  *a. Настройте имена устройств в соответствии с топологией.* - configure terminal; host S1
  *  *b. Настройте IP-адреса, как указано в таблице адресации.* - interface vlan 1; ip address 192.168.1.11 255.255.255.0; no shutdown; exit; ip default-gateway 192.168.1.11
  *  *c. Назначьте cisco в качестве паролей консоли и VTY.* - Выполнено в пункте инициализации
  *  *d. Назначьте class в качестве пароля доступа к привилегированному режиму EXEC.* - Выполнено в пункте инициализации

##### Проверочные ping-запросы к коммутаторам:
![2_ping_pcA_to_s1](https://user-images.githubusercontent.com/18709313/111237994-8e2bf480-85cc-11eb-8d2d-a19820f21c9f.png)
![2_ping_pcB_to_s2](https://user-images.githubusercontent.com/18709313/111238006-92f0a880-85cc-11eb-8b8f-02b6882a619e.png)

### Часть 2. Изучение таблицы МАС-адресов коммутатора
#### Шаг 1. Запишите МАС-адреса сетевых устройств.
  * *a. Откройте командную строку на PC-A и PC-B и введите команду ipconfig /all.*
       *Назовите физические адреса адаптера Ethernet.*
       *MAC-адрес компьютера PC-A:* - 0002.16B4.1272   
           ![2_1_mac_pcA](https://user-images.githubusercontent.com/18709313/111381589-4795d300-867c-11eb-8b78-5d6c9184dde7.png)
            
       *MAC-адрес компьютера PC-B:* - 000D.BD04.2C45   
           ![2_1_mac_pcB](https://user-images.githubusercontent.com/18709313/111381599-4bc1f080-867c-11eb-8a5a-b3901331ec2b.png)

  * *б. Подключитесь к коммутаторам S1 и S2 через консоль и введите команду show interface F0/1 на каждом коммутаторе.*
       *МАС-адрес коммутатора S1 Fast Ethernet 0/1:* - bia 0040.0bba.3c01
           ![2_1_mac_S1](https://user-images.githubusercontent.com/18709313/111383683-f0453200-867e-11eb-9a56-14d0978817af.png)

       *МАС-адрес коммутатора S2 Fast Ethernet 0/1:* - bia 0030.a376.5301
           ![2_1_mac_S2](https://user-images.githubusercontent.com/18709313/111383699-f4714f80-867e-11eb-84e0-ceeaa60754b8.png)

  * *в. Просмотрите таблицу МАС-адресов коммутатора.*
     *Подключитесь к коммутатору S2 через консоль и просмотрите таблицу МАС-адресов до и после тестирования сетевой связи с помощью эхо-запросов.*
       
       *До ping-запроса:*       
          ![2_1_show_mac_table_before_ping](https://user-images.githubusercontent.com/18709313/111385307-f9370300-8680-11eb-9130-5bf3494df2b4.png)

       *После отправки ping-запроса:*       
          ![2_1_show_mac_table_after_ping](https://user-images.githubusercontent.com/18709313/111385325-fe944d80-8680-11eb-8126-1852df7274cd.png)

       *Если вы не записали МАС-адреса сетевых устройств в шаге 1, как можно определить, каким устройствам принадлежат МАС-адреса, используя только выходные данные команды show mac address-table? Работает ли это решение в любой ситуации?* 
       
       Какому устройству какой MAC-адрес пренадлежит можно по наименованию подключенного порта. Данное решение сработает только в случае наличии физического соединения между устройствами(в данном случае Switch S1 и PC PC-A)
       
  * *c. Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов.*
  
       После очистки таблицы MAC-адресов командой clear mac address-table dynamic, ранее отображаемые адреса изчезли. Спустя 10 секунд после повторного вывода списка MAC-адресов, в списке появился MAC-адрес switch S1, подключенного через порт Fa0/1
       
  * *d. С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора.*
       *а. На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.*
       
       ![2_2_pcB_arp](https://user-images.githubusercontent.com/18709313/111388070-1a015780-8685-11eb-9ae2-727b5395b7b2.png)
        
       После отправки широковещательного запроса через протокол ARP, было найдено 2 устройства - Switch S1 и Switch S2. 
       
       *б. Из командной строки PC-B отправьте эхо-запросы на компьютер PC-A, а также коммутаторы S1 и S2.*
       
       После отправки точечных ping-запросов на S1, S2, PC-A, к списку устройств, отображаемых при повторной отправке запроса по ARP, добавился и PC PC-A:
       
       ![2_2_pcB_arp_final](https://user-images.githubusercontent.com/18709313/111389373-40c08d80-8687-11eb-9408-dd7e1a82e43e.png)

       *в. Подключившись через консоль к коммутатору S2, введите команду show mac address-table.*
       
       ![2_2_S2_mac_table](https://user-images.githubusercontent.com/18709313/111389468-677ec400-8687-11eb-8714-b88cbc305768.png)       
           
       В списке доступных устройств сети отображаются все устройства: S1, S2, PC-A, PC-B

#### Вопрос для повторения

   *В сетях Ethernet данные передаются на устройства по соответствующим МАС-адресам. Для этого коммутаторы и компьютеры динамически создают ARP-кэш и таблицы МАС-адресов. Если компьютеров в сети немного, эта процедура выглядит достаточно простой. Какие сложности могут возникнуть в крупных сетях?*
   
   Возможно, в случае крупных сетей, таблицы MAC-адресов как и ARP-кэш будут слишком большими и так как запасы памяти всегда ограничены, могут возникнуть моменты переполнения доступного пространства и встанет вопрос об более оптимальном способе хранения всех доступных устройств сети, возможно в виде отдельных более емких структур данных с удобными средствами поиска и визуализации.
   
#### Итоговая конфигурация устройств сети
##### Switch S1:
      Building configuration...

      Current configuration : 1328 bytes
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
       ip address 192.168.1.11 255.255.255.0
      !
      ip default-gateway 192.168.1.11
      !
      banner motd ^C Unauthorized access is strictly prohibited. ^C
      !
      !
      !
      line con 0
       password 7 0822455D0A16
       logging synchronous
       login
      !
      line vty 0 4
       password 7 0822455D0A16
       login
      line vty 5 15
       login
      !
      !
      !
      !
      end

##### Switch S2:
      Building configuration...

      Current configuration : 1328 bytes
      !
      version 15.0
      no service timestamps log datetime msec
      no service timestamps debug datetime msec
      service password-encryption
      !
      hostname S2
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
       ip address 192.168.1.12 255.255.255.0
      !
      ip default-gateway 192.168.1.12
      !
      banner motd ^C Unauthorized access is strictly prohibited. ^C
      !
      !
      !
      line con 0
       password 7 0822455D0A16
       logging synchronous
       login
      !
      line vty 0 4
       password 7 0822455D0A16
       login
      line vty 5 15
       login
      !
      !
      !
      !
      end

