# BELAJAR DOCKER 

logo ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")

*Docker adalah sebuah aplikasi yang bersifat open source yang berfungsi sebagai wadah/container untuk mengepak/memasukkan sebuah software secara lengkap beserta semua hal lainnya yang dibutuhkan oleh software tersebut dapat berfungsi.*

## Menjalankan Container Docker 
Hal yang pertama dilakukan adalah mengidentifikasi nama Docker Image yang dikonfigurasi untuk menjalankan Redis.
* Perintah untuk menemukan Image Redis :
	~~~
	docker search redis
	~~~
	Hasil :
	
	2 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
	3 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
	4 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
	
* Perintah run => memulai kontainer berdasarkan Image Docker
	~~~
	docker run -d redis
	~~~
	Docker akan menjalankan perintah di latar depan. Untuk berjalan di latar belakang, opsi -d perlu ditentukan. Secara default, Docker akan menjalankan versi terbaru yang tersedia. Hasil dari perintah tersebut adalah :
	5 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
	
* Menemukan Running Containers
	~~~
	docker ps
	~~~	
	Perintah ini akan menampilkan nama dan ID  yang dapat digunakan untuk mencari informasi tentang masing-masing kontainer.
	Hasil :
	6 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")

* Mengakses Redis 
	Perintah untuk menjalankan Redis di latar belakang, dengan nama redisHostPort pada port 6379 :
	~~~
	docker run -d --name redisHostPort -p 6379:6379 redis:latest
	~~~
	Secara default, port pada host dipetakan ke 0.0.0.0, yang berarti semua alamat IP. Kita dapat menentukan alamat IP tertentu ketika akan menentukan pemetaan port, misalnya, -p 127.0.0.1:6379:6379
	7 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
* Mengekspos Redis  pada port yang tersedia secara acak
	~~~
	docker run -d --name redisDynamic -p 6379 redis:latest
	~~~
	8 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
* Menemukan port yang ditugaskan 
	~~~
	docker port redisDynamic 6379
	~~~
	9 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
* Daftar kontainer menampilkan informasi pemetaan port dengan perintah :
	~~~
	docker ps
	~~~
	10 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")

* Persisting Data 
	==> mengubah Container tanpa kehilangan data , data disimpan di Host Docker 
	~~~
	docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis
	~~~
	11 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
* Menjalankan Container Di Foreground
	~~~
	docker run ubuntu ps
	~~~
	meluncurkan sebuah wadah Ubuntu dan mengeksekusi perintah ps untuk melihat semua proses yang berjalan dalam sebuah container.
	12 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
* Mendapatkan akses ke shell bash di dalam wadah.
	~~~
	docker run -it ubuntu bash
	~~~
	13 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
## Menerapkan Situs Web Statis HTML sebagai Container
* Membuat Dockerfile 
	Dockerfile adalah daftar instruksi yang menjelaskan cara menyebarkan aplikasi 
	~~~
	FROM nginx:alpine (mendefinisikan image)
	COPY . /usr/share/nginx/html (menyalin isi direktori saat ini ke lokasi tertentu)
	~~~
* Membangun Docker Image 
	Perintah build menjalankan setiap instruksi dalam Dockerfile. Hasilnya adalah Docker Image yang dibangun yang dapat diluncurkan dan menjalankan aplikasi  yang telah dikonfigurasi.
	~~~
	docker build -t webserver-image:v1 
	~~~
	14 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
	Melihat daftar semua images di host 
	~~~
	docker images
	~~~
	15 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
* Run 
	Karena ini adalah server web, bind port 80 ke host maka menggunakan parameter -p.
	~~~
	docker run -d -p 80:80 webserver-image:v1
	~~~
	16 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
	Untuk dapat mengakses hasil port 80 bisa menggunakan perintah berikut :
	~~~
	curl docker 
	~~~
	17 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
## Membuat Container Images

* Base Images 
	Base Images adalah images yang sama dari Docker Registry yang digunakan untuk memulai kontainer digunakan sebagai dasar untuk perubahan tambahan untuk menjalankan aplikasi 
	~~~
	FROM nginx:1.11-alpine
	~~~
	
* Menjalankan Perintah 
	Dengan mendefinisikan base images , kita perlu menjalankan berbagai perintah untuk mengkonfigurasi images. Ada banyak perintah untuk membantu dengan ini, perintah utama dua adalah COPY dan RUN.
	RUN <command> memungkinkan untuk menjalankan perintah apa pun seperti yang dilakukan pada prompt perintah, misalnya menginstal paket aplikasi yang berbeda atau menjalankan perintah build. Hasil RUN dipertahankan ke images sehingga penting untuk tidak meninggalkan file yang tidak perlu atau sementara pada disk karena ini akan dimasukkan dalam images.
	COPY <src> <dest> memungkinkan  untuk menyalin file dari direktori yang berisi Dockerfile ke images container. 
	~~~
	COPY index.html /usr/share/nginx/html
	~~~
* Exposing Ports
	Menggunakan perintah EXPOSE <port> kita dapat memberi tahu Docker port mana yang harus dibuka. selain itu juga dapat menentukan beberapa port pada satu perintah, misalnya, EXPOSE 80 433 atau EXPOSE 7000-8000
	~~~
	EXPOSE 80
	~~~
	perintah tersebut digunakan agar server web dapat diakses pada port 80 
* Perintah Default 
	Perintah untuk menjalankan NGINX adalah nginx -g daemon off. Dalam contoh ini, NGINX akan menjadi titik masuk dengan -g daemon off; default command .
	~~~
	CMD ["nginx", "-g", "daemon off;"]
	~~~
* Membangun Container
 Perintah build mengambil direktori yang berisi Dockerfile, menjalankan langkah-langkah dan menyimpan images di Docker Engine lokal 
 Perintah untuk melihat daftar gambar di mesin lokal :
 ~~~
 docker images
 ~~~
 18 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
 ~~~
 docker build -t my-nginx-image:latest .
 ~~~
 19 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")
* Peluncuran Images Baru 
~~~
 curl -i http://docker
~~~

~~~
docker ps
~~~
20 ![alt text](https://github.com/EnjangDwiKartini/tct/blob/master/images/IMG_20180913_151527.jpg "Enjang DK")

	