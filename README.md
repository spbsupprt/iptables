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

[openSSH]
        sequence    = 7000,8000,9000
        seq_timeout = 5
        command     = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[closeSSH]
        sequence    = 9000,8000,7000
        seq_timeout = 5
        command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[openHTTPS]
        sequence    = 12345,54321,24680,13579
        seq_timeout = 5
        command     = /usr/local/sbin/knock_add -i -c INPUT -p tcp -d 443 -f %IP%
        tcpflags    = syn
```
