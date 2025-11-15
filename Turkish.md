# Aave Token – Teknik Açıklama ve Mimari İnceleme

Bu depo, **Aave protokolünün yerel yönetişim token’ı olan AAVE’nin** akıllı sözleşme implementasyonunu içerir.  
Kod tabanı; ERC-20 standardı, snapshot sistemi, EIP-2612 permit desteği ve LEND → AAVE migrasyon mekanizmasını bir araya getirir. Bu sayede protokolün yönetişim altyapısını ve token ekonomisini teknik olarak tanımlar.

---

## 📌 İçindekiler
- [Giriş](#giriş)
- [Genel Bakış](#genel-bakış)
- [Depo Yapısı](#depo-yapısı)
- [AaveToken Sözleşmesi](#aavetoken-sözleşmesi)
- [LEND → AAVE Migrator](#lend--aave-migrator)
- [Güvenlik](#güvenlik)
- [Kullanılan Standartlar](#kullanılan-standartlar)
- [Teknoloji Yığını](#teknoloji-yığını)
- [Sonuç](#sonuç)

---

## 🟦 Giriş

AAVE token, Aave ekosisteminin yönetişim, protokol iyileştirme önerileri (AIP’ler) ve ekosistem teşvik mekanizmalarının merkezinde yer alır.  
Bu repo, AAVE tokenının teknik implementasyonunu ve eski LEND tokenından geçiş sürecini (“migration”) içeren akıllı sözleşmeleri sağlar.

---

## 🟪 Genel Bakış

Bu depodaki kodlar:

- **AAVE ERC-20 token implementasyonu**
- **Snapshot özellikli yönetişim altyapısı**
- **EIP-2612 Permit (gas’siz onaylama)**
- **LEND → AAVE dönüşüm sözleşmesi (migrator)**
- **EIP-1967 Transparent Proxy ile upgrade edilebilir mimari**
- **Audit edilmiş güvenlik altyapısı**

gibi bileşenleri barındırır.

AAVE’nin toplam arzı sabittir: **16.000.000 AAVE**

---

## 🟩 Depo Yapısı


---

## 🟥 AaveToken Sözleşmesi

`AaveToken.sol`, AAVE tokenının ana akıllı sözleşmesidir ve standart bir ERC-20 tokenından daha fazlasını sunar:

### 🔹 Snapshots (ERC20Snapshot)
AAVE yönetişimi, belirli blok numaralarında kullanıcı bakiyelerini dondurup kaydetmek için snapshot’lar kullanır.  
Bu, oylama sırasında manipülasyonu engeller ve güvenlik sağlar.

### 🔹 EIP-2612 Permit
Kullanıcılar, bir işlem göndermeden **imza ile** `approve` verebilir.  
Bu özellik, gas maliyetlerini azaltır ve merkeziyetsiz uygulamalara entegrasyonu kolaylaştırır.

### 🔹 Upgrade Edilebilirlik (EIP-1967 Proxy)
Sözleşme, Aave Governance tarafından denetlenebilen upgrade edilebilir bir proxy yapısında çalışır.  
Veri proxy’de tutulur, mantık (logic contract) gerektiğinde güncellenebilir.

---

## 🟧 LEND → AAVE Migrator

`LendToAaveMigrator.sol`, eski token **LEND** → yeni token **AAVE** geçişini sağlayan güvenilir bir akıllı sözleşmedir.

### 📌 Dönüşüm Oranı

### 📌 Çalışma Süreci
1. Kullanıcı `migrateFromLend(amount)` fonksiyonunu çağırır.  
2. LEND token’ları yakılır.  
3. Karşılık gelen miktarda AAVE mint edilir.  
4. Toplam arz doğru şekilde güncellenir.

### 📌 Güvenlik
- Reentrancy koruması  
- Yanlış dönüşüm riskine karşı validasyonlar  
- Hem LEND hem AAVE tarafında basım/yakım güvenlik kontrolleri  
- Audit edilmiş süreç  

---

## 🟨 Güvenlik

Bu depo, iki büyük firma tarafından denetlenmiştir:

- **ConsenSys Diligence**
- **CertiK**

Ayrıca:

- SafeMath / güvenli hesaplama altyapısı  
- ReentrancyGuard  
- Minimum admin yetkisi (merkeziyetsizliğe uygun)  
- Proxy admin haklarının yönetişim tarafından kontrolü  

kullanılmıştır.

---

## 🟫 Kullanılan Standartlar

| Standart | Açıklama |
|---------|----------|
| **ERC-20** | Temel token işlevleri |
| **ERC20Snapshot** | Oylama için snapshot destekli altyapı |
| **EIP-2612 Permit** | Gas’siz onaylama |
| **EIP-1967 Proxy** | Upgrade edilebilir sözleşme mimarisi |

---

## 🟦 Teknoloji Yığını

| Katman | Teknoloji |
|--------|-----------|
| Dil | Solidity |
| Test | Hardhat / Foundry |
| Güvenlik | OpenZeppelin Libraries |
| Proxy Yapısı | EIP-1967 Transparent Proxy |

---

## 🟣 Sonuç

Bu repo, Aave protokolünün çekirdek bileşenlerinden biri olan AAVE tokenının tüm teknik implementasyonunu sağlar.  
Modern ERC-20 genişletmeleri, snapshot tabanlı yönetişim, gas’siz işlem desteği, upgrade edilebilir mimari ve güvenli migrasyon sistemi ile kurumsal seviye bir token altyapısı oluşturur.

Aave ekosisteminin uzun vadeli yönetişim, şeffaflık ve sürdürülebilirlik hedeflerini teknik olarak destekleyen kritik bir yapı taşıdır.

---

