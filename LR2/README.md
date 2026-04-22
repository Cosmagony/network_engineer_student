# 网作业

Работа выполнена на ПК с установленным Cisco Packet Tracer.

### Часть 1. Создание и настройка сети
#### 1.1 Топология

![alt text](image.png)

#### 1.2 Настроить узлы ПК

Назначены IP и MAC в соответствии с таблицей.

![alt text](image-1.png)

#### 1.3 Выполнить инициализацию и перезагрузку коммутаторов

#### 1.4 Настроить базовые параметры каждого коммутатора

a. Настраиваем имена устройств в соответствии с топологией.

```
Switch>en
Switch>enable 
Switch#conf t
Switch(config)#hostname S1
S1(config)#
```

```
Switch>enable
Switch#
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#ho
Switch(config)#hostname 
Switch(config)#hostname S2
```


b. Настройте IP-адреса, как указано в таблице адресации.

Коммутатор - S1

```
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#no ip domain-lookup 
S1(config)#ba
S1(config)#banner m
S1(config)#banner motd # NIHAO #
S1(config)#int vlan 1
S1(config-if)#no shutdown
S1(config-if)#ip ad
S1(config-if)#ip address 192.168.1.1 255.255.255.0
S1(config-if)#
```

Коммутатор - S2

```
S2(config)#ban
S2(config)#banner m
S2(config)#banner motd # NIHAO #
S2(config)#int vl
S2(config)#int vlan 
% Incomplete command.
S2(config)#int vlan 1
S2(config-if)#no sh
S2(config-if)#no shutdown 
S2(config-if)#ip a
S2(config-if)#ip address  192.168.1.12 255.255.255.0
```

c. Назначьте cisco в качестве паролей консоли и VTY.

Коммутатор - S1

```
S1>ena
S1>enable 
Password: 
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#line 
S1(config)#line c
S1(config)#line console 0
S1(config-line)#pass
S1(config-line)#exit
S1(config)#line vty 0 15
S1(config-line)#pass
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#
S1(config-line)#exit
S1(config)#line console 0
S1(config-line)#pas
S1(config-line)#password cisco
S1(config-line)#login
```

Коммутатор - S2

```
S2>enable
Password: 
Password: 
S2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#line v
S2(config)#line vty 0 15
S2(config-line)#pass
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#line co
S2(config)#line console 
% Incomplete command.
S2(config)#line console 0
S2(config-line)#pas
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#
```

d. Назначьте class в качестве пароля доступа к привилегированному режиму EXEC.

Коммутатор - S1

```
S1(config)#ser
S1(config)#service p
S1(config)#service password-encryption 
S1(config)#en
S1(config)#ena
S1(config)#enable  se
S1(config)#enable  secret class
```

Коммутатор - S2
```
S2(config)#serv
S2(config)#service 
S2(config)#service pa
S2(config)#service password-encryption 
S2(config)#ena
S2(config)#enable  se
S2(config)#enable  secret class
```

### Часть 2. Настройка базовых параметров сетевых устройств

#### 1.1 Настройка базовых параметров коммутатора

a. В режиме глобальной конфигурации выставляем базовые параметры

     *enable;* 
     *conf t;*
     *no ip domain-lookup
     hostname S1
     service password-encryption
     enable secret class
     banner motd # Unauthorized access is strictly prohibited. #* 

b. Назначаем IP-адрес интерфейсу SVI на коммутаторе. Благодаря этому получаем возможность удаленного управления коммутатором. Прежде чем сможем управлять коммутатором S1 удаленно с компьютера PC-A, коммутатору нужно назначить IP-адрес. Согласно конфигурации по умолчанию коммутатором можно управлять через VLAN 1.
 
 Устройство | Интерфейс | IP-адрес/префикс
:----------:|:---------:|:------------------:
 S1         | VLAN 1    | 192.168.1.11/24
 S2         | VLAN 1    | 192.168.1.12/24
 PC-A       | NIC       | 192.168.1.1/24
 PC-B       | NIC       | 192.168.1.2/24

       *enable*
       *conf t*
       *int vlan 1*
       *ip address 192.168.1.2 255.255.255.0*


c. Доступ через порт консоли ограничиваем с помощью пароля. Используем *cisco* в качестве пароля для входа в консоль. Конфигурация по умолчанию разрешает все консольные подключения без пароля. Чтобы консольные сообщения не прерывали выполнение команд, используем параметр *logging synchronous*.
  
      *enable*
      *conf t*
      *enable secret cisco*
      *exit*
      *line con 0*
      *logging synchronous*

d. Настроим каналы виртуального соединения для удаленного управления (vty), чтобы коммутатор разрешил доступ через Telnet. Если не настроить пароль VTY, будет невозможно подключиться к коммутатору по протоколу Telnet.

      *enable*
      *conf t*
      *line vty 0 15*
      *password cisco*
      *login*

Для чего нужна команда login?

- Для включения требования аутентификации, которая обязательна для подключения.

#### 1.2 Настройка IP-адрес на компьютере PC-A.



### Часть 3. Проверка сетевых подключений

#### 3.1 Отображаем конфигурацию коммутатора



#### 3.2 Протестируем сквозное соединение, отправив эхо-запрос.



#### 3.3 Проверяем удаленное управление коммутатором S1


### 4. Вопросы для повторения


