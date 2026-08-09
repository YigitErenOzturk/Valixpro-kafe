# AutoFix Pro

Oto tamirhane ve servis yönetim paneli. Randevu takibi, müşteri & araç kaydı, fatura yönetimi, parça envanteri, tedarik zinciri, AI destekli arıza tahmini, AI tedarikçi araştırması ve personel yönetimini tek panelde birleştirir.

## Özellikler

### Dashboard
- Günlük randevular, bekleyen faturalar, stok uyarıları
- Gelir-gider özeti, hızlı erişim kartları

### Randevular
- Takvim ve liste görünümü
- Randevu oluşturma, düzenleme, durum takibi (bekliyor / onaylandı / tamamlandı / iptal)
- Müşteri, araç ve teknisyen ataması

### Müşteriler
- Müşteri kaydı ve detaylı profil
- Servis geçmişi, araç listesi, fatura geçmişi
- Müşteriye özel hızlı bildirim gönderme

### Araçlar
- Araç kaydı ve düzenleme
- **VIN Sorgulama** — NHTSA vPIC API ile 17 haneli VIN'den marka, model, yıl, renk otomatik dolum
- **Fotoğraf Yükleme** — Araç genel ve sorunlu bölge fotoğrafları, etiketleme, lightbox görüntüleme
- **Servis Geçmişi** — Araca bağlı tüm randevu ve fatura kayıtları timeline görünümünde
- **Bakım Hatırlatıcıları** — Periyodik bakım takibi (yağ, fren, lastik vb.)
- **Toplu VIN İçe Aktarma** — Excel/CSV ile çoklu araç kaydı
- **Müşteri Bildirimi** — Araç detayından direkt e-posta bildirimi gönderme

### AI Arıza Tahmini
- Araç kaydı sırasında notlara yazılan arıza belirtilerinden **OpenAI GPT-4** ile tahmini arıza analizi
- Olası nedenler, tahmini maliyet, aciliyet seviyesi ve önerilen aksiyon
- Tahmin araca kaydedilir, detay sayfasında **servis kaydı formatında** kart olarak görünür
- Aciliyet rozeti: Acil / Yüksek / Orta / Düşük

### Envanter
- Parça stok takibi, düşük stok uyarıları
- Kategori, parça kodu, alış/satış fiyatı yönetimi

### Tedarik Yönetimi
- **Tedarikçi Kaydı** — İletişim bilgileri, adres, notlar
- **Parça Sipariş Takibi** — Tedarikçi bazında sipariş edilen parçalar
- Sipariş durumu: Sipariş Edildi / Teslim Alındı / İptal
- Alış fiyatı, satış fiyatı, teslim süresi, kar marjı görünümü
- Envantere otomatik stok girişi

### AI Parça Araştırması
- Parça adı, kodu, kategorisi, markası ve araç bilgisi ile **OpenAI destekli tedarikçi araştırması**
- Piyasa analizi, tahmini fiyat aralığı
- 3 farklı tedarikçi tipi önerisi: Yetkili Servis / Yan Sanayi / Çıkma
- Alternatif muadil parçalar ve satın alma ipuçları
- En iyi seçenek tavsiyesi

### Finans
- Fatura oluşturma ve düzenleme
- Gelir-gider takibi (işlem girişi)
- Aylık finansal özet raporu
- Fatura durumu: Taslak / Bekliyor / Ödendi / Gecikmiş
- Fatura yazdırma görünümü

### Raporlar
- Gelir trendi grafiği
- Kategori bazlı gelir dağılımı
- Teknisyen verimlilik analizi
- Kar-Zarar trend raporu

### Personel
- Çalışan yönetimi (tamirhane sahibi, usta, danışman)
- Rol bazlı yetkilendirme
- **Davet Sistemi** — E-posta ile personel daveti, geçici şifre ile hesap oluşturma

### Hızlı Kayıt
- Müşteri + araç + randevu tek formda hızlı giriş

### Çok Kiracılı Mimari
- Her dükkan kendi kaydını oluşturur
- Supabase RLS ile tam veri izolasyonu
- Her kullanıcı sadece kendi dükkanının verilerini görür

### Bildirimler
- Müşterilere e-posta ile durum bildirimi
- Araç detayından hızlı bildirim modalı
- Bildirim geçmişi sayfası

### Diğer
- **Karanlık Mod** desteği
- Responsive tasarım (masaüstü + tablet + mobil)
- Çok dilli altyapı (i18n hazır)

## AI Edge Functions

| Fonksiyon | Açıklama |
|---|---|
| `predict-fault-v3` | Araç notlarından AI arıza tahmini (OpenAI GPT-4) |
| `find-supplier` | Parça bilgilerinden AI tedarikçi ve fiyat araştırması (OpenAI GPT-4) |
| `send-notification` | Müşteriye e-posta bildirimi gönderme |
| `send-maintenance-reminders` | Periyodik bakım hatırlatıcılarını kontrol ve gönder |
| `invite-staff` | Personel davet e-postası ve hesap oluşturma |
| `register-shop` | Dükkan kaydı (auth kullanıcı + shop + staff kaydı) |

## Teknoloji

React 19 · TypeScript · Tailwind CSS · Supabase (Auth + Database + Edge Functions + RLS) · OpenAI API

## Başlangıç

```bash
npm install
npm run dev
```

## Yapı

```
src/
├── components/
│   ├── base/           # Buton, kart, pagination gibi temel bileşenler
│   └── feature/        # DashboardLayout, Sidebar, TopBar, NotificationDropdown
├── hooks/              # useDarkMode gibi özel React hook'ları
├── i18n/               # Çok dilli altyapı
├── lib/                # Supabase istemcisi (singleton)
├── pages/
│   ├── dashboard/      # Gösterge paneli + KPI kartları + grafikler
│   ├── appointments/   # Randevu takvimi ve liste yönetimi
│   ├── customers/      # Müşteri listesi ve detay sayfası
│   ├── vehicles/       # Araç kaydı, VIN sorgulama, AI arıza tahmini, fotoğraf
│   ├── inventory/      # Parça envanteri
│   ├── supply/         # Tedarikçiler, sipariş takibi, AI parça araştırması
│   ├── invoices/       # Fatura ve gelir/gider yönetimi
│   ├── reports/        # Gelir, kategori, teknisyen raporları
│   ├── staff/          # Personel yönetimi ve davet
│   ├── notifications/  # Müşteri bildirimleri
│   ├── fast-register/  # Hızlı müşteri + araç + randevu kaydı
│   ├── settings/       # Kullanıcı ayarları
│   ├── login/          # Personel girişi
│   └── register/       # Dükkan kaydı
├── router/             # React Router yapılandırması
└── utils/              # VIN çözücü, Excel export
supabase/
└── functions/          # Edge Functions (AI tahmin, tedarikçi araştırması, bildirim vb.)
```