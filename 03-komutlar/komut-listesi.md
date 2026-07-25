# Docker CLI Komutları

> Bu dosya, öğrendiğim tüm Docker CLI komutlarının kategorilere ayrılmış, düzenli bir referansı. Yeni bir komut öğrendikçe ilgili kategoriye ekleyeceğim.

## 1. İmaj (Image) Komutları

| Komut | Ne işe yarar |
|---|---|
| `docker pull <imaj>` | Docker Hub'dan (veya başka registry'den) imaj indirir |
| `docker images` / `docker image ls` | Yerelde indirilmiş imajları listeler |
| `docker build -t <isim> .` | Dockerfile'dan imaj oluşturur (`.` build context'i belirtir) |
| `docker rmi <imaj>` | Bir imajı siler |
| `docker tag <imaj> <yeni-isim>` | İmaja yeni bir isim/etiket verir |
| `docker push <imaj>` | İmajı bir registry'ye (Docker Hub vs.) yükler |
| `docker history <imaj>` | İmajın katmanlarını (layer) gösterir |
| `docker inspect <imaj>` | İmaj hakkında detaylı bilgi (JSON formatında) verir |

> Image kavramı ve layer mantığı hakkında detaylı not → [`02-dockerfile-ve-imajlar/01-image-nedir.md`](../02-dockerfile-ve-imajlar/01-image-nedir.md)

## 2. Container Komutları

| Komut | Ne işe yarar |
|---|---|
| `docker run <imaj>` | İmajdan yeni bir container oluşturur ve çalıştırır |
| `docker ps` | Çalışan container'ları listeler |
| `docker ps -a` | Tüm container'ları (durmuş olanlar dahil) listeler |
| `docker stop <container>` | Container'ı durdurur (silmez) |
| `docker start <container>` | Durdurulmuş container'ı tekrar başlatır |
| `docker restart <container>` | Container'ı yeniden başlatır |
| `docker rm <container>` | Container'ı siler |
| `docker exec -it <container> bash` | Çalışan container'ın içine terminal ile girer |
| `docker logs <container>` | Container'ın loglarını gösterir |
| `docker logs -f <container>` | Logları canlı takip eder (`-f` = follow) |
| `docker inspect <container>` | Container hakkında detaylı bilgi verir |
| `docker rename <eski> <yeni>` | Container'ın adını değiştirir |
| `docker cp <container>:<yol> <yerel-yol>` | Container ile host arasında dosya kopyalar |

## 3. `docker run` ile sık kullanılan bayraklar (flags)

`run` en sık kullanılan komut olduğu için bayrakları ayrı bir tabloda tutuyorum:

| Bayrak | Ne işe yarar |
|---|---|
| `-d` | Arka planda çalıştırır (detached mode) |
| `-it` | İnteraktif terminal açar (`-i` + `-t`) |
| `--name <isim>` | Container'a isim verir |
| `-p <host-port>:<container-port>` | Port yönlendirmesi yapar |
| `-v <host-yol>:<container-yol>` | Volume/bind mount bağlar (kalıcı veri) |
| `-e <DEĞİŞKEN>=<değer>` | Ortam değişkeni tanımlar |
| `--rm` | Container durunca otomatik silinir |
| `-m` | RAM sınırı koyar (örn. `-m 512m`) |
| `--cpus` | CPU sınırı koyar (örn. `--cpus="0.5"`) |
| `--network <ağ>` | Container'ı belirli bir ağa bağlar |

**Örnek birleşik kullanım:**
```bash
docker run -d --name web -p 8080:80 -v /home/user/site:/usr/share/nginx/html nginx
```
Bu komut nginx'i arka planda çalıştırır, `web` ismini verir, host'un 8080 portunu container'ın 80 portuna yönlendirir ve bir klasörü volume olarak bağlar.

## 4. Sistem / Kurulum ile İlgili Komutlar

| Komut | Ne işe yarar |
|---|---|
| `docker version` | Docker Client ve Server (Engine) versiyonlarını gösterir |
| `docker info` | Sistem bilgisi (disk kullanımı, container sayısı vs.) |
| `docker system df` | Docker'ın diskte ne kadar yer kapladığını gösterir |
| `docker system prune` | Kullanılmayan container, imaj, ağ ve cache'leri temizler |
| `sudo systemctl start docker` | Docker servisini başlatır (Linux) |
| `sudo systemctl enable docker` | Docker'ı sunucu her açıldığında otomatik başlatır |
| `sudo usermod -aG docker <kullanıcı>` | Kullanıcıyı `docker` grubuna ekler → `sudo` yazmadan komut çalıştırma |

> Detaylı kurulum notları ve yaşadığım hatalar için → [`00-kurulum/aws-ec2-amazon-linux-kurulumu.md`](../00-kurulum/aws-ec2-amazon-linux-kurulumu.md)
>
> Docker Engine'in (Daemon/REST API/CLI) bu komutların arka planda nasıl işlediğini anlatan not → [`01-temel-kavramlar/06-docker-engine.md`](../01-temel-kavramlar/06-docker-engine.md)

## 5. Volume Komutları

| Komut | Ne işe yarar |
|---|---|
| `docker volume create <isim>` | Yeni bir volume oluşturur |
| `docker volume ls` | Volume'leri listeler |
| `docker volume rm <isim>` | Volume'ü siler |
| `docker volume inspect <isim>` | Volume hakkında detay verir |

## 6. Network Komutları

| Komut | Ne işe yarar |
|---|---|
| `docker network ls` | Ağları listeler |
| `docker network create <isim>` | Yeni bir ağ oluşturur |
| `docker network connect <ağ> <container>` | Container'ı bir ağa bağlar |
| `docker network inspect <ağ>` | Ağ hakkında detay verir |

## 7. Docker Compose Komutları

| Komut | Ne işe yarar |
|---|---|
| `docker compose up` | `docker-compose.yml`'deki tüm servisleri başlatır |
| `docker compose down` | Servisleri durdurur ve kaynakları temizler |
| `docker compose ps` | Compose ile başlatılan servisleri listeler |
| `docker compose logs` | Servislerin loglarını gösterir |

> Detaylı Compose notları henüz eklenmedi → [`04-docker-compose/notlar.md`](../04-docker-compose/notlar.md)

## Notlar
> (Deneyip anladıkça buraya kendi notlarımı ekleyeceğim — özellikle hangi bayrağı ne zaman kullandığımı hatırlamak için pratik örnekler.)
