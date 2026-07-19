# Image Nedir?

## Tanım

Image, bir container'ın **şablonu / kalıbıdır**. İçinde bir uygulamanın çalışması için gereken her şey bulunur:
- Uygulama kodu
- Kütüphaneler ve bağımlılıklar
- Ortam değişkenleri, ayarlar
- Minimal işletim sistemi dosyaları (tam bir OS değil, sadece gerekli dosya sistemi katmanı)

## Image vs Container farkı

Bu ayrım Docker'ı anlamanın en kritik noktalarından biri:

| | Image | Container |
|---|---|---|
| Durum | Pasif, salt-okunur (read-only) şablon | Aktif, çalışan bir örnek |
| Benzetme | Class (sınıf) tanımı | Class'tan oluşturulmuş nesne (instance) |
| Değişebilir mi? | Hayır, image kendisi değişmez | Evet, çalışırken içinde değişiklik yapılabilir (ama container silinince bu değişiklikler de gider — kalıcı olması için volume gerekir) |
| Sayı ilişkisi | Bir image'dan | Birden fazla, birbirinden bağımsız container oluşturulabilir |

**Basit örnek:** `nginx` image'ını bir kez indirirsin (`docker pull nginx`), ama bu image'dan istediğin kadar container çalıştırabilirsin — her biri birbirinden habersiz, kendi izole ortamında.

```bash
docker run --name web1 nginx
docker run --name web2 nginx
docker run --name web3 nginx
```
Bu üç komut, aynı `nginx` image'ından üç ayrı, bağımsız container oluşturur.

## Image nereden gelir?

İki temel yol var:

**1. Hazır image'ları indirmek**
Docker Hub gibi bir "image deposundan" (registry) hazır image indirilebilir:
```bash
docker pull nginx
docker pull postgres
docker pull node
```

**2. Kendi image'ını oluşturmak**
Bir `Dockerfile` yazarak, kendi uygulamana özel bir image inşa edilebilir:
```bash
docker build -t benim-uygulamam .
```
(Dockerfile'ın kendisi ve nasıl yazıldığı ayrı ve daha detaylı bir konu — bu klasörde ayrı not olarak işlenecek.)

## Image katmanlı (layered) bir yapıya sahiptir

Bir image tek parça bir dosya değildir, **katmanlardan (layers)** oluşur. Her katman, önceki katmanın üzerine eklenen bir değişikliği temsil eder.

**Örnek:**
```
Katman 4: Uygulama kodun (COPY . .)
Katman 3: Kurulan bağımlılıklar (RUN npm install)
Katman 2: Node.js kurulumu
Katman 1: Ubuntu temel işletim sistemi dosyaları (FROM ubuntu)
```

### Bu katmanlı yapı neden önemli?

- **Cache (önbellek) avantajı:** Bir katman değişmediyse, Docker o katmanı yeniden oluşturmaz, önbellekten kullanır. Bu da build (inşa etme) süresini büyük ölçüde kısaltır.
- **Depolama verimliliği:** Birden fazla image aynı temel katmanı (örneğin aynı `ubuntu` katmanını) paylaşabilir, bu katman diskte sadece bir kez tutulur.
- **Değişmezlik (immutability):** Bir katman bir kez oluşturulduktan sonra değişmez; yeni bir değişiklik, üstüne yeni bir katman olarak eklenir.

## Image isimlendirme mantığı

```
nginx:1.25
│      │
│      └── tag (versiyon) — belirtilmezse "latest" varsayılır
└── image adı
```

```bash
docker pull nginx:1.25   # belirli bir versiyon
docker pull nginx        # nginx:latest ile aynı anlama gelir
```

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)

## Merak ettiklerim / sonraki sorular
- Dockerfile'daki her satır gerçekten ayrı bir katman mı oluşturuyor, yoksa bazı komutlar birleşiyor mu?
- `docker history <image>` komutu katmanları nasıl gösteriyor, deneyip bakmalıyım
