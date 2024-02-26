+++
title = 'Cara Menggunakan Tema Blankslate Wordpress'
date = 2024-02-26T20:53:42+07:00
draft = false

featured_image = 'img/post-thumbnail/blankslate-wordpress-theme.png'
description = 'Tutorial singkat cara membuat game mobil balap menggunakan vanila Javascript.'

toc = true
+++

*Assalamu'alaikum warahmatullahi wabarakatuh.*

Apa kabar semua. Pada kesempatan kali ini saya mau membagikan sedikit pengalaman dan tips 'n trick cara menggunakan tema Blankslate.

## Apa Itu Tema Blankslate?

[Blankslate](https://wordpress.org/themes/blankslate/) merupakan salah satu tema Wordpress yang dikembangkan oleh [Bryan Hadaway](https://wordpress.org/themes/author/bhadaway/).

Blankslate merupakan tema Wordpress yang masih plain atau blank atau raw, jadi tema ini masih polos, hanya ada plain html yang sudah di reset css.

## Bagaimana Cara Menggunakannya?

Berbeda dengan tema Wordpress yang lainnya, cara men-customize tema Blankslate tidak drag 'n drop seperti kebanyakan tema wordpress melainkan kita harus mengcustom css-nya secara manual atau bisa menggunakan framework css seperti [Bootstrap 5](https://getbootstrap.com/) atau [Tailwind CSS](https://tailwindcss.com).

Di dalam folder tema Blankslate `(folder-instalasi-wordpress/wp-content/themes/blankslate)` ada beberapa file .php yang digunakan sebagai komponen untuk membangun website kita, lalu ada file readme dan screenshot dari tema ini, dan ada juga file 'style.css' yang sudah berisi kode untuk reset css/normalize css.

FYI: Reset css merupakan sebuah teknik untuk menormalisasikan style elemen html untuk mempermudah dalam mendesain website.

## Hierarki Template Tema Wordpress

![Struktur Tema Wordpress](/img/blankslate-wordpress-theme/gambar-01.png)

Sauce: [https://developer.wordpress.org/themes/basics/template-hierarchy/](https://developer.wordpress.org/themes/basics/template-hierarchy/)

Di atas merupakan struktur hierarki template tema Wordpress, saya kurang paham detailnya, tapi intinya itu adalah panduan untuk menyusun file untuk template tema Wordpress.

Beberapa file yang perlu diperhatikan: `index.php`, `function.php`, `style.css` (opsional).

Jika kalian bingung untuk apa saja file .php tersebut, ya, sama, saya awalnya juga bingung 😁.

## Tips untuk Mempelajari Tema Blankslate (Versi Saya) Adalah:

Pertama, kalian buka file `function.php`-nya, dan lihat fungsi apa saja yang di buat oleh author tema ini.

Kedua, untuk memahami workflow dari tema ini kalian mulai dari file `index.php` (karena pada umumnya webserver akan mencari dan membuka file `index.php` terlebih dahulu, jadi file `index.php` adalah kunci xD).

Kalian buka file `index.php` lalu kalian lihat ada kode apa saja didalamnya.

Selain kode html ada beberapa kode php yang memanggil fungsi dengan prefix yang agak mencolok seperti `get_header()` misalnya.

![index.php](/img/blankslate-wordpress-theme/gambar-02.png)

Prefix `get_` biasanya digunakan untuk memanggil atau menginclude file dengan nama khusus. Contohnya seperti `get_header()` untuk menginclude file `header.php` dan `get_footer()` untuk menginclude file `footer.php`.

Sedangkan untuk `get_template_part()` biasanya digunakan untuk memanggil nama file yang kita buat sendiri seperti `get_template_part('entry')` untuk menginclude file `entry.php` dan `get_template_part('nav', 'bellow')` untuk menginclude file `nav-below.php` dan `nav-below-single.php` jika sedang berada di halaman single/konten.

Dan sisanya seperti yang ada pada blok kode `if` merupakan fungsi untuk statement, dan pastinya kalian sudah bisa menebak kegunaan fungsi tersebut 😁.

Sisanya kalian tinggal mengikuti alur/workflow dari tema ini, dari file `index.php` lalu include file `header.php`, lalu ada blok kode untuk melooping konten Wordpress, lalu include `entry.php` dan seterusnya.

Kalian bisa ubah kode didalamnya untuk mempermudah dalam memahami setiap alurnya.

![Preview Tema Blankslate](/img/blankslate-wordpress-theme/gambar-03.png)

Mengganti warna background misalnya.

---

Oke, jadi sekian tips dan review saya mengenai tema Wordpress yang satu ini.

CMIIW.

Mohon maaf jika ada salah kata dan kekurangan.

Oke, Sampai jumpa di postingan berikutnya!

## Source

Thumbnail Image: [https://wall.alphacoders.com/big.php?i=915272](https://wall.alphacoders.com/big.php?i=915272)