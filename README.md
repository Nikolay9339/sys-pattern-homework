# Домашнее задание №8. Балансировка нагрузки (HAProxy) 
Студент: Захарян Николай Артурович

# Конфигурационный файл haproxy.cfg:

haproxy
Копировать
frontend py_tcp_in
    bind *:9000
    default_backend py_back

backend py_back
    mode tcp
    balance roundrobin
    server srv1 127.0.0.1:8001 check
    server srv2 127.0.0.1:8002 check

# Проверка работы (логи curl):

$ curl http://127.0.0.1:9000
Response from port 8001
$ curl http://127.0.0.1:9000
Response from port 8002

# Конфигурационный файл haproxy.cfg:

haproxy
Копировать
frontend http_in
    bind *:8080
    acl host_example hdr(host) -i example.local
    use_backend app_weighted if host_example
    default_backend deny_all

backend app_weighted
    mode http
    balance roundrobin
    server srv1 127.0.0.1:8001 weight 2 check
    server srv2 127.0.0.1:8002 weight 3 check
    server srv3 127.0.0.1:8003 weight 4 check

backend deny_all
    http-request deny status 404
