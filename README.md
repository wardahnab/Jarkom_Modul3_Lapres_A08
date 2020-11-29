
## Soal


Anri adalah seorang mahasiswa tingkat akhir yang sedang mengerjakan TA mengenai DHCP dan Proxy. Bu Meguri sebagai dosen pembimbing Anri memberikan tugas pertamanya, 

### DHCP
**No 1**

(1) yaitu untuk membuat topologi jaringan demi kelancaran TA-nya dengan kriteria sebagai berikut:Anri sudah pernah mempelajari teknik Jaringan Komputer sehingga Anri dapat membuat topologi tersebut dengan mudah. Bu Meguri memerintahkan Anri untuk menjadikan SURABAYA sebagai router, MALANG sebagai DNS Server, TUBAN sebagai DHCP server, serta MOJOKERTO sebagai Proxy server, dan UML lainnya sebagai client. Bu Meguri berpesan pada Anri untuk menyusun topologi secara hati-hati dan memperhatikan gambar topologi yang diberikan Bu Meguri. Karena TUBAN jauh dari client, maka perlu adanya perantara agar bisa saling terhubung. 


**Jawaban**


[!1](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/1.2.png)


topologi yang dibuat sebagai berikut :


# Switch

uml_switch -unix switch1 > /dev/null < /dev/null &

uml_switch -unix switch2 > /dev/null < /dev/null &

uml_switch -unix switch3 > /dev/null < /dev/null &

# Router

xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA et

h0=tuntap,,,'10.151.72.37' eth1=daemon,,,switch1 eth2=daemon,,,switch3 eth3=daemon,,,switch2 mem=256M &

# Server

xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=160M &

xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &

xterm -T TUBAN -e linux ubd0=TUBAN,jarkom umid=TUBAN eth0=daemon,,,switch2 mem=128M &

# Klien

xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=64M &

xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=64M &

xterm -T BANYUWANGI -e linux ubd0=BANYUWANGI,jarkom umid=BANYUWANGI eth0=daemon,,,switch3 mem=64M &

xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch3 mem=64M &

**No 2**

(2) SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client. Kriteria lain yang diminta Bu Meguri pada topologi jaringan tersebut adalah: Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis. 


**Jawaban**

kami tidak menggunakan relay, maka DHCP server berada di Surabaya. 
maka di Surabaya diinstall DHCP server dengan perintah `apt-get install isc-dhcp-server`

kemudian konfigurasi interface pada `/etc/default/isc-dhcp-server` sebagai berikut

![dhcp](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/2.1.png)


**No 3**

(3) Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.

**No 4**

(4) Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.

**No 5**

(5) Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP

**No 6**

(6) Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan (6) client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.


**jawaban**

untuk konfigurasi subnet 1 (client Gresik dan Sidoarjo)

![Subnet 1](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/3.1.png)
- no (4) ditunjukkan pada `range 192.168.1.50 192.168.1.70;` 
- no (5) ditunjukkan pada `option domain-name-servers 202.46.129.2,10.151.73.74;`
- no (6) ditunjukkan pada `default-lease-time 600;` `max-lease-time 600;`

setting pada client Gresik

![interface_1](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/inf_gresik.png)

hasil `ifconfig` pada Gresik

![ifconfig_1](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/ifc_gresik.png)


untuk konfigurasi subnet 3 (client Banyuwangi dan Madiun)

![Subnet 3](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/4.1.png)
- no (3) ditunjukkan pada `range 192.168.0.10 192.168.0.100;` `range 192.168.0.110 192.168.0.200;` 
- no (5) ditunjukkan pada `option domain-name-servers 202.46.129.2,10.151.73.74;`
- no (6) ditunjukkan pada `default-lease-time 300;` `max-lease-time 300;`

setting pada client Banyuwangi

![interface_3](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/inf_banyuwangi.png)

hasil `ifconfig` pada Banyuwangi

![ifconfig_3](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/ifc_banyuwangi.png)


### PROXY
**No 7**

Bu Meguri adalah dosbing yang suka overthinking. Ia tidak ingin jaringan lokalnya terhubung ke internet secara langsung. Sehingga ia memberi tugas tambahan pada Anri untuk membuatkan Proxy sebagai penghubung jaringan lokal ke internet. Ada beberapa ketentuan yang harus dipenuhi dalam pembuatan Proxy ini. Pertama, akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. 


(7) User autentikasi milik Anri memiliki format:

User : userta_yyy

Password : inipassw0rdta_yyy

Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01

Anri sudah menjadwal pengerjaan TA-nya


**Jawaban**

[!7](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/7.1.png)


[!7](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/7.2.png)


[!7](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/7.3.png)


**No 8**

(8) setiap hari Selasa-Rabu pukul 13.00-18.00. Bu Meguri membatasi penggunaan internet Anri hanya pada jadwal yang telah ditentukan itu saja. Maka diluar jam tersebut, Anri tidak dapat mengakses jaringan internet dengan proxy tersebut. Jadwal bimbingan dengan Bu Meguri adalah 


**Jawaban**


[!8](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/8.1.png)


[!8](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/8.2.png)


**No 9**

(9) setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00). Agar Anri bisa fokus mengerjakan TA, 


**Jawaban**


[!9](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/9.1.png)


[!9](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/9.2.png)


**No 10**

(10) setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA🙂.
Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri, 


**Jawaban**

menambahkan perintah di `/etc/squid3/squid.conf` pada Mojokerto yaitu sebagai berikut yang berarti mengarahkan ke web `monta.if.its.ac.id` apabila user mencoba membuka `google.com`

![ifconfig_3](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/10.1.png)


**No 11**

(11) Bu Meguri meminta Anri untuk mengubah error page default squid menjadi seperti berikut:

Note : File error page bisa diunduh dengan cara wget 10.151.36.202/ERR_ACCESS_DENIED

       Tidak perlu di extract, cukup cp -r



**Jawaban**

- perintah yang dilakukan antara lain mendownload file dengan perintah `wget 10.151.36.202/ERR_ACCESS_DENIED`
- kemudian merename file *ERR_ACCESS_DENIED* yang lama menjadi *ERR_ACCESS_DENIED_OLD* dnegan perintah `mv /usr/share/squid/errors/English/ERR_ACCESS_DENIED usr/share/squid/errors/English/ERR_ACCESS_DENIED_OLD`
- lalu mengcopy file hasil download ke tempat *ERR_ACCESS_DENIED* dengan perintah `cp -r ERR_ACCESS_DENIED /usr/share/squid/errors/English/ERR_ACCESS_DENIED`


hasil operasi

![11.1](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/11.2.png)

isi file error

![11.2](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/11.3.png)

namun karena penjadwalan tidak berhasil, maka error page tidak bisa ditest.

source : https://www.linuxquestions.org/questions/linux-server-73/change-squid-error-page-763114/

**No 12**

(12) Karena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080. 

Keterangan : yyy adalah nama kelompok masing-masing. Contoh: janganlupa-ta.c01.pw


**Jawaban**

membuat konfigurasi di Malang sebagai DNS, yaitu pada file `/etc/bind/named.conf.local`

![12.1](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/12.1.png)

lalu konfigurasi pada file `/etc/bind/jarkom/janganlupa-ta.a08.pw`

![12.2](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/12.2.png)

kemudian apabila disetting proxy pada browser

![12.3](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/12.3.png)

maka akan muncul login seperti kalau mengakses dengan proxy IP Mojokerto `10.151.73.75`

![12.2](https://github.com/wardahnab/Jarkom_Modul3_Lapres_A08/blob/main/jarkom%20soal%20shift%202/12.4.png)




