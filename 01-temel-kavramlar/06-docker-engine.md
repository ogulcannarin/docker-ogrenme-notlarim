# Docker Engine (Daemon, REST API, CLI)

## Docker Engine nedir?

Docker Engine, Docker'ın **çalışma motorudur** — yani container'ları oluşturan, çalıştıran, durduran ve yöneten asıl yazılımdır. Docker kurulduğunda aslında bilgisayarına kurulan şey budur.

Docker Engine üç ana parçadan oluşur:

```
┌─────────────────────────────────────────┐
│              Docker CLI                 │  ← sen komut yazarsın
│         (docker run, docker ps...)      │
└───────────────────┬─────────────────────┘
                     │ REST API isteği
                     ▼
┌─────────────────────────────────────────┐
│           Docker Daemon (dockerd)       │  ← asıl işi yapan
│  image indirme, container oluşturma,    │
│  ağ/volume yönetimi...                  │
└─────────────────────────────────────────┘
```

## 1. Docker Daemon (`dockerd`)

Arka planda **sürekli çalışan bir servis (background process)** — asıl işi yapan bileşen budur.

Görevleri:
- Image'ları indirmek (Docker Hub'dan `pull`)
- Container'ları oluşturmak ve çalıştırmak
- Ağları (network) ve volume'leri yönetmek
- Container'ların durumunu takip etmek

> Hatırlarsan kurulum notlarımda `sudo systemctl start docker` komutuyla başlattığım şey aslında bu daemon'du.

Sen bir `docker` komutu yazdığında, komutu **gerçekte çalıştıran** CLI değil, daemon'dur. CLI sadece isteği daemon'a iletir.

## 2. REST API

CLI ile daemon arasındaki iletişim bir **REST API** üzerinden gerçekleşir. Yani "container oluştur", "container listele" gibi her istek, aslında arka planda HTTP üzerinden daemon'a gönderilen bir API çağrısıdır.

Bunun önemi şu: **CLI, daemon ile konuşmanın tek yolu değildir.**
- Docker Desktop'ın grafik arayüzü de bu API'yi kullanır
- Kendi yazacağın bir script veya program da doğrudan bu API'ye istek atarak container yönetebilir
- Varsayılan olarak bu API, bir Unix socket üzerinden (`/var/run/docker.sock`) dinler; istenirse TCP portu üzerinden de açılabilir

## 3. Docker CLI (`docker` komutu)

Terminalde yazdığın `docker run`, `docker ps`, `docker build` gibi komutlar aslında bu CLI aracının komutlarıdır. CLI'nin tek işi:
1. Senin yazdığın komutu al
2. Bunu bir REST API isteğine çevir
3. İsteği daemon'a gönder
4. Daemon'dan gelen cevabı sana okunabilir şekilde göster

## Tüm akışı örnekle özetlersek

```bash
docker run nginx
```

Bu komutu çalıştırdığında arka planda olanlar:

1. **Docker CLI** bu komutu okur
2. CLI, bunu bir REST API isteğine çevirip **Docker Daemon**'a gönderir
3. **Daemon**:
   - `nginx` image'ı yerelde yoksa Docker Hub'dan indirir (`pull`)
   - Namespace ve cgroups ile izole bir ortam hazırlar
   - Bu ortamda container'ı başlatır
4. Daemon, işlemin sonucunu API üzerinden CLI'ye bildirir
5. CLI, sonucu senin terminalinde okunabilir şekilde gösterir

## Neden bu ayrım önemli?

Bu üçlü yapı (CLI / API / Daemon) Docker'a esneklik kazandırır:
- Farklı arayüzler (CLI, GUI, kendi scriptlerin) aynı daemon ile konuşabilir
- Daemon uzak bir sunucuda çalışırken, CLI kendi bilgisayarından o uzak daemon'a bağlanabilir (`DOCKER_HOST` ayarı ile)

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)

## Merak ettiklerim / sonraki sorular
- `docker.sock` dosyasına erişimi olan herkes root yetkisiyle container çalıştırabilir mi? (güvenlik açısından önemli görünüyor)
- Uzak bir daemon'a CLI ile nasıl bağlanılır (`DOCKER_HOST`, `docker context` komutları)?
