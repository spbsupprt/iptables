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
        sequence      = 8881:tcp,7777:tcp,9991:tcp
        seq_timeout   = 15
        tcpflags      = syn,ack
        start_command = /usr/bin/iptables -A TCP -s %IP% -p tcp --dport 22 -j ACCEPT
        cmd_timeout   = 10
        stop_command  = /usr/bin/iptables -D TCP -s %IP% -p tcp --dport 22 -j ACCEPT
```

Изменяем конфиг:

/etc/default/knockd

```
# control if we start knockd at init or not
# 1 = start
# anything else = don't start
# PLEASE EDIT /etc/knockd.conf BEFORE ENABLING
START_KNOCKD=1

# command line options
#KNOCKD_OPTS="-i eth1"
```
