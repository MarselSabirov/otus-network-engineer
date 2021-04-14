# Лабораторная работа 5. Доступ к сетевым устройствам по протоколу SSH

## Топология
![lab_5_scheme](https://user-images.githubusercontent.com/18709313/114632978-63ff4c80-9cc8-11eb-981a-48c34ca4567b.png)


## Таблица адресации
Устройство | Интерфейс | IP-адрес | Маска подсети | Шлюз по умолчанию
------------ | ------------- | ------------- | ------------- | -------------
R1 | G0/0/1 | 192.168.1.1 | 255.255.255.0 | —
S1 | VLAN 1 | 192.168.1.11 | 255.255.255.0 | 192.168.1.1
PC-A | NIC | 192.168.1.3 | 255.255.255.0 | 192.168.1.1

## Задачи
### Часть 1. Настройка основных параметров устройства
### Часть 2. Настройка маршрутизатора для доступа по протоколу SSH
### Часть 3. Настройка коммутатора для доступа по протоколу SSH
### Часть 4. SSH через интерфейс командной строки (CLI) коммутатора


## Решение
### Часть 1. Настройка основных параметров устройств
#### Шаг 3. Настройте маршрутизатор:
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
  * interface gigabitEthernet 0/0/1
    * *ip address 192.168.1.1 255.255.255.0*
    * *no sh*
  * *exit*




### Часть 3. Настройка коммутатора
##### Коммутатор:
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
