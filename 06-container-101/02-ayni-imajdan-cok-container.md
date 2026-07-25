# Aynı Image'dan Birden Fazla Container

## Görsel: "2 containers from the same image"

Bu, [Union File System](./01-container-felsefesi-ve-union-fs.md) mantığının somut kanıtı. Aynı image'dan (`app1:latest`) iki farklı container (`container1`, `container2`) oluşturulmuş ve her ikisi de **aynı salt-okunur (read-only) katmanlara** bağlanıyor.

```
container1        container2
     \                /
      \              /
       \            /
    [salt-okunur katmanlar]
         app1:latest
```

## Ne anlama geliyor?

- `container1` ve `container2`, image'ın içeriğini (kod, kütüphaneler vs.) **paylaşıyor**, diskte iki kere kopyalanmıyor
- Her birinin **kendi yazılabilir katmanı** var (bkz. Union File System notu) — bu katman her container için ayrı
- Bu yüzden `container1` içinde bir dosya değiştirsen, `container2`'yi hiç etkilemez — ikisi birbirinden tamamen izole, ama temelde aynı image'ı paylaşıyorlar

## Neden önemli?

Bu, "aynı image'dan istediğin kadar bağımsız container oluşturabilirsin" prensibinin görsel kanıtı:

```bash
docker run --name web1 nginx
docker run --name web2 nginx
docker run --name web3 nginx
```

Bu üç komut, aynı `nginx` image'ından üç ayrı, birbirinden bağımsız container oluşturur. Her biri:
- Kendi izole çalışma alanına (yazılabilir katman) sahiptir
- Ama aynı temel image katmanlarını (nginx'in kendisi, kütüphaneleri vs.) diskte **tekrar tekrar kopyalamadan** paylaşır

Bu, hem disk alanından tasarruf sağlar hem de container'ların birbirinden bağımsız çalışmasını garanti eder.

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)
