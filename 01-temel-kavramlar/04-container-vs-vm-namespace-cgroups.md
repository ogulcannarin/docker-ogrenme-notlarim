# Container vs VM Mimarisi — Namespace ve cgroups

## Container'lar aslında ne?

Container'lar, VM'lerin aksine ayrı bir işletim sistemi çalıştırmaz. Bunun yerine **host'un kernel'i üzerinde çalışan, izole edilmiş normal bir process (süreç)** gibi düşünülebilir. Bu izolasyonu sağlayan iki temel Linux mekanizması var: **namespace** ve **cgroups**.

## 1. Namespace — "Ben neyi görebiliyorum?" sorusunun cevabı

Namespace, bir process'in **sistemin neresini görebileceğini** sınırlar. Container içindeki bir process, sanki host'ta tek başınaymış gibi hisseder, ama aslında host'un kaynaklarının sadece kendine ayrılan kısmını görür.

### Başlıca namespace türleri

| Namespace | Ne izole eder |
|---|---|
| **PID** | Process ID'leri. Container içinde `ps` yazdığında sadece kendi process'lerini görürsün, host'takileri değil |
| **Network** | Ağ arayüzleri, IP adresleri, portlar. Her container kendi ağ yığınına sahipmiş gibi davranır |
| **Mount** | Dosya sistemi görünümü. Container kendi root dizinine (`/`) sahipmiş gibi görür, host'un tüm dosya sistemini göremez |
| **UTS** | Hostname ve domain name. Her container kendi hostname'ini ayarlayabilir |
| **IPC** | Process'ler arası iletişim (shared memory, semaphores) |
| **User** | Kullanıcı/grup ID'leri. Container içinde "root" olan biri, host'ta root olmayabilir |

**Somut örnek:** Bir container içinde `ps aux` çalıştırıldığında sadece o container'a ait birkaç process görünür, oysa host makinede yüzlerce process çalışıyor olabilir. Bunun sebebi PID namespace — container kendi izole "process evrenine" sahip.

## 2. cgroups (Control Groups) — "Ne kadar kaynak kullanabilirim?" sorusunun cevabı

Namespace "neyi görebiliyorum" sorusunu çözerken, cgroups **"ne kadar kaynak (CPU, RAM, disk I/O) kullanabilirim"** sorusunu çözer.

cgroups sayesinde:
- Bir container'a maksimum RAM sınırı konabilir (örn. 512MB)
- Bir container'a CPU kullanım oranı sınırlanabilir (örn. %20)
- Bir container aşırı kaynak tüketip diğer container'ları veya host'u etkileyemez

**Somut örnek:**
```bash
docker run -m 512m --cpus="0.5" nginx
```
Bu komut, nginx container'ına maksimum 512MB RAM ve yarım CPU çekirdeği tahsis eder. Container bu sınırı aşmaya çalışırsa (örneğin RAM sınırını), Docker o process'i sonlandırır (OOM kill).

## İkisini birlikte düşünürsek

- **Namespace** = "Bu container sadece kendi küçük dünyasını görsün" (izolasyon)
- **cgroups** = "Bu container en fazla bu kadar kaynak tüketsin" (kaynak sınırlama)

Docker, bir container başlatırken aslında arka planda:
1. Yeni namespace'ler oluşturur (izole bir "balon" yaratır)
2. Bu balona cgroups ile kaynak limitleri koyar
3. İçine image'dan oluşturulan dosya sistemini (root filesystem) bağlar

VM'de olduğu gibi ayrı bir kernel başlatmaz — **aynı kernel'i, farklı "görünürlük ve kaynak kuralları" ile paylaştırır.** VM'in dakikalar süren başlangıcına karşı Docker'ın saniyeler içinde ayağa kalkmasının sırrı burada.

## Şemayla özet

```
Host İşletim Sistemi (tek kernel)
│
├── Namespace + cgroups ile izole edilmiş → Container 1 (nginx)
├── Namespace + cgroups ile izole edilmiş → Container 2 (postgres)
└── Namespace + cgroups ile izole edilmiş → Container 3 (node app)
```

Hepsi aynı kernel'i paylaşır ama birbirlerinin process'lerini, dosyalarını, ağlarını göremez ve belirlenen kaynak sınırlarını aşamazlar.

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)

## Merak ettiklerim / sonraki sorular
- Docker Desktop Windows/Mac'te bu namespace/cgroups mekanizması olmadan (Linux kernel'i olmadan) nasıl çalışıyor? (İpucu: arka planda hafif bir Linux VM kullanıyor olabilir mi?)
