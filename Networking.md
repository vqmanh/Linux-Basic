# Network interfaces

Network interfaces là một kênh kết nỗi giữa thiết bị và mạng. Network interfaces có thể giao tiếp qua card mạng vật lý (NIC) hoặc có thể triển khai dưới dạng phần mềm. Một danh sách các giao diện mạng hiện đang hoạt động được báo cáo bởi tiện ích `ifconfig`. Các tập tin cấu hình mạng là rất cần thiết để đảm bảo các giao diện hoạt động chính xác.

- Đối với Debian, tệp cấu hình mạng cơ bản là `/etc/network/interfaces`
- Đối với RedHat, thông tin định tuyến và máy chủ được chứa trong /etc/sysconfig/network

````
[root@vqmanh ~]# cat /etc/sysconfig/network-scripts/ifcfg-ens33
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=add7b9c5-8da1-41f8-b1e7-c050b9483c09
DEVICE=ens33
ONBOOT=yes
IPADDR=66.0.0.200
NETMASK=255.255.255.0
GATEWAY=66.0.0.1
DNS=8.8.8.8
````
**`ip` là một chương trình rất mạnh mẽ có thể làm nhiều việc.**

Cú pháp: 

`ip addr show`

`ip route show`

````
[root@vqmanh ~]#  ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:6c:77:e6 brd ff:ff:ff:ff:ff:ff
    inet 66.0.0.200/24 brd 66.0.0.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::18d7:17ea:afef:3393/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
[root@vqmanh ~]# ip route show
default via 66.0.0.1 dev ens33 proto static metric 100
66.0.0.0/24 dev ens33 proto kernel scope link src 66.0.0.200 metric 100
````

## Routing table - Bảng định tuyến

- Lệnh route được sử dụng để xem hoặc thay đổi bảng định tuyến IP. Bạn có thể muốn thay đổi bảng định tuyến IP để thêm, xóa hoặc sửa đổi các định tuyến tĩnh thành các máy chủ hoặc mạng cụ thể.

- Bảng định tuyến, gần giống như một bảng chỉ đường cho chúng ta biết muốn đi đến một mạng nào đó thì cần đi qua đâu? qua địa chỉ IP nào? hay qua card mạng nào. 

```
[root@vqmanh ~]# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         66.0.0.1        0.0.0.0         UG    100    0        0 ens33
66.0.0.0        0.0.0.0         255.255.255.0   U     100    0        0 ens33
66.0.0.199      66.0.0.1        255.255.255.255 UGH   0      0        0 ens33
```
***Cột flag:***
- U - route đang up
- H - đối tượng là Host
- G - sử dụng route này là route gateway
- R - routing động
- M - được chỉnh sửa bởi 1 dịch vụ
- A - được cài đặt bởi addrconf
- C - cache entry
- ! - route bị reject

### Thêm route với 1 network
```
route add -net {NETWORK-ADDRESS}/{BETMASK} gw {IP_GATEWAY} {INTERFACE-NAME}
route add -net {NETWORK-ADDRESS} netmask {NETMASK} gw {IP_GATEWAY} {INTERFACE-NAME}
```
VD: `route add -net 66.0.0.0/24 gw 66.0.0.1 eth0`

hoặc

`route add -net 66.0.0.0 netmask 255.255.255.0 gw 66.0.0.1 eth0`

### Thêm route với 1 host cụ thể

`route add -host {IP-ADDRESS} gw {IP_GATEWAY} {INTERFACE-NAME}`

VD: `route add -host 66.0.0.199 gw 66.0.0.1 eth0`

### Xóa route với 1 host cụ thể
`	
route del -host {IP-ADDRESS} gw {IP_GATEWAY} {INTERFACE-NAME}`

### Reject route

Thêm vào 1 route thuộc dạng private, tức khi kernel tìm route cho địa chỉ IP hoặc network đó thì sẽ tìm kiếm thất bại.

`route add -host 66.0.0.199 reject`

`route add -net 66.0.0.0/24 reject`

### Thêm default gateway route

`route add default gw {IP-ADDRESS} {INTERFACE-NAME}`

- **IP-ADDRESS**: địa chỉ của router gateway
- **INTERFACE-NAME**: cổng card mạng sẽ đi ra ngoài đến router gateway

**Xóa default gateway route**

`route del default gw {IP-ADDRESS} {INTERFACE-NAME}`

