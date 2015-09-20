---
title: Code Smell ( Ambune Koding )
tags:
- coding
---
*Codingane* Mambu, Kali ini aku mau bahas sesuatu yang sedikit tidak enak ni, yaitu *code smell*. *Code smell* adalah isitilah dalam bidang teknologi dimana jika kamu melihat sebuah block code yang sudah berjalan lancar tetapi merasa ada bau-bau tidak enak dalam code mu (bisa wangi juga baunya). Aku kemaren sempet *rasan-rasan* sama seorang rekan tentang *refactoring*.

Aku sempet membaca sedikit di bukunya *Martin Fowler* yang berjudul *Refactoring : Improving the Design of Existing Code* pada paragraf - paragraf awal. Di dalam bukunya si *martin* menjelaskan kalau *refactoring* perlu dilakukan karena adanya *Code smell bad* yaitu bau kodinganya gak enak. tetapi, bau Kodingan gak selalu buruk lhoo. ada yang wangi juga. dan menurut saya kekuatan indra penciuman code perlu dilatih.

Saya mengutip sebuah blog [codinghorror](http://blog.codinghorror.com/code-smells/) di blog mengatakan,

>There's nothing wrong with codifying refactoring guidelines in a book. But the most important guideline is to watch for warning signs in your own code – so called "code smells"

artinya tidak ada yang salah dengan buku panduan *refactoring code*, tetapi yang lebih penting adalah bagaimana bisa melihat tanda peringatan pada code yang ditulis sendiri. nha *warning* nya itu disebut *"Code Smell"*

Kalau yang sering coding mungkin bisa lah sedikit demi sedikit merasakan bau yang tidak enak pada code yang dibuat. Dan kasus Kodingan yang *mambu* ini sering aku alami dan masalah yang sering muncul adalah bagaimana menemukan pusat bau nya itu dimana :v. Udah tau bau tapi gak tau ngilangin baunya gimana. Ada sih tanda2 bagaimana *best practice* ngilangin codinganyang Bau, tapi banyak banget cuy. susah memang menemukan baunya itu dimana.

Contoh saja

``` go

package main

import(
  "strconv"
  "fmt"
)

func main() {
  a := "100"
  b := "20"
  c := "3"

  intA, er := strconv.Atoi(a)
  if er != nil {
		panic(er)
	}
  intB, er := strconv.Atoi(b)
  if er != nil {
		panic(er)
	}
  intC, er := strconv.Atoi(c)
  if er != nil {
		panic(er)
	}

  fmt.Println(intA + intB + intC)
}

```

Code diatas adalah contoh code yang berbau buruk hayo tebak kenapa?
**Yap!!!** karena mengulang beberapa hal yang sama. dan itu baunya gak enak.
coba kalau seperti ini.

``` go

package main

import(
  "strconv"
  "fmt"
)

func check(e error) {
	if e != nil {
		panic(e)
	}
}

func main() {
  a := "100"
  b := "20"
  c := "3"

  intA, er := strconv.Atoi(a)
  check(er)
  intB, er := strconv.Atoi(b)
  check(er)
  intC, er := strconv.Atoi(c)
  check(er)

  fmt.Println(intA + intB + intC)
}

```

lebih rapi kah?. saat *check error* mengubah *string* menjadi *integer* fungsi `Atoi` ada return *error* yang *best practice* nya harus di check tu error atau enggak. Karena *return error* banyak digunakan pada bahasa pemogramman golang, sehingga kita bisa bikin fungsi untuk check error.  
itu cuma contoh kecil aja sih. Dua code diatas hasilnya benar semua tapi yang satu agak bau gak enak, yang satunya lagi lumayan lah baunya.

untuk beberapa *Code Smell* yang sering ditemui akan aku bahas di bahasan selanjutnya.
