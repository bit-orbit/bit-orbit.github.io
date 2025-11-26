---
title: "High Avalable کردن دیتابیس Postgesql"
date: 2025-11-24T19:13:58+03:30
draft: false
image: images/post/pg.jpg
---

## چرا؟

- HA
کردن دیتابیس یکی از اولین کار هایی هست که بنظرم باید قبل از
زیر بار رفتن سرویس‌های پروداکشن انجام دهیم.
خب چرا؟ فرض کنید ما یک سرویسی ارائه می‌کنیم که فقط در چند روز
یوزر‌های زیادی پیدا کرده اما از شانس دیتاسنتر بهتون پیام میده که سرور‌های
شما به زودی بخاطر عملیات‌های توسعه‌ی دیتاسنتر برای ساعتی قطع میشه!
از نگاه من، اولین برخورد خیلی مهمه، کاربر‌های شما هم دقیقا توی این موقعیت هستن.
با پایین رفتن سرور دیتابیس، عملا سرویسی که به کاربر ارائه می‌کردین از کار افتاده،
احتمالا کاربران زیادی از دست می‌دهیم.
پس در مرحله‌ی اول، نگاهمون اینه که هیچکاربری
**قطعی سرویس را تجربه نکند.**

- به این فکر کنید که دیتاسنتر تضمین کرده است که سرور‌ها
uptime
بالای 99.99 داشته باشن، این یعنی توی یک سال شاید حداکثر کمتر از یک دقیقه سرور شما داون بشه.

    ‍این یعنی ما نیاز به
HA
دیتابیس نداریم؟
اگه فرض کنیم که سرویس ما کلی کاربر انلاین داره و هر لحظه‌هم کاربران دارن بیشتر می‌شوند
خب دیتابیس ما باید کوئری‌های بسیار زیادی را اجرا کند
و بعد از نقطه‌ای، هرچقدر که منابع سرور دیتابیس را افزایش دهیم، باز هم با محدودیت‌هایی مثل نتورک
و یا حتی نرم افزاری روبرو می‌شویم.

    اینجاست که ما می‌توانیم با
‌HA
**کردن دیتابیس یک تقسیم بار روی کوئری‌های مختلف انجام بدیم.**
کوئری‌هایی که قراره دیتا را تغییر بده
میسپاریم به یک سرور، و کوئری‌هایی که صرفا عملیات خواندن دیتا را بعهده دارد را
هم به سرور‌های دیگه می‌سپاریم.

توی این پست، قراره باهم سه دیتابیس
postgresql
را
HA
کنیم.

---

## نمای کلی

[![postgresql ha](/images/post/overviewha.png)](/images/post/overviewha.png)

ما طبق این تصویر پیش می‌رویم،
سه سرور برای
postgresql
ایجاد می‌کنیم، و همه‌ی ابزار های مورد نیاز را روی همین سه سرور نصب می‌‌کنیم.

روش‌های مختلفی برای
HA
کردن دیتابیس وجود دارد

- multi master
- master slave (روش ما)
- Shared Storage

> هر کدوم از این روش‌ها
> براساس نیاز و موقعیت انتخاب می‌شوند.
> من روش
> master slave
>راانجام میدم، این مورد بیشتر از بقیه عمومی و مورد نیاز است.

به صورت خلاصه
ما روی هر سه سرور
*postgresql*
نصب می‌کنیم،
بعد ابزاری به اسم
*patroni*
رو نصب و کانفیگ می‌کنیم تا مدیریت دیتابیس‌های ما را به عهده بگیرد
خود
*patroni*
نیاز به یک دیتابیس دیگه‌ای داره
به اسم
*etcd*
که خودش به صورت
cluster
نصب میشه.
*patroni*
از این دیتابیس استفاده می‌کنه تا وضعیت نود
master
رو داشته باشه.

> patroni
> دقیقا چیکار می‌کنه؟
>
> نود
> maseter
> با یک
> TTL(Time To Live)
> مشخص یک کلید وارد
> etcd
> می‌کند، و بقیه‌ی نود‌ها
> دائم اون کلید رو تحت نظر دارند
> در صورتی که کلید تمدید نشود، دیگر نود‌ها متوجه این می‌شوند
> و رقابت برای
> master
> شدن شروع می‌شود.


از طرف دیگه ما از
*HAProxy*
استفاده می‌کنیم که ترافیک رو
به نود 
primary
هدایت کنیم.

> توی پروداکشن می‌توانیم
> HAProxy
>را روی سرور‌های مجزا هم نصب کنیم.

ابزار
keepalived
هم نصب می‌کنیم. با این ابزار می‌تونیم به کلاینت‌ها فقط یک
IP
بدیم، و مطمئن بشیم که همیشه یکی از نود‌ها اون 
IP
رو داره. و چون ما
HA Proxy
داریم، حتی اگه نود ما پرایمری نباشه، بازم ترافیک
به نود
primary
ارسال می‌شود.



ابزار‌های مورد نیاز:
1. postgresql
2. etcd
3. patroni
4. HAProxy
5. keepalived

> و از جایی که اکثر سرور‌ها
> ubuntu
> هستند، مراحل را با
> ubuntu
> انجام می‌دهم.

---

## نصب و کانفیگ postgresql


> این سه مرحله رو روی هر سرور باید انجام بدهیم.


1. مرحله‌ی اول ریپازیتوری‌‌های
postgresql
رو اضافه می‌کنیم.

```asm
sudo apt install -y postgresql-common
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
```

2. خود
postgresql
رو نصب می‌کنیم.
```asm
sudo apt install postgresql-17
```

3. بعد از نصب حتما سرویس
postgresql
رو غیر فعال کنید. قراره مدیریت‌ش رو بسپاریم به‌
patroni

```asm
sudo systemctl stop postgresql
sudo systemctl disable postgresql
```


---

## نصب و کانفیگ etcd

1. برای نصب
etcd
اول باید فایل‌های باینری‌اش رو دانلود کنیم.
آخرین ورژن رو از
[این لینک](https://github.com/etcd-io/etcd/releases/)
دانلود کنید.

    توی این تاریخ، آخرین ورژن 3.6.6 هست و من با این دستور روی هر سه سرور
دانلود می‌کنم.

```asm
wget https://github.com/etcd-io/etcd/releases/download/v3.6.6/etcd-v3.6.6-linux-amd64.tar.gz
```

3. بعد از دانلود باید از حالت آرچیو خارج و فایل‌های اجرایی‌ را
توی مسیر
`/usr/bin/`
کپی کنیم.

```asm
tar -xvf etcd-v3.6.6-linux-amd64.tar.gz
sudo cp etcd /usr/bin/
sudo cp etcdctl /usr/bin/
sudo cp etcdutl /usr/bin/
```

4. حالا کافیه یک سرویس
*systemd*
بنویسیم و
*etcd*
را کانفیگ کنیم که به صورت کلاستر کار کنه.

    ‍برای هر نود نسبت به
IP
خودش، کانفیگ متناسب اون رو نیاز داریم.

    ما فرض می‌کنیم که
IP
های ما به این صورت هست:
- 192.168.10.10
- 192.168.10.11
- 192.168.10.12


> احتمالا می‌دانید که مسیر فایل‌های سرویس داخل
> `/lib/systemd/system/`
> قرار دارد. پس فایل‌های سرویس را توی این مسیر کپی کنید.

> اسم فایل را هم
> `etcd.service`
> قرار بدین.

```asm
sudo vim /lib/system/systemd/etcd.service
```

4.1. برای نود اول
```service
[Unit]
Description=etcd Server
After=network.target

[Service]
ExecStart=/usr/bin/etcd \
  --name etcd0 \
  --advertise-client-urls http://192.168.10.10:2379,http://192.168.10.10:4001 \
  --listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001 \
  --initial-advertise-peer-urls http://192.168.10.10:2380 \
  --listen-peer-urls http://0.0.0.0:2380 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster etcd0=http://192.168.10.10:2380,etcd1=http://192.168.10.11:2380,etcd2=http://192.168.10.12:2380 \
  --initial-cluster-state new \
  --data-dir=/var/lib/etcd/etcd

WorkingDirectory=/var/lib/etcd/etcd
Type=notify
User=etcd
Group=etcd
Restart=always
RestartSec=10s

[Install]
WantedBy=multi-user.target
```


4.2. برای نود دوم

```service
[Unit]
Description=etcd Server
After=network.target

[Service]
ExecStart=/usr/bin/etcd \
  --name etcd1 \
  --advertise-client-urls http://192.168.10.11:2379,http://192.168.10.11:4001 \
  --listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001 \
  --initial-advertise-peer-urls http://192.168.10.11:2380 \
  --listen-peer-urls http://0.0.0.0:2380 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster etcd0=http://192.168.10.10:2380,etcd1=http://192.168.10.11:2380,etcd2=http://192.168.10.12:2380 \
  --initial-cluster-state new \
  --data-dir=/var/lib/etcd/etcd

WorkingDirectory=/var/lib/etcd/etcd
Type=notify
User=etcd
Group=etcd
Restart=always
RestartSec=10s

[Install]
WantedBy=multi-user.target
```


4.3. برای نود سوم

```service
[Unit]
Description=etcd Server
After=network.target

[Service]
ExecStart=/usr/bin/etcd \
  --name etcd2 \
  --advertise-client-urls http://192.168.10.12:2379,http://192.168.10.12:4001 \
  --listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001 \
  --initial-advertise-peer-urls http://192.168.10.12:2380 \
  --listen-peer-urls http://0.0.0.0:2380 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster etcd0=http://192.168.10.10:2380,etcd1=http://192.168.10.11:2380,etcd2=http://192.168.10.12:2380 \
  --initial-cluster-state new \
  --data-dir=/var/lib/etcd/etcd

WorkingDirectory=/var/lib/etcd/etcd
Type=notify
User=etcd
Group=etcd
Restart=always
RestartSec=10s

[Install]
WantedBy=multi-user.target
```

5. قبل از اجرا، یوزر و مسیری که
etcd
نیاز دارد را می‌سازیم.

```asm
sudo useradd --system --home /var/lib/etcd --shell /bin/false etcd
sudo mkdir /var/lib/etcd/etcd
sudo chown -R etcd:etcd /var/lib/etcd
```

6. پس از کپی کردن فایل‌ها، کلاستر رو اجرا می‌کنیم.

```asm
systemctl enable etcd.service
systemctl start etcd.service
```

7. حالا کافیه وضعیت سرویس و کلاستر رو برسی کنیم.

وضعیت باید
running
و
active
باشه.

```asm
systemctl status etcd.service
```
لاگ‌های سرویس رو هم نگاه می‌کنیم تا خطایی وجود نداشته باشه.
```asm
journalctl -u etcd.service
```

برای چک کردن وضعیت کلاستر با این دستور پیش میریم.

```asm
sudo etcdctl \
  --endpoints=http://192.168.10.10:2379,http://192.168.10.11:2379,http://192.168.10.12:2379 \
  endpoint status --write-out=table
```

ریزالت باید چنین چیزی باشه

```text
+--------------------------+------------------+---------+-----------------+---------+--------+-----------------------+--------+-----------+------------+-----------+------------+--------------------+--------+--------------------------+-------------------+
|         ENDPOINT          |        ID        | VERSION | STORAGE VERSION | DB SIZE | IN USE | PERCENTAGE NOT IN USE | QUOTA  | IS LEADER | IS LEARNER | RAFT TERM | RAFT INDEX | RAFT APPLIED INDEX | ERRORS | DOWNGRADE TARGET VERSION | DOWNGRADE ENABLED |
+---------------------------+------------------+---------+-----------------+---------+--------+-----------------------+--------+-----------+------------+-----------+------------+--------------------+--------+--------------------------+-------------------+
| http://192.168.10.10:2379 | 229953446ce3ee46 |   3.6.6 |           3.6.0 |  192 kB | 192 kB |                    0% | 2.1 GB |     false |      false |         2 |        589 |                589 |        |                          |             false |
| http://192.168.10.11:2379 | 229953446ce3ee46 |   3.6.6 |           3.6.0 |  192 kB | 192 kB |                    0% | 2.1 GB |     false |      false |         2 |        589 |                589 |        |                          |             false |
| http://192.168.10.12:2379 | aef616a96d41217e |   3.6.6 |           3.6.0 |  192 kB | 192 kB |                    0% | 2.1 GB |      true |      false |         2 |        589 |                589 |        |                          |             false |
+---------------------------+------------------+---------+-----------------+---------+--------+-----------------------+--------+-----------+------------+-----------+------------+--------------------+--------+--------------------------+-------------------+
```

8. اگر همه چیز سالم بود، تنها یک تغییر دیگر باید به
etcd
بدهیم، 

    داخل هر کانفیگ
    *etcd.service*
    یک خط به صورت
  `initial-cluster-state new-- `
  وجود داره، ما باید
  `new`
  رو
  به
  `existing`
  تغییر بدیم.
  و سرویس را ریست کنیم.

```bash
sudo vim /lib/systemd/system/etcd.service
```
و بعد از تغییر
```asm
sudo systemctl daemon-reload
sudo systemctl restart etcd.service
```

---

## نصب و کانفیگ patroni

1. قدم اول نصب خود
patroni
هست

```asm
sudo apt install -y patroni
```

2. بعد از نصب، مرحله‌ی کانفیگ کردن رو داریم، هر نود کانفیگ مورد نیاز خودش را دارد.

> دقت کنید که توی قسمت
> `authentication`
> باید پسورد همه‌ی کانفیگ‌ها یکسان باشند، و خب
> پسوردی که من نوشتم را تغییر بدین.


> و خب من
> patroni(Postgesql)
> رو بجای
> `5432`
> تغییر دادم به
> `5433`
> دلیل اینکار این بود که خب ما قراره از
> *HA Proxy*
> روی همون سرور‌هایی استفاده کنیم که
>‌ patroni(postgres)
> نصب شده!

- مسیر فایل کانفیگ
```asm
sudo vim /etc/patroni/config.yml
```

2.1. برای نود اول

```text
scope: postgresql-cluster
namespace: /service/
name: postgresql-01

etcd3:
  hosts: 192.168.10.10:2379,192.168.10.11:2379,192.168.10.12:2379
  protocol: http

restapi:
  listen: 0.0.0.0:8008
  connect_address: 192.168.10.10:8008

bootstrap:
  dcs:
    ttl: 30
    loop_wait: 10
    retry_timeout: 10
    maximum_lag_on_failover: 1048576
    postgresql:
      parameters:
        pg_hba:
          - host replication replicator 127.0.0.1/32 md5
          - host replication replicator 192.168.10.10/32 md5
          - host replication replicator 192.168.10.11/32 md5
          - host replication replicator 192.168.10.12/32 md5
          - host all all 127.0.0.1/32 md5
          - host all all 0.0.0.0/0 md5
      initdb:
        - encoding: UTF8
        - data-checksums

postgresql:
  listen: 0.0.0.0:5433
  connect_address: 192.168.10.10:5433
  data_dir: /var/lib/postgresql/data
  bin_dir: /usr/lib/postgresql/17/bin
  authentication:
    superuser:
      username: postgres
      password: YourSecretPass
    replication:
      username: replicator
      password: ASecretPassForYourReplication
  parameters:
    max_connections: 100
    shared_buffers: 256MB

tags:
  nofailover: false
  noloadbalance: false
  clonefrom: false
```

2.2. برای نود دوم

```text
scope: postgresql-cluster
namespace: /service/
name: postgresql-02

etcd3:
  hosts: 192.168.10.10:2379,192.168.10.11:2379,192.168.10.12:2379
  protocol: http

restapi:
  listen: 0.0.0.0:8008
  connect_address: 192.168.10.11:8008

bootstrap:
  dcs:
    ttl: 30
    loop_wait: 10
    retry_timeout: 10
    maximum_lag_on_failover: 1048576
    postgresql:
      parameters:
        pg_hba:
          - host replication replicator 127.0.0.1/32 md5
          - host replication replicator 192.168.10.10/32 md5
          - host replication replicator 192.168.10.11/32 md5
          - host replication replicator 192.168.10.12/32 md5
          - host all all 127.0.0.1/32 md5
          - host all all 0.0.0.0/0 md5
      initdb:
        - encoding: UTF8
        - data-checksums

postgresql:
  listen: 0.0.0.0:5433
  connect_address: 192.168.10.11:5433
  data_dir: /var/lib/postgresql/data
  bin_dir: /usr/lib/postgresql/17/bin
  authentication:
    superuser:
      username: postgres
      password: YourSecretPass
    replication:
      username: replicator
      password: ASecretPassForYourReplication
  parameters:
    max_connections: 100
    shared_buffers: 256MB

tags:
  nofailover: false
  noloadbalance: false
  clonefrom: false
```

2.3. برای نود سوم

```text
scope: postgresql-cluster
namespace: /service/
name: postgresql-03

etcd3:
  hosts: 192.168.10.10:2379,192.168.10.11:2379,192.168.10.12:2379
  protocol: http

restapi:
  listen: 0.0.0.0:8008
  connect_address: 192.168.10.12:8008

bootstrap:
  dcs:
    ttl: 30
    loop_wait: 10
    retry_timeout: 10
    maximum_lag_on_failover: 1048576
    postgresql:
      parameters:
        pg_hba:
          - host replication replicator 127.0.0.1/32 md5
          - host replication replicator 192.168.10.10/32 md5
          - host replication replicator 192.168.10.11/32 md5
          - host replication replicator 192.168.10.12/32 md5
          - host all all 127.0.0.1/32 md5
          - host all all 0.0.0.0/0 md5
      initdb:
        - encoding: UTF8
        - data-checksums

postgresql:
  listen: 0.0.0.0:5433
  connect_address: 192.168.10.12:5433
  data_dir: /var/lib/postgresql/data
  bin_dir: /usr/lib/postgresql/17/bin
  authentication:
    superuser:
      username: postgres
      password: YourSecretPass
    replication:
      username: replicator
      password: ASecretPassForYourReplication
  parameters:
    max_connections: 100
    shared_buffers: 256MB

tags:
  nofailover: false
  noloadbalance: false
  clonefrom: false
```

2.4. بعد از ذخیره این کانفیگ‌ها، حالا کافیه که فقط کانفیگ‌ها را اعمال و لاگ را نگاه کنیم.

> روی هر سه سرور اجرا کنید.

```asm
sudo systemctl restart patroni
journalctl -u patroni -f
```

باید چنین ریزالتی را برای نود مستر ببینید.

```text
Dec 03 22:16:05 postgres-01 patroni[770]: 2024-12-03 22:16:05,399 INFO: no action. I am (postgresql-01), the leader with the lock
Dec 03 22:16:15 postgres-01 patroni[770]: 2024-12-03 22:16:15,399 INFO: no action. I am (postgresql-01), the leader with the lock
```
و یا برای نود‌های رپلیکا
```text
Dec 03 22:16:21 postgres-02 patroni[768]: 2024-12-03 22:16:21,780 INFO: Lock owner: postgresql-01; I am postgresql-02
Dec 03 22:16:21 postgres-02 patroni[768]: 2024-12-03 22:16:21,823 INFO: bootstrap from leader 'postgresql-01' in progress
```


> این خط
> `- host all all 0.0.0.0/0 md5`
> اجازه می‌ده تا همه‌ی
> IP ها
> مجاز به اتصال به دیتابیس باشند. بنابراین
> بر اساس
> IP
> سرور‌های خودتون این خط رو تغییر بدین.

---

## نصب و کانفیگ HA Proxy

> روی هر سرور انجام بدین.

> فایل کانفیگ
> HA Proxy
> از قبل یکسری کانفیگ داره، اونها رو حذف نکنید.

مسیر کانفیگ

```asm
sudo vim /etc/haproxy/haproxy.cfg
```

1. خب مثل ابزار‌های قبل، اول نصب
HA Proxy

```asm
sudo apt install haproxy
```

2. حالا خود کانفیگ.

```text
global
    log /dev/log    local0
    chroot /var/lib/haproxy
    pidfile /var/run/haproxy.pid
    maxconn 4000
    user haproxy
    group haproxy
    daemon

defaults
    log global
    mode tcp
    timeout connect 5s
    timeout client 30s
    timeout server 30s

frontend postgres_frontend
    bind 0.0.0.0:5432
    mode tcp 
    default_backend postgres_backend

backend postgres_backend
    mode tcp
    option tcp-check
    option httpchk GET /primary 
    http-check expect status 200
    timeout connect 5s
    timeout client 30s
    timeout server 30s
    
    server postgresql-01 192.168.10.10:5433 check port 8008 inter 1s fall 3 rise 2 
    server postgresql-02 192.168.10.11:5433 check port 8008 inter 1s fall 3 rise 2
    server postgresql-03 192.168.10.12:5433 check port 8008 inter 1s fall 3 rise 2
```


2.1 کافیه تا کانفیگ رو اعمال کنیم و لاگ رو بخونیم.
اگه همه چیز اوکی بود، فقط یک قدم تا یک کلاستر موفق فاصله داریم :)

```asm
sudo systemctl restart haproxy
journalctl -u haproxy
```

---

## نصب و کانفیگ keepalived

1. قدم اول نصب keepalived

```asm
sudo apt install keepalived
```

مسیر کانفیگ

```asm
sudo vim  /etc/keepalived/keepalived.conf
```

> من برای بخش
> *interface*
> از
> `eth0`
> استفاده کردم، شما باید به نسبت سرور خودتون این گزینه رو تغییر دهید..
> با دستور
> `ip l`
> می‌تونید لیست
> *interface*
> های شبکه‌ی سیستمون رو ببینید.

2. و بعد کانفیگ برای هر سرور

2.1. برای اولین سرور

```text
global_defs {
    enable_script_security
    script_user keepalived_script
}

vrrp_script check_haproxy {
    script "/etc/keepalived/check_haproxy.sh"
    interval 2
    fall 3
    rise 2
}

vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 51
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass tDHjh7by # change
    }
    virtual_ipaddress {
        192.168.10.13
    }
    track_script {
        check_haproxy
    }
}

```


2.2. برای سرور دوم

```text
global_defs {
    enable_script_security
    script_user keepalived_script
}

vrrp_script check_haproxy {
    script "/etc/keepalived/check_haproxy.sh"
    interval 2
    fall 3
    rise 2
}

vrrp_instance VI_HA_PROXY {
    state BACKUP
    interface eth0
    virtual_router_id 51
    priority 101
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass QAZxsw123EDCvfr
    }
    virtual_ipaddress {
        192.168.10.13
    }
    track_script {
        check_haproxy
    }
}
```

2.3. برای سرور سوم

```text
global_defs {
    enable_script_security
    script_user keepalived_script
}

vrrp_script check_haproxy {
    script "/etc/keepalived/check_haproxy.sh"
    interval 2
    fall 3
    rise 2
}

vrrp_instance VI_HA_PROXY {
    state BACKUP
    interface eth0
    virtual_router_id 51
    priority 99
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass QAZxsw123EDCvfr
    }
    virtual_ipaddress {
        192.168.10.13/24
    }
    track_script {
        check_haproxy
    }
}
```


2.4. بعد از کانفیگ، نوبت اعمال کانفیگ‌هاست. پس سرویس
keepalived
رو ریست می‌کنیم و لاگش رو می‌خوانیم.


```asm
sudo systemctl restart keepalived
sudo journalctl -u keepalived -f
```

اگر مشکلی داخل لاگ نبود، با پینگ روی
IP
مجازی که ست کردیم یعنی
192.168.10.13
تست می‌کنیم که
Floating IP(Virtual IP)
به درستی ست شده باشه.

```asm
ping 192.168.10.13
```

---
