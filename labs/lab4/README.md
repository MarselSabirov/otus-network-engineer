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
  ##### *a. Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1.* -  
  * S1(config)#interface vlan 1; 
  * S1(config-if)#ipv6 address 2001:db8:acad:1::b/64
  * S1(config-if)#no shutdown 
  * R1(config)#interface gigabitEthernet 0/0/0
  * R1(config-if)#ipv6 address 2001:db8:acad:a::1/64
  * R1(config-if)#no shutdown 
  * R1(config)#interface gigabitEthernet 0/0/1
  * R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
  * R1(config-if)#no shutdown 

