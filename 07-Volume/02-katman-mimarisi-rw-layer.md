# Docker Katman Mimarisi ve R/W Layer (Layer Architecture & Read-Write Layer)

Docker imajları ve konteynerleri, depolama alanını verimli kullanmak ve işlemleri hızlandırmak için **katmanlı bir yapı (layered architecture)** kullanır.

## 1. Docker İmajları ve Katmanlar (Read-Only)

Bir Docker imajı, birbirinin üzerine eklenen salt okunur (read-only) katmanlardan oluşur. Bir `Dockerfile` içindeki her bir komut (`FROM`, `RUN`, `COPY`, `ADD` vb.) genellikle yeni bir katman oluşturur.

- Bu katmanlar **değiştirilemez (immutable)**.
- Eğer aynı katman birden fazla imaj tarafından kullanılıyorsa, Docker bunu önbellekte (cache) tutar ve tekrar indirmez. Bu sayede hem diskten hem de zamandan tasarruf sağlanır.

## 2. R/W Layer (Read-Write Layer / Container Layer)

Bir imajdan bir container başlattığınızda (`docker run`), Docker salt okunur katmanların en üstüne **yeni, ince ve yazılabilir bir katman (Read-Write Layer)** ekler. 

- Container çalışırken yapılan tüm değişiklikler (yeni dosya oluşturma, mevcut dosyayı değiştirme, dosya silme) sadece bu R/W katmanında gerçekleşir.
- Alttaki imaj katmanları kesinlikle değişmez.
- Container silindiğinde (`docker rm`), en üstteki bu R/W katmanı da silinir ve içindeki tüm veriler kaybolur (eğer veriler dışarıya aktarılmadıysa).

## 3. Copy-on-Write (CoW) Stratejisi

Docker bu mimariyi yönetirken **Copy-on-Write** (Yazarken Kopyala) stratejisini kullanır:

- Eğer container, alttaki read-only katmanlarda bulunan bir dosyayı okumak isterse, dosyayı doğrudan oradan okur.
- Eğer container, alttaki katmanlardaki bir dosyayı **değiştirmek** isterse, Docker o dosyayı alt katmandan alır, en üstteki R/W katmanına **kopyalar** ve değişikliği bu kopya üzerinde yapar.
- Orijinal dosya alt katmanda el değmemiş bir şekilde durmaya devam eder.

**Özetle:** Katmanlı mimari ve R/W layer, Docker'ın inanılmaz derecede hafif ve hızlı olmasını sağlayan temel mekanizmadır. Ancak container silindiğinde R/W layer da silineceği için verilerin kalıcı olması gereken durumlarda (veritabanı dosyaları gibi) Volume veya Bind Mount kullanmak zorunludur.
