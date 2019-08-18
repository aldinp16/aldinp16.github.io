---
layout:     post
title:      Catatan Belajar NATS Bagian 1 Pengenalan
date:       2019-08-18 19:00
summary:    NATS (Neural Autonomic Transport System) adalah salah satu sistem aplikasi opensource pendistribusian pesan.
categories: CNCF messaging microservices pubsub
---

[**NATS**](https://nats.io/) adalah salah satu sistem aplikasi untuk melakukan pertukaran pesan asyncronus yang biasa digunakan untuk komunikasi antara aplikas atau suatu services komputer.

Konsep pertukaran pesan menggunakan Publish dan Subscribe mirip dengan MQTT. Dimana service yang menerima data biasa di sebut *Consumer/Subscriber*, dan service yang bertugas mengirim data biasa di sebut *Producer/Publisher*

**NATS** Bisa dipakai untuk kebutuhan komunikasi antar service di arsitektur *microservices*, untuk kebutuhan collecting data device IOT, kebutuhan data streaming, dan lain-lain.

<div style="text-align: center;">
  <img src="/images/catatan-belajar-nats-bagian-1/PubSub.png" />
</div>

Di dalam NATS membutuhkan Topic agar antar service dapat berkomunikasi. topic adalah sebuah string biasa yang harus diketahui oleh *subscriber* ataupun *publisher* agar masing2 tau dimana harus mempublish/melisten topic agar mendapatkan data yang sesuai.

Selain topic biasa, Di NATS bisa juga menggunakan Wilcards topic untuk mendapatkan/mengirim data lebih dari satu nested topic tertentu. Nested topic dalam nats dipisahkan menggunakan (*.*) **dot/titik**. misal:

- `jawabarat.bandung.suhu`
- `jawabarat.bogor.suhu`
- `jawabarat.bandung.suhu.celcius`
- `jawabarat.bandung.suhu.farenheit` 

Wilcard yang pertama menggunakan simbol **asterik** (*\**) untuk mencocokan satu topic di dalam nested topic. misal: `jawabarat.*.suhu` untuk mendapatkan data suhu dari semua kota di jawabarat.

<div style="text-align: center;">
  <img src="/images/catatan-belajar-nats-bagian-1/Asterik-Wilcard.png" />
</div>

Wilcard yang kedua menggunakan simbol **greather-than** (*>*) untuk mencocokan dua topic atau lebih dalam nested topic. misal: `jawabarat.bandung.>` untuk mendapatkan data suhu di kota bandung, jawabarat. dengan skala farenheit dan celcius

<div style="text-align: center;">
  <img src="/images/catatan-belajar-nats-bagian-1/Greather-Than-Wilcard.png" />
</div>

Selain dengan konsep Publish-Subscribe, NATS pun mendukung komunikasi dengan cara Request-Reply dimana salah satu client akan melakukan Permintaan (Request) dan menunggu balasan dengan batas waktu tertantu (timeout)

- [https://nats-io.github.io/docs/developer/concepts/subjects.html](https://nats-io.github.io/docs/developer/concepts/subjects.html)
- [https://nats-io.github.io/docs/developer/concepts/pubsub.html](https://nats-io.github.io/docs/developer/concepts/pubsub.html)
- [https://nats-io.github.io/docs/developer/concepts/reqreply.html](https://nats-io.github.io/docs/developer/concepts/reqreply.html)