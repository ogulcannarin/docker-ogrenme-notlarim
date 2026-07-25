# Container Felsefesi ve Union File System

## Container Felsefesi (Summary)

Bir eğitim videosundan aldığım özet slaytında container'ların temel felsefesi 4 maddede özetleniyordu:

### 1. Container = Tek uygulama (One application per container)

Her container, **tek bir uygulamayı** çalıştırmak için tasarlanır. Bir container içine hem web sunucusu hem veritabanı hem cache koymak "doğru" bir yaklaşım değildir — her biri kendi container'ında olmalı.

> Bu, Docker Compose'da `web` ve `db`'yi neden **ayrı servisler** olarak tanımladığımızın da temel sebebi.

### 2. Image = önceden hazırlanmış gereksinimler

Container'lar **image**'dan oluşturulur ve o uygulamanın çalışması için gereken her şey (kod, kütüphaneler, ayarlar) **image seviyesinde** önceden hazırlanmış olmalıdır. Container çalıştığında "bir şey eksik, hadi şimdi kurayım" durumu olmamalı — her şey image'a inşa aşamasında (build time) konulmuş olmalı.

### 3. Container = geçici (disposable), Image = değişmez (immutable)

- **Image** değişmez — bir kere oluşturulduktan sonra o image'ın içeriği sabittir
- **Container** geçicidir — istediğin an silinip, aynı image'dan yeniden oluşturulabilir

### 4. En kritik felsefe: Sorunu container'ı düzelterek değil, image'ı düzelterek çöz

Bir container'da sorun varsa:
- ❌ Container'ın içine girip (`docker exec`) elle dosya değiştirip "düzeltmeye" çalışmak **yanlış yaklaşım**
- ✅ Doğru yaklaşım: container'ı durdur, sil, **image'ı düzelt** (Dockerfile'ı güncelle, yeniden build et), yeni image'dan yeni bir container oluştur

**Neden?** Container'ın içine girip elle düzeltme yaparsan, o düzeltme **sadece o container'a özgü** kalır. Container silinip yeniden oluşturulduğunda (sunucu yeniden başlayabilir, ölçeklenme olabilir, vs.) düzeltmen kaybolur. Ama image'ı düzeltirsen, o düzeltme **her yeni container'da** kalıcı olur.

> `docker exec` ile container içine girmek **inceleme/debug** için harika ama **kalıcı çözüm** için kullanılmamalı.

## Union File System

Bu, image'ların "layer" (katman) yapısının **teknik temeli**.

### Image'ın yapısı

Bir image, birden fazla **salt-okunur (read-only) katmandan** oluşur:

```
Image (Read-only Layers) — app1:latest
├── /bin /etc /lib /mnt /opt /root /sbin /sys /usr
├── /dev /media /proc /run /srv /tmp
└── /app/app1.java  /test/a.txt
```

Bu dosyalar **image**'ın parçasıdır ve **değiştirilemez**.

### Peki container çalışırken dosya değiştirebiliyoruz, bu nasıl mümkün?

İşte **Union File System**'in sihri burada: Bir container çalıştığında, Docker bu salt-okunur image katmanlarının **üstüne** yeni, **yazılabilir (writable) bir katman** ekler:

```
┌─────────────────────────────────┐
│  Writable Layer of Container     │  ← senin yaptığın her değişiklik burada
├─────────────────────────────────┤
│  Image (Read-only Layers)        │  ← değişmez, sabit
└─────────────────────────────────┘
```

Container içinde bir dosya oluşturduğunda, sildiğinde ya da değiştirdiğinde, bu işlem **sadece en üstteki yazılabilir katmanda** gerçekleşir. Alttaki image katmanlarına asla dokunulmaz.

### Bu neden önemli?

- Aynı image'dan **birden fazla container** çalıştırabilirsin, her biri kendi ince yazılabilir katmanına sahip olur, ama **aynı salt-okunur image katmanlarını paylaşırlar** → disk alanından tasarruf
- Container silindiğinde, sadece o **yazılabilir katman** yok olur — image (alttaki sabit katmanlar) hiç etkilenmez
- Bu yüzden bir container'ı silip aynı image'dan yeniden oluşturduğunda, her zaman **image'ın orijinal, temiz haline** dönersin

### İki fikrin bağlantısı

Felsefedeki "container'ı düzeltme, image'ı düzelt" kuralı, Union File System mimarisinden **doğrudan kaynaklanıyor**: Container'daki değişiklikler geçici bir katmanda tutulduğu için, o katman silindiğinde her şey kaybolur. Kalıcı olmasını istediğin şey, mutlaka image'ın (alt, sabit katmanların) bir parçası olmalı.

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)
