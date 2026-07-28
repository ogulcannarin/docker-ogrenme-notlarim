# Bind Mount Nedir? (Bind Mounts)

Docker'da veri kalıcılığını sağlamanın bir diğer yolu **Bind Mount** kullanmaktır. Bind Mount, host makinenizdeki (kendi bilgisayarınız veya sunucunuz) belirli bir dosya veya klasörü, doğrudan konteynerin içindeki bir yola bağlamanızı (map etmenizi) sağlar.

## 1. Bind Mount'un Özellikleri

- Volume'lerin aksine Bind Mount'lar, host makinede Docker'ın yönettiği özel bir alanda değil, **sizin belirlediğiniz herhangi bir dizinde** çalışır.
- Konteyner içindeki süreçler, host makinenin dosya sistemini değiştirebilir. Host makinede yapılan bir değişiklik anında konteynere yansır, aynı şekilde konteynerde yapılan değişiklik de anında host makineye yansır.
- Tam dosya yolunu (absolute path) belirtmeniz gerekir.

## 2. Kullanım Senaryoları (Ne Zaman Kullanılır?)

Bind mount'lar en çok **Geliştirme (Development) Ortamlarında** tercih edilir:

- **Canlı Kodlama (Live Reload):** Kodunuzu kendi bilgisayarınızdaki bir IDE'de (örneğin VS Code) yazarken, bu kod klasörünü konteynere bind mount ile bağlarsınız. Kodunuzu kaydedip değiştirdiğiniz anda değişiklikler konteyner içindeki uygulamaya yansır ve sunucuyu yeniden başlatmanıza veya imajı yeniden build etmenize gerek kalmaz.
- **Ayar (Config) Dosyalarını Paylaşma:** Host makinedeki bir yapılandırma dosyasını (örneğin Nginx veya DNS ayarları) doğrudan konteynere aktarmak için idealdir.

## 3. Bind Mount Nasıl Kullanılır?

Bind mount kullanırken `-v` veya `--mount` flag'i kullanılabilir. Host makinedeki mutlak yolu belirtmeniz şarttır.

**Örnek:** Host makinedeki `/Users/ogulcan/projem` klasörünü, konteyner içindeki `/app` dizinine bağlamak:

```bash
docker run -d \
  --name web_uygulamam \
  -v /Users/ogulcan/projem:/app \
  node:18
```

## 4. Volume vs Bind Mount Karşılaştırması

| Özellik | Volume | Bind Mount |
| :--- | :--- | :--- |
| **Yönetim** | Docker tarafından yönetilir. | İşletim sistemi / Kullanıcı tarafından yönetilir. |
| **Konum** | `/var/lib/docker/volumes/...` | Host makinedeki herhangi bir dizin. |
| **Kullanım Yeri** | Veritabanları, kalıcı üretim verileri. | Yazılım geliştirme, kod paylaşımı. |
| **Bağımlılık** | Host dosya yapısından bağımsızdır. | Host dosya yapısına bağımlıdır (yollar değişebilir). |

Özetle; geliştirmeyi hızlandırmak için projenizin kaynak kodlarını konteynere aktarırken **Bind Mount**, uygulamanın ürettiği kalıcı verileri (veritabanı gibi) saklarken **Volume** kullanmalısınız.
