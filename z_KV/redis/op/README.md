# Redis OP

* Install
```md
http://download.redis.io/releases
```
```sh
./bin/redis-server etc/redis.conf
```

* Config
```md
daemonize yes
pidfile run/redis.pid
logfile "log/redis.log"
dir ./data/
```

* Test
```sh
./bin/redis-cli
```
```md
127.0.0.1:6379> set sunny_test_key_01 "sunny"
OK

127.0.0.1:6379> keys *
1) "sunny_test_key_01"

127.0.0.1:6379> get sunny_test_key_01
"sunny"
```
