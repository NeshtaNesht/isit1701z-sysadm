# isit1701z-sysadm
Бум. Инструкция подъехала.

Данная инструкция - набор команд из лекции. https://drive.google.com/file/d/1ZlLNoTG_Ji5qqUH7i2vihpAYcL-wjkjE/view?usp=sharing

debian берём тут: https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-10.9.0-amd64-netinst.iso
винду - сами найдёте где-нибудь :) лучше, если это будет семёрка. WinXP - для дедов и может плохо работать с самбой. Win10 - в принципе норм, но тоже может выеживаться.

Версия VirtualBox у меня: 6.1.16 r140961 (Qt5.6.2) но не вижу смысла морочиться и искать прям такую же. Вряд ли что-то радикально зависит от версии. 
Главное чтобы она была +- актуальной (если в прошлом году всё работало нормально, то скорее всего и в этом тоже всё будет ОК)

Подготавливаем виртуальную среду.

Прежде чем начать, необходимо создать внутреннюю сеть. Для этого идём в главном окне VirtualBox в "Файл" - "Настройки" - "Сеть"
и тыкаем там зелёненькую кнопочку справа - "создать сеть NAT".
Затем нажимаем справа кнопочку с шестерёнкой "Изменяет выбранную сеть NAT" и задаем там имя сети, адрес сети (можно оставить тот, что по умолчанию) и ставим крыжик "Поддержка DHCP"
У меня уже есть сеть такая, создавать другую не буду, просто в дальнейшем буду указывать, что вот эти-то циферки нужно поменять на те, что вы указывали в этом окне настройки сети. 

Подготовительные штуки вроде бы всё. Дальше для каждой машины инструкция будет в соответствующем каталоге если объём будет выходить большим - буду разбивать на несколько файлов.

gate - шлюз, на котором настроим Firewall и всякое такое, чтобы пускало нас в интернеты. 
samba - сервер, на котором будет samba и учетные записи
windows - комп "босса"

