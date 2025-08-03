# 🇹🇷 Türkiye Adres Veritabanı SQL Betiği

Bu proje, Türkiye'nin idari bölünmesine ait güncel verileri (ülke, il ve ilçe) bir SQL veritabanına otomatik olarak eklemek için hazırlanmış bir SQL betiği içerir. Betik, **Mayıs 2021** tarihine kadar Türkiye'deki 81 il ve bu illere bağlı tüm ilçeleri kapsamaktadır.

## 📌 Amaç

Veritabanı üzerinde adres işlemleri, bölgesel analizler, coğrafi bilgi sistemleri veya kullanıcı formlarında dinamik adres seçimleri için Türkiye'nin ülke-il-ilçe hiyerarşisini hazır ve ilişkilendirilmiş şekilde sunmak.

---

## 🌟 Özellikler

- ✅ **Yapısal Veri**: Ülke, il ve ilçe tabloları ilişkisel olarak tanımlanmıştır.  
- 🏙️ **Tam Kapsam**: Türkiye’nin 81 iline ve tüm ilçelerine dair bilgiler içerir.  
- 📅 **Güncel**: Veriler Mayıs 2021 itibariyle günceldir.  
- ⚡ **Kolay Kurulum**: Tek bir SQL betiği ile veriler yüklenebilir.  
- 🔁 **Esnek Kullanım**: IDENTITY kullanılan ya da kullanılmayan senaryolara uygun yorum satırları içerir.

---

## 🗃️ Veritabanı Yapısı

Proje 3 ana tabloyu kullanır:

### `Countries`
| Sütun | Tür | Açıklama |
|-------|-----|----------|
| `Id` | `INT` (PK, IDENTITY) | Ülke ID |
| `Name` | `NVARCHAR(100)` | Ülke Adı |

### `Cities`
| Sütun | Tür | Açıklama |
|-------|-----|----------|
| `Id` | `INT` (PK, IDENTITY) | İl ID |
| `CountryId` | `INT` (FK) | `Countries(Id)` ile ilişkilidir |
| `Name` | `NVARCHAR(100)` | İl Adı |

### `Districts`
| Sütun | Tür | Açıklama |
|-------|-----|----------|
| `Id` | `INT` (PK, IDENTITY) | İlçe ID |
| `CityId` | `INT` (FK) | `Cities(Id)` ile ilişkilidir |
| `Name` | `NVARCHAR(100)` | İlçe Adı |

---

## 🛠️ Kurulum

1. **Veritabanı Tablolarını Oluşturun**  
Aşağıdaki SQL sorgularını kullanarak tabloları oluşturun:

```sql
CREATE TABLE Countries (
    Id INT PRIMARY KEY IDENTITY(1,1),
    Name NVARCHAR(100) NOT NULL
);

CREATE TABLE Cities (
    Id INT PRIMARY KEY IDENTITY(1,1),
    CountryId INT NOT NULL,
    Name NVARCHAR(100) NOT NULL,
    CONSTRAINT FK_Cities_Countries FOREIGN KEY (CountryId) REFERENCES Countries(Id)
);

CREATE TABLE Districts (
    Id INT PRIMARY KEY IDENTITY(1,1),
    CityId INT NOT NULL,
    Name NVARCHAR(100) NOT NULL,
    CONSTRAINT FK_Districts_Cities FOREIGN KEY (CityId) REFERENCES Cities(Id)
);
