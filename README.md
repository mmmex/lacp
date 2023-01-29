## LACP

Задачи:

- [X] в Office1 в тестовой подсети появляются сервера с доп интерфесами и адресами в internal сети testLAN:
  - testClient1 - 10.10.10.254
  - testClient2 - 10.10.10.254
  - testServer1- 10.10.10.1
  - testServer2- 10.10.10.1
- [X] развести вланами
  - testClient1 <-> testServer1
  - testClient2 <-> testServer2
- [X] между centralRouter и inetRouter "пробросить" 2 линка (общая inernal сеть) и объединить их в бонд
- [X] проверить работу c отключением интерфейсов
- [X] Формат сдачи ДЗ - vagrant + ansible

### Запуск проекта

1. Клонируем репозиторий: `git clone ...`
2. Переходим в папку: `cd lacp`
3. Запускаем проект: `vagrant up`

Будут подняты 6 ВМ:

| Имя хоста     | Сетевые интерфейсы | Соседи |
|:--------------|:---|:---|
| inetRouter    | eth0: 10.0.2.2 (DHCP); bond0: 192.168.255.1/30 (Static); eth1: (bond-slave); eth2: (bond-slave) | centralRouter |
| centralRouter | bond0: 192.168.255.2/30 (Static); eth1: (bond-slave); eth2: (bond-slave); eth3: <no IP>(Trunk); eth3.100: <no IP> (Access vlanid: 100); eth3.101: <no IP> (Access vlanid: 101) | inetRouter, testClient1, testClient2, testServer1, testServer2 |
| testClient1   | eth1: <no IP> (Trunk); vlan100@eth1: 10.10.10.1/24 (Access vlanid: 100) | testServer1, centralRouter |
| testServer1   | eth1: <no IP> (Trunk); vlan100@eth1: 10.10.10.254/24 (Access vlanid: 100) | testClient1, centralRouter |
| testClient2   | eth1: <no IP> (Trunk); vlan101@eth1: 10.10.10.1/24 (Access vlanid: 101) | testServer2, centralRouter |
| testServer2   | eth1: <no IP> (Trunk); vlan101@eth1: 10.10.10.254/24 (Access vlanid: 101) | testClient2, centralRouter |

Настройка выполнена согласно схемы:

![image](https://raw.githubusercontent.com/mmmex/lacp/master/network23-1801-024140.png)

### Демонстрация с отключением интерфейсов

[![asciicast](https://asciinema.org/a/UV5DwX1vyW0sWtWtle8v9i2KQ.svg)](https://asciinema.org/a/UV5DwX1vyW0sWtWtle8v9i2KQ)