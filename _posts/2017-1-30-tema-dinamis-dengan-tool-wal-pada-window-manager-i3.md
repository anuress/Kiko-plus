---
layout: post
title: Tema Dinamis dengan Tool wal pada Window Manager i3
---
Tampilan desktop merupakan hal yang cukup penting bagi saya. Ditambah saya merupakan orang yang cepat bosan dengan tampilan desktop, sehingga seringkali saya melakukan [*ricing*](https://answers.yahoo.com/question/index?qid=20130321202718AAW9yDU) pada desktop. Saya melakukannya dengan niatan having fun, meskipun terkadang *kebablasen* sehingga memakan jatah waktu kegiatan yang lain.

Nah, belakangan terbesit keinginan untuk melakukan semacam *dynamic instant ricing* (istilah karangan saya) untuk memangkas porsi waktu *ricing* dan menghambat kebosanan saya. Kemudian saya menemukan tool bernama [`wal`](https://github.com/dylanaraps/wal).

>`wal` is a script that takes an image (or a directory of images), generates a colorscheme (using imagemagick) and then changes all of your open terminal's colorschemes to the new colors on the fly. `wal` then caches each generated colorscheme so that cycling through wallpapers while changing colorschemes is instantaneous. `wal` finally merges the new colorscheme into the Xresources db so that any new terminal emulators you open use the new colorscheme.

Jadi, apa sih yang `wal` lakukan? `wal` melakukan otomasi pembuatan colorscheme pada terminal berdasarkan sebuah image kemudian menerapkannya pada konfigurasi. Colorscheme yang dibuat oleh `wal` dapat dipakai pada beberapa program lain yang saya gunakan, seperti launcher [rofi](https://davedavenport.github.io/rofi/), dan konfigurasi window manager [i3](http://i3wm.org/).

Untuk `rofi` saya tidak melakukan konfigurasi apa-apa, karena `rofi` mengambil colorscheme Xresources. Untuk konfigurasi window manager i3, saya mensinkronkan warna pada konfigurasi i3 dengan Xresources.

```
set_from_resource $fg i3wm.color7
set_from_resource $bg i3wm.color0
```

Kemudian, saya mengatur warna border dan bar i3 dengan variabel diatas.

```
bar {
     colors {
			 statusline $fg
			 background $bg
					    #Border #Backgr #Font
			 focused_workspace  $bg $fg $bg
			 active_workspace   $bg $bg $fg
			 inactive_workspace $bg $bg $fg
			 urgent_workspace   $bg #CB4F29 $bg
	     
	}
			status_command conky -c ~/.config/i3/conkyrc
			position bottom
			tray_padding 3
			workspace_buttons yes    
			font pango:GohuFont 8
			height 25
}

gaps inner 5
gaps outer 3
for_window [class="^.*"] border pixel 3

# class                 border  backgr. text    indicator child_border
client.focused          $fg     $fg     $bg  $fg       $fg
client.focused_inactive $fg     $fg     $bg  $fg       $fg
client.unfocused        $bg     $bg     $fg  $bg       $bg
client.urgent           $fg     $fg     $bg  $fg       $fg
client.placeholder     $fg     $fg     $bg  $fg       $fg
```

Konfigurasi selesai. Untuk mengganti tema (dan wallpaper), saya menggunakan perintah `wal -i 'path-to-image'` (path-to-image merujuk pada letak gambar yang diinginkan).

Apabila path-to-image diisi dengan alamat direktori yg berisi lebih dari satu gambar, `wal` akan memilih gambar secara acak. Nah, perintah `wal -i 'path-to-image'` saya ikutkan dalam file `.xinitrc` sehingga akan otomatis tereksekusi saat saya menjalankan [X](https://en.wikipedia.org/wiki/X_Window_System). Dengan begitu, tema dekstop saya dapat berubah secara acak setiap kali saya menjalankan X.
