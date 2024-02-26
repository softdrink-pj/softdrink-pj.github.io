+++
title = 'Membuat Game Mobil Balap Javascript'
date = 2024-02-02T17:33:40+07:00
draft = false

featured_image = 'img/post-thumbnail/default.png'
description = 'Tutorial singkat cara membuat game mobil balap menggunakan vanila Javascript.'

toc = true
+++

*Assalamu'alaikum warahmatullahi wabarakatuh.*

Apa kabar semua. Pada kesempatan kali ini saya mau membagikan tutorial cara membuat game mobil balap sederhana menggunakan canvas javascript.

Sebelum memulai tutorial, siapkan beberapa tools yang akan digunakan seperti code editor, web browser, dan web server (opsional).

Buat folder project untuk menampung file dan assets dari game yang akan kita buat. Nama folder bebas. Disarankan disimpan didalam folder `htdocs` (Windows) atau `/var/www/html` (Linux).

## Langkah Pertama: Membuat Canvas

Langkah pertama yaitu membuat canvas sebagai media dari game yang akan kita buat.

Buat file `index.html`, `style.css`, dan `script.js` lalu tulis kode seperti dibawah.

`index.html`

``` index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Game Canvas Javascript</title>
    <link rel="stylesheet" href="style.css">
</head>
<body onload="startGame()">

    <div class="container">
        <canvas id="canvas"></canvas>
    </div>

    <script src="script.js"></script>
</body>
</html>
```

`style.css`

``` style.css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background-color: #1e1e1e;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.container {
    width: 640px;
    height: 360px;
    position: relative;
}

canvas {
    background-color: white;
}
```

`script.js`

``` script.js
function startGame() {
    myGameArea.start()
}

let myGameArea = {
    canvas: document.getElementById('canvas'),
    start: function() {
        this.canvas.width = 640
        this.canvas.height = 360
        this.context = this.canvas.getContext('2d')
    }
}
```

Kurang lebih seperti ini hasilnya.

![Screenshot 1](/img/game-mobil-balap-javascript/gambar-1.png)

## Langkah Kedua: Membuat Game Component

Langkah kedua, kita akan membuat komponen yang dibutuhkan dalam game.

Pada file `script.js`, kita akan membuat object constructor dengan nama `component`.

``` script.js
// dibawah function startGame()

function component(width, height, color, x, y) {
    this.width = width
    this.height = height
    this.x = x
    this.y = y

    ctx = myGameArea.context
    ctx.fillStyle = color
    ctx.fillRect(this.x, this.y, this.width, this.height)
}
```

Lalu kita coba buat sebuah komponen kedalam `canvas`.

Deklarasikan nama variable komponen yang akan kita buat.

Tambahkan kode dibawah ini dibagian paling atas dari file `script.js`.

``` script.js
let myCar
let myRoad
```

Lalu ubah kode `function startGame()` menjadi seperti berikut.

``` script.js
function startGame() {
    myGameArea.start()

    myRoad = new component(216, 360, 'gray', 216, 0)
    myCar = new component(40, 72, 'red', 277, 280)
}
```

Kurang lebih seperti ini hasilnya.

![Screenshot 2](/img/game-mobil-balap-javascript/gambar-2.png)

Anggap saja kotak merah tersebut adalah mobil yang akan kita gunakan dalam game 😅.

## Langkah Ketiga: Mencoba Membuat Komponen Bergerak

Pada langkah ketiga, kita akan mencoba membuat game kita berjalan.

Buka file `script.js`.

Buat fungsi baru dengan nama `updateGameArea()`.

``` script.js
// dibawah function component()

function updateGameArea() {
    myGameArea.clear()

    myRoad.update()
    myCar.update()
}
```

Lalu ubah `myGameArea` menjadi seperti berikut.

``` script.js
let myGameArea = {
    canvas: document.getElementById('canvas'),
    start: function() {
        this.canvas.width = 640
        this.canvas.height = 360
        this.context = this.canvas.getContext('2d')
        this.interval = setInterval(updateGameArea, 20)
    },
    clear: function() {
        this.context.clearRect(0, 0, this.canvas.width, this.canvas.height)
    }
}
```

Dan ubah `component()` menjadi seperti berikut.

``` script.js
function component(width, height, color, x, y) {
    this.width = width
    this.height = height
    this.x = x
    this.y = y

    this.update = function() {
        ctx = myGameArea.context
        ctx.fillStyle = color
        ctx.fillRect(this.x, this.y, this.width, this.height)
    }
}
```

**Penjelasan:**

Jadi, kita membuat fungsi `updateGameArea()` untuk merefresh canvas kita setiap 20ms.

Clear canvas menggunakan fungsi `myGameArea.clear()`.

Lalu menggambarnya kembali menggunakan fungsi `.update()` yang ada pada `component()`.

Untuk mengetes apakah game kita sudah jalan, coba tambahkan kode berikut kedalam fungsi updateGameArea() menjadi seperti berikut.

``` script.js
function updateGameArea() {
    myGameArea.clear()

    myCar.y--

    myRoad.update()
    myCar.update()
}
```

Jika mobil merah berhasil bergerak ke atas, berarti game kita sudah jalan.

## Langkah Keempat: Membuat Controller

Pada langkah keempat, kita akan membuat controller untuk mengatur pergerakan *playable character* kita yang mana disini kita menggunakan komponen `myCar` sebagai *playable character* kita.

Kita akan menggerakan komponen `myCar` menggunakan keyboard `a` dan `d` untuk steer.

Dan untuk gasnya kita buat otomatis.

Buka kembali file `script.js`.

Tambahkan kode berikut kedalam `myGameArea.start()` menjadi seperti berikut.

``` script.js
let myGameArea = {
    canvas: document.getElementById('canvas'),
    start: function() {
        this.canvas.width = 640
        this.canvas.height = 360
        this.context = this.canvas.getContext('2d')
        this.interval = setInterval(updateGameArea, 20)

        window.addEventListener('keydown', function(e) {
            myGameArea.keys = (myGameArea.keys || [])
            myGameArea.keys[e.key] = true
        })
        window.addEventListener('keyup', function(e) {
            myGameArea.keys[e.key] = false
        })
    },
    clear: function() {
        this.context.clearRect(0, 0, this.canvas.width, this.canvas.height)
    }
}
```

Lalu tambahkan kode berikut kedalam fungsi `updateGameArea()` menjadi seperti berikut.

``` script.js
function updateGameArea() {
    myGameArea.clear()

    if (myGameArea.keys && myGameArea.keys['a']) { myCar.x -= 1 }
    if (myGameArea.keys && myGameArea.keys['d']) { myCar.x += 1 }

    myRoad.update()
    myCar.update()
}
```

Jika dirasa pergerakannya kurang cepat, tambah nilainya.

``` script.js
if (myGameArea.keys && myGameArea.keys['a']) { myCar.x -= 3 }
if (myGameArea.keys && myGameArea.keys['d']) { myCar.x += 3 }
```

... diubah menjadi 3 misalnya.

## Langkah Kelima: Membuat Rintangan

Dilangkah kelima ini, kita akan membuat beberapa rintangan yang mana jika berhasil di lewati akan menambah score player.

Buka kembali file `script.js`.

Deklarasikan komponen `myObstackle`.

``` script.js
let myCar
let myRoad
let myObstacles = []
```

Variable `myObstacle` dibuat dengan tipe data array karena isinya akan lebih dari 1 nilai.

Ubah kode `myGameArea` menjadi seperti berikut.

``` script.js
let myGameArea = {
    canvas: document.getElementById('canvas'),
    start: function() {
        this.canvas.width = 640
        this.canvas.height = 360
        this.context = this.canvas.getContext('2d')
        this.frameNo = 0
        this.interval = setInterval(updateGameArea, 20)

        window.addEventListener('keydown', function(e) {
            myGameArea.keys = (myGameArea.keys || [])
            myGameArea.keys[e.key] = true
        })
        window.addEventListener('keyup', function(e) {
            myGameArea.keys[e.key] = false
        })
    },
    clear: function() {
        this.context.clearRect(0, 0, this.canvas.width, this.canvas.height)
    },
    stop: function() {
        clearInterval(this.interval)
    }
}
```

Kita membuat fungsi `myGameArea.stop()` untuk menghentikan permainan ketika 'Game Over'.

Lalu tambahkan fungsi `.crashWith()` di dalam `component()`.

``` script.js
// didalam function component()

this.crashWith = function(otherobj) {
    let myleft = this.x
    let myright = this.x + this.width
    let mytop = this.y
    let mybottom = this.y + this.height

    let otherleft = otherobj.x
    let otherright = otherobj.x + otherobj.width
    let othertop = otherobj.y
    let otherbottom = otherobj.y + otherobj.height

    let crash = true

    if ((mybottom < othertop) || (mytop > otherbottom) || (myright < otherleft) || (myleft > otherright)) {
        crash = false
    }

    return crash
}
```

Setelah itu, buat fungsi `everyinterval()` untuk mengatur setiap berapa interval suatu kode program akan dijalankan.

``` script.js
// dibawah function updateGameArea()

function everyinterval(n) {
    if ((myGameArea.frameNo / n) % 1 == 0) {
        return true
    }

    return false
}
```

Terakhir, ubah fungsi `updateGameArea()` menjadi seperti berikut.

``` script.js
function updateGameArea() {
    for (i = 0; i < myObstacles.length; i++) {
        if (myCar.crashWith(myObstacles[i])) {
            myGameArea.stop()
        }
    }

    myGameArea.clear()
    myGameArea.frameNo++

    if (myGameArea.frameNo == 1 || everyinterval(60)) {
        myObstacles.push(new component(40, 72, 'green', 277, -72))
    }

    if (myGameArea.keys && myGameArea.keys['a']) { myCar.x -= 4 }
    if (myGameArea.keys && myGameArea.keys['d']) { myCar.x += 4 }
    
    myRoad.update()

    for (i = 0; i < myObstacles.length; i++) {
        myObstacles[i].y += 4
        myObstacles[i].update()
    }

    myCar.update()
}
```

Jika berhasil, maka akan muncul kotak hijau di kolom ke 2 setiap 60ms.

Belum selesai sampai disini!

Kita ingin rintangannya bermacam-macam dan keluar secara acak.

Dan mobilnya masih bisa keluar dari jalur yang mana itu merupakan sebuah bug.

Jadi kita perlu menambahkan beberapa kode kedalam fungsi `updateGameArea()`.

``` script.js
function updateGameArea() {
    for (i = 0; i < myObstacles.length; i++) {
        if (myCar.crashWith(myObstacles[i])) {
            myGameArea.stop()
        }
    }

    myGameArea.clear()
    myGameArea.frameNo++

    if (myGameArea.frameNo == 1 || everyinterval(60)) {
        myObstacleColor = ['green', 'yellow', 'red', 'black']
        myObstacleX = [223, 277, 331, 385]
        
        color = myObstacleColor[Math.floor(Math.random() * myObstacleColor.length)]
        x = myObstacleX[Math.floor(Math.random() * myObstacleX.length)]

        if (color == 'black') {
            myObstacles.push(new component(40, 40, color, x, -80))
        } else {
            myObstacles.push(new component(40, 72, color, x, -80))
        }
    }

    if ((myGameArea.keys && myGameArea.keys['a']) && myCar.x > 216) { myCar.x -= 4 }
    if ((myGameArea.keys && myGameArea.keys['d']) && myCar.x < 392) { myCar.x += 4 }
    
    myRoad.update()

    for (i = 0; i < myObstacles.length; i++) {
        myObstacles[i].y += 4
        myObstacles[i].update()
    }

    myCar.update()
}
```

## Langkah Keenam: Membuat Game Score

Kurang rasanya jika tidak ada skor dalam game.

Maka pada tahap ini kita akan membuat game score.

Kita akan menghitung score setiap mobil kita berhasil melewati rintangan dan scorenya pun beragam.

Tapi sebelum itu, kita akan sedikit merubah kode pada `component()` supaya bisa menampilkan score.

``` script.js
function component(width, height, color, x, y, type) {
    this.width = width
    this.height = height
    this.x = x
    this.y = y
    this.type = type

    this.update = function() {
        ctx = myGameArea.context

        if (this.type == 'text') {
            ctx.font = this.width + ' ' + this.height
            ctx.fillRect = color
            ctx.fillText(this.text, this.x, this.y)
        } else {
            ctx.fillStyle = color
            ctx.fillRect(this.x, this.y, this.width, this.height)
        }
    }

    ...
}
```

Lalu ubah kode paling atas menjadi seperti berikut.

``` script.js
let myCar
let myRoad
let myObstacles = []
let myScore

function startGame() {
    myGameArea.start()

    myRoad = new component(216, 360, 'gray', 216, 0)
    myCar = new component(40, 72, 'red', 277, 270)
    myScore = new component('30px', 'Consolas', 'black', 440, 40, 'text')
}

...
```

Dan ubah fungsi `upgradeGameArea()` menjadi seperti berikut.

``` script.js
var myScoreText = 0

function updateGameArea() {
    for (i = 0; i < myObstacles.length; i++) {
        if (myCar.crashWith(myObstacles[i])) {
            myGameArea.stop()
        }
    }

    myGameArea.clear()
    myGameArea.frameNo++

    if (myGameArea.frameNo == 1 || everyinterval(60)) {
        myObstacleColor = ['green', 'yellow', 'red', 'black']
        myObstacleX = [223, 277, 331, 385]
        
        color = myObstacleColor[Math.floor(Math.random() * myObstacleColor.length)]
        x = myObstacleX[Math.floor(Math.random() * myObstacleX.length)]

        if (color == 'black') {
            myObstacles.push(new component(40, 40, color, x, -80))
        } else {
            myObstacles.push(new component(40, 72, color, x, -80))
        }
    }

    if ((myGameArea.keys && myGameArea.keys['a']) && myCar.x > 216) { myCar.x -= 4 }
    if ((myGameArea.keys && myGameArea.keys['d']) && myCar.x < 392) { myCar.x += 4 }
    
    myRoad.update()

    for (i = 0; i < myObstacles.length; i++) {
        if (myObstacles[i].y == 344) {
            if (myObstacles[i].color == 'green') {
                myScoreText += 50
            } else if (myObstacles[i].color == 'yellow') {
                myScoreText += 100
            } else if (myObstacles[i].color == 'red') {
                myScoreText += 150
            } else if (myObstacles[i].color == 'black') {
                myScoreText += 200
            } else {
                myScoreText += 1
                console.log(myObstacles[i].color)
            }
        }
    }
        
    for (i = 0; i < myObstacles.length; i++) {
        myObstacles[i].y += 4
        myObstacles[i].update()
    }

    myCar.update()

    myScore.text = 'Score : ' + myScoreText
    myScore.update()
}
```

Kurang lebih seperti ini hasilnya.

![Screenshot 3](/img/game-mobil-balap-javascript/gambar-3.png)

## Langkah Ketujuh: Menambahkan Gambar

Agak aneh sebenarnya kalau cuma kotak-kotak kayak gitu.

Jadi, disini kita akan mengganti kotak-kotak tersebut dengan gambar.

Pertama ubah kode `component()` menjadi seperti berikut.

``` script.js
function component(width, height, color, x, y, type) {
    this.width = width
    this.height = height
    this.color = color
    this.x = x
    this.y = y
    this.type = type

    if (this.type == 'image') {
        this.image = new Image()
        this.image.src = color
    }

    this.update = function() {
        ctx = myGameArea.context

        if (this.type == 'text') {
            ctx.font = this.width + ' ' + this.height
            ctx.fillStyle = color
            ctx.fillText(this.text, this.x, this.y)
        } else if (this.type == 'image') {
            ctx.drawImage(this.image, this.x, this.y, this.width, this.height)
        } else {
            ctx.fillStyle = color
            ctx.fillRect(this.x, this.y, this.width, this.height)
        }
    }

    this.crashWith = function(otherobj) {
        let myleft = this.x
        let myright = this.x + this.width
        let mytop = this.y
        let mybottom = this.y + this.height

        let otherleft = otherobj.x
        let otherright = otherobj.x + otherobj.width
        let othertop = otherobj.y
        let otherbottom = otherobj.y + otherobj.height

        let crash = true

        if ((mybottom < othertop) || (mytop > otherbottom) || (myright < otherleft) || (myleft > otherright)) {
            crash = false
        }

        return crash
    }
}
```

Lalu ubah parameter `color` pada komponen yang akan kita ganti menjadi gambar dengan url atau path dari gambar yang akan kita gunakan.

Jangan lupa ganti juga yang ada di dalam fungsi `updateGameArea()`.

``` script.js
...

function startGame() {
    myGameArea.start()

    myRoad = new component(216, 360, 'gray', 216, 0)
    myCar = new component(40, 72, 'img/redCar.png', 277, 270, 'image')
    myScore = new component('30px', 'Consolas', 'black', 440, 40, 'text')
}

...

function updateGameArea() {

    ...

    if (myGameArea.frameNo == 1 || everyinterval(60)) {
        myObstacleColor = ['greenCar', 'yellowCar', 'redCar', 'oil']
        myObstacleX = [223, 277, 331, 385]
        
        color = myObstacleColor[Math.floor(Math.random() * myObstacleColor.length)]
        x = myObstacleX[Math.floor(Math.random() * myObstacleX.length)]

        myObstacles.push(new component(40, 72, 'img/' + color + '.png', x, -80, 'image'))
    }

    ...

    for (i = 0; i < myObstacles.length; i++) {
        if (myObstacles[i].y == 344) {
            if (myObstacles[i].color == 'img/greenCar.png') {
                myScoreText += 50
            } else if (myObstacles[i].color == 'img/yellowCar.png') {
                myScoreText += 100
            } else if (myObstacles[i].color == 'img/redCar.png') {
                myScoreText += 150
            } else if (myObstacles[i].color == 'img/oil.png') {
                myScoreText += 200
            } else {
                myScoreText += 1
                console.log(myObstacles[i].color)
            }
        }
    }

    ...
}

...
```

Gambar yang saya gunakan saya simpan di dalam folder `img`.

---

Oke, sampai sini gamenya sudah bisa dibilang jadi. Tinggal diberi home screen atau welcome screen untuk menyambut user.

Gambar dan source code bisa di download [disini](https://github.com/softdrink-pj/game-mobil-balap-javascript/archive/refs/heads/main.zip).

## Referensi:

- [W3Schools Online Web Tutorials - Game Tutorial](https://www.w3schools.com/graphics/game_intro.asp)
