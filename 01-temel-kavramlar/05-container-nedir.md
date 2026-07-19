# Container Nedir?

> Bu not, "namespace/cgroups" detayına girmeden önce container'ın kendisinin ne olduğunu net şekilde tanımlamak için var. Teknik detaylar için → [`04-container-vs-vm-namespace-cgroups.md`](./04-container-vs-vm-namespace-cgroups.md)

## Tanım

Container, bir uygulamayı ve çalışması için gereken **her şeyi** (kod, kütüphaneler, bağımlılıklar, ayarlar) tek bir pakette bir araya getiren, **izole edilmiş, hafif ve çalıştırılabilir** bir birimdir.

En basit tanımla: **host işletim sisteminin kernel'ini paylaşan, ama kendi izole "dünyasında" çalışan bir process (süreç).**

## Container'ın temel özellikleri

| Özellik | Açıklama |
|---|---|
| **İzole** | Kendi dosya sistemi, ağı, process listesi vardır — diğer container'ları veya host'u görmez |
| **Hafif** | Ayrı bir işletim sistemi taşımaz, host'un kernel'ini paylaşır → MB'larca yer kaplar (VM'in GB'larına karşı) |
| **Taşınabilir** | Bir image'dan oluşturulduğu için, o image nerede çalıştırılırsa çalıştırılsın aynı şekilde davranır |
| **Kısa ömürlü olabilir** | Container'lar hızlıca oluşturulup, işini bitirip yok edilebilir (özellikle mikroservis mimarilerinde yaygın) |
| **Tekrar üretilebilir** | Aynı image'dan istediğin kadar aynı container'ı (kopya gibi) oluşturabilirsin |

## Container ile Image ilişkisi (önizleme)

- **Image** = pasif, salt-okunur bir şablon/kalıp
- **Container** = o şablondan oluşturulmuş, **çalışan, canlı** bir örnek

> Detaylı bilgi → [`02-dockerfile-ve-imajlar/notlar.md`](../02-dockerfile-ve-imajlar/notlar.md)

## Basit bir benzetme

Container'ı bir **nesne yönelimli programlamadaki "instance" (örnek)** gibi düşünebilirsin:
- Image = class (sınıf) tanımı
- Container = o sınıftan oluşturulmuş bir nesne (object)

Aynı class'tan birden fazla, birbirinden bağımsız nesne oluşturabildiğin gibi, aynı image'dan da birbirinden bağımsız birden fazla container çalıştırabilirsin.

## Container yaşam döngüsü (temel adımlar)

```
docker pull   → Image'ı indir
docker run    → Image'dan yeni bir container oluştur ve başlat
docker stop   → Container'ı durdur (silinmez, sadece durur)
docker start  → Durdurulmuş container'ı tekrar başlat
docker rm     → Container'ı tamamen sil
```

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)
