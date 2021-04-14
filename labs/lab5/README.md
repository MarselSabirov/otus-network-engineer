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
  * reload *
#### Шаг 4. Настройте компьютер PC-A.
![1_PC_A_ip_config](https://user-images.githubusercontent.com/18709313/114718582-1f91a200-9d04-11eb-9620-adcf398e7fd6.png)

#### Шаг 5. Проверьте подключение к сети.
![1_PC_A_ping_to_R1](https://user-images.githubusercontent.com/18709313/114718779-57004e80-9d04-11eb-9126-7eca0b30545f.png)

### Часть 2. Настройка маршрутизатора для доступа по протоколу SSH
#### Шаг 1. Настройте аутентификацию устройств.
##### *a. Задайте имя устройства* - hostname R1
##### *a. Задайте домен для устройства* - ip domain-name R1.domain

#### Шаг 2. Создайте ключ шифрования с указанием его длины.
![2_generate_ssh_key_R1](https://user-images.githubusercontent.com/18709313/114721237-c119f300-9d06-11eb-82d7-9eef0aef0b8d.png)

#### Шаг 3. Создайте имя пользователя в локальной базе учетных записей.
![2_generate_ssh_key_R1](https://user-images.githubusercontent.com/18709313/114722863-27534580-9d08-11eb-9b95-9c560b206cc7.png)

#### Шаг 4. Активируйте протокол SSH на линиях VTY.
##### *a. Активируйте протоколы Telnet и SSH на входящих линиях VTY с помощью команды transport input.*
![2_4_a_activate_ssh_R1](https://user-images.githubusercontent.com/18709313/114723467-acd6f580-9d08-11eb-887d-7792a6176efa.png)

##### *Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.*
![2_4_b_ssh_local_login_R1](https://user-images.githubusercontent.com/18709313/114723832-fb848f80-9d08-11eb-98a4-1b561f2a5f2f.png)

#### Шаг 5. Сохраните текущую конфигурацию в файл загрузочной конфигурации.
##### reload

#### Шаг 6. Установите соединение с маршрутизатором по протоколу SSH.
![2_6_ssh_connecting_from_PC_A](https://user-images.githubusercontent.com/18709313/114724597-b7de5580-9d09-11eb-9aae-9fef8210915c.png)
![2_6_ssh_connected_from_PC_A](https://user-images.githubusercontent.com/18709313/114724604-ba40af80-9d09-11eb-9184-609f0bc6dc48.png)


### Часть 3. Настройка коммутатора для доступа по протоколу SSH
#### Шаг 1. Настройте основные параметры коммутатора.
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
  * *exit*
  * interface vlan 1*
    * *ip address 192.168.1.11 255.255.255.0*
    * *no sh*
    * *exit*
  * ip default-gateway 192.168.1.1 
  * reload *
