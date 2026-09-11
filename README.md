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

- Melihat spike grafik dengan I/O graph
  
  <img width="640" height="502" alt="Screenshot 2026-09-12 061545" src="https://github.com/user-attachments/assets/fd5cbb94-7e97-4c43-a26d-d68df0d57342" />

- Membandingkan volume paket TCP menggunakan Protocol Hierarchy  

  <img width="597" height="406" alt="Screenshot 2026-09-12 061611" src="https://github.com/user-attachments/assets/b9d4ea88-f34d-4ed4-b3fc-1724fe46f5c1" />

# Diskusi dan interpretasi 

## Gejala SYN Flood berdasarkan hasil capture?
Berdasarkan hasil capture dari I/O graph gejala dari SYN Flood sebagai berikut:

1. Lonjakan trafik paket yang ekstrem: ada I/O Graph (sekitar detik ke-520–530), terjadi lonjakan lalu lintas paket data yang sangat tajam hingga melebihi 5.000–6.000 paket per detik (5–6 kpkts) secara mendadak.

2. Tingginya TCP error: Area merah yang sangat dominan pada lonjakan tersebut menunjukkan tingginya tingkat kesalahan/masalah koneksi TCP (TCP Errors), yang mengindikasikan koneksi menggantung (half-open connections) atau paket yang dropping.
   
## Mengapa jumlah SYN/ACK tetap sedikit meskipun jumlah SYN sangat banyak?  

Meskipun attacker membanjiri target dengan belasan ribu paket SYN, balasan SYN/ACK tetap sangat sedikit karena beberapa alasan teknis:

1. Penumpukan Syn-Queue / Buffer Overload: Ketika target/server menerima banjir permintaan SYN dalam kurun waktu sangat singkat, antrean koneksi setengah terbuka (SYN Backlog / Syn-Queue) milik server dengan cepat menjadi penuh (exhausted).

2. Server Menolak / Mengabaikan Paket Tambahan: Karena memori buffer antrean sudah penuh, sistem operasi server tidak dapat lagi memproses permintaan koneksi baru dan akan mulai drop/mengabaikan paket SYN pendatang baru tanpa mengirimkan SYN/ACK balasan.

3. Mekanisme Rate Limiting / Proteksi: Jika target atau perangkat jaringan (seperti firewall/router) memiliki fitur proteksi dasar, sistem akan membatasi (limit) jumlah balasan SYN/ACK untuk mencegah kehabisan sumber daya lebih lanjut.
   
## Apa dampak penggunaan IP spoofing terhadap proses deteksi dan mitigasi?  

Jika penyerang menggunakan teknik IP Spoofing (memalsukan IP sumber):

### Dampak pada Proses Deteksi:

- Sulit Mengidentifikasi Penyerang Asli: Deteksi menjadi rumit karena trafik tampak datang dari ribuan alamat IP acak yang berbeda (bahkan IP sah milik pengguna lain), sehingga sulit membedakan mana trafik legal dan mana trafik serangan.

- Log Terkontaminasi: Log sistem dan analisis statistik jaringan menjadi sangat liar dan tidak valid karena mencatat alamat-alamat IP palsu.

### Dampak pada Proses Mitigasi:

- Blokir IP Tradisional Menjadi Tidak Efektif: Mitigasi sederhana seperti melakukan pemblokiran IP (IP Blocking / Blacklisting) berdasarkan log akan sia-sia karena IP penyerang terus berubah-ubah.

- Risiko Denial of Service Tambahan (Backscatter Traffic): Server balasan (SYN/ACK) akan dikirimkan ke IP korban asli (pemilik IP palsu tersebut). Jika IP yang dipalsukan adalah IP internal/sah, hal itu bisa memicu masalah konektivitas bagi pengguna legitimasi.

- Memerlukan Mitigasi Tingkat Lanjut: Pengelola jaringan terpaksa harus menerapkan solusi yang lebih kompleks seperti SYN Cookies, Rate Limiting berbasis perilaku (behavioral analysis), atau proteksi DDoS dedicated di layer border/cloud.

