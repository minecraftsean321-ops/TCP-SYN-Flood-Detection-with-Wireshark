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
-Instalasi nmap
<img width="479" height="133" alt="Screenshot 2026-09-12 060519" src="https://github.com/user-attachments/assets/dafe4879-be6d-4ff6-bdd0-7f707ed7e40c" />

-Simulai serangan menggunakan nmap
<img width="497" height="340" alt="Screenshot 2026-09-12 061353" src="https://github.com/user-attachments/assets/dea83edf-4f0f-4034-ae9c-1e9555f5731d" />

# Analisis trafik dengan wireshark

- Menggunakan filter untuk melihat SYN tanpa ACK

`    
tcp.flags.syn == 1 and tcp.flags.ack == 0
`

<img width="960" height="540" alt="Screenshot 2026-09-12 061452" src="https://github.com/user-attachments/assets/ea7b0b50-a637-4f94-ac5f-52a3c89912ba" />

- Menggunakan filter berikut untuk melihat SYN/ACK:

  `
  tcp.flags.syn == 1 and tcp.flags.ack == 1
  `

  <img width="960" height="540" alt="Screenshot 2026-09-12 061510" src="https://github.com/user-attachments/assets/9f6b4223-700d-4b80-b06f-ace7ba6583b9" />




