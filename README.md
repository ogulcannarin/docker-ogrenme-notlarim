# 🐳 Docker Öğrenme Notlarım

Bu repo, Docker öğrenme sürecimde tuttuğum kişisel notlardan oluşuyor. Amaç: sadece komutları ezberlemek değil, **neden var olduğunu ve ne sorunu çözdüğünü** anlayarak ilerlemek.

## 🎉 Kilometre Taşı: Docker 101 Tamamlandı!

Temel kavramlar, kurulum, image/container ilişkisi, CLI komutları ve container'lara bağlanma/inceleme konularını kapsayan "Docker 101" aşamasını tamamladım. Sıradaki hedef: Dockerfile yazımı ve gerçek bir uygulamayı containerize etmek.

## 📌 Neden bu repo?

Bir şeyi öğrenirken yazıya dökmek, hatırlamayı kalıcı hale getiriyor. Bu yüzden her yeni konuyu öğrendikçe buraya kendi cümlelerimle not düşeceğim.

## 📂 Klasör Yapısı

| Klasör | İçerik |
|---|---|
| [`00-kurulum`](./00-kurulum) | Gerçek sunucularda (AWS EC2 vs.) Docker kurulum deneyimlerim, karşılaştığım hatalar ve çözümleri |
| [`01-temel-kavramlar`](./01-temel-kavramlar) | Docker nedir, neden çıktı, sanallaştırma, hypervisor, container nedir, namespace/cgroups, Docker Engine, Windows Container |
| [`02-dockerfile-ve-imajlar`](./02-dockerfile-ve-imajlar) | Image kavramı, layer mantığı, Dockerfile yazımı |
| [`03-komutlar`](./03-komutlar) | Sık kullanılan Docker komutları ve açıklamaları |
| [`04-docker-compose`](./04-docker-compose) | Çoklu container yönetimi, docker-compose.yml örnekleri |
| [`05-pratik-projeler`](./05-pratik-projeler) | Öğrenirken yaptığım küçük denemeler, mini projeler |

### `01-temel-kavramlar` içeriği (sırasıyla okuma önerisi)

1. [Docker neden çıktı](./01-temel-kavramlar/01-docker-neden-cikti.md)
2. [Sanallaştırma nedir](./01-temel-kavramlar/02-sanallastirma.md)
3. [Hypervisor nedir (Type 1 / Type 2)](./01-temel-kavramlar/03-hypervisor.md)
4. [Container vs VM mimarisi — namespace ve cgroups](./01-temel-kavramlar/04-container-vs-vm-namespace-cgroups.md)
5. [Container nedir (net tanım)](./01-temel-kavramlar/05-container-nedir.md)
6. [Docker Engine — Daemon, REST API, CLI](./01-temel-kavramlar/06-docker-engine.md)
7. [Windows Container nedir](./01-temel-kavramlar/07-windows-container.md)

### `02-dockerfile-ve-imajlar` içeriği

1. [Image nedir, layer mantığı](./02-dockerfile-ve-imajlar/01-image-nedir.md)
2. [Dockerfile (henüz doldurulmadı)](./02-dockerfile-ve-imajlar/02-dockerfile.md)

### `03-komutlar` içeriği

1. [Kapsamlı CLI komut listesi (image, container, volume, network, sistem, compose)](./03-komutlar/komut-listesi.md)
2. [Container'lara bağlanmak ve içini incelemek (exec, inspect)](./03-komutlar/02-container-icine-baglanma-ve-inceleme.md)

### `04-docker-compose` içeriği

1. [Docker Compose nedir, neden ihtiyaç var, docker-compose.yml yapısı](./04-docker-compose/01-docker-compose-nedir.md)

### `05-pratik-projeler` içeriği

1. [İlk container denemem — run, ps, start/stop/rm farkları](./05-pratik-projeler/01-ilk-container/notlar.md)

## ✅ İlerleme Takibi

### Docker 101 (tamamlandı 🎉)
- [x] Docker neden ortaya çıktı, hangi sorunları çözdü
- [x] Sanallaştırma (Virtualization) nedir
- [x] Hypervisor nedir, Type 1 vs Type 2
- [x] Container vs VM mimarisi (namespace, cgroups)
- [x] Container nedir (net tanım)
- [x] Docker Engine (Daemon, REST API, CLI)
- [x] Windows Container nedir
- [x] Image nedir, layer (katman) mantığı
- [x] Docker kurulumu (AWS EC2 - Amazon Linux 2023 üzerinde)
- [x] Sudo olmadan Docker kullanımı (`usermod -aG docker`)
- [x] Temel komutlar (`run`, `ps`, `images`, `stop`, `rm`, `rmi`, `pull`...)
- [x] Kapsamlı CLI komut referansı (image, container, volume, network, sistem, compose)
- [x] İlk pratik container denemem (`run`, `ps`, `start`/`stop`/`rm` farkları, `-d`, `--name`)
- [x] Container'lara bağlanmak (`docker exec -it ... sh`) ve inceleme (`docker inspect`)

### Sıradaki hedefler
- [ ] İlk Dockerfile yazımı (FROM, RUN, COPY, CMD...)
- [ ] Volume ve bind mount farkı
- [ ] Docker network kavramı
- [x] Docker Compose nedir, docker-compose.yml yapısı, temel komutlar
- [ ] Bir uygulamayı containerize etme (ilk gerçek proje)

> Bu liste ilerledikçe güncellenecek. Yeni bir konu öğrendiğimde hem ilgili klasöre not ekleyeceğim hem de burada işaretleyeceğim.

## 🔑 Nasıl kullanıyorum bu repoyu?

1. Yeni bir konu öğrendiğimde ilgili klasöre `.md` dosyası olarak not düşüyorum.
2. Kendi cümlelerimle yazıyorum (kopyala-yapıştır değil) — çünkü bir şeyi kendi cümlelerinle anlatabiliyorsan gerçekten anlamışsındır.
3. Mümkünse küçük örnek/kod ekliyorum.
4. Anladığım analojileri de not ediyorum, ileride tekrar okuduğumda hatırlamamı kolaylaştırıyor.
