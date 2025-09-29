# Modul 1

## No.1
**SOAL:** Untuk mempersiapkan pembuatan entitas selain mereka, Eru yang berperan sebagai Router membuat dua Switch/Gateway . Dimana Switch 1 akan menuju ke dua Ainur yaitu Melkor dan Manwe. Sedangkan Switch 2 akan menuju ke dua Ainur lainnya yaitu Varda dan Ulmo. Keempat Ainur tersebut diberi perintah oleh Eru untuk menjadi Client.

**PENJELASAN:** Buatlah topologi LAN seperti gambar di bawah ini:

<img width="362" height="401" alt="image" src="https://github.com/user-attachments/assets/5c8cdd3b-6c12-45b3-8f07-c0ed594b4fee" />

## No.2
**SOAL:** Karena menurut Eru pada saat itu Arda (Bumi) masih terisolasi dengan dunia luar, maka buat agar Eru dapat tersambung ke internet.

**PENJELASAN:**  Hubungkan node Eru dengan internet dengan menghubungkannya terhadap sebuah node NAT dan menuliskan konfigurasi interfacenya. Interface yg dipilih adalah `eth0`.

<img width="72" height="174" alt="image" src="https://github.com/user-attachments/assets/e63b10cd-0a77-4100-afd0-cfa6a7eb7335" />

```
auto eth0
iface eth0 inet dhcp
```
_block 1: Konfigurasi interface `eth0` pada node Eru._

## No.3
**SOAL:** Sekarang pastikan agar setiap Ainur (Client) dapat terhubung satu sama lain.

**PENJELASAN:** Berikan alamat IP pada kedua interface yang akan menjadi gateway bagi client-client di bawahnya agar bisa saling mengirim dan menerima pesan antar client. Setting agar masing-masing client mendapatkan alamat IPnya masing-masing juga.

<img width="484" height="361" alt="image" src="https://github.com/user-attachments/assets/dc79a119-0850-4cf8-b6cc-17959a3c68e7" />

```
auto eth1
iface eth1 inet static
	address 192.240.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.240.2.1
	netmask 255.255.255.0
```
_block 2: Konfigurasi interface `eth1` dan `eth2` pada node Eru._


```
auto eth0
iface eth0 inet static
	address 192.240.1.2
	netmask 255.255.255.0
	gateway 192.240.1.1
```
_block 3: Konfigurasi interface `eth0` pada node Melkor._

## No.4
**SOAL:** Setelah berhasil terhubung, sekarang Eru ingin agar setiap Ainur (Client) dapat mandiri. Oleh karena itu pastikan agar setiap Client dapat tersambung ke internet

**PENJELASAN:**  Install `iptables` dan masukkan perintah konfigurasi ke `.bashrc`. Perintah `iptables` akan menyamarkan sumber IP pengirim (para client) dengan yang ada di perintah, lalu node Eru akan berlaku sebagai NAT bagi mereka dan menghubungkan masing-masing LAN dengan internet.

```
apt update -y && apt install iptables -y
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.240.0.0/16
```
_block 4: Konfigurasi `.bashrc` pada node Eru._
