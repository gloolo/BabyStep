# NAT
NAT: Network Address Translation, it translates a set of IP addresses to another set of IP addresses.

### Why we need NAT?
Main reason is the shortage of IPv4 **public** addresses. With NAT, when you need to get source from Internet, you use a router to translate your local IP address(Private address, like 192.168.2.1) to an Internet IP address(Public address, like 67.1.4.67). More common scenario is like your company's network, it may have only one public IPv4 address, but it can provide service for thousands of PC to surfing in Internet.

### Private Address
```
10.0.0.0 - 10.255.255.255
172.16.0.0 - 172.31.255.255
192.168.0.0 - 192.168.255.255
```
## How NAT works?
There're 3 ways to archive the goal.

### 1. Overload
PAT: Port address translation. 
```
192.168.0.1               --- Router ----         Internet
Source 192.168.0.1:8897                           Destination 55.66.77.88:80
Source 11.22.33.44:8897                           Destination 55.66.77.88:80
```
It caches a table to save the relationships.
| Inside | OutSide |
| ------ | ------- |
| 192.168.0.1:8897 | 11.22.33.44:8897 |

### 2. Dynamic
There is a pool, when you need request from the public, you request a address from pool, and use it to get what you want, then the public return resource to the pool ip, and the router translate it back to private IP, when it's done, the pool will recycle the pool ip address.


### 3. Static
It bonds a private IP address to a public IP address, the common usage is Web Service, when someone request a public address, the NAT translate it to a specific private IP address.
| Inside | OutSide |
| ------ | ------- |
| 192.168.0.1:8897 | 11.22.33.44:1111 |