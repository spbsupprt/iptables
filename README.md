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
