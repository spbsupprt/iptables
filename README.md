# iptables


- реализовать knocking port

- centralRouter может попасть на ssh inetrRouter через knock скрипт

- пример в материалах.

- добавить inetRouter2, который виден(маршрутизируется (host-only тип сети для виртуалки)) с хоста или форвардится порт через локалхост.

- запустить nginx на centralServer.

- пробросить 80й порт на inetRouter2 8080.

- дефолт в инет оставить через inetRouter.

---

За основу взят плейбук https://github.com/spbsupprt/Network-architecture/blob/main/network.yml и добавлен сервер inetRouter2

Дано:

![image](https://github.com/user-attachments/assets/5769d05f-7e9f-44e3-8c0e-782262290859)


### реализовать knocking port

- реализовать knocking port
- centralRouter может попасть на ssh inetrRouter через knock скрипт
- пример в материалах.

Устанавливаем apt install knockd

Смотрим конфиг /etc/knockd.conf:

```
[options]
        UseSyslog
        Interface = enp1s0

[opencloseSSH]
        sequence = 8881:tcp,7777:tcp,9991:tcp
        seq_timeout   = 15
        tcpflags      = syn
        start_command = /sbin/iptables -I INPUT 1 -s %IP% -p tcp --dport 22 -j ACCEPT
        cmd_timeout   = 30
        stop_command  = /sbin/iptables -D INPUT -s %IP% -p tcp --dport ssh -j ACCEPT
```

Так же добавляем правила в /etc/iptables/rules.v4

```
*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [0:0]
:TRAFFIC - [0:0]
:SSH-INPUT - [0:0]
:SSH-INPUTTWO - [0:0]
# TRAFFIC chain for Port Knocking. The correct port sequence in this example is  8881 -> 7777 -> 9991; any other sequence will drop the traffic
-A INPUT -j TRAFFIC
-A TRAFFIC -p icmp --icmp-type any -j ACCEPT
-A TRAFFIC -m state --state ESTABLISHED,RELATED -j ACCEPT
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 22 -m recent --rcheck --seconds 30 --name SSH2 -j ACCEPT
-A TRAFFIC -m state --state NEW -m tcp -p tcp -m recent --name SSH2 --remove -j DROP
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 9991 -m recent --rcheck --name SSH1 -j SSH-INPUTTWO
-A TRAFFIC -m state --state NEW -m tcp -p tcp -m recent --name SSH1 --remove -j DROP
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 7777 -m recent --rcheck --name SSH0 -j SSH-INPUT
-A TRAFFIC -m state --state NEW -m tcp -p tcp -m recent --name SSH0 --remove -j DROP
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 8881 -m recent --name SSH0 --set -j DROP
-A SSH-INPUT -m recent --name SSH1 --set -j DROP
-A SSH-INPUTTWO -m recent --name SSH2 --set -j DROP
-A TRAFFIC -j DROP
COMMIT
# END or further rules
```

Рестартим сервис iptables и пробуем подключиться c entralRouter, предварительно поставив на него пакет knockd

Итог:


![image](https://github.com/user-attachments/assets/113c4a68-ea4d-4548-9fa4-54a879d8cc34)





