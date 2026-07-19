# Docker Neden Ortaya Çıktı?

## Docker öncesi yaşanan sorunlar

### 1. "Bende çalışıyor" sorunu
Bir uygulama geliştiricinin bilgisayarında sorunsuz çalışırken, başka bir ortama (test sunucusu, production) taşındığında bozuluyordu. Sebep genelde:
- Farklı programlama dili / kütüphane versiyonları
- Farklı işletim sistemi ayarları
- Farklı ortam değişkenleri

### 2. Sanal makineler (VM) çok ağırdı
İzolasyon için kullanılan VM'ler:
- Kendi işletim sistemi çekirdeğini taşıdığı için GB'larca yer kaplıyordu
- Başlaması dakikalar sürüyordu
- Kaynak (RAM/CPU) tüketimi yüksekti

### 3. Kurulum / dependency cehennemi
Yeni bir sunucuya veya yeni bir ekip üyesinin bilgisayarına projeyi kurmak, uzun ve hataya açık bir süreçti (paket kur, versiyon indir, ayar yap...).

### 4. Ölçeklenme ve dağıtım zordu
Aynı uygulamayı birden fazla sunucuya kurmak/güncellemek manuel ve karmaşıktı.

## Docker'ın çözümü

Docker, **container** denen hafif paketler sunuyor:
- Uygulama + tüm bağımlılıkları tek pakette
- Her yerde aynı şekilde çalışır (ortam tutarlılığı)
- VM gibi ağır değil → host'un kernel'ini paylaşır, sadece uygulama katmanını izole eder
- Saniyeler içinde başlar
- Image'lar kolayca paylaşılabilir, versiyonlanabilir

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde 2-3 cümlelik bir özet yazacağım — böylece gerçekten anlayıp anlamadığımı test etmiş olurum.)
