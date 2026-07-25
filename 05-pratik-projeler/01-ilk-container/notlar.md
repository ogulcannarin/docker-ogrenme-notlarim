# İlk Container Denemem

Bu not, ilk kez `docker run` ile container çalıştırdığımda denediğim komutları ve her birinden ne öğrendiğimi içeriyor.

## 1. `docker run hello-world`

```bash
docker run hello-world
```

Docker kurulumunu test etmek için kullanılan klasik komut. Arka planda şunlar olur:
1. `hello-world` imajı yerelde yoksa Docker Hub'dan indirilir (`pull`)
2. İmajdan yeni bir container oluşturulur ve çalıştırılır
3. Container basit bir mesaj basar ("Hello from Docker!") ve **işi bitince kendiliğinden durur**

## 2. `docker ps`

```bash
docker ps
```

Sadece **şu an çalışan** container'ları listeler. `hello-world` işini bitirip durduğu için bu komuttan sonra listede görünmez — çünkü artık çalışmıyor.

## 3. `docker ps -a`

```bash
docker ps -a
```

`-a` (all) bayrağı ile **durmuş olanlar dahil tüm container'ları** görürsün. `hello-world`'ün çalışıp durduğunu burada görebilirsin, `STATUS` sütununda "Exited" yazar.

## 4. `docker run --name container1 hello-world`

```bash
docker run --name container1 hello-world
```

`--name` bayrağı ile container'a **kendi seçtiğin bir isim** verirsin. İsim vermezsen Docker rastgele bir isim üretir (örn. `funny_einstein`). İsimli container'lar üzerinde işlem yapmak (`stop`, `rm`, `start`) çok daha kolaydır.

## 5. `docker rm container1`

```bash
docker rm container1
```

Bu, **durmuş olan** `container1`'i tamamen siler. Container zaten `hello-world` görevini bitirip durduğu için (Exited durumda), doğrudan silinebildi.

> ⚠️ Önemli kural: `docker rm` sadece **durmuş** container'ları siler. Çalışan bir container'ı silmeye çalışırsan hata alırsın — önce `docker stop` gerekir (ya da `-f` bayrağı, aşağıda).

## 6. `docker start container1` — DİKKAT: sıralama hatası

Bir önceki adımda `container1`'i **silmiştim** (`docker rm container1`). Silinen bir container'ı `docker start` ile tekrar başlatmak mümkün değil, çünkü artık sistemde yok.

`docker start`, sadece **durdurulmuş ama silinmemiş** bir container'ı tekrar çalıştırmak için kullanılır. Doğru kullanım sırası şöyle olmalı:
```bash
docker stop container1   # durdur (silinmez)
docker start container1  # tekrar başlat
```
`rm` ile `start` birbirini takip edemez — `rm` = kalıcı silme, `start` = var olan bir şeyi yeniden ayağa kaldırma.

**Ders:** Bir container'ı sonradan tekrar kullanmayı düşünüyorsan `rm` değil `stop` kullanmalıyım.

## 7. `docker run -d --name container2 httpd`

```bash
docker run -d --name container2 httpd
```

- `httpd` = Apache web sunucusu imajı
- `-d` (detached) = container'ı **arka planda** çalıştırır, terminali kilitlemez

`httpd` gibi sürekli çalışması gereken (bir web sunucusu gibi) uygulamalar için `-d` kullanmak standarttır. `docker ps` yazdığında bu container'ı **çalışır durumda** görürsün — çünkü Apache, `hello-world`'ün aksine sürekli "dinlemede" kalan bir servistir, işi bitip durmaz.

## 8. `docker run --name con3 httpd` — `-d` olmadan

```bash
docker run --name con3 httpd
```

Burada `-d` **yok**. Bu yüzden container ön planda (foreground) çalışır — terminal Apache'nin loglarını canlı basmaya başlar ve komut satırı **kilitlenir**. Container'ı durdurmak için `Ctrl+C` yapmak gerekir (bu genelde container'ı da durdurur).

**Ders:** Aynı imaj (`httpd`), ama `-d` olup olmaması deneyimi tamamen değiştiriyor. `container2` arka planda sessizce çalışırken, `con3` terminali işgal ediyor.

## 9. `docker run -d name con4 httpd` — yazım hatası

```bash
docker run -d name con4 httpd
```

Burada küçük ama önemli bir hata var: `name` yazılmış ama başında `--` eksik. Docker bunu bir bayrak olarak değil, **çalıştırılacak komutun bir parçası** olarak yorumlar. Doğrusu:

```bash
docker run -d --name con4 httpd
```

**Ders:** Bayrakların başındaki `--` (çift tire) çok önemli — unutulursa komut beklendiği gibi çalışmaz, ya hata alınır ya da container rastgele bir isimle oluşur.

## 10. Çalışan bir container'ı silme: `docker rm -f con4`

```bash
docker rm -f con4
```

Normalde `docker rm`, **sadece durmuş** container'ları siler; çalışan birini silmeye çalışırsan hata alınır:
```
Error response from daemon: You cannot remove a running container
```

`-f` (force) bayrağı bu kuralı aşar: Docker önce container'ı **zorla durdurur**, sonra siler — tek komutla. Yani `docker stop con4 && docker rm con4` yapmak yerine kısayol olarak `docker rm -f con4` kullanılabilir.

## Genel özet — çıkardığım dersler

1. **`ps` vs `ps -a`**: `ps` sadece çalışanı, `ps -a` her şeyi (durmuşlar dahil) gösterir.
2. **`rm` kalıcıdır, `stop` değildir**: Bir container'ı ileride tekrar kullanmayı düşünüyorsam `stop` kullanmalıyım, `rm` değil.
3. **`start`, silinmiş bir container'ı geri getiremez** — sadece durdurulmuş olanı yeniden başlatır.
4. **`-d` bayrağı, kalıcı/sunucu tipi uygulamalar için şart** (`httpd` gibi) — yoksa terminal kilitlenir.
5. **Bayrakların başındaki `--` unutulmamalı**, yoksa komut yanlış yorumlanır.
6. **`docker rm -f`**, çalışan bir container'ı durdurup silmenin hızlı yolu.

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)
