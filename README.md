# isit1701z-sysadm
Версия VirtualBox у меня: 6.1.16 r140961 (Qt5.6.2) но не вижу смысла морочиться и искать прям такую же. Вряд ли что-то радикально зависит от версии. 
Главное чтобы она была +- актуальной (если в прошлом году всё работало нормально, то скорее всего и в этом тоже всё будет ОК)

Подготавливаем виртуальную среду.

Прежде чем начать, необходимо создать внутреннюю сеть. Для этого идём в главном окне VirtualBox в "Файл" - "Настройки" - "Сеть"
и тыкаем там зелёненькую кнопочку справа - "создать сеть NAT".
Затем нажимаем справа кнопочку с шестерёнкой "Изменяет выбранную сеть NAT" и задаем там имя сети, адрес сети (можно оставить тот, что по умолчанию) и ставим крыжик "Поддержка DHCP"
У меня уже есть сеть такая, создавать другую не буду, просто в дальнейшем буду указывать, что вот эти-то циферки нужно поменять на те, что вы указывали в этом окне настройки сети. 

Подготовительные штуки вроде бы всё. Дальше для каждой машины инструкция будет в соответствующем каталоге.

gate - шлюз, на котором настроим Firewall и всякое такое, чтобы пускало нас в интернеты. 
samba - сервер, на котором будет samba и учетные записи
windows - комп "босса"


1. создаем виртуальную машину для 
