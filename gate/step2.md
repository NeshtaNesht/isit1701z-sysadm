Итак, вы перезагрузились, команда `ip a` показывает нам ip адреса наших интерфейсов. Если да, то всё круто, едем дальше.

Перед тем, как продолжить, пропишем dns сервер в /etc/resolv.conf

Открываем файл:

`nano /etc/resolv.conf`

делаем чтобы там было написано вот так:

`nameserver 8.8.8.8`

Сохраняем файл ctrl+O, затем ctrl+X
и перезагружаемся

Устанавливаем nftables:
`apt install nftables`

настроим запуск nftables после запуска сетевых интерфейсов:

`systemctl edit nftables`

Туда пишем вот так:
```
[Unit]
After=networking.service
```
После этого добавим nftables для старта при загрузке системы:
`systemctl enable nftables`

и перезагрузимся.

Настройка nftables
создадим цепочки и таблицы правил.
```
nft flush rulesetclea
nft add table ip filter
nft add table ip nat
nft add chain ip filter input "{ type filter hook input priority 0; policy drop; }"
nft add chain ip filter forward "{ type filter hook forward priority 0; policy drop; }"
nft add chain ip filter output "{ type filter hook output priority 0; policy accept; }"
nft add chain ip nat prerouting "{ type nat hook prerouting priority -100;}"
nft add chain ip nat postrouting "{ type nat hook postrouting priority 100;}"

```
добавляем правила:

```
nft add rule ip filter input ct state invalid counter drop
nft add rule ip filter input ct state {established, related} accept
nft add rule ip filter input meta iif lo ct state new accept
nft add rule ip filter input "meta iif != lo ip daddr 127.0.0.1/8 counter drop"
nft add rule ip filter input ip protocol icmp accept
nft add rule ip filter input ip protocol igmp accept
nft add rule ip filter input udp dport {33434-33524} accept
nft add rule ip filter input tcp dport 22 accept
```
Выведем полученную конфигурацию:
`nft list ruleset`

Должно получиться как-то так:
![image](https://user-images.githubusercontent.com/65608414/118846015-b9410600-b8e5-11eb-92df-17f569d83cf8.png)
