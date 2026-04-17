# 网作业

Работа выполнена на ПК с установленным Cisco Packet Tracer.

### Часть 1. Создание сети и проверка настроек коммутатора по умолчанию

#### 1.1 Топология

![alt text](image.png)

#### 1.2 Проверка настроек коммутатора по умолчанию 

a. Введём команду *enable*, чтобы войти в привилегированный режим EXEC. Далее вводим команду *show running-config*. На коммутатор находиться пустой файл конфигурации по умолчанию. Оставляем. 

b. Изучите текущий файл running configuration.

Сколько интерфейсов FastEthernet имеется на коммутаторе 2960? 
- 24

Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?
- 2

Каков диапазон значений, отображаемых в vty-линиях?

- 0-4; 5-15.

c. Изучите файл загрузочной конфигурации (startup configuration), который содержится в энергонезависимом ОЗУ (NVRAM).

Вводим команду *show startup-config*

-	startup-config is not present

d. Изучите характеристики SVI для VLAN 1.

Вводим команду *show vlan* и *show vlan*

Назначен ли IP-адрес сети VLAN 1?

- Нет

Данный интерфейс включен?

- Нет

e.	Изучите IP-свойства интерфейса SVI сети VLAN 1.

Какие выходные данные вы видите?

  SW0#show ip int br
  Interface              IP-Address      OK? Method Status                Protocol 
  FastEthernet0/1        unassigned      YES manual down                  down 
  FastEthernet0/2        unassigned      YES manual down                  down 
  FastEthernet0/3        unassigned      YES manual down                  down 
  FastEthernet0/4        unassigned      YES manual down                  down 
  FastEthernet0/5        unassigned      YES manual down                  down 
  FastEthernet0/6        unassigned      YES manual doun                  down 
  FastEthernet0/7        unassigned      YES manual down                  down 
  FastEthernet0/8        unassigned      YES manual down                  down 
  FastEthernet0/9        unassigned      YES manual down                  down 
  FastEthernet0/10       unassigned      YES manual down                  down 
  FastEthernet0/11       unassigned      YES manual down                  down 
  FastEthernet0/12       unassigned      YES manual down                  down 
  FastEthernet0/13       unassigned      YES manual down                  down 
  FastEthernet0/14       unassigned      YES manual down                  down 
  FastEthernet0/15       unassigned      YES manual down                  down 
  FastEthernet0/16       unassigned      YES manual down                  down 
  FastEthernet0/17       unassigned      YES manual down                  down 
  FastEthernet0/18       unassigned      YES manual down                  down 
  FastEthernet0/19       unassigned      YES manual down                  down 
  FastEthernet0/20       unassigned      YES manual down                  down 
  FastEthernet0/21       unassigned      YES manual down                  down 

f.	Подсоедините кабель Ethernet компьютера PC-A к порту 6 на коммутаторе и изучите IP-свойства интерфейса SVI сети VLAN 1. Дождитесь согласования параметров скорости и дуплекса между коммутатором и ПК.

Какие выходные данные вы видите?

     SW0#show ip int br
  Interface              IP-Address      OK? Method Status                Protocol 
  FastEthernet0/1        unassigned      YES manual down                  down 
  FastEthernet0/2        unassigned      YES manual down                  down 
  FastEthernet0/3        unassigned      YES manual down                  down 
  FastEthernet0/4        unassigned      YES manual down                  down 
  FastEthernet0/5        unassigned      YES manual down                  down 
  FastEthernet0/6        unassigned      YES manual up                    up 
  FastEthernet0/7        unassigned      YES manual down                  down 
  FastEthernet0/8        unassigned      YES manual down                  down 
  FastEthernet0/9        unassigned      YES manual down                  down 
  FastEthernet0/10       unassigned      YES manual down                  down 
  FastEthernet0/11       unassigned      YES manual down                  down 
  FastEthernet0/12       unassigned      YES manual down                  down 
  FastEthernet0/13       unassigned      YES manual down                  down 
  FastEthernet0/14       unassigned      YES manual down                  down 
  FastEthernet0/15       unassigned      YES manual down                  down 
  FastEthernet0/16       unassigned      YES manual down                  down 
  FastEthernet0/17       unassigned      YES manual down                  down 
  FastEthernet0/18       unassigned      YES manual down                  down 
  FastEthernet0/19       unassigned      YES manual down                  down 
  FastEthernet0/20       unassigned      YES manual down                  down 
  FastEthernet0/21       unassigned      YES manual down                  down        

g.	Изучите сведения о версии ОС Cisco IOS на коммутаторе.

Вводим команду *shoe version*

Под управлением какой версии ОС Cisco IOS работает коммутатор?

- Version 15.0(2)SE4

Как называется файл образа системы?

- C2960 Software (C2960-LANBASEK9-M)

h.	Изучите свойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A.

Вводим *show interface f0/6*.

Интерфейс включен или выключен?

- Выключен.

Что нужно сделать, чтобы включить интерфейс?

- Зайти в глобальную конфигурацию *conf t*; войти в нужный интерфейс *int f0/6* ; разрешить интерфейсу работать *no shutdown*.

i.	Изучите флеш-память.

Выполнияем следующие команды, чтобы изучить содержимое флеш-каталога.(команды равнозначны):

*show flash*

*dir flash:* 

В конце имени файла указано расширение, например .bin. Каталоги не имеют расширения файла.

Какое имя присвоено образу Cisco IOS?

- 2960-lanbasek9-mz.150-2.SE4.bin

### Часть 2. Настройка базовых параметров сетевых устройств

#### 1.1 Настройка базовых параметров коммутатора


#### 1.2 Настройка IP-адрес на компьютере PC-A.



