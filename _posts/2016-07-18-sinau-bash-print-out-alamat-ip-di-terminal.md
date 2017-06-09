---
layout: post
title: 'Sinau: bash - print-out alamat IP di terminal'
date: 2016-07-18 00:54:43.000000000 +07:00
type: post
published: true
status: publish
categories:
- Tanpa kategori
tags:
- bash
- foss
- linux
- programming
- script
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '24872737401'
  _publicize_done_external: a:2:{s:7:"twitter";a:1:{i:14020344;s:53:"https://twitter.com/anuress/status/754736123151196160";}s:8:"facebook";a:1:{i:14020349;s:37:"https://facebook.com/1170950276259780";}}
  _publicize_done_14100807: '1'
  _wpas_done_14020344: '1'
  publicize_twitter_user: anuress
  _publicize_done_14100813: '1'
  _wpas_done_14020349: '1'
author:
  login: anuress
  email: mightyfellas@gmail.com
  display_name: anuress
  first_name: ''
  last_name: ''
---
Pada saat libur super panjang biasanya cuma berarti dua hal bagi saya: santai, *leyeh-leyeh*. Hehe. Tapi, alhamdulillah di sela-sela *berleyeh-leyeh* ria saya masih diingatkan untuk belajar.

Secara terpaksa (karena fungsi di kode hasil copas dari agan [lucafav](http://dotshare.it/dots/1011/) yang entah kenapa tidak jalan di OS saya) saya membuat skrip kecil. Fungsinya cuma print-out alamat IP saja. He he.

**UPDATE (18-07-2016)**

Saya menemukan masalah dengan skrip ini yang disebabkan `file` `/sys/class/net/$interface/operstate` adakalanya berisi "*unknown*" (padahal koneksi dalam posisi "*up*"). Akhirnya saya mencoba beralih menggunakan [NetworkManager](https://wiki.archlinux.org/index.php/NetworkManager). Kode lama saya jadikan *comment*.

{% highlight bash %}
netIntf="$(ls /sys/class/net)"
for interface in $netIntf
do
 #state="$(cat /sys/class/net/$interface/operstate)"
 state="$(nmcli device show $interface | grep GENERAL.STATE | cut -d &quot; &quot; -f 27)"
 #if [[ $state == "up" ]]
 if [[ $state == 100 ]]
 then
   #echo "$(ifconfig | grep -A1 $interface | grep inet | sed -n p | cut -d &quot; &quot; -f 10)"
echo "$(nmcli device show $interface | grep IP4.ADDRESS | cut -d &quot; &quot; -f 26)"
   break
 fi
done
{% endhighlight %}
