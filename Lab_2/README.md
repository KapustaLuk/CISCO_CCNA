## Лабораторная работа. Настройка расширенных сетей VLAN, VTP и DTP
###	Топология
![](https://github.com/KapustaLuk/CISCO_CCNA/blob/master/Lab_2/Topol.png)
### 	Таблица адресации

|Заголовок |таблицы	|Интерфейс	IP-адрес|	Маска подсет|
|----------|:------:|------------------:|------------:|
|S1        |VLAN 99	|192.168.99.1	      |255.255.255.0|
|S2	       |VLAN 99	|192.168.99.2	      |255.255.255.0|
|S3	       |VLAN 99	|192.168.99.3	      |255.255.255.0|
|PC-A	     |NIC	    |192.168.10.1	      |255.255.255.0|
|PC-B	     |NIC	    |192.168.20.1	      |255.255.255.0|
|PC-C	     |NIC	    |192.168.10.2	      |255.255.255.0|

### Задачи
* Часть 1. Настройка VTP
* Часть 2. Настройка DTP
* Часть 3. Добавление сетей VLAN и назначение портов
* Часть 4. Настройка расширенной сети VLAN

## Часть 1: VTP
#### Настроить S2 в качестве сервера VTP в домене CCNA с паролем cisco.
Switch>enable  
Switch# configure terminal  
Switch(config)# hostname S2  
S2(config)# enable secret pass1234  
S2(config)# service password-encryption  
S2(config)# vtp domain CCNA  
S2(config)# vtp mode server
####	Настроить S1 и S3
Аналогично только меняем **mode server** на **mode client**
####	Проверить конфигурации VTP
S3# show vtp status
![](https://github.com/KapustaLuk/CISCO_CCNA/blob/master/Lab_2/Screenshot_398.png)
## Часть 2:	Настройка динамического протокола транкинга (DTP)
#### Динамический магистральный канал между S1 и S2
S1(config)# interface e0/1  
S1(config-if)# switchport mode dynamic desirable
#### Между коммутаторами S1 и S3  статический магистральный канал
S1(config)# interface e0/2  
S1(config-if)# switchport mode trunk  
*Command rejected: An interface whose trunk encapsulation is "Auto" can not be configured to "trunk" mode.*  
S1(config-if)# switchport trunk encapsulation  
S1(config-if)# switchport mode trunk  
S1# show interface trunk
![](https://github.com/KapustaLuk/CISCO_CCNA/blob/master/Lab_2/Screenshot_399.png)
#### Постоянная магистраль между коммутаторами S2 и S3.
S2(config)# interface e0/1  
S2(config-if)# switchport mode trunk  
*Command rejected: An interface whose trunk encapsulation is "Auto" can not be configured to "trunk" mode.*  
S2(config-if)# switchport trunk encapsulation  
S2(config-if)# switchport mode trunk
## Часть 3:	Добавление сетей VLAN и назначение портов
#### Добавление сети VLAN на коммутаторах
|VLAN	|Имя  |
|-----|:---:|
|10	  |Red| |
|20	  |Blue |
|30	  |Yellow|
|99	  |Management|

S2(config)# vlan 10  
S2(config-vlan)# name Red  
и т.д.
#### Назначение портов сетям VLAN.
|Назначение портов|	VLAN|	IP-адрес и префикс прикрепленного компьютера|
|-----------------|-----|---------------------------------------------|
|S1 E0/3	  |VLAN 10	|PC-A: 192.168.10.1 /24|
|S2 E0/0	  |VLAN 20	|PC-B: 192.168.20.1 /24|
|S3 E0/0	  |VLAN 10	|PC-C: 192.168.10.2 /24|

S1(config)# interface e0/3  
S1(config-if)# switchport mode access  
S1(config-if)# switchport access vlan 10  
![](https://github.com/KapustaLuk/CISCO_CCNA/blob/master/Lab_2/Screenshot_401.png)
#### Настройка IP-адреса на коммутаторах.
S1(config)# interface vlan 99  
S1(config-if)# ip address 192.168.99.1 255.255.255.0  
S1(config-fi)# no shutdown  

Аналогично S2 и S3
#### Проверить наличие сквозного соединения
1. ping-запрос с компьютера PC-B на PC-A  
   * *Ответа нет в разных VLAN*  
2. ping-запрос с компьютера PC-A на PC-C
   * *Ответ есть в одном VLAN*
3. ping-запрос с коммутатора S1 на компьютер PC-A
   * *Ответа нет не назначен ip-адрес на S1 в Vlan 10*
4. ping-запрос с коммутатора S2 на коммутатор S1
   * *В одном VLAN и назначены ip-адреса*
