```table ip filter {
        set RAWTRACK {
                type ipv4_addr . inet_service . inet_service
                size 65535
                flags dynamic,timeout
                counter
                timeout 1m
        }

        chain output {
                type filter hook postrouting priority mangle; policy accept;
                oif "eth0" update @RAWTRACK { ip daddr . udp sport . 17 }
                oif "eth0" update @RAWTRACK { ip daddr . tcp sport . 6 }
        }

        chain input {
                type filter hook input priority raw; policy drop;
                ip length 59 ip saddr . udp dport . 17 @RAWTRACK @ih,0,8 0x5c counter packets 0 bytes 0 accept
        }
}```
