---
layout:     post
title:      Mengenal Framework Web AdonisJs
date:       2019-04-23 22:40
summary:    AdonisJs adalah web framework NodeJs dengan konsep MVC.
categories: nodejs adonisjs mvc
---

<div style="text-align: center;">
  <img src="https://camo.githubusercontent.com/738622745b8c9b67eb681460a83371ad20f5b704/68747470733a2f2f7265732e636c6f7564696e6172792e636f6d2f61646f6e69736a732f696d6167652f75706c6f61642f715f3130302f76313439373131323637382f61646f6e69732d707572706c655f707a6b6d7a742e737667" />
</div>
[**AdonisJs**](https://adonisjs.com/) adalah web framework **NodeJs** dengan konsep **MVC**. Sebelum kita mengenal jauh tentang Adonis ini ada baiknya kita mengenal terlebih dahulu apa itu NodeJs dan konsep MVC itu sendiri.

<div style="text-align: center;">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Node.js_logo.svg/590px-Node.js_logo.svg.png" />
</div>
> NodeJs adalah Javascript runtime yang bergguna untuk menjalankan kode Javascript disisi server (Server Side). Atau lebih mudahnya kita anggap Javscript untuk keperluan Server Side. Seperti yang kita semua tau sebelumnya bahwa Javascript adalah pemograman yang berjalan di sisi client bersamaan dengan CSS dan HTML untuk membuat antarmuka Web menjadi lebih interaktif.

<div style="text-align: center;">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b5/ModelViewControllerDiagram2.svg/1200px-ModelViewControllerDiagram2.svg.png" />
</div>
> MVC Atau Model View Controller adalah sebuah konsep untuk membuat aplikasi dengan memisahkan Data (Model), Tampilan (View), dan proses (Controller). Konsep MVC ini akan memudahkan dalam membuat atau me-maintance suatu aplikasi karna struktur aplikasi yang jelas.

Saya sangat sukda dengan AdonisJS ini karna dilengkapi banyak fitur sepeti [**adonis-ally**](https://github.com/adonisjs/adonis-ally), [**adonis-lucid**](https://github.com/adonisjs/adonis-lucid) dan lain2 sehingga bisa mempercepat proses pembuatan aplikasi tanpa harus bangun dari awal. Selain itu Adonisjs sudah mengadopsi *async / await* sehingga kode terlihat lebih indah dibanding kita memakai callback ataupun promise. [Dokumentasinya](https://adonisjs.com/docs/) yang cukup lumayan lengkap, dapat mudah dipahami dan adanya komunitas yang aktif di [forum](https://forum.adonisjs.com/) adalah salah satu kenapa saya cukup yakin framework ini akan berkembang menjadi besar.

```javascript
const User = use('App/Models/User')

class UserController {
  async index ({ request, response }) {
    const users = await User.all()
    return response.ok({
      status: 200,
      error: false,
      data: users,
      message: null
    })
  }

  async show ({ request, response, params: { userId } }) {
    const user = await User.findOrFail(userId)
    return response.ok({
      status: 200,
      error: false,
      data: user,
      message: null
    })
  }
}
```

Walaupun AdonisJS ini memakai konsep MVC tetapi kitapun bisa membuat REST API karna Adonisjs ini menyediakan 3 blueprint yang bisa kita pakai yaitu Fullstack, Api only, dan slim. Jadi jika ingin membuat suatu REST API kita tidak membutuhkan fitur view karna yang di return hanyalah sebuah JSON ataupun XML, jadi kita bisa memakai blueprint "Api only" untuk mengurangi dependensi yang tidak di pakai.

Seperti yang sebelumnya saya bilang, AdonisJS dilengkapi banyak fitur sehingga mempercepat proses development dan pembuatan suatu aplikasi web.
- Router
- View (**EdgeJS**)
- SQL ORM (**Lucid**)
- Migrasi
- Exception Handler
- Data Validation / Data Sanitization
- Unit / Functional Test
- Dan masih banyak yang lainya.

Untuk memulai dengan AdonisJs cukup mudah, pertama kita harus install Nodejs versi 8 atau terbaru terlebih dahulu. Saya memakai Ubuntu dan kebutulan instalasi di ubuntu tidak terlalu ribet, untuk sistem operasi yang berbeda bisa di sesuaikan, [Download NodeJS](https://nodejs.org/en/download/). Silakan buka terminal dan paste baris kode di bawah ini dan ikuti langkah2 selanjutnya yang diperintahkan.
```bash
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
```

Bila Nodejs sudah terpasang, selanjutnya kita install adonis-cli untuk memudahkan kita membuat web menggunakan Adonisjs.
```bash
npm i -g @adonisjs/cli
```
Bila anda mengalami error EACCES saat menginstall adonis-cli secara global, coba ikuti tutor berikut [Resolving EACCES permissions errors when installing packages globally](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally)

Lalu kita cek apakah adonis-cli sudah terpasang dengan benar di mesin kita atau belum dengan perintah dibawah. bila telah terpasang akan keluar perintah2 apa saja yang kita bisa panggil.
```bash
adonis --help
```

Lanjut dengan membuat webnya dengan perintah di bawah. Perintah dibawah akan membuat directory bernama *adonisjs-web-app* dan melakukan clone blueprint Adonisjs versi fullstack serta melakukan instalasi semua depedensi yang di butuhkan (`npm install`). *adonisjs-web-app adalah nama aplikasinya*.
```bash
adonis new adonisjs-web-app
```

Jika telah seslai melakukan perintah di atas, selanjutnya kita masuk ke hasil directory yang telah dilakukan perintah di atas dan menjalankan webnya dengan environment development
```bash
cd adonisjs-web-app
adonis serve --dev
```

Lalu akses *http://localhost:3333* di browser dan akan terlihat bacaan *It Works!* yang menandakan bawah kita siap untuk membangun web dengan AdonisJS yang kita bahas di lain hari! Terimakasih.