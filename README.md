CREATE DATABASE KargoTakip_FatmaBilge

CREATE TABLE Musteriler (
    MusteriID INT IDENTITY(1,1) PRIMARY KEY,
    Ad NVARCHAR(50),
    Soyad NVARCHAR(50),
    Telefon NVARCHAR(20),
    Email NVARCHAR(100),
    Adres NVARCHAR(200),
    Sehir NVARCHAR(50))
    
CREATE TABLE Kargolar (
    KargoID INT IDENTITY(1,1) PRIMARY KEY,
    TakipNo NVARCHAR(20),
    GondericiID INT,
    AliciAd NVARCHAR(50),
    AliciTelefon NVARCHAR(20),
    AliciAdres NVARCHAR(200),
    KargoDurumu NVARCHAR(50),
    GonderimTarihi DATE,
    TeslimTarihi DATE,

    FOREIGN KEY (GondericiID)
    REFERENCES Musteriler(MusteriID))

CREATE TABLE Subeler (
    SubeID INT IDENTITY(1,1) PRIMARY KEY,
    SubeAdi NVARCHAR(100),
    Sehir NVARCHAR(50),
    Telefon NVARCHAR(20))

CREATE TABLE KargoHareketleri (
    HareketID INT IDENTITY(1,1) PRIMARY KEY,
    KargoID INT,
    SubeID INT,
    HareketTarihi DATETIME,
    DurumAciklamasi NVARCHAR(200),

    FOREIGN KEY (KargoID)
    REFERENCES Kargolar(KargoID),

    FOREIGN KEY (SubeID)
    REFERENCES Subeler(SubeID))

INSERT INTO Musteriler
(Ad, Soyad, Telefon, Email, Adres, Sehir)
VALUES
('Ahmet','Yılmaz','05551112233','ahmet@gmail.com','Melikgazi','Kayseri'),
('Ayşe','Demir','05552223344','ayse@gmail.com','Kocasinan','Kayseri'),
('Mehmet','Kara','05553334455','mehmet@hotmail.com','Talas','Kayseri'),
('Zeynep','Çelik','05554445566','zeynep@gmail.com','Keçiören','Ankara'),
('Fatma','Koç','05555556677','fatma@yahoo.com','Bornova','İzmir'),
('Ali','Şahin','05556667788','ali@gmail.com','Osmangazi','Bursa'),
('Elif','Aydın','05557778899','elif@gmail.com','Selçuklu','Konya'),
('Burak','Arslan','05558889900','burak@hotmail.com','Merkez','Sivas'),
('Can','Öztürk','05559990011','can@gmail.com','Ortahisar','Trabzon'),
('Ece','Aksoy','05550001122','ece@gmail.com','Merkez','Antalya');

INSERT INTO Subeler
(SubeAdi, Sehir, Telefon)
VALUES
('Kayseri Merkez','Kayseri','03521234567'),
('Ankara Şube','Ankara','03121234567'),
('İzmir Şube','İzmir','02321234567'),
('Bursa Şube','Bursa','02241234567'),
('Konya Şube','Konya','03321234567'),
('Sivas Şube','Sivas','03461234567'),
('Trabzon Şube','Trabzon','04621234567'),
('Antalya Şube','Antalya','02421234567'),
('Adana Şube','Adana','03221234567'),
('İstanbul Şube','İstanbul','02121234567')

INSERT INTO Kargolar
(TakipNo, GondericiID, AliciAd, AliciTelefon,
AliciAdres, KargoDurumu, GonderimTarihi, TeslimTarihi)
VALUES
('TR1001',1,'Merve Kaya','05001112233','Ankara','Teslim Edildi','2026-05-01','2026-05-03'),
('TR1002',2,'Kemal Aslan','05002223344','İzmir','Yolda','2026-05-02',NULL),
('TR1003',3,'Selin Yıldız','05003334455','Bursa','Dağıtımda','2026-05-04',NULL),
('TR1004',4,'Okan Demir','05004445566','Konya','Teslim Edildi','2026-05-05','2026-05-06'),
('TR1005',5,'Buse Kara','05005556677','Sivas','Yolda','2026-05-07',NULL),
('TR1006',6,'Emre Çetin','05006667788','Trabzon','Hazırlanıyor','2026-05-08',NULL),
('TR1007',7,'Gamze Şen','05007778899','Antalya','Dağıtımda','2026-05-09',NULL),
('TR1008',8,'Tolga Acar','05008889900','İstanbul','Teslim Edildi','2026-05-10','2026-05-12'),
('TR1009',9,'Derya Kurt','05009990011','Adana','Yolda','2026-05-11',NULL),
('TR1010',1,'Kaan Eren','05001010101','Kayseri','Hazırlanıyor','2026-05-12',NULL)

INSERT INTO KargoHareketleri
(KargoID, SubeID, HareketTarihi, DurumAciklamasi)
VALUES
(1,1,'2026-05-01 10:00','Kargo alındı'),
(2,2,'2026-05-02 11:30','Transfer merkezinde'),
(3,3,'2026-05-04 09:15','Dağıtıma çıktı'),
(4,4,'2026-05-05 14:00','Teslim edildi'),
(5,5,'2026-05-07 16:20','Yola çıktı'),
(6,6,'2026-05-08 12:10','Hazırlanıyor'),
(7,7,'2026-05-09 18:00','Dağıtıma çıktı'),
(8,8,'2026-05-10 08:40','Teslim edildi'),
(9,9,'2026-05-11 13:50','Transfer aşamasında'),
(10,10,'2026-05-12 15:00','Şubede bekliyor')

SELECT * FROM Kargolar;

SELECT * 
FROM Kargolar
WHERE TeslimTarihi IS NULL;

SELECT *
FROM Kargolar
WHERE GondericiID = 1;

SELECT *
FROM Kargolar
WHERE GonderimTarihi > '2026-05-05';

SELECT *
FROM Kargolar
WHERE KargoDurumu = 'Yolda';

SELECT *
FROM Kargolar
WHERE KargoDurumu = 'Dağıtımda';

SELECT *
FROM Musteriler
WHERE Email LIKE '%@gmail.com';

SELECT TOP 1
M.Ad,
M.Soyad,
COUNT(K.KargoID) AS ToplamKargo
FROM Musteriler M
INNER JOIN Kargolar K
ON M.MusteriID = K.GondericiID
GROUP BY M.Ad, M.Soyad
ORDER BY ToplamKargo DESC;

SELECT
K.TakipNo,
S.SubeAdi,
KH.HareketTarihi,
KH.DurumAciklamasi
FROM KargoHareketleri KH
INNER JOIN Kargolar K
ON KH.KargoID = K.KargoID
INNER JOIN Subeler S
ON KH.SubeID = S.SubeID
WHERE K.KargoID = 1;

SELECT *
FROM Subeler
WHERE Sehir = 'Kayseri';

SELECT *
FROM Kargolar
WHERE TeslimTarihi = CAST(GETDATE() AS DATE);

SELECT *
FROM Musteriler
WHERE MusteriID NOT IN
(
    SELECT GondericiID
    FROM Kargolar)

SELECT
S.SubeAdi,
COUNT(KH.HareketID) AS HareketSayisi
FROM Subeler S
LEFT JOIN KargoHareketleri KH
ON S.SubeID = KH.SubeID
GROUP BY S.SubeAdi;

SELECT TOP 5
K.KargoID,
K.TakipNo,
KH.HareketTarihi
FROM Kargolar K
INNER JOIN KargoHareketleri KH
ON K.KargoID = KH.KargoID
ORDER BY KH.HareketTarihi DESC;

SELECT
GondericiID,
GonderimTarihi,
COUNT(*) AS KargoSayisi
FROM Kargolar
GROUP BY GondericiID, GonderimTarihi
HAVING COUNT(*) > 1;
