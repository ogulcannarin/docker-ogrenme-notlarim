# Standard Input-Output-Error ve `docker logs`

## Standard Input-Output-Error nedir?

Bu, Linux/Unix dünyasının temel bir kavramı. Her process'in (süreç) 3 standart "veri kanalı" vardır:

| Kanal | Numara (file descriptor) | Ne işe yarar |
|---|---|---|
| **stdin** | 0 | Process'e **girdi** verilen kanal (klavyeden gelen veri gibi) |
| **stdout** | 1 | Process'in **normal çıktısını** bastığı kanal (ekrana yazdığı normal mesajlar) |
| **stderr** | 2 | Process'in **hata mesajlarını** bastığı ayrı kanal |

```
Klavye → stdin (0) → [Process] → stdout (1)  → normal çıktı
                                → stderr (2)  → hata çıktısı
```

Normal çıktı (stdout) ile hata çıktısının (stderr) **ayrı kanallardan** gitmesinin sebebi: bir programın çıktısını başka bir yere yönlendirirken (örneğin bir dosyaya), hataları normal çıktıdan ayırabilmek. Örneğin:
```bash
komut > normal_cikti.log 2> hatalar.log
```

## Bunun Docker ile ilgisi ne?

**Docker container'ları, container içindeki ana process'in `stdout` ve `stderr`'ini yakalar** ve bunları "log" olarak saklar. Yani bir container çalıştırdığında, o container'ın içindeki uygulama ekrana ne yazıyorsa (normal mesaj ya da hata), Docker bunu arka planda tutar.

## `docker logs` komutu

```bash
docker logs container_id_or_container_name

# örnek:
docker logs 207adb2b63
```

Bu komut, container içindeki process'in **stdout ve stderr**'ine yazdığı her şeyi gösterir — yani container'ın "günlüğünü" okursun.

### Sık kullanılan bayraklar (options)

| Bayrak | Ne işe yarar |
|---|---|
| `--details` | Loglara ek detay bilgisi ekler |
| `-f`, `--follow` | Logları **canlı takip eder** (yeni loglar geldikçe ekrana akar, terminal kapanmaz) |
| `--since <string>` | Belirli bir zamandan **sonraki** logları gösterir (örn. `2013-01-02T13:23:37` veya `42m` gibi göreceli süre) |
| `--tail <string>` | Logların **sonundan** kaç satır gösterileceğini belirtir (varsayılan: hepsi) |
| `-t`, `--timestamps` | Her log satırının başına **zaman damgası** ekler |
| `--until <string>` | Belirli bir zamana **kadar** olan logları gösterir |

### Kullanım örnekleri

```bash
docker logs -f container1              # canlı takip et
docker logs --tail 50 container1       # son 50 satırı göster
docker logs -t container1              # zaman damgalı göster
docker logs --since 10m container1     # son 10 dakikanın loglarını göster
```

## Neden önemli?

`docker logs`, bir container'ın içine girmeden (`exec` kullanmadan) **ne olup bittiğini anlamanın en hızlı yolu**. Özellikle:
- Container beklenmedik şekilde durduysa → `docker logs` ile neden durduğunu (hata mesajını, yani stderr çıktısını) görebilirsin
- Bir uygulama beklenen şekilde çalışmıyorsa → stdout'a bastığı mesajlarla neyin yanlış gittiğini takip edebilirsin

## Bağlantıyı toparlarsak

stdin/stdout/stderr temel bir Linux kavramı → Docker, container'ın ana process'inin stdout/stderr'ini otomatik yakalar → `docker logs` komutu bu yakalanmış veriyi gösterme aracıdır.

> `docker logs` komutu ayrıca [`03-komutlar/komut-listesi.md`](../03-komutlar/komut-listesi.md) dosyasındaki genel komut referansına da eklenmeli.

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)
