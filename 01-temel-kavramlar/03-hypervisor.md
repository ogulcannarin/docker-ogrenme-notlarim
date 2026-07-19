# Hypervisor Nedir?

## Tanım
Fiziksel donanım kaynaklarını (CPU, RAM, disk, ağ) birden fazla sanal makine arasında paylaştıran ve yöneten yazılım katmanı.

Sanal makinelerin "trafik polisi" gibi düşünülebilir: hangi VM ne zaman CPU kullanacak, ne kadar RAM alacak — bunları hypervisor koordine eder.

## İki tip Hypervisor

### Type 1 — Bare-metal (Çıplak metal)
Doğrudan donanımın üzerine kurulur, altında başka bir işletim sistemi yoktur.

- Örnekler: VMware ESXi, Microsoft Hyper-V, KVM, Xen
- Kullanım alanı: Veri merkezleri, bulut sağlayıcıları (AWS, Azure, GCP)
- Avantaj: Donanıma direkt erişim → yüksek performans

```
[ VM 1 ] [ VM 2 ] [ VM 3 ]
[   Type 1 Hypervisor    ]
[   Fiziksel Donanım     ]
```

### Type 2 — Hosted (Barındırılan)
Normal bir işletim sisteminin üzerine kurulan bir uygulamadır.

- Örnekler: VirtualBox, VMware Workstation, Parallels Desktop
- Kullanım alanı: Kişisel bilgisayarda test amaçlı başka işletim sistemi çalıştırmak
- Dezavantaj: Ana işletim sistemi de kaynak tükettiği için bir katman fazla var, biraz daha yavaş

```
[ VM 1 ] [ VM 2 ]
[  Type 2 Hypervisor (VirtualBox vs.)  ]
[  Ana İşletim Sistemi (Windows)       ]
[  Fiziksel Donanım                    ]
```

## Docker ile ilişkisi
Docker bir hypervisor DEĞİLDİR ve (Linux'ta) hypervisor'a ihtiyaç duymaz. Docker, container'ları çalıştırmak için host işletim sisteminin kernel'ini doğrudan paylaşır.

- VM = Hypervisor + tam işletim sistemi (ağır)
- Container = Host kernel'i paylaşan izole süreç (hafif)

## Kendi cümlelerimle özet
> (Buraya kendi anladığım şekilde yazacağım.)

## Merak ettiklerim / sonraki sorular
- Namespace ve cgroups tam olarak nasıl çalışıyor?
- Docker Desktop Windows/Mac'te nasıl çalışıyor (madem Linux kernel'i gerekiyor)?
