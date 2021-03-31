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
  ![2_a_c_router](https://user-images.githubusercontent.com/18709313/113209369-fc192280-9240-11eb-8a46-8b255f24d668.png)


  ##### *d. Какие группы многоадресной рассылки назначены интерфейсу G0/0?* - Интерфейсу назначены 3 группы рассылки: FF02::1, FF02::2, FF02::1:FF00:1
  ![2_a_d_router](https://user-images.githubusercontent.com/18709313/113209771-7649a700-9241-11eb-86f5-66a2febab154.png)

  
#### b. Активируйте IPv6-маршрутизацию на R1.
  ##### *a. В командной строке на PC-B введите команду ipconfig, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК.* 
  ##### *Назначен ли индивидуальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B?* - Да, детали далее в скриншоте
  ![2_b_a_pcB](https://user-images.githubusercontent.com/18709313/113173760-b98f2000-9217-11eb-88e9-0b7ace77480a.png)
  
  ##### *b. Активируйте IPv6-маршрутизацию на R1 с помощью команды IPv6 unicast-routing.* - Активировано
  ##### *c. Теперь, когда R1 входит в группу многоадресной рассылки всех маршрутизаторов, еще раз введите команду ipconfig на PC-B. Проверьте данные IPv6-адреса.* - Конфигурация осталась без изменений. Вероятно, из-за того, что ранее IPv6 адрес для PC-B был настроен вручную на этапе подготовки сети к лабораторной работе

#### c. Назначьте IPv6-адреса интерфейсу управления (SVI) на S1.
  ##### b. Проверьте правильность назначения IPv6-адресов интерфейсу управления с помощью команды show ipv6 interface vlan1.
  ![2_c_a_pcA](https://user-images.githubusercontent.com/18709313/113175875-e5aba080-9219-11eb-8580-40120a6a4828.png)

#### *d. Назначьте компьютерам статические IPv6-адреса. - IPv6 адреса на PC-A и PC-B были назначены на предыдущих шагах* - Адреса назначены

### Часть 3. Проверка сквозного подключения
  #### С PC-A отправьте эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1.
  
  
  ####

