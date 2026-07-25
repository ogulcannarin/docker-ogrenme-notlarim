# Container'lara Bağlanmak ve İçini İncelemek

## `docker exec -it con8 sh`

```bash
docker exec -it con8 sh
```

Bu komut, **çalışan bir container'ın içine girip orada komut çalıştırmayı** sağlar.

- **`exec`** = "execute" (çalıştır) — container zaten çalışıyorken, içinde yeni bir komut/process başlatır
- **`-i`** = interactive — komut satırından girdi (input) alabilmeni sağlar
- **`-t`** = tty — düzgün görünen bir terminal arayüzü açar (satır kaydırma, imleç vs. doğru çalışır)
- **`-it`** ikisi birlikte kullanıldığında, sanki container'ın içinde bir terminal açmış gibi olursun
- **`con8`** = hangi container'a bağlanacağın
- **`sh`** = container içinde çalıştırılacak komut/program — burada `sh` (shell) çalıştırılıyor, yani container'ın içine bir kabuk (shell) açıyorsun

### Neden `sh`, neden bazen `bash` değil?

Bazı imajlar (özellikle küçük/minimal olanlar, örn. Alpine tabanlı imajlar) `bash` içermez, sadece `sh` bulunur. `bash` içeren daha büyük imajlarda (`ubuntu`, `debian` tabanlı) genelde şu da kullanılır:
```bash
docker exec -it con8 bash
```
Hangisinin çalıştığı, imajın içine hangi shell'in kurulu olduğuna bağlı. Emin değilsen önce `sh` denemek güvenli bir başlangıçtır, çünkü neredeyse her Linux tabanlı imajda bulunur.

### Ne işe yarar pratikte?

Container içine girdiğinde, sanki o container'ın kendi terminaline SSH ile bağlanmışsın gibi davranabilirsin:
```bash
docker exec -it con8 sh
# artık container'ın içindesin
ls                     # dosyaları görebilirsin
cat /etc/os-release    # container'ın içindeki işletim sistemi bilgisine bakabilirsin
exit                   # container'dan çıkıp host'a geri dönersin
```

⚠️ Önemli nokta: `exit` yazdığında sadece o shell oturumundan çıkarsın, **container durmaz** — çünkü `sh` sadece container içinde ek bir process olarak açılmıştı, container'ın asıl process'i (örneğin `httpd`) çalışmaya devam eder.

## `docker container inspect con8`

```bash
docker container inspect con8
```

Bu komut, `exec`'in aksine container'ın **içine girmez** — bunun yerine container hakkında **çok detaylı, JSON formatında bir bilgi dökümü** verir. Yani "bağlanmak" değil, "container hakkında her şeyi öğrenmek" için kullanılır.

`docker inspect con8` yazmak da aynı sonucu verir (`container` kelimesi opsiyonel, ama açık yazmak "hangi kaynak türünü inceliyorum" konusunda netlik sağlar — network veya image de inspect edilebildiği için).

### Ne tür bilgiler görürsün?

| Alan | Ne gösterir |
|---|---|
| `State` | Container'ın durumu (çalışıyor mu, ne zaman başladı, PID'si ne) |
| `Config.Image` | Hangi imajdan oluşturulduğu |
| `NetworkSettings.IPAddress` | Container'ın iç ağdaki IP adresi |
| `Mounts` | Bağlı olan volume'ler / bind mount'lar |
| `HostConfig.PortBindings` | Port yönlendirmeleri (`-p` ile verilenler) |
| `Config.Env` | Tanımlı ortam değişkenleri |

### Pratik kullanım örneği

Sadece belirli bir bilgiyi çekmek istersen `--format` ile filtreleyebilirsin:
```bash
docker inspect --format='{{.NetworkSettings.IPAddress}}' con8
```
Bu, sadece container'ın IP adresini basar, koca JSON'u okumana gerek kalmaz.

## İkisi arasındaki temel fark

| | `docker exec -it con8 sh` | `docker container inspect con8` |
|---|---|---|
| Amaç | Container'ın **içine girip** komut çalıştırmak | Container **hakkında bilgi** almak |
| Etkileşim | Canlı, interaktif | Tek seferlik, statik çıktı (JSON) |
| Container'ı etkiler mi? | Hayır (sadece ek process açar) | Hayır (sadece okur, değiştirmez) |
| Ne zaman kullanılır | "Container'ın içinde neler var, dosyaları görmek istiyorum" | "Bu container'ın IP'si ne, hangi portu kullanıyor, hangi imajdan geldi" |

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)
