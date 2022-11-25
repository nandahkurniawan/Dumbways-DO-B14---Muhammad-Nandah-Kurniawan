1. Web Server

Web Server adalah sebuah software yang memberikan layanan berupa data. Berfungsi untuk menerima permintaan HTTP atau HTTPS dari client atau di kenal dengan web browser (chrome atau firefox). Kemudian web server akan mengirimkan respon atas permintaan tersebut dalam bentuk halaman web.


2. Jalankan 2 VM
Disini saya akan menginstall 2 virtual machine dengan menggunakan multipass yang masing2 saya beri nama VM1 dan VM2

VM1
pertama yaitu dengan ketik (untuk menginstall sebuah VM dengan berbasis ubuntu 20.04 dan nama VMnya
multipass launch 20.04 --name VM1

lalu untuk melaunchnya ketik
multipass shell VM1
![2022-11-25 08 16 16](https://user-images.githubusercontent.com/108359083/203883770-0d941e76-1597-4da7-adf6-d50050933cd6.png)

VM2
pertama yaitu dengan ketik (untuk menginstall sebuah VM dengan berbasis ubuntu 20.04 dan nama VMnya
multipass launch 20.04 --name VM2

lalu untuk melaunchnya ketik
multipass shell VM2
![2022-11-25 08 42 22](https://user-images.githubusercontent.com/108359083/203883716-de9bb55e-ea72-44bf-85bc-016dd39b51a8.png)


3. Menjalankan aplikasi dumbflix-frontend dengan gunakan PM2 (VM 2)
- appserver ( dumbflix frontend ) 
Langkah pertama yaitu, terlebih dahulu kita update dan upgrade server, dengan memberikan "-y" maka proses update dan upgrade akan menskip pertanyaan yes or no dengan otomatis langsung mengexecute yes
sudo apt update -y; sudo apt upgrade -y
![2022-11-25 06 49 08](https://user-images.githubusercontent.com/108359083/203879511-653cb3eb-3b7a-4737-8482-44bb3c0a3295.png)

Lalu buat BASH script untuk menginstall node agar proses menjadi lebih cepat
![2022-11-25 07 31 29](https://user-images.githubusercontent.com/108359083/203879584-afd35c9d-2d0a-4d9c-8808-1532e614bdcd.png)

Lalu ketik script BASH pada install_node.sh
![2022-11-25 07 31 16](https://user-images.githubusercontent.com/108359083/203879633-33ecfaaf-4e5a-40fd-8e1a-fe22f72b8923.png)

Selanjutnya, jangan lupa untuk memberikan akses terhadap file install_node.sh
![2022-11-25 07 32 06](https://user-images.githubusercontent.com/108359083/203879811-1fb9c5c5-48b0-430d-add6-7d24b6b86823.png)

Jalankan script BASH
install_node.sh
![2022-11-25 07 39 48](https://user-images.githubusercontent.com/108359083/203879905-ed6cff76-71c5-4b8f-b4e7-02883ad79f34.png)

Selanjutnya clone dumbflix frontend pada github dengan link 
git clone https://github.com/dumbwaysdev/dumbflix-frontend
![2022-11-25 07 44 10](https://user-images.githubusercontent.com/108359083/203880317-7b34027c-601f-4f33-a338-0a4cd0ae96f7.png)

selanjutnya masuk kedalam directory dumbflix-frontend
cd dumbflix-frontend

install npm pada directory dumbflix-frontend
npm install
![2022-11-25 08 04 03](https://user-images.githubusercontent.com/108359083/203880514-c0bdcc09-00f4-4f10-af44-8063ebcffffc.png)

jalankan dengan pm2
![2022-11-25 08 07 57](https://user-images.githubusercontent.com/108359083/203881849-e42471f6-2697-490f-8542-2df111f86397.png)
![2022-11-25 08 06 58](https://user-images.githubusercontent.com/108359083/203881900-cb118a24-8bb8-4217-b10c-8596f7fe426b.png)

lalu selanjutnya beri akses pada port 3000, pertama cek list firewall
sudo ufw app list

lalu cek status firewall untuk mengetahui port mana saja yang sudah di beri akses
sudo ufw status

cek status firewall active atau inactive, lalu jika inactive, enable firewall dengan
sudo ufw enable

lalu beri akses pada port 3000
sudo ufw allow 3000

lalu cek kembali apakah port 3000 sudah di beri akses
sudo ufw status

untuk melihat IP kita yaitu
hostname -I

![2022-11-25 08 10 13](https://user-images.githubusercontent.com/108359083/203881916-ae2749f6-dabe-4748-9aa1-6fe74b4a7d32.png)

lalu kita cek ip 10.0.2.15:3000 melalui terminal , dan ini hasilnya, sudah berhasil terhubung
![2022-11-25 09 00 32](https://user-images.githubusercontent.com/108359083/203885523-30f62b88-beef-48fd-b146-c75d9134683a.png)


4. VM1 : jalankan nginx
- nginx
Langkah pertama yaitu install engine nginx dengan
sudo apt install nginx -y

lalu enable nginx
sudo systemctl enable nginx

lalu start nginx
sudo systemctl start nginx

lalu cek status nginx
sudo systemctl status nginx
![2022-11-25 08 33 19](https://user-images.githubusercontent.com/108359083/203882997-9fec4ac2-3f9c-4903-8121-978000a9fbc9.png)

lalu kita cek ip 10.0.2.15 melalui terminal , dan ini hasilnya, sudah berhasil terhubung
![2022-11-25 08 53 29](https://user-images.githubusercontent.com/108359083/203885107-8dce65da-1dc2-48f5-ad15-8a2bfd34223f.png)

- buat konfigurasi reverse proxy

![2022-11-25 09 25 53](https://user-images.githubusercontent.com/108359083/203915641-fafba9d1-9951-4062-92a3-ec60ebe4edfa.png)

![2022-11-25 09 26 39](https://user-images.githubusercontent.com/108359083/203915657-dbe83e32-02cc-4d1d-8770-12336fe2f8ed.png)

![2022-11-25 09 27 54A](https://user-images.githubusercontent.com/108359083/203915851-3309bc19-ccb2-476a-baf6-19e875d9253e.png)


![2022-11-25 09 27 54](https://user-images.githubusercontent.com/108359083/203915677-147327e2-ac4c-4c61-aa66-9d1a99ae110a.png)

![2022-11-25 09 28 49](https://user-images.githubusercontent.com/108359083/203915875-fcc3d1fe-1a14-4ec1-b450-a37c13a9a38f.png)



- buat konfigurasi load balance antara VM1 dan VM2


5. Mengakses domain dengan nama sendiri
![2022-11-25 09 50 53](https://user-images.githubusercontent.com/108359083/203915220-dd83064f-67cb-4bfe-9c6e-38cc69aad3ae.png)

Challenge
1. Load Balancer berjalan dengan baik

