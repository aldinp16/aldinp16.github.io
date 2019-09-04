---
layout:     post
title:      Catatan Belajar NATS Bagian 3 Queue Groups
date:       2019-09-05 00:00
summary:    Mencoba Queue Groups
categories: CNCF messaging microservices pubsub
---

- [Catatan Belajar NATS Bagian 1 Pengenalan](https://aldi.dev/cncf/messaging/microservices/pubsub/2019/08/18/catatan-belajar-nats-bagian-1/)
- [Catatan Belajar NATS Bagian 2 Simple Publish Subscribe dengan Docker dan Nodejs](https://aldi.dev/cncf/messaging/microservices/pubsub/2019/08/19/catatan-belajar-nats-bagian-2/)

Setelah kita melakukan praktek simple publish subscribe menggunakan **NATS**. **NATS** juga menyediakan fitur **Queue Groups** yang digunakan biasanya untuk kebutuhan replika clustering. Jika suatu service mempunyai replika lebih dari satu, maka pesan tidak akan di kirimkan kesemua service yang sama karna akan mengakitbatkan duplikat. Maka dari itu **Queue Groups** di sini berfungsi untuk menyeimbangkan (Seperti load balancer) semua data yang dikirim ke suatu kelompok service yang sama.

<div style="text-align: center;">
  <img src="/images/catatan-belajar-nats-bagian-3/queue-groups.png" /><br />
  <small>diambil dari https://nats-io.github.io/docs/developer/concepts/queue.html</small>
</div>

Untuk pengimplementasian di **NATS.js** nya pun cukup mudah, kita hanya butuh menambahkan `{ queue: '<namaGroup>' }`. di parameter kedua di fungsi `nats.subscribe()`. contoh:
```javascript
nats.subscribe('foo', {'queue':'job.workers'}, function() {
  received += 1;
});
```

Untuk langsung mencoba mari kita ubah subscriber yang postingan sebelumnya kita buat lalu tambahkan queue groupsnya.
```javascript
nats.subscribe(TOPIC, {'queue': 'job.workers'}, handleBandungSuhuTopic)
```

Lalu kita coba test jalankan 2 subcriber dengan queue groups yang sama, dan satu publisher. Jika berhasil, makan pesan tidak akan di terima oleh semua subscriber. Tetapi akan di bagi2 ke semua subscriber. seperti berikut:
<div style="text-align: center;">
  <img src="/images/catatan-belajar-nats-bagian-3/contoh-queue-groups.png" />
</div>

Repo catatan hasil belajar ini bisa diliat di github:
[https://github.com/aldinp16/learn-nats/](https://github.com/aldinp16/learn-nats/)