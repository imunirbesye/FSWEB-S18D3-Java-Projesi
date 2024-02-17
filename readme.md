# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın. 1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci as o, islem as i WHERE o.ogrno = i.ogrno;

    2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
    	SELECT k.kitapadi, t.turadi from kitap as k, tur as t WHERE k.turno = t.turno AND t.turadi = "Fıkra" OR t.turadi = "Hikaye";

    3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
    	SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi FROM ogrenci as o, islem as i, kitap as k WHERE o.ogrno=i.ogrno AND i.kitapno=k.kitapno;
    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
    	SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci as o INNER JOIN islem as i ON o.ogrno=i.ogrno;

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
    	SELECT k.kitapadi, t.turadi FROM kitap as k INNER JOIN tur as t ON k.turno=t.turno WHERE t.turadi="Fıkra" OR t.turadi="Hikaye";

    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
    	SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi, o.sinif FROM ogrenci as o INNER JOIN islem as i ON i.ogrno=o.ogrno INNER JOIN kitap as k ON k.kitapno=i.kitapno WHERE o.sinif in ('10B', '10C');

    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
    	SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci as o LEFT JOIN islem as i ON o.ogrno = i.ogrno;

    8) Kitap almayan öğrencileri listeleyin.
    	SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci as o LEFT JOIN islem as i ON o.ogrno = i.ogrno WHERE i.ogrno IS NULL;

    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
    	SELECT k.kitapno, k.kitapadi, count(i.kitapno) FROM kitap k INNER JOIN islem i ON i.kitapno=k.kitapno GROUP BY k.kitapno, k.kitapadi ORDER BY k.kitapno asc;

    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
    	SELECT k.kitapno, k.kitapadi, count(i.kitapno) as total FROM islem i RIGHT JOIN kitap as k ON i.kitapno=k.kitapno GROUP BY k.kitapno, k.kitapadi ORDER BY k.kitapno asc;

    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
    	SELECT o.ograd, o.ogrsoyad, k.kitapadi FROM ogrenci o LEFT JOIN islem i ON i.ogrno=o.ogrno LEFT JOIN kitap k ON k.kitapno=i.kitapno;

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
    	SELECT o.ograd, o.ogrsoyad, k.kitapadi, i.atarih, t.turadi, y.yazarad, y.yazarsoyad FROM ogrenci o
    	LEFT JOIN islem i ON i.ogrno=o.ogrno
    	LEFT JOIN kitap k ON k.kitapno=i.kitapno
    	LEFT JOIN yazar y ON y.yazarno=k.yazarno
    	LEFT JOIN tur t ON t.turno=k.turno

    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
    	SELECT o.ograd, o.ogrsoyad, count(i.ogrno) as total_read FROM ogrenci as o INNER JOIN islem as i ON o.ogrno = i.ogrno WHERE o.sinif IN ('10A', '10B') GROUP BY o.ograd, o.ogrsoyad ORDER BY total_read desc;

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG

    	SELECT AVG(k.sayfasayisi) from kitap as k;

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
    	SELECT k.* FROM kitap k WHERE k.sayfasayisi > (SELECT AVG(k.sayfasayisi) FROM kitap as k);

    16) Öğrenci tablosundaki öğrenci sayısını gösterin
    	SELECT count(o.ogrno) FROM ogrenci o;

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
    	SELECT count(o.ogrno) as toplam_sayi from ogrenci o;


    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
    	SELECT distinct o.ograd from ogrenci o;

    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
    	SELECT k.kitapadi, k.sayfasayisi FROM kitap as k ORDER BY k.sayfasayisi DESC LIMIT 1;

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    	SELECT k.kitapadi, k.sayfasayisi FROM kitap as k ORDER BY k.sayfasayisi DESC LIMIT 1;

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
    	SELECT MIN(k.sayfasayisi) from kitap as k;

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    	SELECT k.kitapadi, k.sayfasayisi FROM kitap as k WHERE k.sayfasayisi = (SELECT MIN(k.sayfasayisi) FROM kitap as k);

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
    	SELECT k.kitapadi, k.sayfasayisi FROM kitap as k INNER JOIN tur as t ON t.turno=k.turno WHERE t.turadi='DRAM' ORDER BY k.sayfasayisi DESC LIMIT 1;

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
    	SELECT count(k.sayfasayisi) FROM ogrenci o INNER JOIN islem i ON o.ogrno=i.ogrno INNER JOIN kitap k ON i.kitapno=k.kitapno WHERE o.ogrno=15;

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
    	SELECT o.ograd, count(o.ograd) FROM ogrenci o GROUP BY o.ograd ORDER BY o.ograd;

    26) Her sınıftaki öğrenci sayısını bulunuz.
    	SELECT o.sinif, count(o.ogrno) from ogrenci o group by o.sinif ORDER BY o.sinif;

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
    	SELECT o.sinif, o.cinsiyet, count(o.ogrno) FROM ogrenci o GRUOPU BY o.sinif, o.cinsiyet ORDER BY o.sinif, o.cinsiyet;

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
    	SELECT o.ograd, o.ogrsoyad, sum(k.sayfasayisi) as total_read from ogrenci o INNER JOIN islem i ON o.ogrno=i.ogrno INNER JOIN kitap k on k.kitapno = i.kitapno GROUP BY o.ograd, o.ogrsoyad ORDER BY total_read DESC;


    29) Her öğrencinin okuduğu kitap sayısını getiriniz.
    	SELECT o.ograd, o.ogrsoyad, count(i.islemno) as total_book from ogrenci o INNER JOIN islem i ON o.ogrno=i.ogrno GROUP BY o.ograd, o.ogrsoyad ORDER BY total_book DESC;
