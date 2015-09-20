---
title: Cara Install ruby nggunain RVM di Ubuntu
tags:
- ruby
- iseng
---
Kemaren itu saya pengen buat catetan kecil gitu biar yang tak kerjain enggak lupa.
nha! akhirnya saya putuskan buat *github-page* biar enak gitu, kalau ngeblog tinggal *push* .
Sebelumnya saya mempertimbangkan menggunakan [hubpress](http://hubpress.io/),
tetapi akhirnya hati ini beralih ke [jekyll](http://jekyllrb.com/) yang menggunakan ruby, otomatis aja sih tinggal push udah publish gitu.

Sebelum install *jekyll* harus install ruby kan?, nha akhirnya aku putuskan untuk make RVM biar bisa pindah pindah versi ruby gitu.
nha disini aku sedikit mengulang saat kemaren itu,

### install *RVM*.


Sebelum install *RVM*, install dulu *mpapis public key*, ini kayak *security* gitu (kadang butuh *gpg2* atau/dan *sudo*)

``` python
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
```

Abis itu Download installer *RVM* (harus punya *curl*)

``` python
curl -O https://raw.githubusercontent.com/rvm/rvm/master/binscripts/rvm-installer
curl -O https://raw.githubusercontent.com/rvm/rvm/master/binscripts/rvm-installer.asc
```

Nha terus verifikasi dulu *installer signature* nya.

``` python
gpg --verify rvm-installer.asc &&
```
dan jika berhasil kayak gini tekan *ctrl + c*
![Image of primaadi](/img/01.png)

lalu jalanin command ini untuk install *RVM* *stable release*

``` python
bash rvm-installer stable
```
coba ketik `rvm` di *terminal* berhasil nggak?
jika masih ada error kemungkinan kamu belum memasukin *path* *rvm/bin* kedalam PATH di ubuntu.
letaknya di `.bash_profile` atau `.profile` di home dir kamu (`home/username`).

kalau yang seperi ini udah ada seharusnya sudah berhasil

``` python
export PATH="$PATH:$HOME/.rvm/bin"
```

Dan coba ketik *rvm* lagi

### Install *Ruby*  melalui *RVM*
Caranya gampang kok kalau kamu liat di [rvm.io](https://rvm.io/rvm/security)
kamu tinggal jalanin *command*

``` python
rvm install 2.2
```
yang diatas itu maksudnya kamu install ruby versi 2.2 kalau pengen pindah versi ruby tinggal

``` python
rvm use 2.1
```

**yap!** cuma segitu aja lah. aku install rvm dan ruby hanya untuk jekyll. bayangin rekoso kan. . .
