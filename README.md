# DNS Blast
> A DNS performance testing utility

**Note:** Not in a usable stage yet 

*Tue Oct 29 21:37:12 DST 2019*

## Installation 

```
go get github.com/sandeeprenjith/dnsblast

```

## Usage

```

$ dnsblast -h
Usage of dnsblast:
  -len int
        Duration to run load (default 60)
  -port string
        The destination UDP port (default "53")
  -rate int
        Packets per second to send (default 100)
  -server string
        The address of the target server (default "127.0.0.1")

```

## Sample Output

```

$ dnsblast -server 192.168.130.9 -rate 3000 -len 10
2019/11/14 14:58:55 QPS:  1858  Latency:  237.799µs
2019/11/14 14:58:56 QPS:  1858  Latency:  236.285µs
2019/11/14 14:58:57 QPS:  1847  Latency:  233.041µs
2019/11/14 14:58:58 QPS:  1881  Latency:  232.893µs
2019/11/14 14:58:59 QPS:  1867  Latency:  233.41µs
2019/11/14 14:59:00 QPS:  1866  Latency:  232.374µs
2019/11/14 14:59:01 QPS:  1880  Latency:  229.484µs
2019/11/14 14:59:02 QPS:  1882  Latency:  230.578µs
2019/11/14 14:59:03 QPS:  1810  Latency:  237.021µs

```

