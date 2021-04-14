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

#### Шаг 2. Настройте коммутатор для соединения по протоколу SSH.
##### *a. Настройте имя устройства, как указано в таблице адресации.* - hostname S1

##### *b. Задайте домен для устройства.* - ip domain-name S1.domain

##### c. Создайте ключ шифрования с указанием его длины.
![3_2_c_generate_ssh_key_S1](https://user-images.githubusercontent.com/18709313/114736064-c7fb3280-9d13-11eb-9e7c-1c4f76a05955.png)

##### d. Создайте имя пользователя в локальной базе учетных записей.
![3_2_d_add_ssh_username_S1](https://user-images.githubusercontent.com/18709313/114736317-fd078500-9d13-11eb-91b8-3df92bdef108.png)

##### e. Активируйте протоколы Telnet и SSH на линиях VTY.
![3_2_e_activate_ssh_S1](https://user-images.githubusercontent.com/18709313/114736537-29230600-9d14-11eb-8000-80112b0b2480.png)

##### f. Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.
![3_2_f_enable_local_ssh_S1](https://user-images.githubusercontent.com/18709313/114736695-4f48a600-9d14-11eb-98c2-8cd08cc21d19.png)

#### Шаг 3. Установите соединение с коммутатором по протоколу SSH.
![3_3_1_ssh_connecting_from_PC_A](https://user-images.githubusercontent.com/18709313/114737144-b9f9e180-9d14-11eb-9ffd-21906fc251e2.png)

![3_3_2_ssh_connected_from_PC_A](https://user-images.githubusercontent.com/18709313/114737167-bebe9580-9d14-11eb-81b7-1b77860efd28.png)


### Часть 4. Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора
#### Шаг 1. Посмотрите доступные параметры для клиента SSH в Cisco IOS.
![4_1_ssh_params_S1](https://user-images.githubusercontent.com/18709313/114738387-e9f5b480-9d15-11eb-97a1-b2b920287e00.png)

#### Шаг 2. Установите с коммутатора S1 соединение с маршрутизатором R1 по протоколу SSH.
![4_2_a_ssh_connect_from_S1_to_R1](https://user-images.githubusercontent.com/18709313/114738548-127dae80-9d16-11eb-8942-bc1acfd47fbf.png)
![4_2_b_ssh_hack_from_S1_to_R1](https://user-images.githubusercontent.com/18709313/114739274-ba937780-9d16-11eb-9330-fa0f3adee241.png)
![4_2_c_ssh_megahack_from_S1_to_R1](https://user-images.githubusercontent.com/18709313/114739290-bebf9500-9d16-11eb-8d0a-aac6c32e0c6d.png)
![4_2_d_ssh_exit](https://user-images.githubusercontent.com/18709313/114739313-c1ba8580-9d16-11eb-8131-0eabcae37b68.png)

#### *Какие версии протокола SSH поддерживаются при использовании интерфейса командной строки?* - Поддерживаются две версии: Protocol Version 1, Protocol Version 2
