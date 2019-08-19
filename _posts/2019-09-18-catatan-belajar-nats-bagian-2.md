---
layout:     post
title:      Catatan Belajar NATS Bagian 2 Simple Publish Subscribe dengan Docker dan Nodejs
date:       2019-08-19 00:00
summary:    Instalasi nats-server menggunakan Docker dan Nodejs Sebagai client
categories: CNCF messaging microservices pubsub
---

- [Catatan Belajar NATS Bagian 1 Pengenalan](https://aldi.dev/cncf/messaging/microservices/pubsub/2019/08/18/catatan-belajar-nats-bagian-1/)

Setelah pengenalan tentang NATS, disini saya akan memulai praktek belajar simple Publish Subscribe menggunakan [NATS.js](https://github.com/nats-io/nats.js) sebagai client dan instalasi nats server menggunakan Docker.

Pertama pull docker image NATS di [dockerhub](https://hub.docker.com/_/nats)
```
$ docker pull nats:latest
```

Lalu jalankan nats-server menggunakan perintah `docker run`. disini kita melakukan expose port `4222` dan `8222` dari container agar dapat di akses di mesin lokal kita. nats berjalan di port `4222` sementara port `8222` di gunakan untuk monitoring server nats via http.
```
$ docker run -d --name nats-server -p 4222:4222 -p 8222:8222 nats
```

Lalu cek apakah server nats telah siap digunakan atau tidak menggunakan docker logs
```
$ docker ps
$ docker logs -f <container-id>
```

Jika nats server siap di gunakan akan mengeluarkan log kurang lebih sebagai berikut
<div style="text-align: center;">
  <img src="/images/catatan-belajar-nats-bagian-2/dockerlog.png" />
</div>

Lalu menyiapkan client nodejsnya. install dulu **nats.js** melalui npm di folder root projectnya.
```
npm install nats
```

Lalu kita mulai membuat client untuk publishernya. publisher akan melakukan publish angka random antara 0 - 99 ke topic
`jawabarat.bandung.suhu` setiap 5000 milisecond (5 detik). Disini parameter `NATS.connect()` tidak di isi, maka akan automatis mengset host menjadi `localhost` dan port default yaitu `4222`. Kita berinama `publisher.js`
```javascript
#!/usr/bin/env node

'use strict'

const NATS = require('nats')
const nats = NATS.connect()

const interval = 5000

function sendRandomSuhu () {
  // generate random number beetwen 0 and 99
  const randomNumber = Math.floor(Math.random() * 100)
  const topic = 'jawabarat.bandung.suhu'
  nats.publish(topic, randomNumber.toString())
  console.log(`[PUBLISHER] Publish ${randomNumber} to ${topic}`)
}

function onConnectHandler () {
  console.log(`[PUBLISHER] Connected, ready for publish random suhu every ${interval/1000} Second`)
}

nats.on('connect', onConnectHandler)

// publish random number every 5 second
setInterval(sendRandomSuhu, interval)
```

Lalu membuat subscribernya, disini saya buat subscriber agar memiliki `ID` diambil dari parameter pertama saat script dijalankan. Kegunaanya agar dapat dapat mengidentifikasi subscriber karna kita akan menjalankan lebih dari satu subscriber. Lalu berinama `subscriber.js`
```javascript
#!/usr/bin/env node

'use strict'

const NATS = require('nats')

const nats = NATS.connect()
const ID = process.argv[2]
const TOPIC = 'jawabarat.bandung.suhu'

// check if id filled
if (typeof ID === 'undefined') {
  console.error(`ID can't empty.`)
  process.exit(1)
}

function onConnectHandler () {
  console.log(`[SUBSCRIBER-${ID}] (${TOPIC}) Connected, ready receive message.`)
}

function handleBandungSuhuTopic (msg) {
  console.log(`[SUBSCRIBER-${ID}] (${TOPIC}) Received data: ${msg}`)
}

nats.on('connect', onConnectHandler)
nats.subscribe(TOPIC, handleBandungSuhuTopic)
```

Setelah semuanya siap. pertama kita jalankan subscribernya. disini kita menjalankan dua subcriber untuk melihat apakah pesan diterima oleh semua subscriber atau tidak.
```
$ node subscriber.js 1
$ node subscriber.js 2
```

Lalu jalankan publishernya
```
$ node publisher.js
```

Bila tidak mengalami error, seharusnya message dapat di publish oleh publisher dan di terima oleh semua subscriber.
<div style="text-align: center;">
  <img src="/images/catatan-belajar-nats-bagian-2/pubsub.png" />
</div>

Bisa juga monitoring client yang sedang connect menggunakan http api, dengan mengakses `http://localhost:8222/connz`
<div style="text-align: center;">
  <img src="/images/catatan-belajar-nats-bagian-2/monitor-nats.png" />
</div>

Repo catatan hasil belajar ini bisa diliat di github:
[https://github.com/aldinp16/learn-nats/](https://github.com/aldinp16/learn-nats/)