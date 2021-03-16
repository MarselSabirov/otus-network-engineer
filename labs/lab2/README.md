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
   *MAC-адрес компьютера PC-A:*   
        ![2_1_mac_pcA](https://user-images.githubusercontent.com/18709313/111381589-4795d300-867c-11eb-8b78-5d6c9184dde7.png)

   *MAC-адрес компьютера PC-B:*   
        ![2_1_mac_pcB](https://user-images.githubusercontent.com/18709313/111381599-4bc1f080-867c-11eb-8a5a-b3901331ec2b.png)

