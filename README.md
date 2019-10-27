# Jarkom-modul-3-2019

1. Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.
seluruh Client
untuk "nano /etc/network/interfaces" hanya perlu

auto eth0
iface eth0 inet dhcp

untuk menerima ip dari server DHCP

2. (1) Client di subnet 2 mendapatkan peminjaman alamat IP dengan range 192.168.0.2 s.d.
192.168.0.10 dan 192.168.0.20 s.d. 192.168.0.30 dengan netmask 255.255.255.0.

di server DHCP bagian "nano /etc/dhcp/dhcpd.conf" memiliki settingan seperti dibawah
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.2 192.168.0.10;
    range 192.168.0.20 192.168.0.30;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 202.46.129.2, 10.151.36.7, 10.151.73.34;
    default-lease-time 600;
    max-lease-time 6000;
}

dan yang mempernagruhi range ialah 
    range 192.168.0.2 192.168.0.10;
    range 192.168.0.20 192.168.0.30;

3. (2) Client di subnet 3 mendapatkan peminjaman alamat IP dengan range dari 192.168.1.2 s.d.
192.168.1.99 dengan netmask 255.255.255.0.
di server DHCP bagian "nano /etc/dhcp/dhcpd.conf" memiliki settingan seperti dibawah
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.2 192.168.1.99;
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 202.46.129.2, 10.151.36.7, 10.151.73.34;
    default-lease-time 300;
    max-lease-time 3000;
} 

dan yang mempernagruhi range ialah
  range 192.168.1.2 192.168.1.99;

4. (3) Client di subnet 2 mendapatkan peminjaman alamat IP selama 10 menit dan client di
subnet 3 mendapatkan peminjaman alamat IP selama 5 menit.
di server DHCP bagian "nano /etc/dhcp/dhcpd.conf" memiliki settingan seperti dibawah
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.2 192.168.1.99;
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 202.46.129.2, 10.151.36.7, 10.151.73.34;
    default-lease-time 300;
    max-lease-time 3000;
} 
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.2 192.168.0.10;
    range 192.168.0.20 192.168.0.30;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 202.46.129.2, 10.151.36.7, 10.151.73.34;
    default-lease-time 600;
    max-lease-time 6000;
}

dan yang memperngaruhi waktu ialah
  default-lease-time(max-lease-time adalah total waktu perpanjangan ip yang dimiliki, supaya ip tidak berubah terlalu cepat)

5. (4) Masing-masing client harus mendapatkan konfigurasi DNS / nameserver yang terdiri atas
IP ARTICUNO , 202.46.129.2 , dan 10.151.36.7 secara otomatis.
di server DHCP bagian "nano /etc/dhcp/dhcpd.conf" memiliki settingan seperti dibawah
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.2 192.168.1.99;
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 202.46.129.2, 10.151.36.7, 10.151.73.34;
    default-lease-time 300;
    max-lease-time 3000;
} 
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.2 192.168.0.10;
    range 192.168.0.20 192.168.0.30;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 202.46.129.2, 10.151.36.7, 10.151.73.34;
    default-lease-time 600;
    max-lease-time 6000;
}
dan yang memperngaruhi nama DNS yanga keluar ialah
  option domain-name-servers(untuk penulisannya adalah IP1, IP2, ..., ...)

6. (5) Client PSYDUCK selalu mendapatkan IP 192.168.1.28, TANPA menggunakan
konfigurasi IP Statis.
di server DHCP bagian "nano /etc/dhcp/dhcpd.conf" memiliki settingan seperti dibawah
host psyduck {
    hardware ethernet 4e:15:5b:3a:a2:4d;
    fixed-address 192.168.1.28;
} 
dan untuk client bagian "nano /etc/network/interfaces" memiliki settingan
    auto eth0
    iface eth0 inet dhcp
    hwaddress ether 4e:15:5b:3a:a2:4d (bagian ini untuk mengunci hwaddress, yang biasanya akan berganti ketika UML dimatikan) 

7. (6) Prof. Oak meminta anda membuatkan user
otentikasi dengan format:
● User : yy_moltres_user
● Password : yy_password

di server Proxy setalah install "apache2-utils" dan sudah memasangkan user dan passwor yang di ingin kan dengan command

htpasswd -c /etc/squid3/passwd "USER NAME"(setelah melaksanakan command anda akan diminta memasukan password yang di inginkan)

User dan password mengikuti permintaan soal
● User : a3_moltres_user
● Password : a3_password

dan mengubah bagian "nano /etc/squid3/squid.conf" seperti dibawah

http_port 8888
visible_hostname moltres

auth_param basic program /usr/lib/squid3/ncsa_auth /etc/squid3/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS 

lalu men-setting proxy secara manual dengan memasukan address dengan ip Moltres dan port 8888 (port mengikuti konfigurasi yang di buat di http_port)




8. (7) Untuk Client pada subnet 2 HANYA BISA mengakses internet pada hari Senin
s.d. Jumat pukul 11.00 s.d. 13.00 WIB


9  (8) Untuk Client pada subnet 3 HANYA BISA mengakses internet pada hari Senin
s.d. Jumat pukul 20.00 s.d. 07.00 WIB pada keesokan harinya.

10  (9) setiap ada koneksi dari subnet AJK (10.151.36.0/24) yang mengakses google.com akan langsung dialihkan
menuju duckduckgo.com. 

11  (10) Selain itu, demi menjaga keamanan kota Pallet, anda diminta untuk
mem-block setiap koneksi yang berasal dari subnet Informatics_wifi (10.151.252.0/22) menuju
http://10.151.36.5:5000 , sebab subnet Informatics_wifi adalah milik tim Rocket.


12.  (11) Karena menurut Prof. Oak menghafalkan IP Proxy kota Pallet cukup merepotkan, kalian diminta
untuk mempermudah trainer dan penduduk dalam menggunakan Proxy yaitu cukup dengan
mengetikkan proxy.yy.com dan memasukkan port 8888.

ini berarti mengubah address dan port di pengaturan proxy
untuk address perlu membuat salah satu dari server dijadikan DNS server yang nanti membuat Domain 

membuat domain ini perlu men-setting "nano /etc/bind/named.conf.local" seperti dibawah
(Mengikuti soal)
zone "proxy.a3.com"{
        type master;
        file "/etc/bind/proxy/proxy.a3.com";
        }; 
lalu mensetting nano /etc/bind/proxy/proxy.a3.com (directory ini mengikuy=ti soal, directory bisa diubah seperti /etc/bind/(bebas)/(nama websitenya))
;(mengikuti soal)
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     proxy.a3.com. root.proxy.a3.com. (
                         201910262      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      proxy.a3.com.
@       IN      A       10.151.73.36    ;MOLTRES 

dan setelah itu melakukan restart 

untuk port maka di server proxy bagian "nano /etc/squid3/squid.conf" hanay perlu nambah kan http_port yyyy(y berarti angka unutk port yang diinginkan)
