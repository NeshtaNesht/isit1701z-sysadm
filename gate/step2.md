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
В ответ на все эти команды сервер не должен ничего возвращать. Если что-то вывелось, то скорее всего это сообщение об ошибке.
```
nft flush ruleset
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

Теперь настроим форвардинг пакетов:

```
nft add rule ip filter forward ct state invalid drop
nft add rule ip filter forward iif wan0 ct state related,established accept
nft add rule ip filter forward iif lan0 oif wan0 accept
```
Выведем полученную конфигурацию:

`nft list ruleset`

Выглядеть станет примерно так:

![image](https://user-images.githubusercontent.com/65608414/118847813-93b4fc00-b8e7-11eb-8294-48933e3d66c2.png)

Настроим цепочки pre- и postrouting
```
nft add rule ip nat postrouting oif wan0 masquerade
```
Выведем полученную конфигурацию:

`nft list ruleset`

Выглядеть станет примерно так:

![image](https://user-images.githubusercontent.com/65608414/118848356-1e95f680-b8e8-11eb-83b8-ff86b8e02824.png)
