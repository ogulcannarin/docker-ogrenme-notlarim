# Docker Kurulumu — AWS EC2 (Amazon Linux 2023)

Bu not, Docker'ı gerçek bir AWS EC2 sunucusuna kurarken yaşadığım deneyimi ve öğrendiklerimi içeriyor.

## Kurulum Ortamı

Kurulumu **AWS EC2** üzerinde çalışan bir sunucuya yaptım.

⚠️ **Önemli ders:** EC2'nin varsayılan işletim sistemi genelde **Amazon Linux**'tur, **Ubuntu değildir**. Bu ikisi farklı paket yöneticileri kullanır:

| İşletim Sistemi | Paket Yöneticisi | Örnek Komut |
|---|---|---|
| Ubuntu / Debian | `apt` | `sudo apt install docker.io` |
| Amazon Linux | `dnf` (veya eski sürümlerde `yum`) | `sudo dnf install docker` |

Sunucunun hangi işletim sistemi olduğunu öğrenmek için:
```bash
cat /etc/os-release
```

## Amazon Linux 2023 Üzerine Kurulum

Benim sunucum Amazon Linux 2023 çıktı, bu yüzden Ubuntu dokümantasyonundaki `apt` komutları çalışmadı. Doğru yöntem şu oldu:

### 1. Docker'ı kur
```bash
sudo dnf install docker -y
```

### 2. Docker servisini başlat
```bash
sudo systemctl start docker
```

### 3. Sunucu her açıldığında Docker'ın otomatik başlaması için
```bash
sudo systemctl enable docker
```

## Kurulumu Test Etme

Docker'ın düzgün çalışıp çalışmadığını test etmek için:
```bash
sudo docker run hello-world
```

Bu komut küçük bir test image'ı indirir, çalıştırır ve "Hello from Docker!" mesajı basıp çıkar. Bu mesajı görürsen kurulum başarılı demektir.

Versiyon bilgilerini görmek için:
```bash
docker version
```

## Sudo Olmadan Docker Kullanma

Varsayılan olarak her `docker` komutunun başına `sudo` yazmak gerekir. Bunu kaldırmak için kullanıcını `docker` grubuna eklemek gerekiyor:

```bash
sudo usermod -aG docker ec2-user
```

> **Not:** `ec2-user` yerine kendi kullanıcı adını yaz.

Bu değişikliğin etkili olması için:
1. Mevcut SSH oturumundan çık: `exit`
2. Sunucuya tekrar SSH ile bağlan

Bundan sonra `sudo` yazmadan `docker ...` komutlarını çalıştırabilirsin.

## Sık Karşılaşılan Hatalar

### `-bash: dpkg: command not found` veya `sudo: apt: command not found`
**Sebep:** Ubuntu için yazılmış komutları Amazon Linux üzerinde çalıştırmaya çalışıyorsun.
**Çözüm:** `apt` yerine `dnf` (veya `yum`) kullan.

### `sudo: docker: command not found`
**Sebep:** Docker henüz kurulmamış ya da kurulum başarısız olmuş.
**Çözüm:** Kurulum adımlarını tekrar kontrol et, `sudo dnf install docker -y` komutunu çalıştır.

### `permission denied` (docker.sock ile ilgili)
**Sebep:** Kullanıcı `docker` grubunda değil, bu yüzden `sudo` gerekiyor.
**Çözüm:** [Sudo Olmadan Docker Kullanma](#sudo-olmadan-docker-kullanma) bölümündeki adımları izle.

## Kurulum Günlüğüm (Timeline)

- ✅ AWS EC2 sunucusuna SSH ile bağlandım
- ❌ İlk denemede Ubuntu (`apt`) komutlarını kullandım → hata aldım
- 🔍 Sunucunun aslında Amazon Linux 2023 olduğunu keşfettim
- ✅ `sudo dnf install docker -y` ile kurulumu başarıyla tamamladım
- ✅ `sudo systemctl start docker` ile servisi başlattım
- ✅ `docker run hello-world` testi başarılı oldu
- ✅ `usermod -aG docker` ile sudo'suz kullanım sağladım
- ✅ `docker version` çıktısını doğruladım (Client ve Server API uyumlu)

## Faydalı Kaynaklar

- [Docker Resmi Dokümantasyonu - Ubuntu Kurulumu](https://docs.docker.com/engine/install/ubuntu/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Desktop İndirme (Windows/Mac)](https://www.docker.com/products/docker-desktop/)
