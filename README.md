# Proof Of Concept Praktikum Jaringan Komputer Modul 2

- [Proof Of Concept Praktikum Jaringan Komputer Modul 2](#proof-of-concept-praktikum-jaringan-komputer-modul-2)
  - [Kelompok : B13](#kelompok--b13)
  - [Soal 1](#soal-1)
    - [Deskripsi](#deskripsi)
    - [Solusi](#solusi)
    - [Hasil](#hasil)
  - [Soal No 2](#soal-no-2)
    - [Deskripsi](#deskripsi-1)
    - [Solusi](#solusi-1)
    - [Hasil](#hasil-)
  - [Soal No 3](#soal-no-3)
    - [Deskripsi](#deskripsi-2)
    - [Solusi](#solusi-2)
    - [Hasil](#hasil-1)
  - [Soal No 4](#soal-no-4)
    - [Deskripsi](#deskripsi-3)
    - [Solusi](#solusi-3)
    - [Hasil](#hasil-2)
  - [Soal No 5](#soal-no-5)
    - [Deskripsi](#deskripsi-4)
    - [Solusi](#solusi-4)
  - [Soal No 6](#soal-no-6)
    - [Deskripsi](#deskripsi-)
    - [Solusi](#solusi-5)
    - [Hasil](#hasil-3)
  - [Soal No 7](#soal-no-7)
    - [Deskripsi](#deskripsi-5)
    - [Solusi](#solusi-6)
    - [Hasil](#hasil-4)
  - [Soal No 8](#soal-no-8)
    - [Deskripsi](#deskripsi-6)
    - [Solusi](#solusi-7)
    - [Hasil](#hasil-5)
  - [Soal No 9](#soal-no-9)
    - [Deskripsi](#deskripsi-7)
    - [Solusi](#solusi-8)
  - [Soal No 10](#soal-no-10)
    - [Deskripsi](#deskripsi-8)
    - [Solusi](#solusi-9)
    - [Hasil](#hasil-6)

## Kelompok : B13

| NRP        | Nama                 |
| ---------- | -------------------- |
| 5025211042 | Robby Ulung Pambudi  |
| 5025211151 | Tsaqif Deniar Bhakti |

# Soal 1
## Deskripsi
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian [sebagai berikut.](https://docs.google.com/spreadsheets/d/1OqwQblR_mXurPI4gEGqUe7v0LSr1yJViGVEzpMEm2e8/edit?usp=sharing) Folder topologi dapat diakses pada [drive berikut](https://drive.google.com/drive/folders/1Ij9J1HdIW4yyPEoDqU1kAwTn_iIxg3gk?usp=sharing)&#x20;
## Solusi
Untuk membuat topologi tersebut pertama kita perlu setup gns sampai muncul tampilan sebagai berikut :

![img1](Image/1.1.png)

Kemudian kita buka alamat yang terterah di gns tersebut yaitu : http://172.16.175.129

Setelah terbuka pada browser langkah dan sambungkan masing - masing node sesuai dengan gambar pada soal

![img1](Image/1.2.png)

Sesuai nama

![img1](Image/1.3.png)

Kemudian Sesuaikan masing-masing IP pada node dengan prefix ip adalah `10.13.xxx.xxx`
dengan cara setting configuration pada router dengan script sebagai berikut :

![img1](Image/1.4.png)

Kemudian jika berhasil maka sebagai berikut

![img1](Image/1.5.png)

Kemudian setting node `Yudhistira (eth1 -> eth0)`

![img1](Image/1.6.png)

Kemudian setting node `Werkudara (eth2 -> eth0)`

![img1](Image/1.7.png)

Kemudian kita setting bagian pada **eth3** dan kita bagi ip antara sadewa dan nakula menjadi berikut :

**Sadewa** 	: 10.13.3.2

**Nakula**	: 10.13.3.3

Selanjutnya kita buat configuration jaringan nya menjadi berikut : 

![img1](Image/1.8.png)

Kemudian kita setting bagian pada **eth4** dan kita bagi ip antara Arjuna-LB, Prabukusuma, Abimanyu,  dan Wissanggeni menjadi berikut : 

**Arjuna-LB** 	: `10.13.4.2`

**Prabukusuma**	: `10.13.4.3`

**Abimanyu**		: `10.13.4.4`

**Wissanggeni**	: `10.13.4.5`

Selanjutnya kita buat configuration jaringan nya menjadi berikut :

![img1](Image/1.9.png)
![img1](Image/1.10.png)

Kemudian kita setting iptable agar seluruh node di bawah router bisa terkoneksi ke internet : 

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.13.0.0/16
```

Dan semua node pada project tersebut sudah terhubung dan terkoneksi dengan internet.

Langkah selanjutnya setting node **Yudhistira** menjadi DNS Master dan node **Werkudara**, 
Langkah pertama kita perlu melakukan instalasi bind9 dengan cara sebagai berikut 

```
apt-get update
apt-get install bind9 -y
service bind9 start
```
Maka node **Yudhistira** dan **Werkudara** siap untuk dijadikan sebuah DNS Server. 
Selanjutnya kita siapkan node **Arjuna-LB** sebagai load balancer dengan cara : 

```
apt-get update
apt-get install nginx -y
service nginx start
```
## Hasil
Selesai configuration node sudah dilakukan dan node yang bertugas sebagai DNS Server dan Load Balancer siap untuk menerima task yang akan diberikan.

# Soal No 2

## Deskripsi

Buatlah website utama dengan akses ke **arjuna.yyy.com** dengan alias **www\.arjuna.yyy.com** dengan yyy merupakan kode kelompok.

## Solusi

Untuk membuat DNS tersebut pertama-tama kita aktifkan dulu seluruh node dan kita langsung ke console dari node **Yudhistira** sebagai DNS Master pada jaringan ini.

gambar 2.1

Setelah `/etc/bind` sudah terbuka di node Yudhistira, langkah selanjutnya membuat sebuah folder yaitu **arjuna.b13** dan kemudian membuat sebuah konfigurasi sebagai berikut : 

File : `/etc/bind/arjuna.b13`

```
;
; BIND data file for local loopback interface
;
$TTL	604800
@   	IN  	SOA 	b13.com. root.b13.com. (
                    	2023091001  	; Serial
                     	604800     	; Refresh
                      	86400     	; Retry
                    	2419200     	; Expire
                     	604800 )   	; Negative Cache TTL
;
@   	IN  	NS  	arjuna.b13.com.
@   	IN  	A   	10.13.4.2   	; IP Arjuna
@   	IN  	AAAA	::1
```
Dan kita tambahkan pada file : `/etc/bind/named.conf.local `

sebagai berikut :
```
zone "arjuna.b13.com" {
    	type master;
    	file "/etc/bind/arjuna.b13/arjuna.b13.com";
};
```
Kemudian kita ganti salah satu nameserver pada node **Sadewa** untuk melakukan testing DNS tersebut.

```
nameserver 10.13.1.2
```
dan lakukan test pada DNS tersebut dengan cara ping **arjuna.b13.com** dan didapatkan hasil sebagai berikut : 

gambar 2.2

Untuk menambahkan alias www.arjuna.b13.com maka kita harus menambahkan alias pada `/etc/bind/arjuna.b13/arjuna.b13.com` sebagai berikut : 

File : `/etc/bind/arjuna.b13/arjuna.b13.com`

```
;
; BIND data file for local loopback interface
;
$TTL	604800
@   	IN  	SOA 	b13.com. root.b13.com. (
                    	2023091001  	; Serial
                     	604800     	; Refresh
                      	86400     	; Retry
                    	2419200     	; Expire
                     	604800 )   	; Negative Cache TTL
;
@   	IN  	NS  	arjuna.b13.com.
@   	IN  	A   	10.13.4.2   	; IP Arjuna
www   IN   CNAME arjuna.b13.com.
@   	IN  	AAAA	::1
```
## Hasil

Sehingga apabila kita ping yang kedua kalinya akan menampilkan hasil sebagai berikut : 

gambar 2.3

Yang artinya konfigurasi DNS kita sudah berhasil dan siap untuk digunakan.

# Soal No 3
## Deskripsi

Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke **abimanyu.yyy.com** dan alias **www.abimanyu.yyy.com.**
## Solusi
Sama seperti langkah langkah sebelumnya maka akan jadi sebuah configuration file seperti berikut : 

File : `/etc/bind/abimanyu.b13/abimanyu.b13.com`

```
;
; BIND data file for local loopback interface
;
$TTL	604800
@   	IN  	SOA 	b13.com. root.b13.com. (
                    	2023091001  	; Serial
                     	604800     	; Refresh
                      	86400     	; Retry
                    	2419200     	; Expire
                     	604800 )   	; Negative Cache TTL
;
@   	IN  	NS  	abimanyu.b13.com.
@   	IN  	A   	10.13.4.4   	; IP Abimanyu
www   IN   CNAME abimanyu.b13.com.
@   	IN  	AAAA	::1
```

Dan kita tambahkan pada file : `/etc/bind/named.conf.local` sebagai berikut : 

```

zone "arjuna.b13.com" {
    	type master;
    	file "/etc/bind/arjuna.b13/arjuna.b13.com";
};

zone "abimanyu.b13.com" {
    	type master;
    	file "/etc/bind/abimanyu.b13/abimanyu.b13.com";
};
```
## Hasil
Apabila kita coba ping lagi maka akan menampilkan respond seperti berikut :

gambar 3.1

# Soal No 4
## Deskripsi
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

## Solusi
Untuk membuat sebuah subdomain maka yang perlu kita butuhkan adalah menambahkan sebuah identitas pada DNS Server (Node **Yudhisthira**), tambahan tersebut adalah :

```
;
; BIND data file for local loopback interface
;
$TTL	604800
@   	IN  	SOA 	b13.com. root.b13.com. (
                    	2023091001  	; Serial
                     	604800     	; Refresh
                      	86400     	; Retry
                    	2419200     	; Expire
                     	604800 )   	; Negative Cache TTL
;
@   	IN  	NS  	abimanyu.b13.com.
@   	IN  	A   	10.13.4.4   	; IP Abimanyu
www 	IN  	CNAME   abimanyu.b13.com.
parikesit IN	A   	10.13.4.4   	; IP Abimanyu
@   	IN  	AAAA	::1
```
## Hasil

Yang apabilah kita test dengan melakukan ping ke parikesit.abimanyu.b13.com yang hasilnya sebagai berikut :

Gambar 4.1

Bisa kita lihat ping diatas menghasilkan sebuah respond ip ..4.4 yaitu IP node Abimanyu, yang artinya sambungan tersebut sudah berhasil.

# Soal No 5
## Deskripsi
Buat juga reverse domain untuk domain utama.

## Solusi
Untuk membuat reverse domain tersebut kita bisa pergi ke file `/etc/bind/named.conf` dan menambahkan script sebagai berikut :
```
zone "4.13.10.in-addr.arpa" {
    	type master;
    	file "/etc/bind/4.13.10.in-addr/4.13.10.in-addr.arpa";
};
```
Save dan kita buat sebuah file sebagai tempat penampungan reverse domain dengan nama folder `4.13.10.in-addr` tambahkan pada file sesuai dengan file diatas yaitu :

gambar 5.1

Kemudian kita bisa restart dan check apakah konfigurasi diatas sudah benar dengan cara tulis command di bawah ini pada node **Sadewa**: 

gambar 5.2

gambar 5.3

## Hasil 
Dari hasil diatas kita bisa melihat bahwa masing masing ip sudah memberikan respond domain masing yang artinya settingan reverse domain sudah berhasil terpasang sempurna.

# Soal No 6
## Deskripsi
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

## Solusi
Untuk menambahkan DNS Slave pada server **Werkudara** kita perlu mengatur node pada Yudhistira untuk mengarahkan ke IP Yudhistira apabila terdapat kegagalan dengan cara edit `conf /etc/bind/named.conf.local` dengan menambahkan script sebagai berikut :

```
zone "arjuna.b13.com" {
    	type master;
    	notify yes;
    	also-notify { 10.13.2.2; } // IP Werkudara
    	allow-transfer { 10.13.2.2; } // IP Werkudara
    	file "/etc/bind/arjuna.b13/arjuna.b13.com";
};

zone "abimanyu.b13.com" {
    	type master;
    	also-notify { 10.13.2.2; } // IP Werkudara
    	alow-transfer { 10.13.2.2; } // IP Werkudara
    	file "/etc/bind/abimanyu.b13/abimanyu.b13.com";
};

zone "4.13.10.in-addr.arpa" {
    	type master;
    	file "/etc/bind/4.13.10.in-addr/4.13.10.in-addr.arpa";
};
```
Save dan restart bind9, kemudian setting node **Werkudara** pada file `/etc/bind/named.conf.local` menjadi sebagai berikut :
```
zone "arjuna.b13.com" {
    	type slave;
    	masters { 10.13.1.2; }; // IP Yudhistira
    	file "/var/lib/arjuna.b13.com";
};

zone "abimanyu.b13.com" {
    	type slave;
    	masters { 10.13.1.2; }; // IP Yudhistira
    	file "/var/lib/abimanyu.b13.com";
};
```
Kemudian restart bind9 pada node **Werkudara**, dan kita lakukan percobaan pada Node **Sadewa**, namun pertama-tama kita perlu menambahkan nameserver dari IP **Werkudara** sehingga akan tampil sebagai berikut :

File : `/etc/resolv.conf`
```
nameserver 10.13.2.2
nameserver 10.13.1.2
```
## Hasil

Setelah itu matikan bind dari server Yudhistira dan lakukan percobaan pada node Sadewa dengan cara ping arjuna.b13.com dan menampilkan hasil sebagai berikut:


Gambar 6.1

Dari hasil diatas kita bisa simpulkan bahwa dns slave sudah bekerja dengan baik.

# Soal No 7
## Deskripsi
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu **baratayuda.abimanyu.yyy.com** dengan alias **www.baratayuda.abimanyu.yyy.com** yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

## Solusi
Untuk membuat subdomain tersebut kita bisa pergi ke server Yudhistira  dan pergi ko directory `/etc/bind/abimanyu.b13` kemudian tambahkan script berikut :

```
;
; BIND data file for local loopback interface
;
$TTL     604800
@   	IN  	SOA 	b13.com. root.b13.com. (
                    	2023091001  	; Serial
                     	604800     	; Refresh
                      86400     	      ; Retry
                    	2419200     	; Expire
                     	604800 )   	; Negative Cache TTL
;
@           	IN  	NS  	abimanyu.b13.com.
@           	IN  	A   	10.13.4.4   	; IP Abimanyu
www         	IN  	CNAME   abimanyu.b13.com.
parikesit   	IN  	A   	10.13.4.4   	; IP Abimanyu
ns1         	IN  	A   	10.13.2.2   	; IP Werkudara
baratayuda  	IN  	NS  	ns1
@   	IN  	AAAA	::1
```
File diatas bertugas untuk mendelegasikan ke IP **Werkudara** save dan edit file pada folder `/etc/bind/named.conf.options` Kemudian comment dnssec-validation auto; dan tambahkan baris berikut pada `/etc/bind/named.conf.options`
```
allow-query{any;};
```
Langkah selanjutnya adalah restart server dan atur kembali configuration pada Node **Werkudara** dengan cara ubah named.conf.local menjadi seperti berikut : 
```
zone "arjuna.b13.com" {
    	type slave;
    	masters { 10.13.1.2; }; // IP Yudhistira
    	file "/var/lib/arjuna.b13.com";
};

zone "abimanyu.b13.com" {
    	type slave;
    	masters { 10.13.1.2; }; // IP Yudhistira
    	file "/var/lib/abimanyu.b13.com";
};

zone "baratayuda.abimanyu.b13.com" {
    	type master;
    	file "/etc/bind/baratayuda/baratayuda.abimanyu.b13.com";
};

```
Setelah itu kita bisa bikin sebuah folder pada **/etc/bind/** dengan nama folder delegasi dan kita bikin sebuah file dengan nama **baratayuda.abimanyu.b13.com** yang selanjutnya berisikan konfigurasi sebagai berikut :
```
;
; BIND data file for local loopback interface
;
$TTL	604800
@   	IN  	SOA 	abimanyu.b13.com. root.abimanyu.b13.com. (
                    	2023101001  	; Serial
                     	604800     	; Refresh
                      	86400     	; Retry
                    	2419200     	; Expire
                     	604800 )   	; Negative Cache TTL
;
@   	IN  	NS  	baratayuda.abimanyu.b13.com.
@   	IN  	A   	10.13.4.4   	; IP Abimanyu
www 	IN  	CNAME   baratayuda.abimanyu.b13.com.
```
Setelah itu kita bisa lakukan uji coba dengan melakukan ping pada domain tersebut dari Node Sadewa dan mendapatkan hasil sebagai berikut :

gambar 7.1

Dan selesai dari kedua ping diatas mendapatkan respond dan tidak ada yang gagal artinya soal ini berhasil terjawab.















