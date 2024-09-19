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

<p align="center"><img src="" width=700></p>

<p align="center"><img src="" width=700></p>



