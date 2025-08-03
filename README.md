<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Türkiye Adres Veritabanı SQL Betiği</title>
    <style>
        /* Genel Sayfa Stilleri */
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            line-height: 1.6;
            background-color: #f4f7f9;
            color: #333;
            margin: 0;
            padding: 20px;
        }

        /* Ana İçerik Konteyneri */
        .container {
            max-width: 900px;
            margin: 0 auto;
            background-color: #ffffff;
            padding: 30px 40px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
        }

        /* Başlık Stilleri */
        h1, h2, h3 {
            color: #1a2b4d;
            border-bottom: 2px solid #eef2f7;
            padding-bottom: 10px;
            margin-top: 40px;
            margin-bottom: 20px;
        }

        h1 {
            font-size: 2.5em;
            text-align: center;
            border-bottom: none;
        }

        h2 {
            font-size: 1.8em;
        }

        h3 {
            font-size: 1.4em;
            border-bottom-style: dashed;
        }

        /* Paragraf ve Liste Stilleri */
        p, ul, ol {
            margin-bottom: 16px;
        }

        ul, ol {
            padding-left: 30px;
        }

        li {
            margin-bottom: 8px;
        }

        /* Kod Blokları Stilleri */
        pre {
            background-color: #eef2f7;
            padding: 20px;
            border-radius: 8px;
            font-family: "Fira Code", "Courier New", Courier, monospace;
            font-size: 14px;
            white-space: pre-wrap; /* Uzun satırların taşmasını önler */
            word-wrap: break-word;
            overflow-x: auto; /* Yatay kaydırma çubuğu */
            border: 1px solid #d1dce8;
        }

        code {
            font-family: "Fira Code", "Courier New", Courier, monospace;
        }

        /* Satır içi kod vurgulama */
        p code, li code {
            background-color: #eef2f7;
            padding: 3px 6px;
            border-radius: 4px;
            font-size: 0.9em;
        }
        
        /* Emoji Stilleri */
        h2 .emoji {
            display: inline-block;
            margin-right: 10px;
            font-size: 1.2em;
        }

        /* Üstbilgi ve Altbilgi */
        header, footer {
            text-align: center;
            color: #777;
            margin-bottom: 20px;
        }

        footer {
            margin-top: 40px;
            font-size: 0.9em;
            border-top: 1px solid #e0e0e0;
            padding-top: 20px;
        }

        /* Güçlü Metin */
        strong {
            color: #0d47a1;
        }
    </style>
</head>
<body>

    <div class="container">
        <header>
            <h1>Türkiye Adres Veritabanı SQL Betiği</h1>
        </header>

        <p>Bu proje, Türkiye'nin idari bölünmesine ait güncel verileri (ülke, il ve ilçe) bir SQL veritabanına eklemek için hazırlanmış bir SQL betiği (script) içerir. Veriler, Mayıs 2021 itibarıyla Türkiye'deki 81 il ve bu illere bağlı tüm ilçeleri kapsamaktadır.</p>
        <p>Bu betik, özellikle adres seçimi modüllerine, coğrafi bilgi sistemlerine veya Türkiye'nin idari yapısına ihtiyaç duyan herhangi bir uygulamaya hızlı bir şekilde veri sağlamak amacıyla oluşturulmuştur.</p>

        <section id="features">
            <h2><span class="emoji">🌟</span>Özellikler</h2>
            <ul>
                <li><strong>Yapısal Veri:</strong> Ülke, il ve ilçe verileri ilişkisel bir modelle sunulmuştur.</li>
                <li><strong>Tam Kapsam:</strong> Türkiye'nin 81 ilinin tamamını içerir.</li>
                <li><strong>Güncel İlçe Listesi:</strong> Tüm illere ait güncel ilçe listelerini barındırır.</li>
                <li><strong>Kolay Kurulum:</strong> Tek bir SQL betiği çalıştırılarak tüm veriler ilgili tablolara eklenebilir.</li>
                <li><strong>Esneklik:</strong> Betik, <code>IDENTITY</code> (otomatik artan ID) kullanılan ve kullanılmayan senaryolara uyum sağlayacak şekilde yorum satırları ile desteklenmiştir.</li>
            </ul>
        </section>

        <section id="structure">
            <h2><span class="emoji">🗃️</span>Veritabanı Yapısı</h2>
            <p>Bu betiği çalıştırmadan önce, veritabanınızda aşağıdaki yapıya sahip üç adet tablonun oluşturulmuş olması gerekmektedir.</p>
            <ol>
                <li>
                    <strong><code>Countries</code> Tablosu:</strong> Ülkeleri saklar.
                    <ul>
                        <li><code>Id</code> (int, Primary Key, IDENTITY)</li>
                        <li><code>Name</code> (nvarchar)</li>
                    </ul>
                </li>
                <li>
                    <strong><code>Cities</code> Tablosu:</strong> İlleri saklar ve ülkelere bağlar.
                    <ul>
                        <li><code>Id</code> (int, Primary Key, IDENTITY)</li>
                        <li><code>CountryId</code> (int, Foreign Key -> Countries.Id)</li>
                        <li><code>Name</code> (nvarchar)</li>
                    </ul>
                </li>
                <li>
                    <strong><code>Districts</code> Tablosu:</strong> İlçeleri saklar ve illere bağlar.
                    <ul>
                        <li><code>Id</code> (int, Primary Key, IDENTITY)</li>
                        <li><code>CityId</code> (int, Foreign Key -> Cities.Id)</li>
                        <li><code>Name</code> (nvarchar)</li>
                    </ul>
                </li>
            </ol>

            <h3>Örnek Tablo Oluşturma Kodları (T-SQL)</h3>
            <p>Eğer tablolarınız henüz mevcut değilse, aşağıdaki T-SQL kodlarını kullanarak oluşturabilirsiniz:</p>
            <pre><code>
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
            </code></pre>
        </section>

        <section id="usage">
            <h2><span class="emoji">🚀</span>Nasıl Kullanılır?</h2>
            <ol>
                <li><strong>Ön Koşul:</strong> Yukarıda belirtilen <code>Countries</code>, <code>Cities</code> ve <code>Districts</code> tablolarının veritabanınızda oluşturulduğundan emin olun.</li>
                <li><strong>Betiği Çalıştırma:</strong> Sağlanan SQL betiğinin tamamını kopyalayın.</li>
                <li><strong>Veritabanı İstemcisi:</strong> Kullandığınız veritabanı yönetim aracını (SQL Server Management Studio, Azure Data Studio, DBeaver vb.) açın.</li>
                <li><strong>Sorgu Penceresi:</strong> Verilerin eklenmesini istediğiniz veritabanını seçin ve yeni bir sorgu penceresi açın.</li>
                <li><strong>Çalıştır:</strong> Betiği bu pencereye yapıştırın ve çalıştırın (Execute).</li>
            </ol>
            <p>İşlem tamamlandığında, tablolarınız Türkiye'nin il ve ilçe verileriyle doldurulmuş olacaktır.</p>
        </section>
        
        <section id="description">
            <h2><span class="emoji">📝</span>Betik Açıklaması</h2>
            <ul>
                <li><strong>Ülke Ekleme:</strong> Betik ilk olarak <code>Countries</code> tablosuna 'Türkiye' kaydını ekler.</li>
                <li><strong>ID Ataması:</strong> Daha sonra eklenen 'Türkiye' kaydının <code>Id</code>'sini bir değişkene atar. Bu sayede, iller eklenirken <code>CountryId</code> alanı dinamik olarak doldurulur ve ID'nin manuel olarak bilinmesine gerek kalmaz.</li>
                <li><strong>İl Ekleme:</strong> 81 il, <code>Cities</code> tablosuna alfabetik sırayla (plaka koduna göre değil) eklenir. Her il, bir önceki adımda alınan <code>@CountryId</code> değişkeni ile Türkiye'ye bağlanır.</li>
                <li><strong>İlçe Ekleme:</strong> Son olarak, tüm ilçeler <code>Districts</code> tablosuna eklenir. İlçeler, <code>Cities</code> tablosuna eklenme sırasına göre (Adana: 1, Adıyaman: 2, ...) ilgili <code>CityId</code> ile ilişkilendirilir.</li>
            </ul>
        </section>

        <section id="notes">
            <h2><span class="emoji">⚠️</span>Önemli Notlar</h2>
            <ul>
                <li>Bu betikteki <code>INSERT INTO Cities</code> sorgusu, illeri <strong>plaka koduna göre değil, alfabetik sıraya göre</strong> eklemektedir. Bu nedenle <code>CityId</code> değerleri il plaka kodlarıyla eşleşmez (Örn: Adana'nın <code>CityId</code>'si 1 iken, Ankara'nın <code>CityId</code>'si 6 olacaktır).</li>
                <li>Eğer ID değerlerini manuel olarak (örneğin plaka kodlarına göre) vermek isterseniz, betikte <code>SET IDENTITY_INSERT</code> komutlarını içeren yorum satırlarını aktif hale getirebilir ve <code>INSERT</code> sorgularınızı buna göre düzenleyebilirsiniz.</li>
            </ul>
        </section>
        
        <footer>
            <p>Veri Tarihi: Mayıs 2021</p>
        </footer>
    </div>

</body>
</html>
