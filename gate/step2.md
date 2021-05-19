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

`nft flush ruleset`
`nft add table ip filter`
`nft add table ip nat`


