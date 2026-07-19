# Windows Container Nedir?

## Buraya kadarki bağlam

Şimdiye kadar konuştuğumuz her şey (namespace, cgroups, kernel paylaşımı) aslında **Linux container'ları** için geçerliydi. Docker, aynı mantığı Windows üzerinde de sunuyor — buna **Windows Container** deniyor.

## Temel fark: Kernel

| | Linux Container | Windows Container |
|---|---|---|
| Paylaşılan kernel | Linux kernel'i | Windows kernel'i |
| İzolasyon mekanizması | namespace, cgroups | Windows'a özgü benzer mekanizmalar (Job Objects, vs.) |
| Hangi uygulamalar için | Linux'a özgü uygulamalar | Windows'a özgü uygulamalar (örn. eski .NET Framework uygulamaları) |

Container mantığının temeli aynı: **host'un kernel'ini paylaşan, izole edilmiş, hafif bir çalışma birimi**. Ama Windows Container'lar Linux kernel'i yerine Windows kernel'ini paylaşır.

## Neden önemli?

Bazı şirketlerin eski, Windows'a bağımlı uygulamaları vardır (örn. eski .NET Framework — .NET Core değil). Bu uygulamalar Linux'ta çalışmaz. Windows Container, bu tür uygulamaları da container mantığıyla paketleyip taşınabilir hale getirmeyi sağlar.

## Önemli kısıtlama: Image uyumsuzluğu

⚠️ **Linux container image'ları Windows Container'da çalışmaz, Windows container image'ları da Linux'ta çalışmaz.** Çünkü kernel'ler tamamen farklı ve bir container, kendi kernel'ini getirmez — host'unkini kullanır.

Yani:
- `FROM ubuntu` ile başlayan bir Dockerfile → Linux container üretir
- `FROM mcr.microsoft.com/windows/servercore` ile başlayan bir Dockerfile → Windows container üretir

Bu ikisi karışık kullanılamaz.

## Peki Windows'ta Linux container'ları nasıl çalışıyor?

Bu can alıcı bir soru, çünkü çoğu geliştirici Windows bilgisayarında Docker Desktop kurup rahatça Linux tabanlı image'lar (`nginx`, `postgres` vs.) çalıştırabiliyor. Bunun sebebi:

**Docker Desktop, Windows'ta arka planda hafif bir Linux sanal makinesi (VM) çalıştırır** (WSL2 - Windows Subsystem for Linux 2 altyapısını kullanarak). Yani sen aslında:
- Görünürde: Windows'ta doğrudan Linux container çalıştırıyormuşsun gibi hissedersin
- Gerçekte: Docker Desktop, arka planda hafif bir Linux kernel'i (WSL2 üzerinden) ayağa kaldırır ve Linux container'lar o kernel üzerinde çalışır

Bu yüzden Docker Desktop'ta iki mod vardır:
- **Linux containers modu** (varsayılan, WSL2 tabanlı) → Linux image'ları çalıştırır
- **Windows containers modu** → gerçek Windows kernel'i üzerinde Windows image'ları çalıştırır

İkisi arasında geçiş yapılabilir ama aynı anda ikisi birden çalışmaz.

## Özet karşılaştırma

```
Linux sunucu
└── Docker Engine → doğrudan Linux kernel'ini paylaşır → Linux container'lar

Windows (Docker Desktop, Linux containers modu)
└── Docker Engine → WSL2 üzerinden hafif bir Linux VM → o VM'in kernel'i paylaşılır → Linux container'lar

Windows (Docker Desktop, Windows containers modu)
└── Docker Engine → doğrudan Windows kernel'ini paylaşır → Windows container'lar
```

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)
