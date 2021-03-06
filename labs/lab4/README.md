## Лабораторная работа 4. Настройка IPv6-адресов на сетевых устройствах 
### Топология сети:
![0_scheme](https://user-images.githubusercontent.com/18709313/112730860-5301bd80-8f0a-11eb-87f3-194ba1e3ef44.png)

### Таблица адресации:
Устройство | Интерфейс | IPv6-адрес | Длина префикса | Шлюз по умолчанию
------------ | ------------- | ------------- | ------------- | -------------
R1 | G0/0/0 | 2001:db8:acad: a::1 | 64 | —
R1 | G0/0/1 | 2001:db8:acad:1::1 | 64 | —
S1 | VLAN 1 | 2001:db8:acad:1::b | 64 | —
PC-A | NIC | 2001:db8:acad:1::3 | 64 | fe80::1
PC-B | NIC | 2001:db8:acad: a::3 | 64 | fe80::1

### Задачи
#### Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
#### Часть 2. Ручная настройка IPv6-адресов
#### Часть 3. Проверка сквозного соединения

### Решение
#### Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
##### a. Настройте маршрутизатор.
  * *enable*
  * *show flash*
  * *erase startup-config*
  * *reload*
  * *enable*
  * *configure terminal*
  * *no ip domain-lookup*
  * *host R1*
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
##### b. Настройте коммутатор.
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

### Часть 2. Ручная настройка IPv6-адресов
#### a. Назначьте IPv6-адреса интерфейсам Ethernet на R1.
  ##### *a.a. Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1.* 
  * S1(config)#interface vlan 1; 
  * S1(config-if)#ipv6 address 2001:db8:acad:1::b/64
  * S1(config-if)#no shutdown 
  * R1(config)#interface gigabitEthernet 0/0/0
  * R1(config-if)#ipv6 address 2001:db8:acad: a::1/64
  * R1(config-if)#no shutdown 
  * R1(config)#interface gigabitEthernet 0/0/1
  * R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
  * R1(config-if)#no shutdown 
  ##### *a.b. Введите команду show ipv6 interface brief, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес.*
  ![2_a_b_switch](https://user-images.githubusercontent.com/18709313/112732602-50569680-8f11-11eb-94c1-2aa3c0d6a453.png)
  ![2_a_b_switch](https://user-images.githubusercontent.com/18709313/112732651-9ad81300-8f11-11eb-9b96-c2e9332d0b1c.png)

  ##### *a.c. Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, вручную введите локальные адреса канала на каждом интерфейсе Ethernet на R1.*
  ![2_a_c_router](https://user-images.githubusercontent.com/18709313/113209369-fc192280-9240-11eb-8a46-8b255f24d668.png)


  ##### *a.d. Какие группы многоадресной рассылки назначены интерфейсу G0/0?* - Интерфейсу назначены 3 группы рассылки: FF02::1, FF02::2, FF02::1:FF00:1
  ![2_a_d_router](https://user-images.githubusercontent.com/18709313/113209771-7649a700-9241-11eb-86f5-66a2febab154.png)

  
#### b. Активируйте IPv6-маршрутизацию на R1.
  ##### *b.a. В командной строке на PC-B введите команду ipconfig, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК.* 
  ##### *Назначен ли индивидуальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B?* - Да, детали далее в скриншоте
  ![2_b_a_pcB](https://user-images.githubusercontent.com/18709313/113173760-b98f2000-9217-11eb-88e9-0b7ace77480a.png)
  
  ##### *b.b. Активируйте IPv6-маршрутизацию на R1 с помощью команды IPv6 unicast-routing.* - Активировано
  ##### *b.c. Теперь, когда R1 входит в группу многоадресной рассылки всех маршрутизаторов, еще раз введите команду ipconfig на PC-B. Проверьте данные IPv6-адреса.* - Конфигурация осталась без изменений. Вероятно, из-за того, что ранее IPv6 адрес для PC-B был настроен вручную на этапе подготовки сети к лабораторной работе

#### c. Назначьте IPv6-адреса интерфейсу управления (SVI) на S1.
  ##### c.a.-c.b. Проверьте правильность назначения IPv6-адресов интерфейсу управления с помощью команды show ipv6 interface vlan1.
  ![2_c_a_pcA](https://user-images.githubusercontent.com/18709313/113175875-e5aba080-9219-11eb-8580-40120a6a4828.png)

#### *c.d. Назначьте компьютерам статические IPv6-адреса. - IPv6 адреса на PC-A и PC-B были назначены на предыдущих шагах* - Адреса назначены

### Часть 3. Проверка сквозного подключения
  #### С PC-A отправьте эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1.
  ![3_1_ping_from_pcA_to_DG](https://user-images.githubusercontent.com/18709313/113211464-7c408780-9243-11eb-8daf-098fdc425e77.png)
  
  #### Отправьте эхо-запрос на интерфейс управления S1 с PC-A.
  ![3_2_ping_from_pcA_to_S1](https://user-images.githubusercontent.com/18709313/113211493-86fb1c80-9243-11eb-9e23-b0552461e59a.png)
  
  #### Введите команду tracert на PC-A, чтобы проверить наличие сквозного подключения к PC-B.
  ![3_3_tracert_from_pcA_to_pcB](https://user-images.githubusercontent.com/18709313/113211613-b6118e00-9243-11eb-958c-731a4629b942.png)
  
  #### С PC-B отправьте эхо-запрос на PC-A. 
 ![3_4_ping_from_pcB_to_pcA](https://user-images.githubusercontent.com/18709313/113211635-bc076f00-9243-11eb-9c63-793f67171006.png)
  
  #### С PC-B отправьте эхо-запрос на локальный адрес канала G0/0 на R1.
  ![3_5_ping_from_pcB_to_DG](https://user-images.githubusercontent.com/18709313/113211659-c45faa00-9243-11eb-92ef-6a7c89ef1897.png)

  
### Вопросы для повторения
  #### *1. Почему обоим интерфейсам Ethernet на R1 можно назначить один и тот же локальный адрес канала — FE80::1?* - Потому, что пакеты внутри локальной сети IPv6 не покидают пределы этой локальной сети. В связи с чем, имеется возможность назначать одинаковые IPv6 локальные адреса, не опасаясь проблем с дублирующимися адресами.
  
  #### *2. Какой идентификатор подсети в индивидуальном IPv6-адресе 2001:db8:acad::aaaa:1234/64?* - Идентификатор 0000


### Конфигурация коммутатора S1
```
S1#show running-config 
Building configuration...

Current configuration : 1306 bytes
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
 no ip address
 ipv6 address 2001:DB8:ACAD:1::B/64
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


S1#
```

### Конфигурация маршутизатора R1
```
Building configuration...

Current configuration : 990 bytes
!
version 15.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname R1
!
!
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
!
!
!
no ip cef
ipv6 unicast-routing
!
no ipv6 cef
!
!
!
!
!
!
!
!
!
!
no ip domain-lookup
!
!
spanning-tree mode pvst
!
!
!
!
!
!
interface GigabitEthernet0/0/0
 no ip address
 duplex auto
 speed auto
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:A::1/64
!
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:1::1/64
!
interface GigabitEthernet0/0/2
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
!
ip flow-export version 9
!
!
!
banner motd ^C Unauthorized access is strictly prohibited. ^C
!
!
!
!
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line aux 0
!
line vty 0 4
 password 7 0822455D0A16
 login
!
!
!
end


R1#
```

### [Ссылка](https://github.com/MarselSabirov/otus-network-engineer/blob/main/labs/lab4/4.pkt) на файл проекта
