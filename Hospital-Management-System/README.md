
# 🏥 Hastane Yönetim Sistemi

Bu proje, SQL Server veritabanı ve Power BI kullanılarak geliştirilmiş gerçekçi bir hastane yönetim sistemini kapsamaktadır. Amaç; hasta takibi, doktor planlaması, teşhis süreci, yatak yönetimi ve faturalandırma gibi temel sağlık hizmetlerinin dijital ortamda yönetimini sağlamaktır.

---

## 🧱 Veritabanı Yapısı

Projede hastane yönetimi için kapsamlı ve ilişkisel bir veritabanı modeli oluşturulmuştur. Bu model, hem ayakta hem de yatarak tedavi gören hastaları, doktorları, teşhis süreçlerini, randevuları ve faturalandırmayı kapsayan bütünsel bir yapıya sahiptir. Veri tutarlılığı `foreign key`, `constraint`, `trigger` ve `stored procedure` destekleriyle sağlanmıştır.

### 📄 Kullanılan Tablolar ve Görevleri

| Tablo Adı              | Açıklama |
|------------------------|----------|
| **Patients**           | Hastaların temel kimlik bilgileri. |
| **PatientDetails**     | Hastaların adres, telefon, doğum tarihi gibi ek bilgileri. |
| **Doctors**            | Doktorların kişisel ve mesleki bilgileri. |
| **Departments**        | Hastanedeki tıbbi departmanlar ve bölümler. |
| **Diagnosis**          | Doktorlar tarafından konulan teşhislerin kaydı. |
| **Appointments**       | Hastaların doktorlarla oluşturduğu randevu kayıtları. |
| **AppointmentSchedule**| Doktorların günlük programları, uygunluk saatleri. |
| **DoctorAvailability** | Doktorların haftalık/aylık müsaitlik planları. |
| **Invoices**           | Sağlık hizmetleri sonrası oluşturulan fatura bilgileri. |
| **Cities**             | Türkiye’deki şehirler listesi, hasta adresleriyle ilişkilendirilir. |
| **Beds**               | Hastanedeki mevcut yataklar ve doluluk durumları. |
| **Inpatients**         | Yatarak tedavi gören hastaların kayıtları. |
| **ChangeLog**          | Sistem içindeki önemli değişikliklerin log kaydı (örneğin: randevu iptali, veri güncellemesi vs.).

---

### ⚙️ Stored Procedure'ler

Veritabanı içinde sık kullanılan işlemler prosedürleştirilerek hem kod tekrarı önlendi hem de performans artırıldı.

**Bazı örnek prosedürler:**
- `sp_CreateAppointment`: Doktorun uygunluk durumuna göre yeni randevu oluşturur.
- `sp_MoveToCompletedAppointments`: Geçmiş randevuları arşiv tablosuna taşır.
- `sp_AssignBedToInpatient`: Yatış yapan hastaya uygun yatağı atar.
- `sp_GenerateInvoice`: Gerçekleşen muayene ve tedaviler sonrası fatura üretir.
- `sp_UpdateDiagnosis`: Hastanın teşhis bilgilerini günceller.

---

### 🔁 Trigger'lar

Veri bütünlüğünü ve otomatik işlem akışlarını sağlamak için bazı kritik tablolar üzerine tetikleyiciler kurulmuştur.

### 🔁 Trigger'lar

Veri bütünlüğünü ve otomatik işlem akışlarını sağlamak için bazı kritik tablolar üzerine tetikleyiciler kurulmuştur.

**Kullanılan trigger örnekleri ve işlevleri:**

- **`trg_AfterDischarge`**  
  Hasta taburcu olduğunda devreye girer. Kalış süresine göre yatak ve ameliyat ücretlerini hesaplar, fatura oluşturur, yatağı boşaltır ve randevuyu günceller.

- **`trg_Appointment_Update`**  
  Randevu güncellendiğinde ilgili hasta detaylarında yeni kayıt oluşturur (örneğin, bekleyen randevular için takip kaydı).

- **`trg_Appointments_Log`**  
  Randevularda `Status` ve `Control` alanları değiştiğinde bu değişiklikleri `ChangeLog` tablosuna kaydeder.

- **`trg_InPatients_Log`**  
  Yatan hastalarda yatak (`BedID`) veya taburcu tarihi (`DischargeDate`) değişimlerini `ChangeLog`’a yazar.

- **`trg_InsertInpatientsOnAppointment`**  
  Randevu durumu “Inpatient Ongoing” olunca boş yatak arar, yatağı işaretler ve yatan hasta kaydı oluşturur.

- **`trg_InvoiceOnAppointmentInsert`**  
  Randevu durumu değiştikçe (örneğin “Completed” ya da “Inpatient Ongoing”) fatura oluşturur ve fatura ID’sini randevuya bağlar.

- **`trg_Invoices_Log`**  
  Faturalarda ödeme durumu ve tutar değişikliklerini `ChangeLog` tablosuna yazar.

- **`trg_PatientDetails_Log`**  
  Hasta detaylarında ilaç, doktor notu ve durum değişikliklerini takip eder ve loglar.

- **`trg_Patients_Log`**  
  Hasta ana kayıtlarında (isim, telefon gibi) değişiklikleri ve silme işlemlerini `ChangeLog`’a yazar.


---

## 📊 Power BI Dashboard'ları

Power BI kullanılarak sistemdeki veriler görselleştirilmiştir. Hazırlanan paneller, hastane yönetimi ve klinik verimliliği takip etmek için yöneticilere içgörü sağlar.

### Öne Çıkan Dashboard'lar:

### 📊 Power BI Dashboard'ları

Power BI ile görselleştirilen veriler, hastane yönetiminin genel durumu, hasta takibi, mali analizler ve departman bazlı ödeme raporlarını içermektedir. Dashboard’lar aşağıdaki başlıklarda düzenlenmiştir:

- **Overview**  
  Hastane genel istatistikleri, toplam hasta sayısı, doluluk oranları, doktor sayısı gibi genel özet bilgileri içerir.

- **Daily Data**  
  Günlük hasta giriş-çıkış sayıları, yapılan randevular ve yatış işlemleri gibi günlük operasyonlara odaklanır.

- **Today's Appointment Information**  
  Bugünkü randevuların detaylarını, hangi doktorla ne zaman randevu olduğu gibi bilgileri gösterir. Anlık takip için idealdir.

- **Patient Tracking**  
  Hastaların teşhisleri, ilaç kullanımları, tedavi süreçleri ve yatış durumları gibi bireysel hasta takibini sağlar.

- **Financial**  
  Oluşan faturalar, tahsilatlar, ödenmeyen borçlar, ortalama hasta maliyetleri ve dönemsel gelir/gider analizlerini sunar.

- **Department Paid**  
  Her bir departmanın ürettiği finansal gelir, ödeme yapan hasta oranları ve departman bazlı gelir karşılaştırmalarını içerir.


Dashboard’larda DAX formülleri kullanılarak hesaplamalar yapılmıştır.  Kullanılan tüm DAX formülleri `DAX_Formula.txt` dosyasında belgelenmiştir.

---

## 🗂️ Klasör Yapısı

```
📦 Hospital-Management-System
 ┣ 📂 SQL-Scripts
 ┃ ┣ 📜 Tables.sql
 ┃ ┣ 📜 StoredProcedures.sql
 ┃ ┣ 📜 Triggers.sql
 ┃ ┗ 📜 Views.sql
 ┣ 📂 PowerBI
 ┃ ┣ 📊 DashboardScreenshots/
 ┃ ┣ 📜 DAX_Formula.txt
 ┃ ┗ 📜 HospitalDashboard.pbix
 ┗ 📜 README.md
```

---

## ⚙️ Kullanılan Teknolojiler

- **SQL Server 2019** – Veritabanı altyapısı
- **T-SQL** – Veri işleme ve yönetim
- **Power BI Desktop** – Görsel analiz ve raporlama
- **DAX** – Hesaplamalar ve metrik tanımlamaları
- **Git & GitHub** – Versiyonlama ve proje paylaşımı

---

> 💡 Bu proje, hem bireysel portföy sunumu hem de sağlık sektörü üzerine veri odaklı çözüm geliştirme pratiği açısından güçlü bir örnektir.

### 🧾 Sürüm Bilgisi

**Versiyon: v1.0 - İlk Yayın (Initial Release)**

- SQL Server üzerinde hastane yönetimi için temel veritabanı yapısı kuruldu.
- Trigger ve stored procedure'lerle veri bütünlüğü ve otomasyon sağlandı.
- Power BI ile 6 farklı dashboard oluşturularak operasyonel ve finansal analizler yapıldı.
- Proje ilk sürümüyle birlikte GitHub’a yüklendi.

İlerleyen sürümlerde kullanıcı arayüzü, veri giriş formları veya API ile veri akışı gibi geliştirmeler hedeflenebilir.

