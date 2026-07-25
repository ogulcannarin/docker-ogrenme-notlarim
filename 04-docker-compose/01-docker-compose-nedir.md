# Docker Compose Nedir?

## Neden ihtiyaç var?

Buraya kadar hep **tek tek** container çalıştırdık (`docker run ...`). Ama gerçek uygulamalar genelde birden fazla parçadan oluşur: bir web sunucusu, bir veritabanı, belki bir cache servisi... Bunların hepsini elle, tek tek `docker run` ile ayağa kaldırmak hem yorucu hem hataya açık.

**Docker Compose**, birden fazla container'ı **tek bir YAML dosyasında tanımlayıp, tek bir komutla hep birlikte başlatmayı/durdurmayı** sağlayan bir araç.

## Compose olmadan nasıl olurdu?

Diyelim ki bir web uygulamanın bir de PostgreSQL veritabanına ihtiyacı var. Compose olmadan:
```bash
docker network create benim-agim
docker run -d --name db --network benim-agim -e POSTGRES_PASSWORD=1234 postgres
docker run -d --name web --network benim-agim -p 8080:80 -e DB_HOST=db benim-web-app
```
Her seferinde bu uzun komutları elle yazmak, doğru sırayla çalıştırmak gerekir. Compose ile bunların hepsi tek bir `docker-compose.yml` dosyasında tanımlanır ve tek komutla çalışır: `docker compose up`.

## Temel yapı: `docker-compose.yml`

```yaml
version: "3.9"

services:
  web:
    image: benim-web-app
    ports:
      - "8080:80"
    environment:
      - DB_HOST=db
    depends_on:
      - db

  db:
    image: postgres
    environment:
      - POSTGRES_PASSWORD=1234
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

### Yapıyı parça parça açalım

| Anahtar | Ne işe yarar |
|---|---|
| `services` | Çalıştırılacak her container'ı bir "servis" olarak tanımlar |
| `image` | Hangi imajın kullanılacağı |
| `build` | (image yerine) yerel bir Dockerfile'dan imaj inşa etmek için kullanılır, örn. `build: .` |
| `ports` | Port yönlendirmesi (`docker run -p` ile aynı mantık) |
| `environment` | Ortam değişkenleri (`docker run -e` ile aynı) |
| `depends_on` | Bu servisin, başka bir servisten sonra başlamasını sağlar |
| `volumes` | Kalıcı veri saklamak için (container silinse bile veri kaybolmaz) |

> ⚠️ Dikkat: `depends_on` sadece **başlatma sırasını** garanti eder, veritabanının "gerçekten hazır" olmasını değil — uygulama çok hızlı başlayıp veritabanına henüz hazır olmadan bağlanmaya çalışabilir. Bu, ayrı bir konu olan `healthcheck` ile çözülür.

## Sık kullanılan komutlar

| Komut | Ne işe yarar |
|---|---|
| `docker compose up` | Tüm servisleri başlatır (ön planda, loglar akar) |
| `docker compose up -d` | Tüm servisleri arka planda başlatır |
| `docker compose down` | Tüm servisleri durdurur ve kaynakları (network vs.) temizler |
| `docker compose down -v` | Servisleri durdurur, **volume'leri de siler** (dikkat: veri kaybı!) |
| `docker compose ps` | Compose ile başlatılan servisleri listeler |
| `docker compose logs` | Tüm servislerin loglarını gösterir |
| `docker compose logs -f web` | Sadece `web` servisinin loglarını canlı takip eder |
| `docker compose build` | `build:` ile tanımlı servisleri (Dockerfile'dan) yeniden inşa eder |
| `docker compose restart` | Servisleri yeniden başlatır |
| `docker compose exec web sh` | Çalışan `web` servisinin içine girer (`docker exec` ile aynı mantık) |
| `docker compose stop` | Servisleri durdurur (siler, kaynakları temizlemez — `down`'dan farklı) |

## Docker Compose'un getirdiği asıl avantajlar

- **Tekrarlanabilirlik:** Aynı `docker-compose.yml` dosyasını farklı bir bilgisayarda çalıştırdığında, aynı ortam (web + db + network + volume) birebir kurulur
- **Tek komutla yönetim:** `up` / `down` ile tüm sistemin ayağa kalkması/inmesi
- **Servisler arası otomatik ağ:** Compose, servisleri otomatik olarak aynı ağa koyar; `web` servisi içinden `db` servisine sadece **isimle** (`db`) erişebilirsin — IP adresi bilmene gerek yok, Compose kendi içinde bir DNS gibi çalışır

## `docker run` ile `docker compose` karşılaştırması

| | `docker run` | `docker compose` |
|---|---|---|
| Kaç container yönetir | Tek seferde bir tane | Tek dosyada birden fazla |
| Tanım şekli | Terminalde uzun komut satırı | YAML dosyasında yapılandırılmış tanım |
| Tekrar kullanılabilirlik | Komutu tekrar yazman/hatırlaman gerekir | Dosya saklanır, her zaman aynı şekilde çalışır |
| Servisler arası ağ | Elle network oluşturup bağlaman gerekir | Otomatik oluşturulur |

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)

## Merak ettiklerim / sonraki sorular
- `healthcheck` ile "gerçekten hazır mı" kontrolü nasıl yapılır?
- Birden fazla `docker-compose.yml` dosyasını (örn. dev/prod ortamları için) birleştirmek (`-f` bayrağı) nasıl çalışır?
- `.env` dosyası ile Compose içindeki değişkenleri nasıl yönetebilirim?
