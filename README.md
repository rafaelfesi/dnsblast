# DNS Blast
> A DNS performance testing utility


[![Go Report Card](https://goreportcard.com/badge/github.com/sandeeprenjith/dnsblast)](https://goreportcard.com/report/github.com/sandeeprenjith/dnsblast)

| Currently Supported Protocols |
| ----------------------------- |
| UDP DNS                      	|
| TCP DNS                      	|
| TCP DNS over TLS             	|

## Documentation

Take a look at the  [Wiki](https://github.com/sandeeprenjith/dnsblast/wiki) for testing instructions.

## New Features

*Sat Mar 14 00:28:50 DST 2020*

* IPv6 Support
* Generate load using query names from a query file. The file file should have one FQDN per line
* Configurable number of concurrent queries
* Ability to disable certificate validation for DNS over TLS


## Installation 

#### Download the executable

##### v1
Go to [downloads](https://github.com/sandeeprenjith/dnsblast/tree/v1/builds)

#### Build for your platform

##### Requirements

* go

```
go get github.com/sandeeprenjith/dnsblast

```

#### Build for other platforms 

##### Requirements

* go
* make

```
$ git clone https://github.com/sandeeprenjith/dnsblast.git
$ cd dnsblast
$ make
```
This creates a directory named "builds". The directory contains archives with executables for different platforms. 

```
╰$ tree builds
builds
├ dnsblast-v1-darwin-386.tar.gz
├ dnsblast-v1-darwin-amd64.tar.gz
├ dnsblast-v1-linux-386.tar.gz
├ dnsblast-v1-linux-amd64.tar.gz
├ dnsblast-v1-linux-arm.tar.gz
├ dnsblast-v1-linux-arm64.tar.gz

```
The archives contain the executable for the platform which the name of the archive suggests.

```
$ tar -tf builds/dnsblast-v1-linux-386.tar.gz
dnsblast
```


## Usage

```
$ ./dnsblast
  -c int
        Value 0 for random QNAMES (for uncached responses), 100 for Predictable QNAMES (for cached responses)
  -f string
        Input file with query names
  -l int
        Duration to run load (default 60)
  -noverify
        Skip SSL verification ( to be used with '-proto tls')
  -p string
        The destination UDP port (default "53")
  -proto string
        Protocol to use for DNS queries ( udp, tcp, tls) (default "udp")
  -q int
        Concurrent queries to send (default 10)
  -r int
        Packets per second to send (default 1000)
  -s string
        [Required] The address of the target server
  -t int
        Number of threads (default 2)
```

## Sample Output

### DNS (UDP)

> Tested against BIND configured to provide fake responses. 
> Details of configuration available [here](fake-responders/Bind)

```
$ ./dnsblast -s 192.168.130.9 -r 40000 -q 200 -l 10

EXECUTING TEST
+-----------------------------------------------------------+
2020/03/15 03:45:26 QPS/Thread:  6000  Latency:  16.677117ms
2020/03/15 03:45:27 QPS/Thread:  11800  Latency:  15.787841ms
2020/03/15 03:45:28 QPS/Thread:  6400  Latency:  45.398035ms
2020/03/15 03:45:29 QPS/Thread:  18400  Latency:  20.184322ms
2020/03/15 03:45:30 QPS/Thread:  6200  Latency:  75.833313ms
2020/03/15 03:45:31 QPS/Thread:  19000  Latency:  30.486501ms
2020/03/15 03:45:32 QPS/Thread:  12000  Latency:  55.365939ms
2020/03/15 03:45:33 QPS/Thread:  6000  Latency:  126.906884ms
2020/03/15 03:45:34 QPS/Thread:  5800  Latency:  149.469651ms
2020/03/15 03:45:35 QPS/Thread:  8400  Latency:  116.027951ms
+-----------------------------------------------------------+

  REPORT
+---------------------+------------------------+
| Target Server       | udp://192.168.130.9:53 |
| Test                | Uncached Responses     |
| Send Rate           | 40000 Queries/Sec      |
| Threads             | 2                      |
| Duration of test    | 10 Sec                 |
| Protocol            | UDP                    |
| Average Queries/Sec | 20933                  |
| Average Latency     | 80.066453ms            |
+---------------------+------------------------+
```

### DNS over TLS

> Tested against [Coredns](https://coredns.io) running DNS over TLS with erratic plugin configured to give fake responses.
> Details on configuration available [here](fake-responders/Coredns)

```
$ ./dnsblast -s 192.168.130.9 -l 10 -r 1000 -q 20 -proto tls -noverify

EXECUTING TEST
+-----------------------------------------------------------+
2020/03/15 03:24:37 QPS/Thread:  140  Latency:  84.2214ms
2020/03/15 03:24:38 QPS/Thread:  300  Latency:  81.66225ms
2020/03/15 03:24:39 QPS/Thread:  300  Latency:  114.151156ms
2020/03/15 03:24:40 QPS/Thread:  300  Latency:  154.465046ms
2020/03/15 03:24:41 QPS/Thread:  280  Latency:  213.839306ms
2020/03/15 03:24:42 QPS/Thread:  300  Latency:  229.693182ms
2020/03/15 03:24:43 QPS/Thread:  320  Latency:  272.623912ms
2020/03/15 03:24:44 QPS/Thread:  300  Latency:  309.042142ms
2020/03/15 03:24:45 QPS/Thread:  320  Latency:  350.795333ms
2020/03/15 03:24:46 QPS/Thread:  160  Latency:  774.228763ms
+-----------------------------------------------------------+

  REPORT
+---------------------+-------------------------+
| Target Server       | tls://192.168.130.9:853 |
| Test                | Uncached Responses      |
| Send Rate           | 1000 Queries/Sec        |
| Threads             | 2                       |
| Duration of test    | 10 Sec                  |
| Protocol            | TCP-TLS                 |
| Average Queries/Sec | 506                     |
| Average Latency     | 301.643311ms            |
+---------------------+-------------------------+
```

### IPv6 

```
$ ./dnsblast -s dead:face::2 -r 500 -q 2 -proto tls -noverify  -l 10

EXECUTING TEST
+-----------------------------------------------------------+
2020/03/17 23:26:15 QPS/Thread:  36  Latency:  32.141583ms
2020/03/17 23:26:16 QPS/Thread:  32  Latency:  70.730121ms
2020/03/17 23:26:17 QPS/Thread:  114  Latency:  33.450903ms
2020/03/17 23:26:18 QPS/Thread:  88  Latency:  57.15537ms
2020/03/17 23:26:19 QPS/Thread:  88  Latency:  73.337146ms
2020/03/17 23:26:20 QPS/Thread:  42  Latency:  182.624116ms
2020/03/17 23:26:21 QPS/Thread:  34  Latency:  258.311857ms
2020/03/17 23:26:22 QPS/Thread:  34  Latency:  292.833628ms
2020/03/17 23:26:23 QPS/Thread:  36  Latency:  309.374108ms
2020/03/17 23:26:24 QPS/Thread:  236  Latency:  54.491316ms
2020/03/17 23:26:25 QPS/Thread:  88  Latency:  158.465191ms
2020/03/17 23:26:26 QPS/Thread:  46  Latency:  328.005638ms
2020/03/17 23:26:27 QPS/Thread:  44  Latency:  370.920222ms
+-----------------------------------------------------------+

  REPORT
+---------------------+------------------------+
| Target Server       | tls://dead:face::2:853 |
| Test                | Uncached Responses     |
| Send Rate           | 500 Queries/Sec        |
| Threads             | 2                      |
| Duration of test    | 10 Sec                 |
| Protocol            | TCP-TLS                |
| Average Queries/Sec | 196                    |
| Average Latency     | 53.629597ms            |
+---------------------+------------------------+
```

## Credit where due

* https://github.com/miekg/dns
