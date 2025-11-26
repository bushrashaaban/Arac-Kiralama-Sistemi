-- Arac-Kiralama-Sistemi
-- MS SQL Server kullanılarak geliştirilmiş, ilişkisel veri tabanı tasarımı içeren araç kiralama sistemi projesi.
CREATE DATABASE Arac_kiralama_s;
GO

USE Arac_kiralama_s
GO

CREATE TABLE tMusteriler
(
MusterilerID INT IDENTITY (1,1) not null PRIMARY KEY,
İsim NVARCHAR(15) not null,
Soyisim NVARCHAR(31) not null,
Telefon CHAR(10) UNIQUE not null,
Adres VARCHAR(127) not null
);
GO

CREATE TABLE tAraclar
(
AraclarID INT IDENTITY (1,1) not null PRIMARY KEY,
Marka VARCHAR(63) not null,
Model VARCHAR(63) not null,
Plaka VARCHAR(15) not null,
SigortaPolicesi VARCHAR(63) null,
);
GO

CREATE TABLE tSigortaPoliceleri
(
SigortaID INT IDENTITY(1,1) PRIMARY KEY,
Fiyat DECIMAL(10, 2) not null,
PoliceTuru NVARCHAR(63) null,
AraclarID_SigortaID INT, CONSTRAINT FK_AraclarID_SigortaID FOREIGN KEY (AraclarID_SigortaID) REFERENCES tAraclar(AraclarID)
);
GO

CREATE TABLE tFiyatlandirma
(
FiyatlandirmaID INT IDENTITY (1,1) not null PRIMARY KEY,
Fiyat DECIMAL(10,2)
);
GO

CREATE TABLE tKiralamaİslemleri
(
KiralamaİslemleriID INT IDENTITY (1,1) not null  PRIMARY KEY,
KullanimDurumu VARCHAR(15),
CHECK (KullanimDurumu IN ('Kullanilamaz', 'Kullanilabilir')),
KiralamaTarihi DATE not null,
TeslimTarihi DATE not null,
MusterilerID_KiralamaİslemleriID INT, CONSTRAINT FK_MusterilerID_KiralamaİslemleriID FOREIGN KEY (MusterilerID_KiralamaİslemleriID) REFERENCES tMusteriler(MusterilerID),
AraclarID_KiralamaİslemleriID INT, CONSTRAINT FK_AraclarID_KiralamaİslemleriID FOREIGN KEY (AraclarID_KiralamaİslemleriID) REFERENCES tAraclar(AraclarID)
);
GO

CREATE TABLE tKiralamaFiyatlandirma
(
PRIMARY KEY (KiralamaİslemleriID, FiyatlandirmaID),
KiralamaİslemleriID INT, CONSTRAINT FK_KiralamaİslemleriID FOREIGN KEY (KiralamaİslemleriID) REFERENCES tKiralamaİslemleri(KiralamaİslemleriID),
FiyatlandirmaID INT, CONSTRAINT FK_FiyatlandirmaID FOREIGN KEY (FiyatlandirmaID) REFERENCES tFiyatlandirma(FiyatlandirmaID)
);

-- VERİ GİRİŞİ (INSERT) İŞLEMLERİ
-- Veri tabanı şeması hazırlandıktan sonra, ilişkileri doğrulamak ve veri akışını görmek için kullanılan veri giriş (INSERT) kodları şunlardır:
USE Arac_kiralama_s
GO

INSERT INTO tMusteriler (İsim, Soyisim, Telefon, Adres)
VALUES ('Sema', 'Sahin', '5555555555', 'Ankara')
GO

INSERT INTO tAraclar (Marka, Model, Plaka)
VALUES ('BMW', '2022', '06ABC123' )
GO

INSERT INTO tSigortaPoliceleri (Fiyat, AraclarID_SigortaID )
VALUES ('1000', 1)
GO

INSERT INTO tFiyatlandirma (Fiyat)
VALUES ('20000')
GO

INSERT INTO tKiralamaİslemleri (KullanimDurumu, KiralamaTarihi, TeslimTarihi, MusterilerID_KiralamaİslemleriID,AraclarID_KiralamaİslemleriID)
VALUES ('Kullanilabilir','2024-08-23', '2024-09-01', 1, 1)
GO

INSERT INTO tKiralamaFiyatlandirma (KiralamaİslemleriID, FiyatlandirmaID )
VALUES (1, 1)
GO

/*
-- VERİ TABANI SIFIRLAMA / TEMİZLEME (OPTIONAL RESET)
-- Not: Aşağıdaki kodlar, veritabanını tamamen silmek ve
-- sistemi sıfırlamak istendiğinde kullanılmak üzere hazırlanmıştır.
-- Güvenlik amacıyla yorum satırı haline getirilmiştir.
-- Çalıştırmak için yorum işaretlerini (/* ... */) kaldırınız.

USE Arac_kiralama_s
GO

-- Önce Foreign Key Kısıtlamaları Kaldırılır
ALTER TABLE tKiralamaİslemleri
DROP CONSTRAINT FK_MusterilerID_KiralamaİslemleriID, FK_AraclarID_KiralamaİslemleriID
GO

ALTER TABLE tKiralamaFiyatlandirma
DROP CONSTRAINT FK_KiralamaİslemleriID, FK_FiyatlandirmaID
GO

ALTER TABLE tSigortaPoliceleri
DROP CONSTRAINT FK_AraclarID_SigortaID
GO

-- Sonra Tablolar Silinir
DROP TABLE tKiralamaFiyatlandirma
DROP TABLE tKiralamaİslemleri
DROP TABLE tFiyatlandirma
DROP TABLE tSigortaPoliceleri
DROP TABLE tAraclar
DROP TABLE tMusteriler
GO
*/
