# Monitoring

## Metric Store / TSDB

A time series database (TSDB) is a software system that is optimized for handling time series data, arrays of numbers indexed by time (a datetime or a datetime range). In some fields these time series are called profiles, curves, or traces. A time series of stock prices might be called a price curve. A time series of energy consumption might be called a load profile. A log of temperature values over time might be called a temperature trace.

## Data Type

For my purposes, time-series can be defined as follows:

A series is identified by a source name or ID (for example: host ID) and a metric name or ID.

A series consists of a sequence of {timestamp, value} measurements ordered by timestamp, where the timestamp is probably a high-precision Unix timestamp and the value is a floating-point number.

## Dimensionality

Having thorough experience with systems that have and lack dimensionality, I have to say that it really is absolutely vital; at the "product" level, lacking support for it is a guarantee that something that does support it will eventually replace you. This is true of both companies attempting to enter the market and open source solutions.

Stuffing all your metadata into the metric key and then treating things as a key/value lookup isn't a particularly great model for querying; you want to be able to have at least or/and logic across dimensions; eg: az=us-east-1 AND role=db-master, version=2.1 OR version=2.3, and preferably a richer language. Graphite's metric name globbing system (system.cpu.az-us-east-1.role-db-master.host-\*.avg) definitely isn't it.

Alerting

## Monit

Some examples:

```
check file shadow with path /etc/shadow
       if failed uid root then alert
check file shadow with path /etc/shadow
       if failed gid shadow then alert
check process sshd with pidfile /var/run/sshd.pid
       if changed pid then alert
check process myproc with pidfile /var/run/myproc.pid
       if changed ppid then exec "/my/script"
check process myapp with pidfile /var/run/myapp.pid
    start program = "/etc/init.d/myapp start"
    stop program = "/etc/init.d/myapp stop"
    if uptime > 3 days then restart
check program list-files with path "/bin/ls -lrt /tmp/"
       if status != 0 then alert
check host mmonit.com with address mmonit.com
       if failed ping then alert 
if failed
      port 443
      protocol https
      and certificate valid > 30 days
  then alert
if failed
     port 443
     protocol https
     certificate checksum = "9F:2A:37:5B:CE:E7:BC:EC:93:1F:98:17:E6:A6:5F:E3"
 then alert
if failed
    port 25 and
    expect "^220.*"
    send   "HELO localhost.localdomain\r\n"
    expect "^250.*"
    send   "QUIT\r\n"
 then alert
check network eth0 with interface eth0
       if upload > 500 kB/s then alert
       if total downloaded > 1 GB in last 2 hours then alert
       if total downloaded > 10 GB in last day then alert
check process nginx with pidfile /var/run/nginx.pid
       start program = "/etc/init.d/nginx start"
check process freeswitch
    with pidfile /usr/local/freeswitch/log/freeswitch.pid
  start program = "/usr/local/freeswitch/bin/freeswitch -nc -hp"
  stop program = "/usr/local/freeswitch/bin/freeswitch -stop"
  if total memory > 1000.0 MB for 5 cycles then alert
  if total memory > 1500.0 MB for 5 cycles then alert
  if total memory > 2000.0 MB for 5 cycles then restart
  if cpu > 60% for 5 cycles then alert
  if failed
     port 5060 type udp protocol SIP
     target me@foo.bar and maxforward 10
  then restart
```
