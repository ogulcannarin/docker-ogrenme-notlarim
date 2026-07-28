# Docker Mimari ve Eklentiler (Docker Architecture & Plugins)

Docker'ın temel mimarisi istemci-sunucu (client-server) modeline dayanır. Bu yapı sayesinde uygulamaları izole bir şekilde, hızlı ve güvenilir olarak dağıtabilirsiniz.

## 1. Docker Mimarisi Temel Bileşenleri

Docker mimarisini oluşturan üç temel bileşen bulunmaktadır:

- **Docker Client (İstemci):** Kullanıcıların Docker ile etkileşime geçtiği yerdir. `docker run`, `docker build`, `docker pull` gibi komutlar çalıştırıldığında istemci bu istekleri Docker Daemon'a gönderir.
- **Docker Host:** Container'ların çalıştığı ortamdır. İçerisinde **Docker Daemon (dockerd)**, İmajlar (Images), Konteynerler (Containers), Ağlar (Networks) ve Veri Alanları (Volumes) bulunur. Docker Daemon, istemciden gelen API isteklerini dinler ve konteynerleri, imajları vb. yönetir.
- **Docker Registry:** Docker imajlarının depolandığı yerdir. En bilineni genel kullanıma açık olan Docker Hub'dır. Kendi özel registry'lerinizi de kurabilirsiniz.

İstemci ve Daemon aynı makinede çalışabileceği gibi, bir Docker istemcisi uzaktaki bir Docker Daemon'a REST API, UNIX soketleri veya ağ arayüzleri üzerinden bağlanabilir.

## 2. Docker Eklentileri (Plugins)

Docker'ın işlevselliği, eklentiler (plugins) aracılığıyla genişletilebilir. Eklentiler, Docker motoruna ekstra özellikler kazandıran ayrı işlemler olarak çalışır. 

Temel eklenti türleri şunlardır:

- **Network Plugins (Ağ Eklentileri):** Container'ların birbirleriyle veya dış dünyayla iletişim kurma şeklini özelleştirmek için kullanılır. (Örn: Weave, Calico).
- **Volume Plugins (Birim Eklentileri):** Container verilerinin nerede ve nasıl saklanacağını belirler. Verileri bulut depolama sistemlerine (AWS EBS, Azure Disk vb.) veya uzak dosya sunucularına yazmanıza olanak tanır.
- **Authorization Plugins (Yetkilendirme Eklentileri):** Docker Daemon'a yapılan istekleri kontrol ederek güvenlik politikaları oluşturmanıza yarar. Hangi kullanıcının hangi Docker komutlarını çalıştırabileceğini belirleyebilirsiniz.

Docker eklentileri sayesinde Docker motorunu, kendi altyapı gereksinimlerinize en uygun hale getirecek şekilde esnetebilirsiniz.
