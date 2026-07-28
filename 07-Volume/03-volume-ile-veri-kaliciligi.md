# Docker Volume ile Veri Kalıcılığı (Data Persistence with Volumes)

Konteynerler doğası gereği "ephemeral" yani geçicidir. Bir konteyner silindiğinde, içerisindeki `Read-Write Layer` (yazılabilir katman) da silinir ve veriler kaybolur. Özellikle veritabanları gibi kalıcı veri saklaması gereken uygulamalar için bu durum istenmez. Verileri kalıcı hale getirmek için **Docker Volume** kullanılır.

## 1. Docker Volume Nedir?

Volume'ler, Docker tarafından yönetilen ve host makinenin dosya sisteminde belirli bir alanda (Linux için genellikle `/var/lib/docker/volumes/` altında) saklanan depolama alanlarıdır.

- Konteyner silinse bile Volume içindeki veriler **silinmez**.
- Volume'ler doğrudan Docker tarafından yönetildiği için, host makinenin dizin yapısından veya işletim sisteminden bağımsızdır.
- Farklı konteynerler aynı Volume'ü paylaşabilir (böylece konteynerler arası veri paylaşımı yapılabilir).

## 2. Neden Volume Kullanmalıyız?

- **Veri Kalıcılığı:** Konteyner yeniden başlatılsa veya silinip baştan oluşturulsa bile verileriniz güvende kalır.
- **Performans:** Docker Desktop (özellikle Mac ve Windows) üzerinde, Volume kullanmak host makine üzerinden klasör bağlamaya (bind mount) göre çok daha yüksek I/O performansı sunar.
- **Yönetim Kolaylığı:** Docker CLI üzerinden (`docker volume create`, `docker volume ls`, `docker volume rm`) kolayca yönetilebilirler.
- **Yedekleme ve Taşıma:** Volume verilerini yedeklemek, geri yüklemek veya başka bir host makineye taşımak daha kolaydır.

## 3. Temel Volume Komutları

- **Volume Oluşturma:** `docker volume create veritabani_volume`
- **Volume'leri Listeleme:** `docker volume ls`
- **Volume Detaylarını Görme:** `docker volume inspect veritabani_volume`
- **Konteyneri Volume ile Başlatma:** 
  ```bash
  docker run -d --name benim_veritabanim -v veritabani_volume:/var/lib/mysql mysql:latest
  ```
  *(Burada `veritabani_volume` isimli volume, konteyner içindeki `/var/lib/mysql` dizinine bağlanmıştır.)*
- **Kullanılmayan Volume'leri Temizleme:** `docker volume prune`

Verilerinizin yaşam döngüsünü konteynerin yaşam döngüsünden ayırmak, modern Docker kullanımının en temel kurallarından biridir.
