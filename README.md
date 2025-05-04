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
        logfile = /var/log/knockd.log
[opencloseSSH]
        sequence      = 1111:tcp,2222:tcp,3333:tcp
        seq_timeout   = 15
        tcpflags      = syn,ack
        start_command = /sbin/iptables -A TCP -s %IP% -p tcp --dport 22 -j ACCEPT
        cmd_timeout   = 10
        stop_command  = /sbin/iptables -D TCP -s %IP% -p tcp --dport 22 -j ACCEPT
```


