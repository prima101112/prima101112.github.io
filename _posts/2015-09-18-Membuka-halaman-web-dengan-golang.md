---
title: Membuka Halaman Web Dengan Golang
tags:
- golang
- scraping
---
**Golang** sudah menyediakan package untuk *mengambil* web page. yaitu `net/http` dalam package tersebut terdapat banyak sekali fungsi.
dalam artikel aku ini mau nyontohin aja gimana ngambil web page dengan golang trus aku tulis di sebuah file.

Penjelasan code ada di dalam code ya.
kelemahan menggunakan net/http kita takdapat menyimpan cookies (dari beberapa artikel yang saya baca sih). kalau ada yang menemukan sesuatu tentang ngambil cookies trus bisa disimpen di file. kabaru aku yak.

berikut contoh code mengambil web page dengan golang. simpel kok buat aja file `GOPATH/src/terserah.monggo/goHttpGet.go` dalam `GOPATH`

``` go

package main

import (
  "fmt"
  "net/http"
  "io/ioutil"
)

func check(e error)  {
  if e != nil {
      panic(e)
  }
}

func main()  {
  //langsung get url (direct hit)
  resp, err := http.Get("https://golang.org/pkg/net/http/")
  check(err)

  //Read respon body (type body = io.ReadCloser)
  body, err := ioutil.ReadAll(resp.Body)
	resp.Body.Close()
  check(err)

  //masukan body kedalam file body.html
  err = ioutil.WriteFile("body.html", body, 0755)
  check(err)

  //cuma bilang yay!
	fmt.Println("yay!")
}

```

untuk menjalankanya
masuk kedalam working directory file tersebut dan jalankan

``` go
go run goHttpGet.go
```

jika berhasil dilakukan *(tidak panik)* coba buka `GOPATH/src/terserah.monggo/body.html`
