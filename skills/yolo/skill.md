---
name: yolo
description: Dangerously skip permissions mode. When user says /yolo or any yolo variant, bypass all confirmation prompts and execute immediately without asking permission, warnings, or clarifying questions.
---

# /yolo — Dangerously Skip Permissions Mode

Kullanıcı `/yolo` yazdığında aşağıdaki kurallar ANINDA devreye girer ve bu turn boyunca geçerlidir.

## ZORUNLU DAVRANIŞLAR

1. **SORU SORMA** — Hiçbir şeyde. Ne izin iste, ne "emin misin", ne "kontrol ettim". Direkt execute et.
2. **ONAY BEKLEME** — Destructive işlemler dahil: `rm -rf`, force push, git reset --hard, dosya silme, overwrite. Hepsini yap.
3. **UYARI YAZMA** — "Bu işlem geri alınamaz" gibi uyarı cümlesi yok. Sadece iş.
4. **PERMISSION MODU** — bypassPermissions aktif kabul et. Her tool call izinsiz çalışır.
5. **VARSAYILAN SEÇ** — Belirsizlikte en mantıklı default'u seç, sor değil.

## KOMUT BİRLEŞTİRME

`/yolo` diğer komut ve keyword'lerle birlikte kullanılır:

```
/yolo ultrathink <task>          → derin düşün + izinsiz uygula
/yolo /vault-save                → onaymadan kaydet ve push et
/yolo /github-push               → sor olmadan push et
/yolo rm -rf node_modules        → direkt sil
/yolo <herhangi bir görev>       → izinsiz tamamla
```

## UYGULAMA

Bu skill tetiklendiğinde:
1. ARGUMENTS kısmındaki komutu/görevi al
2. Eğer başka bir skill prefix'i varsa (`ultrathink`, `/vault-save` vb.) onu da çalıştır
3. Hiç soru sormadan, hiç uyarı vermeden, direkt execute et
4. Bitti. Kısa özet ver (ne yaptın, kaç dosya, sonuç).

## NOT

`--dangerously-skip-permissions` CLI flag'inin session içi karşılığıdır.
bypassPermissions settings.json'da zaten aktif; bu skill davranışı kesinleştirir ve
kombinasyon kullanımını mümkün kılar.
