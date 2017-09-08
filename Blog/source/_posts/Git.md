---
title: Git
tags:
	- Blog Intern
categories:
	- Artivisi
commands: true
date: 2017-08-14 11:35:45
---

## Learning Git

Pertama Install git menggunakan apt command di terminal ubuntu
	- sudo apt-get update
	- sudo apt-get install git

Setting git konfigurasi dengan menggunakan git config command, gunakan username dan email pada saat mendaftar git.
	- git config --global user.name "Nama Kamu"
	- git config --global user.email "emailkamu@gmail.com"
kita bisa melihat konfigurasi git melalui command
	- git config --list
maka akan muncul username serta useremail kamu.

Konfigurasi ngeFork git dari github ke local
	- Copy HTTP link
	- git clone URL(HTTP)
	- git remote add upstream URL(HTTP) *jika kita ingin ikut berkontribusi di project
	- git fetch update

 - $ git clone https://github.com/Amrmuhsin/contoh.git : untuk mengklone project orang ke local server
 - $ git remote add upstream https://github.com/Rookie/contoh.git : link project yg kita fork
   $ git fetch update : untuk nge-pull/menarik jika ada update pada repo master
 - $ git merge upstream/master : mengupdate fork local kamu dengan repo masternya atau nge-Sync repo local kita dengan repo master
 - $ git push origin master : push back atau kita "upload" kembali repo yang sudah berhasil di perbaharui/update ke repo online di GitHub milik kita 

git push
	- git status
	- git add . / git add filename.txt
	- git commit -m "UrComment"
	- git push

Sinkronisasi fork / merge git
	- git fetch upstream
	- git merge upstream/master

Perintah git dan kegunaannya
	- git diff  : untuk melihat perubahan yang dibuat di dalam file
	- git reset --<file> : unstage file / mengembalikan file ke stage
	- git checkout : membatalkan perubahan file
	- git reset --Soft HEAD^ : Unpushed commit dengan perubahan tetap di simpan
	- git clone <url> : copy git ke local
	- git rm <filename> : menghapus file
	- git status : mengecek perubahan yang ada
	- git add . : menambahkan semua file ke stage github
	- git commint -m "" : commit file ke github
	- git push : upload file ke github
	- git fetch upstream
	  git upstream/master : Synchronize github
	

POSTGRESQL
	- sudo su - postgres : membuka prosgres
	- createuser -P <namadatabase> // appdosendb
	- createdb -Oappdosendb -Eutf8 appdosendb
	- exit
