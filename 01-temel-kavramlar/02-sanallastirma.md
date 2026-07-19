# Sanallaştırma (Virtualization) Nedir?

## Tanım
Fiziksel bir kaynağı (donanımı), yazılım aracılığıyla birden fazla "sanal" versiyona bölmek veya soyutlamak.

Tek bir fiziksel bilgisayarı, sanki birden fazla bağımsız bilgisayarmış gibi kullanabilme yeteneği.

## Örnek senaryo
Güçlü bir sunucu üzerinde aynı anda:
- 1. sanal makinede Windows Server + muhasebe programı
- 2. sanal makinede Ubuntu + web sitesi
- 3. sanal makinede farklı bir Linux + test ortamı

çalıştırılabilir. Birbirlerinden habersiz, birbirini etkilemeden.

## Önemli nokta: Her VM kendi kernel'ini taşır
Her sanal makine, kendi tam işletim sistemi çekirdeğini (kernel) taşır. Bu yüzden:
- Her VM = tam bir işletim sistemi kadar disk/RAM tüketir
- Çok sayıda VM açmak ağırdır

## Docker farkı (önizleme)
Docker sanallaştırma yapmaz, **containerization** yapar:
- Host'un kernel'ini paylaşır
- Süreç (process) seviyesinde izolasyon sağlar
- Çok daha hafif ve hızlıdır

Detaylı karşılaştırma → `container-vs-vm.md` (henüz eklenmedi)

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)
