# TRAWPrinter - Direct Text Printing

**Delphi component for RAW or direct printing.**

## Pendahuluan

Mencoba menjawab masalah pencetakan mode teks secara langsung (*raw/direct text printing*) yang sempat dibahas di milis Delphindo beberapa hari yg lalu, saya mencoba memberikan solusinya berupa komponen Delphi. Komponen ini bernama **TRAWPrinter**. Awalnya komponen dibuat oleh Przemyslaw Jankowski (v.1.0) yg hanya berisi fungsi2 dasar pencetakan teks di win32. Kemudian saya kembangkan sedemikian rupa sehingga bisa digunakan lebih mudah dan mendukung pencetakan ke *IBM Passbook* dan *Epson LX* series printer secara *built-in*. Sebenarnya komponen ini sudah saya buat dan pakai sejak tahun 2002 untuk pencetakan kwitansi ke *IBM Passbook* printer (v.1.2). Kemudian beberapa hari yg lalu saya tambahkan dukungan pencetakan ke *Epson LX* series di v.1.5 ini.

Berbeda dengan solusi umum pencetakan teks yg menggunakan cara mengirim teks langsung ke port printer (`LPT1:`), TRAWPrinter mengirim teks ke *spooler* printer di Windows. Akses ke *spooler* printer dari Delphi telah disediakan melalui unit WinSpool (unit standar). Keuntungan dari metode ini adalah printer yg kita tuju tidak harus terpasang di port `LPT1:`, printer yg terpasang di port lain pun (serial, USB, dlsb) tetap bisa diakses. Bahkan printer yg terpasang di jaringan (*shared printer*) juga bisa diakses dgn cara yg sama. Pengiriman data ke printer yg dituju adalah menjadi tugas dari *spooler* printer. Sehingga aplikasi di 2 komputer (atau lebih) bisa menggunakan 1 printer secara bersama-sama, dgn memanfaatkan *printer sharing*. Menarik bukan? :)

## Penggunaan

Penggunaan komponen ini sangat mudah dan mirip dgn penggunaan unit Printers. Berikut adalah contoh penggunaan yg ada dalam aplikasi demo:

```pascal
procedure TForm1.Button1Click(Sender: TObject);
begin
  // empty printer name would print to default printer
  RAWPrinter1.PrinterName := '';

  RAWPrinter1.BeginDoc;  // start printing
  RAWPrinter1.WriteList(Memo1.Lines, true);  // print memo text
  RAWPrinter1.EndDoc;  // stop printing
end;
```

Adapun isi dari TMemo1 adalah sebagai berikut:

```html
<roman>roman</roman>
<courier>courier</courier>
<b>bold</b>
<i>italic</i>
<u>underline</u>
<s>strike</s>
<small>small</small>
normal
<big>big</big>
<double>double</double>
<sub>subtext</sub>
<super>supertext</super>
<right>right-aligned</right>
<center>centered</center>
<left>left-aligned</left>
```

Dan hasil pencetakannya ke printer *Epson LX-300+* adalah…

![](https://github.com/git-bee/TRAWPrinter/blob/master/output.png "TRawPrinter output result")

Perhatikan isi memo dan bandingkan dengan hasil pencetakan! :) Betul… TRAWPrinter v.1.5 mendukung beberapa *tag* dasar HTML sehingga proses penyusunan teks yg akan dicetak menjadi lebih mudah. Anda gak perlu repot2 menghapalkan kode2 ESC untuk tiap jenis format teks. Fitur *tag* HTML ini hanya bisa digunakan untuk method `WriteList()`, sedang untuk method2 lainnya harus menggunakan cara seperti berikut:

```pascal
  // font settings
  RAWPrinter1.FontName  := rfnCourier;
  RAWPrinter1.FontPitch := rfpCondensed;
  RAWPrinter1.FontStyle := [];
  // print text and move to next line
  RAWPrinter1.WriteLn('regular condensed courier');

  // font settings
  RAWPrinter1.FontName  := rfnRoman;
  RAWPrinter1.FontPitch := rfpExpanded;
  RAWPrinter1.FontStyle := [rfsBold];
  // print text and keep on current line
  RAWPrinter1.Write('bold expanded roman');
```

*See*… cara ini pun juga gak terlalu sulit, dan Anda tetap saja gak perlu menghapalkan kode2 ESC. *Property2* setting font dari TRAWPrinter gak jauh berbeda dari property TFont. Memang cara ini gak segampang menggunakan method `WriteList()` di atas. Tapi kadang -dalam situasi tertentu- kita membutuhkan kustomisasi yg lebih rumit daripada yg mampu dilakukan method `WriteList()`.

## Lebih Lanjut

Bukan itu saja fitur2 yg ditawarkan TRAWPrinter, masih banyak lagi… seperti pencetakan berkolom, setting batas (margin) untuk kertas gak standar, opsi untuk menahan kertas setelah mencetak, dan lain sebagainya. Dan kalo bener2 dirasa perlu, Anda juga bisa mengganti kode2 ESC yg digunakan komponen, misalnya untuk jenis printer lain selain Epson dan IBM. Anda cukup membuat turunan dari class ini dan mendefinisikan ulang kode2 ESC yg sesuai dgn printer Anda.

Untuk lebih detilnya, silakan Anda pelajari *source code* dari komponen ini.

Selamat mencoba dan semoga bermanfaat.

-Bee
