# Лабораторная работа 6. Внедрение маршрутизации между виртуальными локальными сетями 
## Топология
![schema](https://user-images.githubusercontent.com/18709313/116937685-493e4900-ac72-11eb-85f4-224f530eb943.png)

## Таблица адресации
Устройство | Интерфейс | IP-адрес | Маска подсети | Шлюз по умолчанию
------------ | ------------- | ------------- | ------------- | -------------
R1 | G0/0/1.10 | 192.168.10.1 | 255.255.255.0 | —
R1 | G0/0/1.20 | 192.168.20.1 | 255.255.255.0 | 
R1 | G0/0/1.30 | 192.168.30.1 | 255.255.255.0 | 
R1 | G0/0/1.1000 | — | — | 
S1 | VLAN 10 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1
S2 | VLAN 10 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1
PC-A | NIC | 192.168.20.3 | 255.255.255.0 | 192.168.20.1 
PC-B | NIC | 192.168.30.3 | 255.255.255.0 | 192.168.30.1

## Таблица VLAN
VLAN | Имя | Назначенный интерфейс
------------ | ------------- | ------------- 
10 | Управление | S1: VLAN 10; S2: VLAN 10
20 | Sales | S1: F0/6
30 | Operations | S2: F0/18
999 | Parking_Lot | С1: F0/2-4, F0/7-24, G0/1-2; С2: F0/2-17, F0/19-24, G0/1-2
1000 | Собственная | —


## Задачи
### Часть 1. Создание сети и настройка основных параметров устройства
### Часть 2. Создание сетей VLAN и назначение портов коммутатора
### Часть 3. Настройка транка 802.1Q между коммутаторами.
### Часть 4. Настройка маршрутизации между сетями VLAN
### Часть 5. Проверка, что маршрутизация между VLAN работает

## Решение
### Часть 1. Создание сети и настройка основных параметров устройства
#### Шаг 2. Настройте базовые параметры для маршрутизатора.
* *enable*
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
  * *exit*
  * interface gigabitEthernet 0/0/1.10
    * *ip address 192.168.10.1 255.255.255.0*
    * *no sh*
  * *exit*
  * *exit*
  * *reload*
#### Шаг 3. Настройте базовые параметры каждого коммутатора.
* *enable*
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
  * *exit*
  * *exit*
  * *reload*
#### Шаг 4. Настройте узлы ПК.
![1_4_pcA_ip](https://user-images.githubusercontent.com/18709313/125963046-607dfd7a-10cd-4b5a-a757-8f47e67a05ea.png)
![1_4_pcB_ip](https://user-images.githubusercontent.com/18709313/125963064-cd56e5c5-bf53-4e7c-9346-018e582e379f.png)

### Часть 2. Создание сетей VLAN и назначение портов коммутатора
#### Шаг 1. Создайте сети VLAN на коммутаторах.
##### b. Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации.
* *enable*
  * *configure terminal*
  * *interface vlan 10*
    * *ip address 192.168.10.11 255.255.255.0*
    * *no sh*
    * *exit*
  * *ip default-gateway 192.168.10.1*
  * *interface vlan 20*
    * *no sh*
    * *exit*
  * *interface vlan 999*
    * *no sh*
    * *exit* 
  * *vlan 10*
    * *name Upravlenie*
    * *end*
  * *conf t*
  * *vlan 20*
    * *name Sales*
    * *end*
  * *conf t*
  * *vlan 999*
    * *name Parking_Lot*  
    * *end*  

* *enable*
  * *configure terminal*
  * *interface vlan 10*
    * *ip address 192.168.10.12 255.255.255.0*
    * *no sh*
    * *exit*
  * *ip default-gateway 192.168.10.1*
  * *interface vlan 30*
    * *no sh*
    * *exit*
  * *interface vlan 999*
    * *no sh*
    * *exit*
  * *vlan 10*
    * *name Upravlenie*
    * *end*
  * *conf t*
  * *vlan 30*
    * *name Operations*
    * *end*
  * *conf t*
  * *vlan 999*
    * *name Parking_Lot*  
    * *end*  
