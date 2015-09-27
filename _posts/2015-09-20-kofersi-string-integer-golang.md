---
title: Konversi string integer dalam golang
tags:
- golang
---

Kemaren aku sempet bingung gimana konversi string ke dalam integer di golang. atau integer ke string.
Ternyata di di dalam golang sudah komplit. aku pake *strconv*

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

func main()  {
  //string to integer
  i, err := strconv.Atoi("-81")
  check(err)

  //integer to string
  s := strconv.Itoa(-81)

  fmt.Println(i)
  fmt.Println(s)
}


```

ingat ya untuk konfersi dari string ke integer harus ada return errornya.
yang asik itu mengingatnya.

- A to i (A String ke integer)
- I to a (Integer ke a String)

Apik to.
