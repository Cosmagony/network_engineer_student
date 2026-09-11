# Настройка DHCPv6

Работа выполнена на ПК с установленным Cisco Packet Tracer.

#### Топология

![alt text](image-1.png)

#### Таблица адресации

 Устройство | Интерфейс | IP-адрес/префикс 
:----------:|:---------:|:----------------:
 R1         | G0/0/0    | 2001:db8:acad:2::1/64 fe80::1  
            | G0/0/1    | 2001:db8:acad:1::1/64 fe80::1     
 R2         | G0/0/0    | 2001:db8:acad:2::2/64 fe80::2     
            | G0/0/1    | 2001:db8:acad:3::1/64 fe80::1 
  PC-A      | NIC       | DHCP             
   PC-B     | NIC       | DHCP           


### Часть 1. Создание сети и настройка основных параметров устройства

#### Шаг 1.1. Настройка базовых параметров каждого коммутатора

a.	Присвойте коммутатору имя устройства.

```
Switch(config)#hostname S1
```

```
Switch(config)#hostname S2
```

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

```
S1(config)#no ip domain-lookup 
```

```
S2(config)#no ip domain-lookup 

```

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

-

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

-

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

-

f.	Зашифруйте открытые пароли.

```
S1(config)#service password-encryption 
```

```
S2(config)#service password-encryption 
```

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

```
S1(config)#banner motd #AAAAA#
```


```
S2(config)#banner motd #BBBBB#
```

h.	Отключите все неиспользуемые порты.

```
S1(config)#vlan 999
S1(config-vlan)#na
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#exit
S1(config)#int range f0/1-4 ,f0/7-24, g0/1-2
S1(config-if-range)#sw
S1(config-if-range)#switchport m
S1(config-if-range)#switchport mode ac
S1(config-if-range)#switchport mode access 
S1(config-if-range)#sw
S1(config-if-range)#switchport ac
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shut
S1(config-if-range)#shutdown 

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/18, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
```


```
S2(config)#vlan 999
S2(config-vlan)#na,
S2(config-vlan)#na
S2(config-vlan)#name Parking_Lot
S2(config-vlan)#exit
S2(config)#int range f0/1-4 , f0/6-17 ,f0/19-24, g0/1-2
S2(config-if-range)#sw
S2(config-if-range)#switchport m
S2(config-if-range)#switchport mode ac
S2(config-if-range)#switchport mode access 
S2(config-if-range)#sw
S2(config-if-range)#switchport ac
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#sg
S2(config-if-range)#sh

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/6, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
S2(config-if-range)#
```


i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
S1#copy running-config st
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

```
S2#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

#### Шаг 1.2. Произведите базовую настройку маршрутизаторов.

a.	Назначьте маршрутизатору имя устройства.

```
Router(config)#hostname R1
```


```
Router(config)#hostname R2
```

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

```
R1(config)#no ip domain-lookup 
```

```
R2(config)#no ip domain-lookup 
```

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

```
R1(config)#enable secret class
```

```
R2(config)#enable secret class
```

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

```
R1(config)#line console 0
R1(config-line)#oas
R1(config-line)#pas
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
```

```
R2(config)#line console 0
R2(config-line)#oas
R2(config-line)#pas
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
```


e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

```
R1(config)#line vt
R1(config)#line vty 0 4
R1(config-line)#pas
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
```

```
R2(config)#line vt
R2(config)#line vty 0 4
R2(config-line)#pas
R2(config-line)#password cisco
R2(config-line)#login
```

f.	Зашифруйте открытые пароли.

```
R1(config)#service password-encryption 
```

```
R2(config)#service password-encryption 
```

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

```
R1(config)#banner motd #GO#
```

```
R2(config)#banner motd #NOT GO#
```

h.	Активация IPv6-маршрутизации

```
R1(config)#ipv6 unicast-routing
```

```
R2(config)#ipv6 un
R2(config)#ipv6 unicast-routing 
```

i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
R1#copy running-config st
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

```
R2#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

#### Шаг 1.3.Настройка интерфейсов и маршрутизации для обоих маршрутизаторов.

a.	Настройте интерфейсы G0/0/0 и G0/1 на R1 и R2 с адресами IPv6, указанными в таблице выше.

```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int g0/0/0
R1(config-if)#des
R1(config-if)#description To_R2
R1(config-if)#description To_R2_g0/0/0
R1(config-if)#ipv6 address 2001:db8:acad:2::1/64
R1(config-if)#ipv6 address fe80::1 li
R1(config-if)#ipv6 address fe80::1 link-local 
R1(config-if)#no sh
R1(config-if)#no shutdown 

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

R1(config-if)#exit
R1(config)#int g0/0/1
R1(config-if)#description To_S1_g0/0/1
R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
R1(config-if)#ipv6 address fe80::1 link-local 
R1(config-if)#no shutdown 

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up
```

```
R2(config)#int g0/0/0
R2(config-if)#de
R2(config-if)#des
R2(config-if)#description To_R1_g0/0/0
R2(config-if)#ipv6 address 2001:db8:acad:2::2/64
R2(config-if)#ipv6 address fe80::2 li
R2(config-if)#ipv6 address fe80::2 link-local 
R2(config-if)#no sh
R2(config-if)#no shutdown 

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up
rxit
              ^
% Invalid input detected at '^' marker.
	
R2(config-if)#int g0/0/1
R2(config-if)#description To_S2_g0/0/1
R2(config-if)#ipv6 address 2001:db8:acad:3::1/64
R2(config-if)#ipv6 address fe80::1 link-local 
R2(config-if)#no shutdown 

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up
```
![alt text](image.png)

b.	Настройте маршрут по умолчанию на каждом маршрутизаторе, который указывает на IP-адрес G0/0/0 на другом маршрутизаторе.

```

```

```

```

c.	Убедитесь, что маршрутизация работает с помощью пинга адреса G0/0/1 R2 из R1

```

```

d.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```

```

```

```

### Часть 2. Проверка назначения адреса SLAAC от R1

#### Шаг 2.1. 

```
S1(config)#vlan 10
S1(config-vlan)#name Management
```


```
S2(config)#vla
S2(config)#vlan  10
S2(config-vlan)#n
S2(config-vlan)#na
S2(config-vlan)#name 
S2(config-vlan)#name Management
```

```
S2#show vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
10   Management                       active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

#### Шаг 2.2. Сконфигруриуйте SVI для VLAN 10.

```

```



```

```


#### Шаг 2.3. Настройте VLAN 333 с именем Native и VLAN 999 с именем ParkingLot на S1 и S2.

```

```


```

```

### Часть 3. Настройки безопасности коммутатора.

#### Шаг 3.1. Релизация магистральных соединений 802.1Q.



#### Шаг 3.2. Настройка портов доступа


#### Шаг 3.3. Безопасность неиспользуемых портов коммутатора



#### Шаг 3.4. 




 
#### Вопрос для повторения


