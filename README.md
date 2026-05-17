# WireGuard Linux + MikroTik Installer

English | [Русский](#russian)

This repository contains a customized version of [Nyr's WireGuard installer](https://github.com/Nyr/wireguard-install) for professional Linux WireGuard server deployments with support for both standard Linux clients and MikroTik RouterOS peers.

The script keeps the simple interactive workflow of the original installer, but adds a MikroTik client mode and removes the external BoringTun download/update logic.

## Features

- Installs WireGuard on Ubuntu, Debian, AlmaLinux, Rocky Linux, CentOS, and Fedora.
- Creates and manages a `wg0` WireGuard server interface.
- Adds Linux/generic WireGuard clients and generates `.conf` files.
- Adds MikroTik RouterOS peers by asking for the MikroTik public key.
- Generates a MikroTik `.rsc` file with RouterOS commands for the peer.
- Supports site-to-site MikroTik use cases by allowing custom `AllowedIPs`.
- Lets you choose between UFW and iptables on Debian/Ubuntu.
- Keeps the original add/remove client workflow.
- Removes BoringTun and all downloads from `wg.nyr.be`.

## Requirements

- A supported Linux server.
- Root privileges.
- A public IPv4 address or a reachable hostname.
- UDP port access to the server, by default `51820`.
- For MikroTik: RouterOS with WireGuard support.

## Usage

Download or copy the script to your server, then run:

```bash
chmod +x wireguard-install.sh
sudo ./wireguard-install.sh
```

During the first run, the script asks for:

- Server IPv4 address.
- Public IPv4 address or hostname, if the server is behind NAT.
- Optional IPv6 address.
- WireGuard UDP port.
- First client name.
- Client type:
  - Linux / generic WireGuard client
  - MikroTik RouterOS
- Firewall backend on Debian/Ubuntu:
  - UFW
  - iptables

## Linux Client Mode

When you select a Linux/generic WireGuard client, the script creates a client configuration file:

```text
client-name.conf
```

It also prints a QR code in the terminal for mobile or desktop WireGuard clients that support QR import.

## MikroTik Mode

When you select MikroTik RouterOS, the script pauses and shows a short instruction:

1. Create a WireGuard interface on MikroTik.
2. Print the MikroTik WireGuard public key.
3. Paste that public key back into the installer.

The script then:

- Adds the MikroTik peer to `/etc/wireguard/wg0.conf`.
- Asks for server-side `AllowedIPs`.
- Asks for MikroTik-side `allowed-address`.
- Generates a RouterOS command file:

```text
client-name-mikrotik.rsc
```

For a simple MikroTik client, you can keep the default server-side `AllowedIPs`:

```text
10.7.0.x/32
```

For site-to-site tunnels, add the MikroTik LAN subnet, for example:

```text
10.7.0.x/32, 192.168.88.0/24
```

On MikroTik, run the generated `.rsc` commands or paste them into the RouterOS terminal.

## Adding Clients Later

Run the script again:

```bash
sudo ./wireguard-install.sh
```

Then select:

```text
1) Add a new client
```

You can add more Linux clients or MikroTik peers to the existing `wg0` configuration.

## Removing Clients or WireGuard

Run the script again and select:

```text
2) Remove an existing client
3) Remove WireGuard
```

If UFW was used during installation, the script also removes the WireGuard NAT block from UFW rules.

## License

The original script was created by Nyr and released under the MIT License.

MIT is a permissive license. In practical terms, you may use, copy, modify, publish, distribute, sublicense, and sell modified versions of the software. The important condition is that the original copyright notice and MIT license text must be included in copies or substantial portions of the software.

This project is a modified version of Nyr's installer, so keep proper attribution to the original author and include the MIT license notice in your repository.

## Credits

- Original project: [Nyr/wireguard-install](https://github.com/Nyr/wireguard-install)
- Customized for Linux + MikroTik WireGuard deployment workflows.

---

<a id="russian"></a>

<details>
<summary>Русский</summary>

# Установщик WireGuard для Linux + MikroTik

[English](#wireguard-linux--mikrotik-installer) | Русский

Этот репозиторий содержит модифицированную версию [установщика WireGuard от Nyr](https://github.com/Nyr/wireguard-install) для профессионального развертывания WireGuard-серверов на Linux с поддержкой обычных Linux-клиентов и peer-клиентов MikroTik RouterOS.

Скрипт сохраняет простой интерактивный сценарий оригинального установщика, но добавляет режим клиента MikroTik и убирает внешнюю загрузку/обновление BoringTun.

## Возможности

- Установка WireGuard на Ubuntu, Debian, AlmaLinux, Rocky Linux, CentOS и Fedora.
- Создание и управление серверным интерфейсом WireGuard `wg0`.
- Добавление Linux/generic WireGuard клиентов с генерацией `.conf`.
- Добавление MikroTik RouterOS peer через ввод Public Key со стороны MikroTik.
- Генерация `.rsc` файла с командами RouterOS.
- Поддержка site-to-site сценариев за счет ручного указания `AllowedIPs`.
- Выбор UFW или iptables на Debian/Ubuntu.
- Сохранена логика добавления и удаления клиентов из оригинального скрипта.
- Убраны BoringTun и все обращения к `wg.nyr.be`.

## Требования

- Поддерживаемый Linux-сервер.
- Права root.
- Публичный IPv4-адрес или доступное доменное имя.
- Доступный UDP-порт на сервере, по умолчанию `51820`.
- Для MikroTik: RouterOS с поддержкой WireGuard.

## Использование

Скопируйте скрипт на сервер и запустите:

```bash
chmod +x wireguard-install.sh
sudo ./wireguard-install.sh
```

При первой установке скрипт спросит:

- IPv4-адрес сервера.
- Публичный IPv4-адрес или hostname, если сервер за NAT.
- IPv6-адрес, если он есть.
- UDP-порт WireGuard.
- Имя первого клиента.
- Тип клиента:
  - Linux / generic WireGuard client
  - MikroTik RouterOS
- Firewall backend на Debian/Ubuntu:
  - UFW
  - iptables

## Режим Linux-клиента

Если выбрать Linux/generic WireGuard client, скрипт создаст файл конфигурации:

```text
client-name.conf
```

Также в терминале будет показан QR-код для клиентов WireGuard, которые поддерживают импорт через QR.

## Режим MikroTik

Если выбрать MikroTik RouterOS, скрипт остановится и покажет мини-инструкцию:

1. Создать WireGuard interface на MikroTik.
2. Вывести Public Key этого интерфейса.
3. Вставить Public Key обратно в установщик.

После этого скрипт:

- Добавит MikroTik peer в `/etc/wireguard/wg0.conf`.
- Спросит `AllowedIPs` на стороне Linux-сервера.
- Спросит `allowed-address` для peer на стороне MikroTik.
- Создаст файл RouterOS-команд:

```text
client-name-mikrotik.rsc
```

Для простого MikroTik-клиента можно оставить стандартный `AllowedIPs`:

```text
10.7.0.x/32
```

Для site-to-site туннеля добавьте LAN-сеть MikroTik, например:

```text
10.7.0.x/32, 192.168.88.0/24
```

На MikroTik выполните команды из `.rsc` файла или вставьте их в терминал RouterOS.

## Добавление клиентов после установки

Запустите скрипт повторно:

```bash
sudo ./wireguard-install.sh
```

Затем выберите:

```text
1) Add a new client
```

Можно добавлять как Linux-клиентов, так и MikroTik peer в уже существующую конфигурацию `wg0`.

## Удаление клиентов или WireGuard

Запустите скрипт повторно и выберите:

```text
2) Remove an existing client
3) Remove WireGuard
```

Если при установке использовался UFW, скрипт также удалит WireGuard NAT-блок из правил UFW.

## Лицензия

Оригинальный скрипт создан Nyr и выпущен под лицензией MIT.

MIT - разрешительная лицензия. Практически это значит, что вы можете использовать, копировать, изменять, публиковать, распространять, сублицензировать и продавать измененные версии ПО. Главное условие: сохранить оригинальное copyright-уведомление и текст лицензии MIT в копиях или существенных частях ПО.

Этот проект является модифицированной версией установщика Nyr, поэтому в репозитории нужно сохранить указание на оригинального автора и MIT license notice.

## Благодарности

- Оригинальный проект: [Nyr/wireguard-install](https://github.com/Nyr/wireguard-install)
- Модификация под рабочие сценарии Linux + MikroTik WireGuard.

</details>
