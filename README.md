CRITICAL Vulnerabillity at Partai Golkar .
Hallo sekian lama tidak membuat Write Up mengenai Security Website pada tulisan kali ini saya ingin membuat Write Up mengenai SQL Injection pada situs Partai Golkar .
Mengapa saya melakukan Penetration Testing pada situs Partai Politik ?
Karena sekarang lagi sangat panas-panasnya Politik di Negara Indonesia dan banyak Pro dan Kontra , Disini saya bantu untuk membuat Website Partai Golkar lebih secure agar Nanti situs Partai Golkar tidak di hack dengan Script deface SARA atau Menjelek-jelekan nanti malah Indonesia makin ricuh .
Beberapa Bug yang saya temukan pada situs Partai Golkar antara lain SQL Injection dan FULL PATH Disclosure .
SQL Injection adalah sebuah teknik yang menyalahgunakan sebuah celah keamanan yang terjadi dalam lapisan basis data sebuah aplikasi.


FULL PATH DISCLOSURE Vulnerabillity adalah suatu Bug yang membuat attacker bisa melihat DOCUMENT_ROOT /home/$user/public_html/
Pada situs Partai Golkar saya tertuju pada menu ( E-LIBRARY > ANGGOTA EKSEKUTIF DAN LEGISLATIF ) karena pada bagian tersebut terdapat SearchBox yang di gunakan untuk mencari para Kader Partai Golkar .
Foto pada bagian /kader_kami atau E-LIBRARY > ANGGOTA EKSEKUTIF DAN LEGISLATIF .Saya sudah mempunyai Insting bahwa pada bagian Search tersebut memiliki kerentanan SQL Injection , Pertama saya meng-input suatu keyword normal berupa " Setya Novanto "
Output yang muncul ketika saya mengetik Keyword Setya Novanto .Baiklah saya sudah mengetahui jika kita meng-input suatu text normal … maka tidak terjadi apa apa , Namun ketika saya menambahkan Character ' di Akhir text yang ingin saya cari Responnya berbeda dari sebelumnya "Whoops, looks like something went wrong."
Respon dari Burpsuite ketika menambahkan Character ' di Akhir Text .Ketika saya memberi text seperti ini "setya+novanto' - -" maka Respon akan kembali Normal disitu saya sudah Anggap Bahwa Benar situs ini telah memiliki kerentanan SQL Injection , Dan sekarang saya mulai melakukan Inject dengan menggunakan teknik Union Based atau BASIC SQLI 
Ketika saya melakukan Injeksi .Pada angka 17 ketika kita melakukan order by itu Responnya 500 Internal Server Error berati saya menyimpulkan bahwa column berada di Angka 16 .
Respon ketika memasukan text "test' order by 17 - -" pada bagian Pencarian KaderDan benar saja ketika saya memberi union select di angka 16 maka Angka Angka ajaib akan muncul yaitu Angka ( 1,6,7,12,13,14 )
Angka Ajaibuser() yang digunakan situs Partai Golkar adalah root@localhost disini saya berinisiatif untuk melakukan INTO OUTFILE namun saya belum mengetahui DOCUMENT_ROOT pada situs Partai Golkar .
Untuk mencari Path Disclosure saya mencari pada bagian file PHP yang Error maka dari itu saya melakukan CTRL + U untuk melihat Directory Publik seperti /assets/ atau /img/ dan sebagainya .
Disini saya memilih Directory /assets/ karena pada Directory tersebut bersifat terbuka ( Index of /assets ) lalu saya tertuju pada Directory Backend/Global karena yang saya Tahu Global adalah theme website seperti Metronic yang dimana pada Directory Global biasanya ada Jquery-File-Uploadnya maka dari itu saya memilih Directory Global , Namun pada saat saya akses Global saya langsung menemui FULL PATH DISCLOSURE
FULL PATH DISCLOSURE pada bagian /assets/backend/global/Sebelum melakukan INTO OUTFILE saya baru menyadari pada bagian Respon Cookie yang di pakai web Tersebut adalah laravel_session artinya situs tersebut memakai Framework Laravel saya langsung mempunyai Inisiatif untuk membaca file .env ( File Configuration Laravel ) dari load_file('//') .
Saya melakukan load_file('/****/****/****/.env')AND BOOM ! Saya langsung mendapatkan file Configuration Partai Golkar dan saya Langsung melakukan Login menggunakan Putty 
Username : root@partaigolkar.or.id
Password : **************
Linux server-golkar 4.4.0–139-generic #165-Ubuntu SMP Wed Oct 24 10:58:50 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
Dan inisiatif saya untuk Melakukan INTO OUTFILE saya Batalkan karena saya sudah mendapatkan VPS dengan Roles Akses Tertinggi yaitu ROOT .
Penutup : Saya ucapkan terima kasih kepada semua orang yang telah membaca post ini dari Awal sampai Habis , dan saya meminta maaf jika ada kesalahan kata atau penjelasan dalam post ini .
Youtube Channel : BugBounty ID
Facebook : fb.com/putra1337
Kami membuka jasa Layanan Penetrasi Testing pada Website maupun Aplikasi , Jika minat silahkan Kontak email dibawah .
Contact for Bussiness : putracraft.theworld@gmail.com
