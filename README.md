# Домашнее задание
# "Базовая настройка коммутатора"

<details>
  <summary>инструменты и оборудование</summary>

```
 Cisco packet tracer v8.0
 создаю виртуальную пару: свитч+ ПК. 
"Соединяю" их между собой консольным и лан-кабелем 
"используя" соответствующие разъёмы:
консольный на свитче, com- порт на ПК 
и GigabitEthernet0/0/1 на свитче+ GigabitEthernet на ПК
для соединения по витой паре.
```


</details>

![This is a alt text.](https://github.com/Aiculedsoul/Aiculedsoul/blob/main/pic.JPG?raw=true")

## Таблица ip адресов

| Устройство  |Интерфейс подключения ip/mask |
| ------------- |:-------------:|
| __SW1__      | VLAN1 10.0.0.1/8    |
| __PK__      | Lan 10.0.0.2/8     |

__Приступаю к настройке свитча через CLI__
<details>
  <summary>базовая настройка</summary>
  
__*Переключаю консоль в расширенный режим:*__
 
 Switch>en
 
__*Проверяю текущую конфигурацию устройства:*__

Switch#sh run

__*Удаляю предыдущую конфигурацию устройства:*__

Switch#erase startup-config

Switch#delete vlan.dat

__*Перезагружаю устройство:*__

Switch#reload

__*Устанавливаю правильные дату и время:*__

Switch>en

Switch#clock set 17:50:30 17 april 2022
  
__*перехожу в режим конфигурации терминала:*__

Switch#conf t
   
__*Переименовываю "Router" в "SW1"*__

Switch(config)#hostname SW1

__*отключаю поиск доменных имён командой:*__

SW1(config)#no ip domain-lookup

</details>

<details>
  <summary>Настройки безопасности</summary>
  
__*устанавливаю пароли:*__

__*1: На интерфейс*__ console-0

SW1(config)#line console 0

SW1(config-line)#password cisco

__*2: На привилегированный режим:*__

ctrl+z

configure

SW1(config)#enab secret class

__*3: На линии vty:*__

SW1(config)#line vty 0 4

SW1(config-line)#password cisco

SW1(config-line)#login

SW1(config-line)#transport input all

__*4: Устанавливаю параметры шифрования паролей при работе в CLI:*__

SW1(config)#service password-encryption

__*5: Настраиваю "приветственное сообщение"

SW1(config)#banner motd 8 Vam tut ne rady!!! GO AWAY!!! 8

</details>

<details>
  <summary>Настраиваю виртуальный интерфейс коммутатора:</summary>
  
SW1(config)#interface vlan 1

__*Назначаю ip адрес и маску подсети интерфейса:*__

SW1(config-if)#ip address 10.0.0.1 255.0.0.0

__*Включаю интерфейс vlan1:*__

SW1(config-if)#no shutdown

__*Добавляю описание интерфейсу:*__

ctrl+z
SW1(config)#int g0/1
SW1(config-if)#desc link to PC

</details>

__*Сохраняю текущую конфигурацию в энергонезависимую память:*__

SW1#copy running-config startup-config

__*Kомандой: ping Проверяю доступность подключённого ПК*__

SW1#ping 10.0.0.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.0.0.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
