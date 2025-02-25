---
title: "03: refactoring"
summary: "learn how to refactor your code"
categories: ["post","lesson"]
tags: ["main-course"]
date: 2025-02-25
draft: false
---

## Introduction

Refactoring adalah proses ngerombak code yang udah ada tanpa ngubah perilakunya. Ibaratnya kayak beberes tapi fungsi tetap sama, cuma jadi lebih rapi. Pelajaran ini bakal ngenalin beberapa aturan praktis buat nentuin kapan perlu refactor, ngasih contoh refactoring di GDScript, dan ngejelasin kenapa kamu ga perlu takut harus langsung sempurna dari hari pertama.

## Growing State of the Project

Seiring project kita makin gede, kompleksitas codebase juga ikut naik. Fitur terus nambah, script makin panjang, variabel makin banyak, dan lama-lama bisa berantakan. Dengan rutin “beres-beres” (alias refactoring), project bakal tetep rapi dan gampang di-maintain. Mungkin keliatannya kayak langkah tambahan yang buang waktu, tapi refactoring sejak dini bisa ngindarin kita dari sesi debugging panjang yang nyebelin nanti.

## To Refactor or Not to Refactor

Penilaian soal “code bersih” itu kadang subjektif—tiap orang bisa punya gaya yang beda-beda. Meski begitu, ada beberapa situasi di mana refactoring hampir selalu disarankan:

1. **Saran dari dokumentasi resmi**  
   Kalo dari dokumentasi resmi engine atau bahasa pemrograman ada anjuran cara yang lebih efisien atau idiomatik, itu tanda kalo code kita mungkin perlu disesuain biar sejalan sama praktik standar.

2. **Kode yang diulang lebih dari tiga kali**  
   Ada yang namanya “rule of three”: kalo kamu ngeliat satu potong kode yang sama diulang tiga kali atau lebih, itu artinya saatnya refactor. Kita bisa mindahin logika itu ke satu fungsi atau metode reusable. Ini bikin code lebih konsisten dan gampang dirawat.

Selain poin-poin di atas, jangan terlalu stres sama “kesempurnaan” code. Yang penting code-nya jalan dan kamu paham cara kerjanya. Kalo pengen, nanti pun masih bisa dibersihin lagi.

## Refactoring Example

Sepanjang pelajaran sebelum-sebelumnya, kamu mungkin ngeliat beberapa code yang “kurang optimal”. Sengaja kita bikin gitu biar belajar dasarnya step by step. Yuk kita lihat contoh sederhana soal inisialisasi variabel di Godot.

Sebelumnya, kita pake:
```gdscript
var animation_player

func _ready():
    animation_player = get_node("animationplayer")
```

Cara ini jelas: kita bikin variabel, terus pas `_ready()` kita assign node-nya. Tapi ada cara lain yang lebih singkat dan keliatan “bersih”:

```gdscript
@onready var animation_player = $animationplayer
```

Penjelasannya:
- `@onready` nyuruh Godot buat inisialisasi variabel ini pas node udah “ready” (masuk scene tree).
- `$animationplayer` itu shortcut dari `get_node("animationplayer")`.

Hasilnya, baris code berkurang dari dua jadi satu, dan kalo udah paham apa yang terjadi, ini jadi lebih cepet ditulis dan enak dibaca.

## Variable Export

Selanjutnya, kita ngomongin soal export variabel ke inspector. Kadang ada variabel yang sering banget kamu ganti-ganti langsung di code (misal, health enemy). Bolak-balik buka script bisa bikin capek.

Mulai Godot 4, kamu bisa export variabel kayak gini:

```gdscript
@export var hp = 3
```

Baris ini bikin variabel `hp` muncul di scene dock (inspector). Jadi kamu tinggal atur dari editor tanpa ngegali script tiap mau ganti nilainya. Ini berguna banget buat balancing parameter game kayak health, damage, atau speed. Tiap instance scene juga bisa punya nilai yang beda, sementara `3` tetep jadi default kalo ga diutak-atik.
<video src="instancing.mp4" controls></video>

Masing-masing instance scene bisa punya nilai beda di inspector.
<video src="export_variable.mp4" controls></video>

## Adding a Speed Variable to the Character

Saat ini, kecepatan gerak karaktermu masih “hardcoded.” Bikin kita repot kalo harus ganti nilainya di beberapa tempat sekaligus, misal dari `200` ke `300`:

```gdscript
....

if input.is_action_pressed("move_left"):
    direction.x = -200
if input.is_action_pressed("move_right"):
    direction.x = 200
if input.is_action_pressed("move_up"):
    direction.y = -200
if input.is_action_pressed("move_down"):
    direction.y = 200

...
```

Cara lebih oke: bikin satu variabel speed:

```gdscript
@export var speed = 200
```

Lalu di bagian movement:

```gdscript
...

if input.is_action_pressed("move_left"):
    direction.x = -1 * speed
if input.is_action_pressed("move_right"):
    direction.x = 1 * speed
if input.is_action_pressed("move_up"):
    direction.y = -1 * speed
if input.is_action_pressed("move_down"):
    direction.y = 1 * speed

...
```

(Angkanya tetap sama, tapi arah gerak ditentuin sama tanda minus atau plus, sedangkan besar kecepatannya tergantung variabel `speed`.) Kalo nanti pengen atur ulang speed, tinggal ubah satu variabel, baik di script maupun di inspector.

## A Word of Caution

Begitu udah tau beberapa trik refactoring, mungkin kamu langsung pengen beresin semua code sekarang juga. Tapi inget, kadang kecepatan bikin fitur itu sama pentingnya dengan kerapihan—atau malah lebih penting. Pastikan fitur jalan dulu; pas fitur udah stabil dan kamu ngerti alurnya, _baru_ refactor.

- **Ga perlu buru-buru sempurna di setiap langkah.** Bikin dan uji fitur dengan cara yang menurutmu paling masuk akal.  
- **Refactor kalo memang bener-bener ngebantu.** Kalo nemu peningkatan besar yang bikin code lebih simpel tanpa nyusahin, silakan.  
- **Jangan matiin flow-mu sebelum selesai.** Kalo lagi prototyping dan masih ngetes ide, kadang lebih baik tunda refactor sampai kamu yakin fungsinya udah oke.

Intinya, cari keseimbangan: jaga code-mu tetap sehat, tapi jangan biarin “perfeksionisme” ngganggu progress.

## Conclusion

Refactoring itu skill: tau _kapan_ dan _gimana_ beresin code bisa bikin kamu jadi developer yang lebih efisien dan bahagia. Ingat:

- Refactor kalo ada anjuran resmi atau kalo nemu kode yang diulang terlalu banyak.
- Export variabel biar gampang diatur di editor.
- Ga harus poles semua baris code sekarang juga; bisa dibersihin kapan aja.

Seiring project makin besar, kamu bakal makin peka kapan waktunya refactor. Makin banyak kamu coba-coba berbagai pendekatan, makin jago kamu nentuin mana yang layak dioptimasi dan mana yang bisa ditunda. Semoga lancar, dan selamat ngoding!

Sampai ketemu di pelajaran berikutnya! 🚀