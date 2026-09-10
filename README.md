# TCP-SYN-Flood-Detection-with-Wireshark
Simulating a TCP SYN flood and detecting it using Wireshark.

# Pembukaan 
## TCP three-way handshake  
The TCP 3-Way Handshake adalah proses yang dilakukan oleh Transmision Control Protocol (TCP) untuk membabangun koneksi yang dapat diandalkan antara sebuah client dan sebuah server sebelum transmisi data dimulai.

Proses:
Step 1 (SYN): Di tahap pertama client menoba membangun koneksi dengan server, dengan mengirimkan sebuah segment SYN(Syncronize Sequence Number) yang menginformasikan ke server bahwa client ingin mencoba berkomunikasi dan dengan nomor urut berapa segment dimulai. 

Step 2(SYN + ACK): Server kemudian merespon request dari client menggunakan sinyal bit SYNC-ACK yang diatur. Acknowledgmend (Ack) menandakan respon dari segmen yang diterima dan (SYN) menandakan dengan nomor sequence berapa untuk kemungkinan segment dimulai.

Step 3 (ACK) : Di bagian akhir client menerima respons dari server dan keduanya berhasil membangun koneksi yang andal dimana mereka mulai beneran melakukan pengiriman data.

## SYN Flood
Definition: adalah tipe serangan denial-of-service yang membuat server kewalahan dengan mengirimkan banyak request koneksi dan meninggalkannya dengan tidak terselesaikan. 

# Setup Lingkungan Lab
- Attacker: Kali linux
  <img width="1318" height="874" alt="image" src="https://github.com/user-attachments/assets/e9a93641-7ff4-4627-bd7c-60f87893de10" />
- Victim: Windows

# Simulasi Serangan TCP SYN Flood
-Instalasi hping3
<img width="1001" height="682" alt="image" src="https://github.com/user-attachments/assets/e1360708-4e48-40fc-9ab0-a14d3b805e38" />
