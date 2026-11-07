নিচে তোমার দেওয়া ল্যাবের সম্পূর্ণ সমাধানটি শুরু থেকে শেষ পর্যন্ত সুন্দরভাবে সাজিয়ে দেওয়া হলো। এতে Subnetting Chart, IP Addressing, Cabling, প্রতিটি Router-এর Configuration, Static Routing, Default Routing, RIP v1, Server ও PC Configuration এবং Connectivity Testing—সবকিছু অন্তর্ভুক্ত করা হয়েছে।

# Complete Lab Solution: Multi-Branch Network Configuration

## Task 1: Addressing Scheme Design & Subnetting

এই Task-এ WAN, Branch LAN এবং ISP Network-এর জন্য IP Address ও Subnet Mask নির্ধারণ করতে হবে।

### 1. WAN Subnetting

প্রদত্ত Network: `10.0.1.0/28`

এখানে মোট ৪টি WAN Subnet তৈরি করতে হবে। প্রতিটি Subnet-এর জন্য `/30` ব্যবহার করা হবে।

- Subnet Mask: `255.255.255.252`
- Total IP Addresses: 4
- Usable IP Addresses: 2
- Block Size: 4

WAN Subnetting Chart

| Subnet | Network Address | Usable IP | Broadcast |
| ------ | --------------- | --------- | --------- |
| 0      | 10.0.1.0/30     | .1 – .2   | .3        |
| 1      | 10.0.1.4/30     | .5 – .6   | .7        |
| 2      | 10.0.1.8/30     | .9 – .10  | .11       |
| 3      | 10.0.1.12/30    | .13 – .14 | .15       |

IP Assignment:

Subnet 0: R1 ↔ B1

- R1 Se0/0: `10.0.1.1`
- B1 Se0/0: `10.0.1.2`

Subnet 1: R1 ↔ B2

- R1 Se0/1: `10.0.1.5`
- B2 Se0/0: `10.0.1.6`

Subnet 2: R1 ↔ B3

- R1 Se0/2: `10.0.1.9`
- B3 Se0/0: `10.0.1.10`

Subnet 3: R1 ↔ B4

- R1 Se0/3: `10.0.1.13`
- B4 Se0/0: `10.0.1.14`

### 2. ISP ও Web Server Network

ISP ↔ Web Server

- Network: `209.165.200.224/30`
- Subnet Mask: `255.255.255.252`

| Device     | Interface | IP Address      |
| ---------- | --------- | --------------- |
| ISP        | Fa0/0     | 209.165.200.225 |
| Web Server | Fa0       | 209.165.200.226 |

ISP ↔ R1

- Network: `209.165.201.0/30`
- Subnet Mask: `255.255.255.252`

| Device | Interface | IP Address    |
| ------ | --------- | ------------- |
| ISP    | Se0/0     | 209.165.201.1 |
| R1     | Se1/0     | 209.165.201.2 |

### 3. Branch LAN Subnetting

প্রদত্ত Network: `10.1.0.0/16`

প্রথমে `/16` Network-কে ৪টি সমান Subnet-এ ভাগ করতে হবে।

- Original Prefix: `/16`
- New Prefix: `/18`
- Borrowed Bits: 2
- Total Subnets: 4

এরপর প্রতিটি `/18` Subnet-কে আবার ৪টি Subnet-এ ভাগ করতে হবে।

- Original Prefix: `/18`
- New Prefix: `/20`
- Borrowed Bits: 2
- Total Subnets per Branch: 4
- Subnet Mask: `255.255.240.0`
- Block Size: 16
- Usable Hosts per Subnet: 4094

প্রথম ধাপ: /16 থেকে /18

| Branch | Network Address | Prefix |
| ------ | --------------- | ------ |
| B1     | 10.1.0.0        | /18    |
| B2     | 10.1.64.0       | /18    |
| B3     | 10.1.128.0      | /18    |
| B4     | 10.1.192.0      | /18    |

দ্বিতীয় ধাপ: প্রতিটি /18 থেকে ৪টি /20 Subnet

B1 LANs — 10.1.0.0/18

| Interface | Network Address | Gateway IP |
| --------- | --------------- | ---------- |
| Fa0/0     | 10.1.0.0/20     | 10.1.0.1   |
| Fa0/1     | 10.1.16.0/20    | 10.1.16.1  |
| Fa1/0     | 10.1.32.0/20    | 10.1.32.1  |
| Fa1/1     | 10.1.48.0/20    | 10.1.48.1  |

B2 LANs — 10.1.64.0/18

| Interface | Network Address | Gateway IP |
| --------- | --------------- | ---------- |
| Fa0/0     | 10.1.64.0/20    | 10.1.64.1  |
| Fa0/1     | 10.1.80.0/20    | 10.1.80.1  |
| Fa1/0     | 10.1.96.0/20    | 10.1.96.1  |
| Fa1/1     | 10.1.112.0/20   | 10.1.112.1 |

B3 LANs — 10.1.128.0/18

| Interface | Network Address | Gateway IP |
| --------- | --------------- | ---------- |
| Fa0/0     | 10.1.128.0/20   | 10.1.128.1 |
| Fa0/1     | 10.1.144.0/20   | 10.1.144.1 |
| Fa1/0     | 10.1.160.0/20   | 10.1.160.1 |
| Fa1/1     | 10.1.176.0/20   | 10.1.176.1 |

B4 LANs — 10.1.192.0/18

| Interface | Network Address | Gateway IP |
| --------- | --------------- | ---------- |
| Fa0/0     | 10.1.192.0/20   | 10.1.192.1 |
| Fa0/1     | 10.1.208.0/20   | 10.1.208.1 |
| Fa1/0     | 10.1.224.0/20   | 10.1.224.1 |
| Fa1/1     | 10.1.240.0/20   | 10.1.240.1 |

## Task 2 & 3: Cabling and Device Configuration

### 1. Cable Connection

Packet Tracer-এ নিচের মতো Cable Connection করতে হবে।

| Connection             | Cable Type              | DCE Side |
| ---------------------- | ----------------------- | -------- |
| ISP ↔ R1               | Serial DCE              | ISP      |
| R1 ↔ B1                | Serial DCE              | R1       |
| R1 ↔ B2                | Serial DCE              | R1       |
| R1 ↔ B3                | Serial DCE              | R1       |
| R1 ↔ B4                | Serial DCE              | R1       |
| ISP ↔ Server           | Copper Straight-Through | N/A      |
| Branch Router ↔ Switch | Copper Straight-Through | N/A      |
| Switch ↔ PC            | Copper Straight-Through | N/A      |

DCE প্রান্তে `clock rate 64000` দিতে হবে।

### 2. ISP Router Configuration

```
enable
configure terminal
hostname ISP

enable secret class

line console 0
password cisco
login
exit

line vty 0 4
password cisco
login
exit

interface FastEthernet0/0
ip address 209.165.200.225 255.255.255.252
no shutdown
exit

interface Serial0/0
ip address 209.165.201.1 255.255.255.252
clock rate 64000
no shutdown
exit

end
```

### 3. R1 Router Configuration

```
enable
configure terminal
hostname R1

enable secret class

line console 0
password cisco
login
exit

line vty 0 4
password cisco
login
exit

interface Serial1/0
ip address 209.165.201.2 255.255.255.252
no shutdown
exit

interface Serial0/0
ip address 10.0.1.1 255.255.255.252
clock rate 64000
no shutdown
exit

interface Serial0/1
ip address 10.0.1.5 255.255.255.252
clock rate 64000
no shutdown
exit

interface Serial0/2
ip address 10.0.1.9 255.255.255.252
clock rate 64000
no shutdown
exit

interface Serial0/3
ip address 10.0.1.13 255.255.255.252
clock rate 64000
no shutdown
exit

end
```

### 4. B1 Router Configuration

```
enable
configure terminal
hostname B1

enable secret class

line console 0
password cisco
login
exit

line vty 0 4
password cisco
login
exit

interface Serial0/0
ip address 10.0.1.2 255.255.255.252
no shutdown
exit

interface FastEthernet0/0
ip address 10.1.0.1 255.255.240.0
no shutdown
exit

interface FastEthernet0/1
ip address 10.1.16.1 255.255.240.0
no shutdown
exit

interface FastEthernet1/0
ip address 10.1.32.1 255.255.240.0
no shutdown
exit

interface FastEthernet1/1
ip address 10.1.48.1 255.255.240.0
no shutdown
exit

end
```

### 5. B2 Router Configuration

```
enable
configure terminal
hostname B2

enable secret class

line console 0
password cisco
login
exit

line vty 0 4
password cisco
login
exit

interface Serial0/0
ip address 10.0.1.6 255.255.255.252
no shutdown
exit

interface FastEthernet0/0
ip address 10.1.64.1 255.255.240.0
no shutdown
exit

interface FastEthernet0/1
ip address 10.1.80.1 255.255.240.0
no shutdown
exit

interface FastEthernet1/0
ip address 10.1.96.1 255.255.240.0
no shutdown
exit

interface FastEthernet1/1
ip address 10.1.112.1 255.255.240.0
no shutdown
exit

end
```

### 6. B3 Router Configuration

```
enable
configure terminal
hostname B3

enable secret class

line console 0
password cisco
login
exit

line vty 0 4
password cisco
login
exit

interface Serial0/0
ip address 10.0.1.10 255.255.255.252
no shutdown
exit

interface FastEthernet0/0
ip address 10.1.128.1 255.255.240.0
no shutdown
exit

interface FastEthernet0/1
ip address 10.1.144.1 255.255.240.0
no shutdown
exit

interface FastEthernet1/0
ip address 10.1.160.1 255.255.240.0
no shutdown
exit

interface FastEthernet1/1
ip address 10.1.176.1 255.255.240.0
no shutdown
exit

end
```

### 7. B4 Router Configuration

```
enable
configure terminal
hostname B4

enable secret class

line console 0
password cisco
login
exit

line vty 0 4
password cisco
login
exit

interface Serial0/0
ip address 10.0.1.14 255.255.255.252
no shutdown
exit

interface FastEthernet0/0
ip address 10.1.192.1 255.255.240.0
no shutdown
exit

interface FastEthernet0/1
ip address 10.1.208.1 255.255.240.0
no shutdown
exit

interface FastEthernet1/0
ip address 10.1.224.1 255.255.240.0
no shutdown
exit

interface FastEthernet1/1
ip address 10.1.240.1 255.255.240.0
no shutdown
exit

end
```

## Task 4 & 5: Static Routing, Default Routing and RIP v1

এই Task-এ Router-গুলোর মধ্যে Routing Configuration করতে হবে, যাতে Branch LAN থেকে ISP এবং Web Server-এর সঙ্গে Communication করা যায়।

### 1. ISP Router-এ Static Route

```
enable
configure terminal

ip route 10.0.1.0 255.255.255.240 Serial0/0

ip route 10.1.0.0 255.255.0.0 Serial0/0

end
```

### 2. R1 Router-এ Default Route এবং Static Route

Default Route:

```
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 Serial1/0
```

Branch LAN-এর জন্য Static Route:

```
ip route 10.1.0.0 255.255.192.0 Serial0/0

ip route 10.1.64.0 255.255.192.0 Serial0/1

ip route 10.1.128.0 255.255.192.0 Serial0/2

ip route 10.1.192.0 255.255.192.0 Serial0/3

end
```

### 3. Branch Router-এ Default Route এবং RIP v1

প্রতিটি Branch Router-এ Default Route এবং RIP v1 Configure করতে হবে।

B1 Configuration:

```
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 Serial0/0

router rip
version 1
network 10.0.0.0
exit

end
```

B2 Configuration:

```
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 Serial0/0

router rip
version 1
network 10.0.0.0
exit

end
```

B3 Configuration:

```
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 Serial0/0

router rip
version 1
network 10.0.0.0
exit

end
```

B4 Configuration:

```
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 Serial0/0

router rip
version 1
network 10.0.0.0
exit

end
```

## Task 6: Server Configuration (DNS & HTTP)

Web Server-এর মাধ্যমে একটি Website Host করতে হবে এবং DNS ব্যবহার করে Domain Name Configure করতে হবে।

### 1. Server IP Configuration

Packet Tracer-এ Web Server-এ ক্লিক করুন।

Desktop → IP Configuration

| Field           | Value           |
| --------------- | --------------- |
| IP Address      | 209.165.200.226 |
| Subnet Mask     | 255.255.255.252 |
| Default Gateway | 209.165.200.225 |
| DNS Server      | 209.165.200.226 |

### 2. HTTP Configuration

Server → Services → HTTP

- HTTP: On
- HTTPS: On

### 3. DNS Configuration

Server → Services → DNS

- DNS Service: On
- Name: `pstu.ac.bd`
- Type: `A Record`
- Address: `209.165.200.226`

এরপর Add বাটনে ক্লিক করুন।

## Task 7: End Device Configuration (PC0 & PC1)

### 1. PC0 Configuration (B1 LAN)

PC0 → Desktop → IP Configuration

| Field           | Value           |
| --------------- | --------------- |
| IP Address      | 10.1.0.10       |
| Subnet Mask     | 255.255.240.0   |
| Default Gateway | 10.1.0.1        |
| DNS Server      | 209.165.200.226 |

### 2. PC1 Configuration (B4 LAN)

PC1 → Desktop → IP Configuration

| Field           | Value           |
| --------------- | --------------- |
| IP Address      | 10.1.192.10     |
| Subnet Mask     | 255.255.240.0   |
| Default Gateway | 10.1.192.1      |
| DNS Server      | 209.165.200.226 |

## Task 8: Verification and Connectivity Testing

সব Configuration সম্পন্ন করার পর Network Connectivity পরীক্ষা করতে হবে।

### 1. PC0 থেকে Web Server Ping Test

PC0 → Desktop → Command Prompt

```
ping 209.165.200.226
```

Reply পাওয়া গেলে PC0 থেকে Web Server পর্যন্ত Connectivity সফল।

### 2. PC0 থেকে Website Access

PC0 → Desktop → Web Browser

URL লিখুন:

`http://pstu.ac.bd`

Website Load হলে DNS Resolution এবং HTTP Service কাজ করছে।

### 3. B1 থেকে Extended Ping Test

B1 Router-এর CLI-তে নিচের Command দিন:

```
B1# ping
```

এরপর নিচের তথ্যগুলো দিন:

```
Protocol [ip]: (Enter)

Target IP address: 209.165.200.226

Repeat count [5]: (Enter)

Datagram size [100]: (Enter)

Timeout in seconds [2]: (Enter)

Extended commands [n]: yes

Source address or interface: 10.1.0.1
```

বাকি সব Option Default রেখে Enter চাপুন।

Ping Successful হলে B1-এর নির্দিষ্ট LAN Interface থেকে Web Server পর্যন্ত Connectivity কাজ করছে।

### 4. Final Result Check

সব Configuration সম্পন্ন হলে Packet Tracer Window-এর নিচের বাম পাশে থাকা Check Results বাটনে ক্লিক করুন।

- Completion Score: 100%
- Connectivity Test: Successful
- DNS Resolution: Successful
- HTTP Access: Successful

## গুরুত্বপূর্ণ: Configuration-এর আগে কয়েকটি বিষয় যাচাই করবে

তোমার দেওয়া IP Addressing-এর হিসাবগুলো ঠিক আছে। তবে বাস্তবে Configuration করার সময় নিচের বিষয়গুলো খেয়াল রাখতে হবে:

1. Interface Names: Router Model অনুযায়ী `Serial0/3`, `Serial1/0` বা `FastEthernet1/1` নাও থাকতে পারে। `show ip interface brief` দিয়ে Interface যাচাই করবে।
2. Clock Rate: শুধু DCE প্রান্তে `clock rate 64000` দিতে হবে। DTE প্রান্তে নয়।
3. RIP v1: RIP v1 Classful Routing ব্যবহার করে। এই Topology-তে `/20` ও `/30`-এর মতো VLSM ব্যবহৃত হয়েছে। তাই RIP v1-এর পরিবর্তে RIP v2 ব্যবহার করা অধিক উপযুক্ত।
4. Routing: ISP-এর Static Route এবং R1-এর Default Route সঠিক Interface দিয়ে দেওয়া হয়েছে কি না যাচাই করবে।
5. RIP Advertisement: Branch Router-এর Default Route স্বয়ংক্রিয়ভাবে RIP-এর মাধ্যমে অন্য Router-এ প্রচারিত হয় না। প্রয়োজন হলে `default-information originate` ব্যবহার করতে হবে।

Verification Commands:

```
show ip interface brief
show ip route
show ip protocols
show controllers serial 0/0
```

সবশেষে মনে রাখবে, শুধু Ping Successful হলেই Packet Tracer-এর Completion Score 100% নিশ্চিত হয় না। Assessment-এর প্রতিটি requirement পূরণ করতে হবে।
