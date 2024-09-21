University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)  
Year: 2024/2025  
Group: K34212  
Author: Denis Fomintsev  
Lab: Lab1  
Date of create: 19.09.2024  
Date of finished: 21.09.2024  

## Лабораторная работа №1 "Установка CHR и Ansible, настройка VPN"
### Оглавление
- [Описание](#section1)
- [Цель работы](#section2)
- [Ход работы](#section4)
  - [Selectl VDS](#section4.1)
  - [VirtualBox и CHR (RouterOS)](#section4.2)
  - [Создание интерфейса Wireguard на RouterOS](#section4.3)
  - [Создание интерфейса Wireguard на Selectl VDS](#section4.4)
  - [Тесты](#section4.5)
- [Вывод](#section4.6)

## <a name="section1">Описание</a>
Данная работа предусматривает обучение развертыванию виртуальных машин (VM) и системы контроля конфигураций Ansible а также организации собственных VPN серверов.

## <a name="section2">Цель работы</a>
Целью данной работы является развертывание виртуальной машины на базе платформы Microsoft Azure с установленной системой контроля конфигураций Ansible и установка CHR в VirtualBox.

## <a name="section4">Ход работы</a>    

### <a name="section4.1">Selectl VDS</a> 
Для удобства было принято решение воспользоваться сервисом Selectl VDS. После приобретения и установки сервера, было воспроизведено подключение к нему по ssh
<p align="center"><img src="https://github.com/user-attachments/assets/d7f8e30a-27f7-4748-b2b9-8965a3dda393" width=700></p>

<p align="center"><img src="https://github.com/user-attachments/assets/71250134-f53d-4a06-a025-f84c42bf6514" width=700></p>

А также установлены необходимые компоненты: python, ansible
<p align="center"><img src="https://github.com/user-attachments/assets/e51322f6-1e3f-4880-82a8-6aadf4b3152d" width=700></p>

### <a name="section4.2">VirtualBox и CHR (RouterOS)</a> 
CHR (Cloud Hosted Router) от MikroTik — это виртуальная версия маршрутизатора RouterOS, предназначенная для использования в виртуализированных средах.

<p align="center"><img src="https://github.com/user-attachments/assets/8af060b6-5cbe-4df5-b22a-90d8f5a55c67" width=700></p>

Переключим режим работы сети на Bridge, чтобы напрямую подключаться к физической сети, используя тот же сетевой интерфейс, что и основной хост.

<p align="center"><img src="https://github.com/user-attachments/assets/ba291d68-ad23-4aaf-ab1d-5156a87ebf7f" width=700></p>

Теперь можно подключиться к ВМ с помощью VinBox

<p align="center"><img src="https://github.com/user-attachments/assets/36ec8770-12f0-4da4-8417-2ce7e3e23131" width=700></p>

### <a name="section4.3">Создание интерфейса Wireguard на RouterOS (клиент)</a> 

В старших версиях RouterOS уже есть встроенный Wireguard интерфейс, поэтому включаем его и настраиваем (ключи автоматически генерируются).

<p align="center"><img src="https://github.com/user-attachments/assets/1f7a871d-648e-4f65-a511-a6b85208cd53" width=700></p>

Кроме самого интерфейса важно указать peer с endpoint ip, портом нашего сервера и публичным ключом, который будет сгенерирован на сервере

<p align="center"><img src="https://github.com/user-attachments/assets/1bccea63-092e-4168-8b6d-fdf5e9bed3f2" width=700></p>

### <a name="section4.4">Создание интерфейса Wireguard на Selectl VDS (сервер)</a> 

На самом сервере создаем пару ключей

```bash
/etc/wireguard/
wg genkey > privatekey
wg pubkey < privatekey > publickey
```
А также создадим конфигурационный файл, с интерфейсом, в котором укажем приватный ключ сервера, IP из той же подсети и peer, в котором будет публичный ключ клиента и его ip 

```bash
root@my-serv-spb:~# cat /etc/wireguard/wg0.conf 
[Interface]

PrivateKey = EDY9niad14YAoKrj8Dr6LexrcglpsHDubtmm5Gy+/GQ=
Address = 10.0.0.1/24
ListenPort = 51820
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

[Peer]

PublicKey = yQr5zsil3vtV0T+Yh+eUpX0Jli8OlJY9RXellkCDlSE=
AllowedIPs = 10.0.0.2/32
```

Команды PostUp и PostDown используются для автоматической настройки правил iptables при поднятии (up) и опускании (down) WireGuard-интерфейса. 

<p align="center"><img src="https://github.com/user-attachments/assets/55506bf2-6fdf-42af-8c64-e5080c7ec318" width=700></p>

### <a name="section4.5">Тесты</a> 

* Клиент -> Сервер

<p align="center"><img src="https://github.com/user-attachments/assets/ed37ab42-33f8-4657-bb51-5ec3fa00e9fb" width=700></p>

* Сервер -> Клиент

<p align="center"><img src="https://github.com/user-attachments/assets/0b94a495-7f78-4fa9-9972-122186107f98" width=700></p>

## <a name="section4.6">Вывод</a> 

В ходе выполнения данной лабораторной работы был поднят сервер на платформе Selectl VDS, на нем установлены система контроля конфигураций Ansible и python, также была поднята ВМ с CHR в VirtualBox. В данных устройствах был поднят Wireguard интерфейс и настроен тоннель между ними.






