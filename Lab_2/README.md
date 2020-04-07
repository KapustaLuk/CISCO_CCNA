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
##### Настройте S2 в качестве сервера VTP в домене CCNA с паролем cisco.
Switch>enable  
Switch# configure terminal  
Switch(config)# hostname S2  
S2(config)# enable secret pass1234  
S2(config)# service password-encryption  
S2(config)# vtp domain CCNA  
S2(config)# vtp mode server
#####	Настройте S1 и S3
Аналогично только меняем **mode server** на **mode client**
#####	Проверьте конфигурации VTP
S3# show vtp status
![](https://github.com/KapustaLuk/CISCO_CCNA/blob/master/Lab_2/Screenshot_398.png)
