# Конфигурация безопасности коммутатора

Работа выполнена на ПК с установленным Cisco Packet Tracer.

#### Топология

![alt text](image.png)

#### Таблица адресации

 Устройство | Интерфейс | IP-адрес/префикс | Маска подсети.
:----------:|:---------:|:----------------:| :---------:
 R1         | G0/0/1      | 192.168.10.1      | 255.255.255.0
            | LOOPBACK 0  | 10.10.1.1       | 
 S1         | VLAN 10  | 192.168.10.201      | 255.255.255.0
 S2         | VLAN 10   | 192.168.10.202      | 255.255.255.0
  PC-A      | NIC       | DHCP             | 255.255.255.0
   PC-B     | NIC        | DHCP           | 255.255.255.0


### Часть 1. Настройка основного сетевого устройства

#### Шаг 1.1. Настройте маршрутизатор R1.

a.	Загрузите следующий конфигурационный скрипт на R1.
Откройте окно конфигурации
enable
configure terminal
hostname R1
no ip domain lookup
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.10.201 192.168.10.202
ip dhcp relay information trust-all
!
ip dhcp pool Students
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 domain-name CCNA2.Lab-11.6.1
!
interface Loopback0
 ip address 10.10.1.1 255.255.255.0
!
interface GigabitEthernet0/0/1
 description Link to S1
 ip address 192.168.10.1 255.255.255.0
 no shutdown
!
line con 0
 logging synchronous
 exec-timeout 0 0


```
Router>enable
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#no ip domain lookup
R1(config)#ip dhcp excluded-address 192.168.10.1 192.168.10.9
R1(config)#ip dhcp excluded-address 192.168.10.201 192.168.10.202
R1(config)#ip dhcp relay information trust-all
R1(config)#!
R1(config)#ip dhcp pool Students
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.1
R1(dhcp-config)# domain-name CCNA2.Lab-11.6.1
R1(dhcp-config)#!
R1(dhcp-config)#interface Loopback0

R1(config-if)# ip address 10.10.1.1 255.255.255.0
R1(config-if)#!
R1(config-if)#interface GigabitEthernet0/0/1
R1(config-if)# description Link to S1
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shutdown

R1(config-if)#!
R1(config-if)#line con 0
R1(config-line)# logging synchronous
R1(config-line)# exec-timeout 0 0
R1(config-line)#
%LINK-5-CHANGED: Interface Loopback0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up

%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up
```


b.	Проверьте текущую конфигурацию на R1, используя следующую команду:
R1# show ip interface brief

```
R1#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   192.168.10.1    YES manual up                    up 
GigabitEthernet0/0/2   unassigned      YES unset  administratively down down 
Loopback0              10.10.1.1       YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
```

c.	Убедитесь, что IP-адресация и интерфейсы находятся в состоянии up / up (при необходимости устраните неполадки).

Всё поднято

#### Шаг 1.2. Настройка и проверка основных параметров коммутатора

a.	Настройте имя хоста для коммутаторов S1 и S2.

```
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#ho
Switch(config)#hostname S
Switch(config)#hostname S1
```


```
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#ho
Switch(config)#hostname S2
```


b.	Запретите нежелательный поиск в DNS.

```
S1(config)#no ip domain-lookup 
```


```
S2(config)#no ip domain-lookup 
```


c.	Настройте описания интерфейса для портов, которые используются в S1 и S2.

```
S1(config)#int
S1(config)#interface f0/5
S1(config-if)#desc
S1(config-if)#description trunk_R1_g0/0/1
S1(config-if)#exit
S1(config)#int
S1(config)#interface F0/1
S1(config-if)#de
S1(config-if)#description trunk_S2_f0/1
S1(config-if)#exit
S1(config)#int
S1(config)#interface f0/6
S1(config-if)#des
% Incomplete command.
S1(config-if)#de
S1(config-if)#description 
% Incomplete command.
S1(config-if)#description access_pc-
S1(config-if)#description access_PC-A
S1(config-if)#EXIT
```


```
S2(config)#int
S2(config)#interface f0/1
S2(config-if)#de
S2(config-if)#description trunk_S1_f0/1
S2(config-if)#exit
S2(config)#interface f0/18
S2(config-if)#des
S2(config-if)#description access_PC-B
```


d.	Установите для шлюза по умолчанию для VLAN управления значение 192.168.10.1 на обоих коммутаторах.

```
S1(config)#ip de
S1(config)#ip default-gateway 192.168.10.1
```


```
S2(config)#ip default-gateway 192.168.10.1
S2(config)#
```


### Часть 2. Настройка сетей VLAN на коммутаторах.

#### Шаг 2.1. Шаг 1. Сконфигруриуйте VLAN 10.

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
S1(config)#interface v
S1(config)#interface vlan 10
S1(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up
de
S1(config-if)#description Management_SVI_VLAN10
S1(config-if)#ip ad
S1(config-if)#ip address 192.168.10.201 255.255.255.0
S1(config-if)#no sh
S1(config-if)#no shutdown 
```



```
S2(config)#interface v
S2(config)#interface vlan 10
S2(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up
des
S2(config-if)#description Management_SVI_VLAN10
S2(config-if)#IP AD
S2(config-if)#IP ADdress 192.168.10.202 255.255.255.0
S2(config-if)#noshu
S2(config-if)#no sh
S2(config-if)#no shutdown 
```


#### Шаг 2.3. Настройте VLAN 333 с именем Native и VLAN 999 с именем ParkingLot на S1 и S2.

```
S1(config)#vlan 333
S1(config-vlan)#na
S1(config-vlan)#name Native
S1(config-vlan)#exit
S1(config)#vlan 999
S1(config-vlan)#na
S1(config-vlan)#name ParkingLot
```


```
S2(config)#vlan 333
S2(config-vlan)#name Native
S2(config-vlan)#exit
S2(config)#vlan 999
S2(config-vlan)#na
S2(config-vlan)#name ParkingLot
S2(config-vlan)#
```

### Часть 3. Настройки безопасности коммутатора.

#### Шаг 3.1. Релизация магистральных соединений 802.1Q.

a.	Настройте все магистральные порты Fa0/1 на обоих коммутаторах для использования VLAN 333 в качестве native VLAN.

```
S1(config)#interface g0/1
S1(config-if)#sw
S1(config-if)#switchport m
S1(config-if)#switchport mode t
S1(config-if)#switchport mode trunk 
S1(config-if)#sw
S1(config-if)#switchport tr
S1(config-if)#switchport trunk vl
S1(config-if)#switchport trunk vla
S1(config-if)#switchport trunk vlan
S1(config-if)#switchport trunk vlan 333
                               ^
% Invalid input detected at '^' marker.
	
S1(config-if)#switchport trunk n
S1(config-if)#switchport trunk native v
S1(config-if)#switchport trunk native vlan 333
S1(config-if)#switchport trunk allowed vlan 10,333,999
```


```
S2(config)#int
S2(config)#interface g0/1
S2(config-if)#sw
S2(config-if)#switchport m
S2(config-if)#switchport mode t
S2(config-if)#switchport mode trunk 
S2(config-if)#sw
S2(config-if)#switchport tr
S2(config-if)#switchport trunk n
S2(config-if)#switchport trunk native v
S2(config-if)#switchport trunk native vlan 333
S2(config-if)#switchport trunk allowed vlan 10,333,999
```

#НА ЭТОМ МОМЕНТЕ ЗАМЕЧЕНА ОШИБКА, ЧТО ВЛАН НАСТРАИВАЛСЯ НЕ НА ТОТ ИНТЕРФЕЙС!!
Повторяю действия. Для безопасности отключаю транк в g0/1 на обоих свичах.


```
S1(config)#default int
S1(config)#default interface g0/1
Building configuration...



Interface GigabitEthernet0/1 set to default configuration
S1(config)#

S1(config-if)#shutdown
```


```
S2(config)#default int
S2(config)#default interface g0/1
Building configuration...


Interface GigabitEthernet0/1 set to default configuration
S2(config)#

S2(config-if)#shutdown
```

Настраиваю нужные интерфейсы


```
S1(config)#int f0/1
S1(config-if)#sw
S1(config-if)#switchport m
S1(config-if)#switchport mode t
S1(config-if)#switchport mode trunk 

S1(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up
sw
S1(config-if)#switchport tr
S1(config-if)#switchport trunk n
S1(config-if)#switchport trunk native v
S1(config-if)#switchport trunk native vlan 333
S1(config-if)#no sh
S1(config-if)#no shutdown 
S1(config-if)#
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (333), with S2 FastEthernet0/1 (1).
%SPANTREE-2-RECV_PVID_ERR: Received BPDU with inconsistent peer vlan id 1 on FastEthernet0/1 VLAN333.

%SPANTREE-2-BLOCK_PVID_LOCAL: Blocking FastEthernet0/1 on VLAN0333. Inconsistent local vlan.


%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (333), with S2 FastEthernet0/1 (1).

%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (333), with S2 FastEthernet0/1 (1).

%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (333), with S2 FastEthernet0/1 (1).
```



```
S2(config)#int f0/1
S2(config-if)#sw
S2(config-if)#switchport m
S2(config-if)#switchport mode tr
S2(config-if)#switchport mode trunk 
S2(config-if)#sw
S2(config-if)#switchport tr
S2(config-if)#switchport trunk n
S2(config-if)#switchport trunk native v
S2(config-if)#switchport trunk native vlan 333
S2(config-if)#%SPANTREE-2-UNBLOCK_CONSIST_PORT: Unblocking FastEthernet0/1 on VLAN0333. Port consistency restored.

%SPANTREE-2-UNBLOCK_CONSIST_PORT: Unblocking FastEthernet0/1 on VLAN0001. Port consistency restored.

no sh
S2(config-if)#no shutdown 
S2(config-if)#
```

b.	Убедитесь, что режим транкинга успешно настроен на всех коммутаторах.
S1# show interface trunk

```
S1#show interfaces trunk 
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      333

Port        Vlans allowed on trunk
Fa0/1       1-1005

Port        Vlans allowed and active in management domain
Fa0/1       1,10,333,999

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       1,10,333,999
```


```
S2#show interfaces trunk 
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      333

Port        Vlans allowed on trunk
Fa0/1       1-1005

Port        Vlans allowed and active in management domain
Fa0/1       1,10,333,999

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       1,10,333,999
```

c.	Отключить согласование DTP F0/1 на S1 и S2. 

```
S1(config)#int f0/1
S1(config-if)#sw
S1(config-if)#switchport ne
S1(config-if)#switchport no
S1(config-if)#switchport nonegotiate 
```


```
S2(config)#int f0/1
S2(config-if)#sw
S2(config-if)#switchport no
S2(config-if)#switchport nonegotiate 
```

d.	Проверьте с помощью команды show interfaces.

```
S1#show interfaces f0/1 switchport | include Negotiation
Negotiation of Trunking: Off
```


```
S2#show interfaces f0/1 switchport | include Negotiation
Negotiation of Trunking: Off
```

#### Шаг 3.2. Настройка портов доступа

a.	На S1 настройте F0/5 и F0/6 в качестве портов доступа и свяжите их с VLAN 10.

```

```


```

```


b.	На S2 настройте порт доступа Fa0/18 и свяжите его с VLAN 10.

```

```



```

```


#### Шаг 3.3. Безопасность неиспользуемых портов коммутатора


#### Шаг 3.4. Документирование и реализация функций безопасности порта.



#### Шаг 3.5. Реализовать безопасность DHCP snooping.



#### Шаг 3.6. Реализация PortFast и BPDU Guard



#### Шаг 3.7. Проверьте наличие сквозного ⁪подключения.






#### Вопрос для повторения


