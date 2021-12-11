# Jarkom-Modul-5-E10-2021

| **No** | **Nama**                 | **NRP**        |
| ------ | ------------------------ | -------------- |
| 1      | Muhammad Rizqi Tsani     | 05111940000045 |
| 2      | Dicksen Alfersius Novian | 05111940000076 |
| 3      | Bill Harit Yafi          | 05111940000114 |

## A. Topologi

![Topologi](https://user-images.githubusercontent.com/65166398/145676279-bb48b454-eafb-45ff-b582-83dc149ad687.png)


## B. Subnetting (VLSM)

### Pohon

![image](https://user-images.githubusercontent.com/68275535/145666401-79a37b23-2d0e-4a33-a86e-02856a3ea723.png)

### Tabel Pembagian IP

| Subnet | Note                   | Jumlah IP | Length | IP          | Subnet Mask     |
| ------ | ---------------------- | --------- | ------ | ----------- | --------------- |
| A1     | Water7 Doriki Jipangu  | 3         | 29     | 10.34.7.128 | 255.255.255.248 |
| A2     | Water7 Blueno          | 101       | 25     | 10.34.7.0   | 255.255.255.128 |
| A3     | Water7 Cipher          | 701       | 22     | 10.34.0.0   | 255.255.252.0   |
| A4     | Water7 Foosha          | 2         | 30     | 10.34.7.144 | 255.255.255.252 |
| A5     | Foosha Guanhao         | 2         | 30     | 10.34.7.148 | 255.255.255.252 |
| A6     | Guanhao Elena          | 301       | 23     | 10.34.4.0   | 255.255.254.0   |
| A7     | Guanhao Fukurou        | 201       | 24     | 10.34.6.0   | 255.255.255.0   |
| A8     | Guanhao Maingate Jorge | 3         | 29     | 10.34.7.136 | 255.255.255.248 |

### Config Node

#### Doriki

```
auto eth0
iface eth0 inet static
address 10.34.7.130
netmask 255.255.255.248
gateway 10.34.7.129

```

#### Jipangu

```
auto eth0
iface eth0 inet static
address 10.34.7.131
netmask 255.255.255.248
gateway 10.34.7.129

```

#### Water7

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.34.7.193
netmask 255.255.255.252
gateway 10.34.7.194

auto eth1
iface eth1 inet static
address 10.34.7.129
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 10.34.7.1
netmask 255.255.255.128

auto eth3
iface eth3 inet static
address 10.34.0.1
netmask 255.255.252.0
```

#### Blueno

```
auto eth0
iface eth0 inet static
address 10.34.7.2
netmask 255.255.255.128
gateway 10.34.7.1
```

#### Cipher

```
auto eth0
iface eth0 inet static
address 10.34.0.2
netmask 255.255.252.0
gateway 10.34.0.1
```

#### Foosha

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
hwaddress ether b2:ab:03:2e:50:76

auto eth1
iface eth1 inet static
address 10.34.7.146
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 10.34.7.149
netmask 255.255.255.252
```

#### Jorge

```
auto eth0
iface eth0 inet static
address 10.34.7.162
netmask 255.255.255.248
gateway 10.34.7.161
```

#### Maingate

```
auto eth0
iface eth0 inet static
address 10.34.7.163
netmask 255.255.255.248
gateway 10.34.7.161
```

#### Guanhao

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.34.7.226
netmask 255.255.255.252
gateway 10.34.7.225

auto eth1
iface eth1 inet static
address 10.34.7.161
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 10.34.4.1
netmask 255.255.254.0

auto eth3
iface eth3 inet static
address 10.34.6.1
netmask 255.255.255.0
```

#### Elena

```
auto eth0
iface eth0 inet static
address 10.34.4.2
netmask 255.255.254.0
gateway 10.34.4.1
```

#### Fukurou

```
auto eth0
iface eth0 inet static
address 10.34.6.2
netmask 255.255.255.0
gateway 10.34.6.1
```

## C. Routing

#### Foosha

```
route add -net 10.34.7.128 netmask 255.255.255.248 gw 10.34.7.193
route add -net 10.34.7.0 netmask 255.255.255.128 gw 10.34.7.193
route add -net 10.34.0.0 netmask 255.255.252.0 gw 10.34.7.193
route add -net 10.34.4.0 netmask 255.255.254.0 gw 10.34.7.226
route add -net 10.34.6.0 netmask 255.255.255.0 gw 10.34.7.226
route add -net 10.34.7.160 netmask 255.255.255.248 gw 10.34.7.226
```

## D. DHCP

### Config DHCP Server (Jipangu)

1. Install isc-dhcp-server
    ```bash
    echo "nameserver 192.168.122.1" > /etc/resolv.conf

    apt-get update
    apt-get install isc-dhcp-server -y
    ```
2. Ganti interface di `/etc/default/isc-dhcp-server`
    ```bash
    INTERFACES="eth0"
    ```
3. Edit `etc/dhcp/dhcpd.conf`
    ```bash
    # Blueno
    subnet 10.34.7.0 netmask 255.255.255.128 {
      range 10.34.7.2 10.34.7.126;
      option routers 10.34.7.1;
      option broadcast-address 10.34.7.127;
      option domain-name-servers 10.34.7.130;
      default-lease-time 600;
      max-lease-time 7200;
    }

    # Cipher
    subnet 10.34.0.0 netmask 255.255.252.0 {
      range 10.34.0.2 10.34.3.254;
      option routers 10.34.0.1;
      option broadcast-address 10.34.3.255;
      option domain-name-servers 10.34.7.130;
      default-lease-time 600;
      max-lease-time 7200;
    }

    # Elena
    subnet 10.34.4.0 netmask 255.255.254.0 {
      range 10.34.4.2 10.34.5.254;
      option routers 10.34.4.1;
      option broadcast-address 10.34.5.255;
      option domain-name-servers 10.34.7.130;
      default-lease-time 600;
      max-lease-time 7200;
    }

    # Fukurou
    subnet 10.34.6.0 netmask 255.255.255.0 {
      range 10.34.6.2 10.34.6.254;
      option routers 10.34.6.1;
      option broadcast-address 10.34.6.255;
      option domain-name-servers 10.34.7.130;
      default-lease-time 600;
      max-lease-time 7200;
    }

    subnet 10.34.7.128 netmask 255.255.255.248 {
      option routers 10.34.7.129;
    }
    ```
4. Jalankan `service isc-dhcp-server restart`

### Config DHCP Relay (Water7 & Guanhao)

1. Install isc-dhcp-relay
    ```bash
    echo "nameserver 192.168.122.1" > /etc/resolv.conf

    apt-get update
    apt-get install isc-dhcp-relay -y
    ```
2. Edit `/etc/default/isc-dhcp-relay`
    ```bash
    # What servers should the DHCP relay forward requests to?
    SERVERS="10.34.7.131"

    # On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
    INTERFACES="eth0 eth1 eth2 eth3"

    # Additional options that are passed to the DHCP relay daemon?
    OPTIONS=""
    ```

### DHCP Client (Blueno, Cipher, Elena, Fukurou)

Ubah `/etc/network/interfaces`, ubah config IP statis menjadi:

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

### Testing

![image](https://user-images.githubusercontent.com/68275535/145667203-63357968-05ef-4a7f-8c7d-fe2ba32a7e17.png)
![image](https://user-images.githubusercontent.com/68275535/145667210-bb7e08c8-2a3c-4e73-a93a-246f27b80bca.png)
![image](https://user-images.githubusercontent.com/68275535/145667238-5ddb5136-194f-4157-bff5-b373fb60a4b9.png)
![image](https://user-images.githubusercontent.com/68275535/145667241-d37b6019-c4c1-4a8b-aca5-8f11aebe56d4.png)

## Soal 1
> Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE

Pada Foosha

`iptables -t nat -A POSTROUTING -s 10.34.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.224`

Pada node lain

`echo "nameserver 192.168.122.1" > /etc/resolv.conf`

### Testing

![image](https://user-images.githubusercontent.com/68275535/145667342-f20cf525-a3b7-452b-8b05-2154e7f18be6.png)

## Soal 2
> Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan

Pada **Doriki** dan **Jipangu**

```bash
iptables -A FORWARD -d 10.34.7.128/29 -i eth0 -p tcp --dport 80 -j DROP
iptables -A FORWARD -d 10.34.7.128/29 -i eth0 -p tcp --dport 443 -j ACCEPT
```

### Testing

Doriki

![image](https://user-images.githubusercontent.com/68275535/145667316-8c7177f4-ab5b-4518-981b-ac856cff24f9.png)

Jipangu

![image](https://user-images.githubusercontent.com/68275535/145667322-13328036-7d93-46ae-b1f9-54d489f430a1.png)

## Soal 3
> Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop

Pada **Doriki** dan **Jipangu**

```bash
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

### Testing

Doriki

![image](https://user-images.githubusercontent.com/68275535/145667422-b18d0c7f-20d4-4569-a57f-5e22731438e1.png)

Jipangu

![image](https://user-images.githubusercontent.com/68275535/145667407-b41bc498-7b6e-44d4-b3e9-5410e1e7ed27.png)
