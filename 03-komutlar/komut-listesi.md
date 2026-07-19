# Sık Kullanılan Docker Komutları

> Her komutu öğrendikçe buraya ekleyeceğim: ne işe yarıyor + basit bir örnek.

## Sistem / kurulum ile ilgili komutlar

| Komut | Ne işe yarar |
|---|---|
| `docker version` | Docker Client ve Server (Engine) versiyonlarını gösterir |
| `docker info` | Sistem bilgisi (disk kullanımı, container sayısı vs.) |
| `sudo systemctl start docker` | Docker servisini başlatır (Linux) |
| `sudo systemctl enable docker` | Docker'ı sunucu her açıldığında otomatik başlatır |
| `sudo usermod -aG docker <kullanıcı>` | Kullanıcıyı `docker` grubuna ekler → `sudo` yazmadan komut çalıştırma |

> Detaylı kurulum notları ve yaşadığım hatalar için → [`00-kurulum/aws-ec2-amazon-linux-kurulumu.md`](../00-kurulum/aws-ec2-amazon-linux-kurulumu.md)

## İmaj (Image) işlemleri

| Komut | Ne işe yarar |
|---|---|
| `docker build -t isim .` | Dockerfile'dan imaj oluşturur |
| `docker images` | Yerel imajları listeler |
| `docker pull isim` | Docker Hub'dan imaj indirir |
| `docker rmi isim` | İmajı siler |

## Container işlemleri

| Komut | Ne işe yarar |
|---|---|
| `docker run isim` | İmajdan yeni bir container çalıştırır |
| `docker ps` | Çalışan container'ları listeler |
| `docker ps -a` | Tüm container'ları (durdurulmuş dahil) listeler |
| `docker stop id` | Container'ı durdurur |
| `docker rm id` | Container'ı siler |
| `docker exec -it id bash` | Çalışan container içine terminal ile girer |
| `docker logs id` | Container loglarını gösterir |

## Notlar
> (Deneyip anladıkça buraya kendi notlarımı ekleyeceğim.)
