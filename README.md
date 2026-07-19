# 🐳 Docker Öğrenme Notlarım

Bu repo, Docker öğrenme sürecimde tuttuğum kişisel notlardan oluşuyor. Amaç: sadece komutları ezberlemek değil, **neden var olduğunu ve ne sorunu çözdüğünü** anlayarak ilerlemek.

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

## ✅ İlerleme Takibi

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
- [ ] İlk Dockerfile yazımı (FROM, RUN, COPY, CMD...)
- [x] Temel komutlar (`run`, `ps`, `images`, `stop`, `rm`, `rmi`, `pull`...)
- [ ] Volume ve bind mount farkı
- [ ] Docker network kavramı
- [ ] Docker Compose ile çoklu servis yönetimi
- [ ] Bir uygulamayı containerize etme (ilk gerçek proje)

> Bu liste ilerledikçe güncellenecek. Yeni bir konu öğrendiğimde hem ilgili klasöre not ekleyeceğim hem de burada işaretleyeceğim.

## 🔑 Nasıl kullanıyorum bu repoyu?

1. Yeni bir konu öğrendiğimde ilgili klasöre `.md` dosyası olarak not düşüyorum.
2. Kendi cümlelerimle yazıyorum (kopyala-yapıştır değil) — çünkü bir şeyi kendi cümlelerinle anlatabiliyorsan gerçekten anlamışsındır.
3. Mümkünse küçük örnek/kod ekliyorum.
4. Anladığım analojileri de not ediyorum, ileride tekrar okuduğumda hatırlamamı kolaylaştırıyor.
