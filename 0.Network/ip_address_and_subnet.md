# IP address and subnet

`192.168.1.1` is a decimal version of IP address, it means it comes from binary number, which is `11000000.10101000.00000001.00000001`.

## IP address
1. it contains 24 bits(0 or 1)
2. it contains 4 segments, every segment contain 8 bits.
3. due to the limit is 8 bits, the biggest number in one segment is 255.
4. 0.0.0.0 is reserved.
5. 255.255.255.255 is used as a broadcast address, when you want broadcast something in the whole net, you can use the 255.255.255.255 as the destination IP address.
6. There are 4 294 967 294 available addresses in theory(2<sup>32</sup> - 2).

## Subnet
### Why we need subnet?
1. 4 billion addresses is way not enough.
2. If everyone is in a big network, When everyone broadcasts message at the same time, the network will overload and be crashed.
3. Hide users in group, and control the visibility between each other.

### How Subnet working?
divide address to two parts, one is Network number, one is host number, and commonly there will be three kinds of network.
| CLASS | FIRST OCTET ADDRESS | DEFAULT SUBNET MASK|
| ---- | ----- | ---------------|
| A | 1 - 126 | 255.0.0.0 |
| B | 128 - 191 | 255.255.0.0 |
| C | 192 - 223 | 255.255.255.0 |
We use a C type IP address as an example
IP: 192.168.1.1
subnet mask: 255.255.255.0
```
IP:    11000000.10101000.00000001.00000001
MASK:  11111111.11111111.11111111.00000000
       --------------------------.========
       <------- Network  ------->.<-host->
           network:192.168.1       host:1    
```
There is a second way to describe mask: CIDR - Classic Inter-Domain Routing(slash notation)
like the example before, it is equivalent to `192.168.1.1/24`, the `24` means 24 bits of `1` from left to right.

### Break network to subnet?
You can break 192.168.1.1/24 into subnet, by borrowing bits from the host part, like we want to 3 subnet.
we can borrow two bits like `x` in `*.*.*.xx000000`, 
then we got

| SUBNET | HOST COUNT | REMARK |
| ------- | ---------- | ------ |
`*.*.*.00vvvvvv` | 62(vvvvvv part) | `*.*.*.00000000` as subnet address, `*.*.*.00111111` as broadcast address 
`*.*.*.01vvvvvv` | 62 | `*.*.*.01000000` as subnet address, `*.*.*.01111111` as broadcast address 
`*.*.*.10vvvvvv` | 62 | `*.*.*.10000000` as subnet address, `*.*.*.10111111` as broadcast address 
`*.*.*.11vvvvvv` | 62 | `*.*.*.11000000` as subnet address, `*.*.*.11111111` as broadcast address 

