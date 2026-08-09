# ValixPro Kafe Paneli

Bu proje ValixPro Oto Panel temelinden Kafe Yönetimi paneline çevrildi.

## Yeni ana modüller
- Gösterge Paneli
- Siparişler
- POS
- Mutfak Ekranı (KDS)
- QR Menü
- Stok Takibi
- Müşteriler
- Personel
- Raporlar
- Bildirimler
- Ayarlar

## Çalışan demo akışı
POS ekranından oluşturulan siparişler `localStorage` içindeki `valix_cafe_orders` kaydına yazılır.
Siparişler sayfası ve Mutfak Ekranı aynı siparişleri okur; durumlar Yeni -> Hazırlanıyor -> Hazır -> Teslim Edildi şeklinde güncellenir.
Mevcut Supabase giriş/oturum altyapısı korunmuştur.

## Kurulum
```bash
npm install
npm run dev
```

## Not
Yeni kafe operasyon tabloları için Supabase şeması henüz eklenmedi. Core kafe modülleri şu aşamada frontend/localStorage demo akışı olarak hazırlanmıştır.
