# 🐳 Docker Öğrenme Notlarım

Bu repo, Docker öğrenme sürecimde tuttuğum kişisel notlardan oluşuyor. Amaç: sadece komutları ezberlemek değil, **neden var olduğunu ve ne sorunu çözdüğünü** anlayarak ilerlemek.

## 📌 Neden bu repo?

Bir şeyi öğrenirken yazıya dökmek, hatırlamayı kalıcı hale getiriyor. Bu yüzden her yeni konuyu öğrendikçe buraya kendi cümlelerimle not düşeceğim.

## 📂 Klasör Yapısı

| Klasör | İçerik |
|---|---|
| [`00-kurulum`](./00-kurulum) | Gerçek sunucularda (AWS EC2 vs.) Docker kurulum deneyimlerim, karşılaştığım hatalar ve çözümleri |
| [`01-temel-kavramlar`](./01-temel-kavramlar) | Docker nedir, neden çıktı, sanallaştırma, hypervisor, container vs VM farkı |
| [`02-dockerfile-ve-imajlar`](./02-dockerfile-ve-imajlar) | Dockerfile yazımı, image kavramı, layer mantığı |
| [`03-komutlar`](./03-komutlar) | Sık kullanılan Docker komutları ve açıklamaları |
| [`04-docker-compose`](./04-docker-compose) | Çoklu container yönetimi, docker-compose.yml örnekleri |
| [`05-pratik-projeler`](./05-pratik-projeler) | Öğrenirken yaptığım küçük denemeler, mini projeler |

## ✅ İlerleme Takibi

- [x] Docker neden ortaya çıktı, hangi sorunları çözdü
- [x] Sanallaştırma (Virtualization) nedir
- [x] Hypervisor nedir, Type 1 vs Type 2
- [x] Container vs VM mimarisi (namespace, cgroups)
- [x] Docker kurulumu (AWS EC2 - Amazon Linux 2023 üzerinde)
- [x] Sudo olmadan Docker kullanımı (`usermod -aG docker`)
- [ ] İlk Dockerfile yazımı
- [x] Temel komutlar (`run`, `ps`, `images`, `stop`, `rm`, `rmi`, `pull`...)
- [ ] Image katmanları (layers) ve cache mantığı
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
