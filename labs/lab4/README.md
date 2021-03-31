## Лабораторная работа 4. Настройка IPv6-адресов на сетевых устройствах 
### Топология сети:
![0_scheme](https://user-images.githubusercontent.com/18709313/112730860-5301bd80-8f0a-11eb-87f3-194ba1e3ef44.png)

### Таблица адресации:
Устройство | Интерфейс | IPv6-адрес | Длина префикса | Шлюз по умолчанию
------------ | ------------- | ------------- | ------------- | -------------
R1 | G0/0/0 | 2001:db8:acad:a::1 | 64 | —
R1 | G0/0/1 | 2001:db8:acad:1::1 | 64 | —
S1 | VLAN 1 | 2001:db8:acad:1::b | 64 | —
PC-A | NIC | 2001:db8:acad:1::3 | 64 | fe80::1
PC-B | NIC | 2001:db8:acad:a::3 | 64 | fe80::1

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
  ##### *a. Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1.* 
  * S1(config)#interface vlan 1; 
  * S1(config-if)#ipv6 address 2001:db8:acad:1::b/64
  * S1(config-if)#no shutdown 
  * R1(config)#interface gigabitEthernet 0/0/0
  * R1(config-if)#ipv6 address 2001:db8:acad:a::1/64
  * R1(config-if)#no shutdown 
  * R1(config)#interface gigabitEthernet 0/0/1
  * R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
  * R1(config-if)#no shutdown 
  ##### *b. Введите команду show ipv6 interface brief, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес.*
  ![2_a_b_switch](https://user-images.githubusercontent.com/18709313/112732602-50569680-8f11-11eb-94c1-2aa3c0d6a453.png)
  ![2_a_b_switch](https://user-images.githubusercontent.com/18709313/112732651-9ad81300-8f11-11eb-9b96-c2e9332d0b1c.png)

  ##### *c. Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, вручную введите локальные адреса канала на каждом интерфейсе Ethernet на R1.*
  ![2_a_c_router](https://user-images.githubusercontent.com/18709313/112732833-bf80ba80-8f12-11eb-8e5f-f6bf4a1e1f78.png)

  ##### *d. Какие группы многоадресной рассылки назначены интерфейсу G0/0?* - Адреса многоадресной рассылки (с префиксом ff02::) отсутсвуют на S1 и R1
#### b. Активируйте IPv6-маршрутизацию на R1.
  ##### *a. В командной строке на PC-B введите команду ipconfig, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК.* 
  ##### *Назначен ли индивидуальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B?* - Да, детали далее в скриншоте
  ![2_b_a_pcB](https://user-images.githubusercontent.com/18709313/113173760-b98f2000-9217-11eb-88e9-0b7ace77480a.png)
  
  ##### *b. Активируйте IPv6-маршрутизацию на R1 с помощью команды IPv6 unicast-routing.* - Активировано
  ##### *c. Теперь, когда R1 входит в группу многоадресной рассылки всех маршрутизаторов, еще раз введите команду ipconfig на PC-B. Проверьте данные IPv6-адреса.* - Конфигурация осталась без изменений. Вероятно, из-за того, что ранее IPv6 адрес для PC-B был настроен вручную на этапе подготовки сети к лабораторной работе
 
