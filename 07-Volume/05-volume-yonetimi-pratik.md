# Terminalde Docker Volume Yönetimi Pratikleri

Terminal üzerinden Docker volume oluşturmak, listelemek ve kullanmak oldukça basittir. İşte adım adım temel volume işlemleri:

## 1. Yeni Bir Volume Oluşturmak

Yeni bir volume yaratmak için `create` komutunu kullanırız:

```bash
docker volume create benim_volumum
```
*(Bu komutu çalıştırdığında terminal sadece oluşturulan volume'un adını `benim_volumum` olarak sana geri döndürür.)*

## 2. Oluşturulan Volume'leri Listelemek

Docker üzerinde var olan tüm volume'leri görmek için:

```bash
docker volume ls
```
Bu komut, sistemindeki tüm volume'leri bir liste halinde gösterir.

## 3. Volume Hakkında Detaylı Bilgi Almak (Inspect)

Oluşturduğun volume'un host makinede tam olarak nerede saklandığı (`Mountpoint` bilgisi) gibi detayları görmek istersen:

```bash
docker volume inspect benim_volumum
```
Bu komut sana volume ile ilgili JSON formatında ayrıntılı bilgiler sunar.

## 4. Volume'u Bir Konteynere Bağlamak (Kullanmak)

Volume'u yarattıktan sonra en önemli adım onu bir konteynere bağlamaktır. Konteyneri başlatırken `-v` veya `--mount` parametresi kullanılır.

**Örnek Senaryo:** Bir Nginx web sunucusu başlatalım ve oluşturduğumuz `benim_volumum` adlı volume'u konteynerin içindeki `/usr/share/nginx/html` klasörüne bağlayalım:

```bash
docker run -d \
  --name benim_nginx_sunucum \
  -p 8080:80 \
  -v benim_volumum:/usr/share/nginx/html \
  nginx
```
Bu sayede konteyner silinse bile, Nginx'in HTML dosyaları `benim_volumum` içerisinde host makinede güvende kalacaktır.

## 5. Volume Silmek

Artık kullanmadığın bir volume'u silmek istersen:

```bash
docker volume rm benim_volumum
```
> **Not:** Bir volume'u silebilmen için onu kullanan aktif veya durdurulmuş hiçbir konteynerin olmaması gerekir. Hata alırsan önce ilgili konteyneri silmelisin (`docker rm konteyner_adi`).

Eğer sisteminde bulunan ve kullanılmayan (boşta duran) **tüm** volume'leri tek seferde temizlemek istersen:

```bash
docker volume prune
```
*(Bu işlem geri alınamaz, dikkatli kullanılmalıdır!)*
