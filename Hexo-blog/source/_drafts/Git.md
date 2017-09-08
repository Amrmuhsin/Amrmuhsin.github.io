---
title: Git
tags:
---
## Learning Git

Pertama Install git menggunakan apt command di terminal ubuntu
	- sudo apt-get update
	- sudo apt-get install git

Setting git konfigurasi dengan menggunakan git config command, gunakan username dan email pada saat mendaftar git.
	- git config --global user.name "Nama Kamu"
	- git config --global user.email "emailkamu@gmail.com"
	kita bisa melihat konfigurasi git melalui 
	- git config --list
	maka akan muncul username serta useremail kamu.

Konfigurasi ngeFork git dari github ke local
	- Copy SSH link
	- git clone URL(SSH)
	- git pull

	git push
	- git status
	- git add . / git add filename.txt
	- git commit -m "UrComment"
	- git push

	Sinkronisasi fork / merge git
	- git fetch upstream
	- git merge upstream/master

	- git diff  --untuk melihat perubahan yang dibuat di dalam file
	- git reset --mereset file yang telah di add
	- git checkout --mengembalikan perubahan yang telah di commit sebelumnya (saat melakukan commit terakhir).