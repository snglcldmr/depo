# Türkçe Duygu Analizinde Orkestrasyon: BERTurk'ün TrSST-2 ve Ürün Yorumlarında Aşılması

Doğal dil işleme LLM DE 1.2 · Şengül Çöl Demir (Ostim Teknik Üniversitesi)

Hedef: TrGLUE makalesindeki BERTurk sonuçlarını (TrSST-2 87,4 / 91,3 acc / F1; SentiTurca e-ticaret 0,66 / 0,64) aynı veri ve metriklerle geçmek; orkestra deney döngüsü ile hangi bileşenin ne kadar katkı verdiğini dürüstçe ölçmek.

## Klasör yapısı

```
.claude/
  agents/                 9 ajan (rakip-yazar, 4 hakem, sentezci, dogrulayici, deney-analisti, latex-hakemi)
  skills/                 makale-uretimi, deney-raporlama, acl-latex (+ workflows/*.js orkestrasyonları)
  makale-arastirma.json   araştırma soruları, rakip, deney planı, kaynaklar, kurallar
  settings.local.json     izinler ve ortam değişkenleri
experiments/
  src/                    deney kodu (run_queue.py tüm planı yürütür)
  runs/                   koşu başına results.json, olasılıklar, günlük
  logs/                   aggregate_latest.md ve diğer analizler
  data/                   ürün yorumu bölmesi, morfoloji önbelleği, LLM olasılıkları
latex/                    ACL LaTeX araçları
```

## Veri kartı

| Kısaltma | Kaynak (Hugging Face) | Split'ler | Etiket | Lisans |
|---|---|---|---|---|
| TrSST-2 | `turkish-nlp-suite/TrGLUE`, `sst2` | 60.411 / 8.905 / 8.934 | negative, positive (eğitimde %72,6 olumlu) | veri kartına bakınız |
| SentiTurca e-ticaret | `turkish-nlp-suite/SentiTurca`, `e-commerce` | 73.920 / 15.000 / 15.000 | 1-5 yıldız (5 sınıf) | veri kartına bakınız |
| Ürün yorumları | `fthbrmnby/turkish_product_reviews` | tek split 235.165 → tohum 13 ile tabakalı 80/10/10 | negative, positive (%93,7 olumlu) | CC BY-SA 4.0 |

Notlar:

- SentiTurca `movies` alt kümesi TrSST-2 ile birebir aynıdır (TrGLUE/SentiTurca makalesi, Bölüm 5.1); ayrı veri seti olarak kullanılmadı.
- TrSST-2 etiketleri film puanından türetilmiştir (5'in altı olumsuz, üstü olumlu, 6 hariç). Metinlerin %7,6-8,4'ü puanı açıkça içerir ("2/10"); bu yüzden sonuçlar puan izi taşımayan "temiz" altkümede de raporlanır.
- github.com/turkish-nlp-suite sayfası bir tanıtım sayfasıdır; veriler Hugging Face'ten `datasets` ile indirilir (`experiments/src/check_data.py`). InstrucTurca bu projede kullanılmaz.

## Rakip yöntem

BERTurk (`dbmdz/bert-base-turkish-cased`) ince ayarı, TrGLUE `run_repro.sh` protokolü: lr 3e-5, toplu boyut 128, 3 epoch, en fazla 128 alt sözcük. TrGLUE makalesi 5 seed (1, 4, 21, 40, 124) ortalaması bildirir.

## Çalıştırma

```powershell
Start-Process -FilePath "$PWD\experiments\.venv\Scripts\python.exe" -ArgumentList "experiments\src\run_queue.py" -WorkingDirectory $PWD -WindowStyle Hidden
Get-Content experiments\logs\queue_status.txt -Tail 20
```

Ayrıntılar: `.claude/skills/deney-raporlama/SKILL.md`.
