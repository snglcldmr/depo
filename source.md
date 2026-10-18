# Türkçe Duygu Analizinde BERTurk'ün Yeniden Üretimi ve Aşılması: TrSST-2 ve SentiTurca E-ticarette Doğrulamaya Dayalı Topluluk Seçimi

Şengül Çöl Demir · Ufuk Asil — Ostim Teknik Üniversitesi, Yazılım Mühendisliği · Oct 12, 2026 · @Şengül Çöl Demir

## Özet

TrGLUE, BERTurk için TrSST-2'de 87,4 doğruluk ve 91,3 F1, SentiTurca e-ticarette 66,0 doğruluk ve 64,0 makro F1 raporlar. Bu değerleri TrGLUE'nun TrSST-2 protokolü ve beş seed'le yeniden ürettik (B0); kaynak SentiTurca ayarlarını vermediği için e-ticarette aynı ayar uygulandı. Her bileşeni aynı seed'lerde B0'a karşı eşleştirilmiş t ile, toplulukları ise bootstrap ile sınadık.

Bileşenler hiperparametre araması, morfolojik önişleme, Türkçe LLM ile etiket tutarlılığı oylaması, puan izi silme, ek kodlayıcılar, validation güdümlü topluluk seçimi (ORK2) ve film → ürün aktarımıdır. B0 yayımlanmış değerleri tek başına aşar: TrSST-2'de 87,75 / 91,54, e-ticarette 68,65 / 66,31. E-ticaretteki 2,65 puanlık farkın nedeni belirlenemedi; bu yüzden yöntem katkısını B0'a göre raporluyoruz.

B0'a karşı anlamlı tek model kazançları uzunluk, sözlük ve kodlayıcıdan gelir. 128k sözlük ve 256 alt sözcük TrSST-2'de 88,50 / 92,05 verir (+0,79 \[0,45; 1,12\], 3 seed; yalnız 256 alt sözcük 5 seed ile +0,59 \[0,30; 0,88\]). XLM-R large e-ticarette 69,45 / 67,30 verir (+0,72 \[0,43; 1,01\], 3 seed).

ORK2 TrSST-2'de 89,20 / 92,56'ya ulaşır (B0 topluluğuna göre +1,33 \[0,96; 1,72\]). Ancak 11 model kullanır ve aday havuzu test sonuçları görüldükten sonra genişletilmiştir. Planlanan bileşen orkestrası hiçbir bileşen seçmedi ve hiperparametre seçimine indirgendi.

Morfoloji (−1,20), LLM bayraklı örnekleri çıkarma (−1,20), ağırlıklandırma (−0,34), hiperparametre araması ve puan izi silme (−0,37) B0'ı geçmez. LLM füzyonu B0 topluluğuna anlamlı katkı yapmaz ve en iyi sistemi düşürür (88,93 < 89,20). Puan izleri B0 doğruluğunu yalnızca 0,28 puan şişirir ve temiz altkümede ilk iki sıra değişmez.

Film modeli ürün yorumlarına sıfır atışla 76,60 makro F1 aktarır. Film modelinden başlamak, 1.000 ürün örneğiyle ince ayarda makro F1'i +3,47 \[0,44; 6,50\] artırır; etki tek bir aykırı seed'e duyarlıdır. Bulgular, rakibe karşı üstünlüğün bir kısmının yeniden üretimden geldiğini ve yöntem katkısının eşleştirilmiş bir yeniden üretime göre raporlanması gerektiğini gösterir.

Sonradan eklenen üçüncü görevde (SentiTurca hate, dört sınıf) rakibin makro F1'i (58,0) B0 tekrarıyla aşıldı (59,83). Ancak mimari ve topluluk bileşenlerinin hiçbiri B0'a karşı topluluk düzeyinde anlamlı kazanç vermedi; ORK2 makro F1'de +0,38 \[−0,28; 1,01\] kaldı. Rakibin %61 doğruluğu çoğunluk sınıfı tabanının (%74,6) altında olduğundan bu görevde doğruluk karşılaştırılmadı. Sonradan eklenen sınıf dengesizliği analizinde validation'da seçilen sınıf eşik ayarı, ağırlıksız toplulukların makro F1'ini 5,8–9,1 puan artırdı. Validation'a göre seçilen eşik ayarlı B128KL topluluğu testte 69,08 makro F1 aldı (ayarsız B0 topluluğuna göre +9,02 \[7,05; 10,84\]); bu farkın büyük kısmı eşik ayarından gelir (eşik ayarlı B0 topluluğu +7,28) ve doğruluk 1,70 \[0,86; 2,62\] puan düşer. Karekök sınıf ağırlığı makro F1'i +6,04 \[5,32; 6,76\] artırdı ve doğruluğu 3,81 puan düşürdü; dengeli ağırlık anlamlı kazanç vermedi ve doğruluğu 12,74 puan düşürdü. Bu sonuçlar keşifseldir.

## Abstract

TrGLUE reports 87.4 accuracy and 91.3 F1 for BERTurk on TrSST-2, and 66.0 accuracy and 64.0 macro-F1 on SentiTurca e-commerce. We reproduce these numbers with the TrGLUE TrSST-2 protocol and five seeds (B0); since no SentiTurca settings are published, the same settings are used for e-commerce. Every component is tested against B0 on matched seeds with a paired t-test, and ensembles are tested with a paired bootstrap.

The components are hyperparameter search, morphological preprocessing, label-consistency voting with a Turkish LLM, rating-trace removal, additional encoders, validation-driven ensemble selection (ORK2), and movie-to-product transfer. B0 alone already exceeds the published numbers: 87.75 / 91.54 on TrSST-2 and 68.65 / 66.31 on e-commerce. We could not identify the cause of the 2.65-point e-commerce gap, so we report method gains against B0.

Significant single-model gains over B0 come from sequence length, vocabulary and encoder choice. A 128k-vocabulary BERTurk with 256 subwords reaches 88.50 / 92.05 on TrSST-2 (+0.79 \[0.45, 1.12\], 3 seeds; 256 subwords alone give +0.59 \[0.30, 0.88\] over 5 seeds). XLM-R large reaches 69.45 / 67.30 on e-commerce (+0.72 \[0.43, 1.01\], 3 seeds).

ORK2 reaches 89.20 / 92.56 on TrSST-2 (+1.33 \[0.96, 1.72\] over the B0 ensemble). However, it uses 11 models and a candidate pool that grew after test results were seen. The pre-planned component orchestration selected no component and reduced to hyperparameter selection.

Morphology (−1.20), dropping LLM-flagged examples (−1.20), down-weighting them (−0.34), hyperparameter search and rating-trace removal (−0.37) do not beat B0. LLM fusion does not significantly improve the B0 ensemble and lowers the best system (88.93 vs. 89.20). Rating traces inflate B0 accuracy by only 0.28 points, and the top two systems keep their rank on the trace-free subset.

A movie model transfers to product reviews with 76.60 zero-shot macro-F1. Initializing from it improves macro-F1 by +3.47 \[0.44, 6.50\] with 1,000 labeled product examples, although the effect is sensitive to one outlier seed. Our results show that part of the advantage over the published baseline stems from reproduction itself, so method contributions should be reported against a matched reproduction.

On a third task added afterwards (SentiTurca hate, four classes), our B0 reproduction already exceeds the published macro-F1 (59.83 vs. 58.0), but no architectural or ensemble component yields a significant ensemble-level gain over B0; ORK2 reaches +0.38 \[−0.28, 1.01\] macro-F1. The published 61% accuracy lies below the 74.6% majority-class baseline, so accuracy is not compared on this task. In a post-hoc class-imbalance analysis, class-specific decision thresholds selected on validation raise the macro-F1 of unweighted ensembles by 5.8–9.1 points. The validation-selected system, a threshold-tuned B128KL ensemble, reaches 69.08 test macro-F1 (+9.02 \[7.05, 10.84\] over the untuned B0 ensemble); most of this gain comes from thresholding itself (threshold-tuned B0 ensemble: +7.28), and accuracy drops by 1.70 \[0.86, 2.62\] points. A square-root class-weighted loss gains +6.04 \[5.32, 6.76\] macro-F1 at a cost of 3.81 accuracy points, whereas fully balanced weights give no significant gain and cost 12.74 accuracy points. These results are exploratory.

## 1. Giriş

Türkçe eklemeli bir dildir: türetim ve çekim, köke art arda eklenen biçimbirimlerle yapılır. Bu yapıda duygu taşıyan bilginin önemli bir kısmı kökte değil ektedir. Örneğin "beğendim" ile "beğenmedim" arasındaki kutup farkını yalnızca olumsuzluk eki "-mA" belirler.

Önceden eğitilmiş Türkçe kodlayıcılar bu bilgiyi alt sözcük belirteçlemesi üzerinden dolaylı olarak öğrenir. BERTurk \[5\], BERT mimarisini \[4\] Türkçe derlemle eğiten ve WordPiece sözlüğü kullanan yerleşik bir temel çizgidir. Ancak WordPiece sınırlarının biçimbirim sınırlarıyla örtüşmesi gerekmez \[15\]\[16\].

Türkçe doğal dil anlama çalışmaları uzun süre dağınık ve birbiriyle güç karşılaştırılan veri setlerine dayandı \[17\]. TrGLUE ve SentiTurca, GLUE \[2\] düzenini Türkçeye taşıyarak ortak bir değerlendirme zemini sundu \[1\]. Bu zeminde BERTurk, TrSST-2 üzerinde 87,4 doğruluk ve 91,3 F1 (TrGLUE Tablo 7), SentiTurca e-ticaret alt kümesinde ise 0,66 doğruluk ve 0,64 makro F1 (TrGLUE Tablo 11) ile raporlanmıştır \[1\].

Yayımlanmış tek bir sayıyı geçmek kolay görünür, ancak ampirik açıdan kırılgan bir hedeftir. İnce ayar sonuçları ağırlık başlatma, veri sırası ve durdurma kararlarıyla belirgin biçimde değişir \[22\]. Bu yüzden rakibe karşı kurulan her iddiayı, aynı seed'lerde kendi yeniden üretimimize (B0) karşı da sınıyoruz.

Rakip protokolün kendisi de tek anlamlı değildir. TrGLUE GitHub deposunun README dosyası, RTE, STS-B ve MRPC dışındaki görevler için toplu boyut 128 ve öğrenme oranı 2e-5 verir. Seed olarak da 1, 4, 21, 40 ve 124'ü bildirir ve sonuçların bu beş seed üzerinden ortalandığını söyler. Aynı depodaki `run_repro.sh` betiği ise sst2 için öğrenme oranı 3e-5, toplu boyut 128, 3 epoch, en fazla 128 alt sözcük ve tek seed (42) kullanır.

B0, betikteki ayarları README'deki beş seed ile birleştirir. B0R ise README okumasını (2e-5, 128, betikteki gibi 3 epoch) aynı beş seed ile koşar. Böylece rakip protokolün iki okuması da aynı kod yolunda ve aynı seed'lerde ölçülmüş olur.

İkinci sorun veri tarafındadır. TrSST-2 etiketleri film puanlarından türetilmiştir \[1\] ve metinlerin %7,6-8,4'ü "2/10" gibi etiketi doğrudan ele veren bir puan izi taşır (Bölüm 3.5). Bu durum, açıklama artefaktlarının skorları şişirmesi riskini doğurur \[21\]. Bu yüzden tüm TrSST-2 sonuçlarını puan izi taşımayan temiz altkümede de raporluyoruz.

Çalışmanın yöntemsel çekirdeği bir "orkestra" deney döngüsüdür. Her bileşen önce tek başına B0'a karşı ölçülür, ardından yalnızca validation kümesinde yardımcı olanlar birleştirilir. Bileşenler şunlardır: hiperparametre araması, morfolojik önişleme \[7\]\[8\], Türkçe LLM \[9\] ile etiket tutarlılığı oylaması \[12\]\[14\], puan izi silme, daha uzun bağlam ve farklı önceden eğitilmiş kodlayıcılar, seed ve model toplulukları \[24\], BERTurk + LLM füzyonu ve film yorumlarından ürün yorumlarına aktarım \[19\]\[20\].

### 1.1 Katkılar

Bu bölüm katkıları ana sayılarıyla özetler; ayrıntılar Bölüm 6-7'dedir.

- **Denetlenebilir yeniden üretim.** Rakip protokolün iki okuması raporlanır. Betik okuması (B0) ve README okuması (B0R) beş seed ve seed başına sonuçlarla koşulur: TrSST-2'de B0 87,75 ± 0,12 / 91,54 ± 0,09 ve B0R 87,66 / 91,49; e-ticarette B0 68,65 / 66,31 ve B0R 68,70 / 66,27 (doğruluk / F1). İki okuma arasındaki fark iki görevde de anlamlı değildir.
- **Bileşen bazında katkı ölçümü.** Her bileşen B0'a karşı aynı seed'lerde eşleştirilmiş t ile sınanır: TrSST-2'de yalnızca bağlam uzunluğu ve sözlük değişiklikleri anlamlı kazanç sağlar (B128KL +0,79, LEN256 +0,59, L256H +0,43, B128K +0,43 doğruluk puanı). Morfoloji (−1,20), LDROP (−1,20), STRIP (−0,37), LWEIGHT (−0,34) ve ELECTRA (−0,22) anlamlı kayıp verir; HBEST anlamlı değildir. E-ticarette XLMRL (+0,72) ve B128K (+0,42) kazanç, MORPH (−1,87) kayıp verir.
- **LLM ile etiket denetimi.** Bayrak oranı, puan izli ve izsiz örneklerde ayrı ayrı raporlanır: eğitim örneklerinin %10,02'si bayraklanır (puan izli örneklerde %5,43, izsizlerde %10,44). Bayraklı örnekleri çıkarmak ya da ağırlığını düşürmek doğruluğu artırmaz, düşürür.
- **Puan izi denetimi.** Tüm TrSST-2 yapılandırmaları temiz test altkümesinde ayrıca değerlendirilir: temiz altkümede B0 87,47 / 91,38 sonuç verir; ilk iki sıra (B128KL, LEN256) korunur, alt sıralarda ss düzeyinde yer değişimleri olur (ör. L256H 3–4. sıradan 5. sıraya iner, STRIP 12. sıradan 8. sıraya çıkar); puan izlerini silmek (STRIP) temiz altkümede de B0'ı geçmez.
- **Alan aktarımı.** Film modelinin ürün yorumlarına sıfır atışlı ve 1.000 örnekli aktarımı ölçülür: sıfır atışta makro F1 76,60, 1.000 örnekte FSBASE 76,68 ve FSFILM 80,15, tüm veriyle FULL 86,00'dır. Film modelinden başlamak az örnekli eğitimde makro F1'i +3,47 puan artırır (%95 GA \[0,44; 6,50\]).
- **Orkestra, topluluk ve füzyon.** Validation güdümlü birleşim (ORK), model topluluğu (ORK2) ve BERTurk + LLM füzyonu B0 topluluğuna karşı bootstrap ile sınanır: ORK hiçbir bileşeni seçmez ve HBEST'e eşit kalır. ORK2 TrSST-2'de 89,20 / 92,56 (B0 topluluğuna göre +1,33 \[0,96; 1,72\]), e-ticarette 69,89 / 67,72 (+0,80 \[0,41; 1,19\]) verir. LLM füzyonu tek modellere değil seed topluluklarına uygulanır. Kendi topluluğuna göre katkı B0'da +0,32, HBEST'te +0,28, LEN256 ve L256H'de +0,07, ORK2'de −0,27'dir (türetilmiş). B0 topluluğuna katkısı anlamlı değildir (+0,32 \[−0,04; +0,67\]).
- **Açık kod ve kayıt.** Ürün yorumu bölmesi, morfoloji önbelleği, LLM olasılıkları ve koşu başına çıktılar dosya olarak saklanır.

Başlıktaki "aşılması" iddiası iki ölçütle sınanmıştır: seed ortalamamızın yayımlanmış değeri aşması ve kendi B0'ımıza karşı farkın %95 güven aralığının sıfırı dışlaması. Birinci ölçütü iki görevde de B0 tek başına sağlar. İkinci ölçütü yalnızca uzunluk ve sözlük değişiklikleri, e-ticarette kodlayıcı (XLMRL) ve bootstrap düzeyinde topluluk seçimi (ORK2) sağlar (Bölüm 6.5, 9). Ürün yorumlarında yayımlanmış bir rakip sonuç yoktur; bu alanda yalnızca aktarım karşılaştırması verilir.

### 1.2 Araştırma soruları

- **AS1.** TrGLUE protokolüyle yeniden üretilen BERTurk, TrGLUE Tablo 7 değerlerine (TrSST-2 87,4 / 91,3) ulaşıyor mu? Hiperparametre araması, daha güçlü kodlayıcılar ve topluluklar bu değeri aşıyor mu?
- **AS2.** Kök ve kök-ek ayrımı biçimindeki morfolojik önişleme, BERTurk'ün alt sözcük belirteçlemesine ek bilgi sağlıyor mu?
- **AS3.** Türkçe LLM ile etiket tutarlılığı oylaması gürültülü eğitim örneklerini bulup ince ayarı iyileştiriyor mu? BERTurk + LLM füzyonu katkı sağlıyor mu?
- **AS4.** Film yorumlarında eğitilen model ürün yorumlarına ne kadar aktarılıyor (sıfır atış, 1.000 örnekle ince ayar)?
- **AS5.** TrSST-2 metinlerindeki puan izleri skorları ne ölçüde etkiliyor; temiz altkümede sıralama değişiyor mu?

### 1.3 Makalenin düzeni

Bölüm 2 ilgili çalışmaları beş eksende özetler: Türkçe ölçütler, belirteçleme ve morfoloji, etiket gürültüsü, alan aktarımı ile artefaktlar ve değerlendirme istatistiği. Bölüm 3 veri setlerini, puan izlerini ve uzunluk dağılımlarını tanımlar. Bölüm 4 yöntemi koda birebir bağlı biçimde verir. Bölüm 5 deney matrisini, seçim kurallarını, metrikleri, istatistiksel çıkarımı ve donanımı açıklar. Bölüm 6 yeniden üretimi (AS1), tek modelleri, toplulukları ve LLM füzyonunu (AS3'ün ikinci yarısı), Bölüm 6.6–6.7 ise sonradan eklenen SentiTurca hate görevini ve sınıf dengesizliği yöntemlerini sunar. Bölüm 7 ablasyonları AS2, AS3, AS5 ve AS4 sırasıyla verir. Bölüm 8 tartışma ve sınırlılıkları, Bölüm 9 araştırma sorularının yanıtlarını içerir.

## 2. İlgili Çalışmalar

### 2.1 Türkçe NLU ölçütleri ve BERTurk

GLUE, çok görevli bir değerlendirme düzeniyle önceden eğitilmiş modellerin karşılaştırılmasını standartlaştırdı \[2\]. İkili duygu görevi SST-2, Stanford Sentiment Treebank'ten türetilmiştir \[3\]. TrGLUE bu düzeni Türkçeye taşır. SentiTurca ise film, müşteri yorumu ve nefret söylemi alt kümeleri içeren bir duygu ölçütüdür \[1\].

TrSST-2, Sinefil.com ve Beyazperde.com'dan derlenen yaklaşık 78 bin film yorumundan oluşur \[1\]. Kaynak makale, SentiTurca'nın film alt kümesini Bölüm 5.1'de TrSST-2 ile aynı veri olarak tanımlar \[1\]. Daha önce Mukayese, Türkçe için birden çok görevi kapsayan düzenli bir karşılaştırma takımı sunmuştu \[17\].

BERT, maskeli dil modellemesiyle ön eğitilip göreve ince ayarlanan bir kodlayıcıdır \[4\]. BERTurk bu mimarinin Türkçe sürümüdür ve model kartına göre 35 GB'lık bir derlemle eğitilmiştir \[5\]. Derlem süzülmüş Türkçe OSCAR, bir Wikipedia dökümü, OPUS derlemleri ve Kemal Oflazer'in sağladığı bir derlemden oluşur \[5\].

TrGLUE'nun BERTurk değerleri, Hugging Face `run_glue.py` betiğinin küçük değişikliklerle kullanıldığı bir protokolden gelir \[1\]\[6\]. Bu çalışma o değerleri sabit bir hedef olarak değil, önce yeniden üretilmesi gereken bir başlangıç noktası olarak ele alır.

### 2.2 Belirteçleme ve Türkçe morfolojisi

Türkçenin bitişken yapısı sözcük başına çok sayıda biçimbirim üretir. Rust vd., belirteçleyici kalitesinin tek dilli ve çok dilli modellerin aşağı akış başarımını etkilediğini göstermiştir \[16\]. Toraman vd. Türkçede beş belirteçleme düzeyini karşılaştırmış ve morfoloji düzeyindeki belirteçleyicinin yaygın yöntemlerle rekabetçi kaldığını bildirmiştir \[15\].

Aynı çalışma, sözlük boyutunu artırmanın morfoloji ve sözcük düzeyindeki belirteçleyicilere daha çok yaradığını raporlar \[15\]. Biz belirteçleyiciyi yeniden eğitmiyoruz. Bunun yerine metni BERTurk'ün WordPiece girdisinden önce kök ya da kök-ek dizisine dönüştürüyoruz ve ayrıca 128 bin sözcüklük BERTurk sürümünü deniyoruz. Bu tasarım, morfolojik bilginin ince ayar girdisinde ya da daha geniş bir sözlükte verilmesinin işe yarayıp yaramadığını sınar.

Çözümleme için Zemberek \[7\] kullanılır. Zemberek, Türkçe için açık kaynaklı bir biçimbilimsel çözümleme çerçevesidir. Çalışmada onun kısmi Python uyarlaması zeyrek \[8\] kullanılır.

### 2.3 Etiket gürültüsü ve LLM ile etiket denetimi

Etiket gürültüsü altında sınıflandırma geniş bir literatüre sahiptir \[13\]. Güvenli öğrenme (Confident Learning), model olasılıklarından gürültülü ve gerçek etiketlerin ortak dağılımını kestirip şüpheli örnekleri ayıklar \[12\]. Gilardi vd., ChatGPT'nin bazı metin etiketleme görevlerinde kitle çalışanlarından daha iyi sonuç verdiğini bildirmiştir \[14\].

Biz bu iki çizgiyi birleştiriyoruz: dış bir Türkçe LLM, eğitim etiketleriyle tutarlılığı oylayan bağımsız bir denetçi olarak kullanılır. Model Turkish-Llama-8b-Instruct-v0.1'dir \[9\]. Bu model, Llama 3 8B Instruct'ın \[10\] Türkçe metinle tam ince ayarlanmış bir sürümüdür. Bellek için QLoRA'da önerilen 4 bit NF4 nicemleme \[11\] uygulanır.

### 2.4 Alan aktarımı ve açıklama artefaktları

Blitzer vd., duygu sınıflandırıcılarının ürün kategorileri arasında aktarıldığında başarım yitirdiğini göstermiştir \[19\]. Gururangan vd., alana ve göreve uyarlanmış ek ön eğitimin aşağı akış başarımını artırabildiğini raporlamıştır \[20\]. Türkçede Demirtas ve Pechenizkiy, film ve ürün yorumlarında makine çevirisiyle diller arası kutup tespitini incelemiştir \[18\]. Biz ön eğitimi sürdürmek yerine, film görevinde ince ayarlanmış modeli az örnekli ürün ince ayarının başlangıç noktası olarak kullanıyoruz.

Açıklama artefaktları, modelin görevi çözmeden yüzeysel ipuçlarıyla yüksek skor almasına yol açabilir \[21\]. Gururangan vd. bunu doğal dil çıkarımında yalnızca hipotezden tahmin yapan modellerle göstermiştir \[21\]. TrSST-2'deki puan izleri de etiketi doğrudan ele veren bu türden bir ipucudur.

### 2.5 İnce ayar varyansı ve anlamlılık sınaması

Dodge vd., yalnızca seed değiştirerek BERT ince ayarında belirgin sonuç farkları elde edilebildiğini göstermiştir \[22\]. Berg-Kirkpatrick vd., NLP'de sistem farklarının anlamlılığını bootstrap ile sınamanın pratiğini ve tuzaklarını incelemiştir \[23\]. Derin topluluklar, bağımsız eğitilmiş modellerin olasılıklarını ortalayarak daha kararlı tahmin üretir \[24\].

### 2.6 Bu çalışmanın konumu

Önceki çalışmalar bu bileşenleri çoğunlukla ayrı ayrı ve farklı protokollerle incelemiştir. Bu çalışma onları tek bir rakip protokolün etrafında, aynı seed'ler ve aynı seçim kuralıyla ölçer. Her bileşenin katkısı hem yayımlanmış değere hem de kendi yeniden üretimimize karşı raporlanır.

## 3. Veri Setleri

Üç veri seti Hugging Face `datasets` kütüphanesiyle indirilir ve `common.load_task` ile ortak biçime getirilir. Her örnek dört sütun taşır: `text`, `label`, split içi sıra `idx` ve puan izi bayrağı `leak`. Tüm istatistikler `experiments/logs/data_stats.md` dosyasından alınmıştır.

### 3.1 TrSST-2

TrSST-2, `turkish-nlp-suite/TrGLUE` deposunun `sst2` alt kümesidir. Kaynak makaleye göre yorumlar Sinefil.com (yaklaşık 40 bin) ve Beyazperde.com'dan (yaklaşık 38 bin) toplanmıştır \[1\]. Her yorum 0-10 arası bir puanla eşlenmiştir. 6 puanlılar karışık sayılıp çıkarılmış, 5'in altı olumsuz, 5'in üstü olumlu etiketlenmiştir \[1\].

Kaynak metin eğitim kümesi için 67 bin, kaynağın Tablo 1'i ise 60 bin örnek verir \[1\]. İndirdiğimiz sürümde eğitim kümesi 60.411 örnektir (Tablo 1); bu, kaynağın Tablo 1'i ile uyumludur. Validation ve test etiketleri açıktır. Test etiketleri yalnızca raporlamada kullanılır.

### 3.2 SentiTurca e-ticaret

SentiTurca e-ticaret alt kümesi `turkish-nlp-suite/SentiTurca` deposunun `e-commerce` yapılandırmasıdır. Yaklaşık 103 bin Hepsiburada ve Trendyol yorumundan oluşur ve 1-5 yıldızla etiketlenmiştir \[1\]. Kaynak makale, görünürlük için 5 yıldız verip ardından olumsuz eleştiri yazan yorumları ayıklamıştır. Bunun için 5 yıldızlı yorumlar anlamsal olarak kümelenmiş, yinelenen örüntüler bir listede toplanmış ve bunlarla eşleşen yorumlar çıkarılmıştır \[1\].

Rakip sonuç 0,66 doğruluk ve 0,64 makro F1'dir (TrGLUE Tablo 11) \[1\]. SentiTurca'nın film alt kümesi TrSST-2 ile aynı veridir \[1\]; bu nedenle ayrı bir veri seti olarak kullanılmamıştır.

### 3.3 Türkçe ürün yorumları

`fthbrmnby/turkish_product_reviews` veri kartı, çevrim içi toplanmış 235.165 ürün yorumu bildirir \[25\]. Kartta 220.284 olumlu ve 14.881 olumsuz yorum yer alır; kaynak site belirtilmemiştir \[25\]. Kart metni lisansı CC BY-SA 4.0 olarak verir, ancak kart kenar çubuğu "unknown" gösterir \[25\].

Veri tek split olarak gelir. `common._prod_splits` veriyi etikete göre tabakalı, iki aşamalı bir bölmeyle ve tohum 13 ile 80/10/10 oranında ayırır. Önce %20'lik bir kalan ayrılır, ardından bu kalan yine tabakalı biçimde ikiye bölünür. Bölme indeksleri `experiments/data/prod_split.json` dosyasına yazılır ve sonraki tüm koşular aynı bölmeyi okur.

Split'lerin etiket sayıları toplandığında 220.284 olumlu ve 14.881 olumsuz örnek elde edilir (türetilmiş). Bu toplamlar veri kartıyla birebir örtüşür; dolayısıyla bölme sırasında örnek kaybı yoktur. Bu veri seti AS4 için hedef alandır.

### 3.4 Tanımlayıcı istatistikler

Alt sözcük sayıları `dbmdz/bert-base-turkish-cased` belirteçleyicisiyle, özel belirteçler dahil ve kesme uygulanmadan hesaplanmıştır. ">128" sütunu, eğitimde kesmeye uğrayacak örneklerin oranıdır. Sözcük sayısı boşluğa göre bölmeyle bulunur.

**Tablo 1.** Veri setlerinin split boyutları, olumlu oranı, uzunlukları, 128 alt sözcük sınırını aşan örnek oranı ve puan izi oranı (`data_stats.md`).

| Görev | Split | n | Olumlu (%) | Ort. sözcük | Ort. / medyan / p95 alt sözcük | >128 alt sözcük (%) | Puan izi (%) |
| --- | --- | --: | --: | --: | --- | --: | --: |
| TrSST-2 | eğitim | 60.411 | 72,6 | 46,9 | 76,5 / 46 / 198 | 12,4 | 8,4 |
| TrSST-2 | validation | 8.905 | 70,9 | 46,2 | 75,5 / 46 / 196 | 12,3 | 7,6 |
| TrSST-2 | test | 8.934 | 71,0 | 46,4 | 76,3 / 45 / 193 | 12,1 | 7,6 |
| E-ticaret | eğitim | 73.920 | – | 13,2 | 20,7 / 15 / 54 | 0,4 | 0,3 |
| E-ticaret | validation | 15.000 | – | 13,4 | 21,0 / 15 / 55 | 0,3 | 0,2 |
| E-ticaret | test | 15.000 | – | 13,4 | 20,9 / 15 / 54 | 0,4 | 0,3 |
| Ürün | eğitim | 188.132 | 93,7 | 22,2 | 35,2 / 29 / 86 | 1,5 | 0,1 |
| Ürün | validation | 23.516 | 93,7 | 22,2 | 35,2 / 29 / 88 | 1,6 | 0,1 |
| Ürün | test | 23.517 | 93,7 | 22,1 | 35,0 / 29 / 85 | 1,4 | 0,1 |

**Tablo 2.** SentiTurca e-ticaret sınıf dağılımı: örnek sayısı ve parantez içinde split içi yüzde (`data_stats.md`).

| Split | 1 yıldız | 2 yıldız | 3 yıldız | 4 yıldız | 5 yıldız |
| --- | --: | --: | --: | --: | --: |
| eğitim | 9.053 (12,2) | 8.099 (11,0) | 12.883 (17,4) | 22.142 (30,0) | 21.743 (29,4) |
| validation | 1.905 (12,7) | 1.711 (11,4) | 2.629 (17,5) | 4.524 (30,2) | 4.231 (28,2) |
| test | 1.915 (12,8) | 1.662 (11,1) | 2.542 (16,9) | 4.541 (30,3) | 4.340 (28,9) |

TrSST-2 yorumları sözcük sayısında ürün yorumlarının yaklaşık 2,1, e-ticaret yorumlarının yaklaşık 3,6 katıdır (türetilmiş: 46,9 / 22,2 ve 46,9 / 13,2). TrSST-2'de örneklerin yaklaşık sekizde biri 128 alt sözcük sınırını aşar. Bu örneklerde rakip protokolün kesmesi metnin sonunu atar.

Tablo 1 ve 2'den çoğunluk sınıfı tabanları türetilebilir. Bunlar deney sonucu değil, veri istatistiğidir. Her şeye "olumlu" diyen bir sistem TrSST-2 testinde %71,0, ürün testinde %93,7 doğruluk alır (türetilmiş). E-ticaret testinde her şeye "4 yıldız" demek %30,3 doğruluk verir (türetilmiş).

Ürün yorumlarındaki bu dengesizlik doğruluğu tek başına bilgisiz kılar. Bu yüzden ürün testinde makro F1 birincil metriktir. TrSST-2'de ise rakiple karşılaştırılabilirlik için doğruluk ve olumlu sınıf F1'i birincil kalır.

### 3.5 Puan izleri

Puan izi `common.RATING_RE` düzenli ifadesiyle tanımlanır ve büyük/küçük harfe duyarsızdır.

```
\b(10|[0-9])([.,]5)?\s*/\s*10\b  |  \b(10|[0-9])\s*puan
```

Birinci seçenek "7/10", "8,5/10" ve "8.5 / 10" gibi on üzerinden puanları yakalar. İkinci seçenek bir rakamı izleyen "puan" dizisini yakalar. Sağda sözcük sınırı aramadığı için "4 puanlık" gibi ekli biçimleri de kapsar.

TrSST-2'de puan izi oranı eğitimde %8,4, validation ve testte %7,6'dır (Tablo 1). E-ticaret ve ürün verilerinde oran %0,3'ü geçmez; yani bu artefakt büyük ölçüde TrSST-2'ye özgüdür. Etiket puandan türetildiği için TrSST-2'deki iz çoğu durumda etiketi doğrudan belirler.

"Temiz altküme", özgün metninde `RATING_RE` eşleşmesi olmayan örneklerdir. Bayrak `load_task` içinde ham metin üzerinden bir kez hesaplanır ve önişlemeden etkilenmez. Temiz altküme ayrı bir model eğitimi gerektirmez; aynı tahminler iz taşıyan örnekler dışarıda bırakılarak yeniden puanlanır.

Düzenli ifade kasıtlı olarak dar tutulmuştur. "On üzerinden yedi" gibi yazıyla verilen puanları ya da yıldız simgelerini yakalamaz. Duyarlılığı ve kesinliği elle doğrulanmamıştır. Bu nedenle temiz altküme puan bilgisinden tamamen değil, yalnızca açık sayısal puan izinden arınmış bir küme olarak okunmalıdır.

### 3.6 Morfolojik varyantların uzunluğa etkisi

Morfoloji önbelleği, üç görevin tüm split'lerindeki 384.081 benzersiz küçük harfli sözcüğü kapsar. Bunların 188.617'si (%49,1) kök ve en az bir ek olarak bölünmüştür. Geri kalanlar ya tek biçimbirimlidir ya da çözümleme yüzey biçimiyle tutarsız olduğu için olduğu gibi bırakılmıştır (Bölüm 4.2).

**Tablo 3.** TrSST-2 validation kümesinde morfolojik varyantların alt sözcük uzunluğuna etkisi (`data_stats.md`).

| Varyant | Örnek dönüşüm | Ort. alt sözcük | >128 alt sözcük (%) |
| --- | --- | --: | --: |
| none (özgün metin) | metin değişmez | 75,5 | 12,3 |
| kok | "evlerimizden" → "ev" | 68,5 | 9,9 |
| kokek | "evlerimizden" → "ev +ler +imiz +den" | 132,6 | 29,7 |

Kök varyantı ortalama uzunluğu kısaltır ve kesilen örnek oranını düşürür. Kök-ek varyantı ise ortalama uzunluğu yaklaşık 1,76 katına çıkarır (türetilmiş: 132,6 / 75,5). 128 alt sözcük sınırında validation örneklerinin %29,7'si kesilir; özgün metinde bu oran %12,3'tür.

Artışın önemli bir bölümü muhtemelen gösterim biçiminden kaynaklanır. BERT'in temel belirteçleyicisi "+" işaretini noktalama olarak ayırır; bu durumda her ek en az iki belirteç üretir. Ayrıca ek, sözcük içi "##" parçası yerine sözcük başı parçası olarak kodlanır. Bu payın büyüklüğü ölçülmemiştir, çünkü "+" işaretsiz bir denetim varyantı yoktur.

Sonuç olarak KOKEK karşılaştırmasında iki etki birbirine karışır. Biri açık biçimbirim sınırlarının olası yararıdır. Diğeri, örneklerin yaklaşık üçte birinde metin sonunun kesilmesinden doğan bağlam kaybıdır. Bölüm 5.6 bu karıştırıcıyı tartışır.

### 3.7 SentiTurca hate (sonradan eklenen üçüncü görev)

SentiTurca'nın `hate` yapılandırması (Türk Nefret Haritası), Ekşi Sözlük entry'lerinden derlenmiş dört sınıflı bir veri setidir: hakaret (offensive), nefret (hate), nötr (neutral) ve medeni (civilized). Split'ler 42.175 / 5.000 / 5.000 örnektir. Görev, ilk 143 koşu ve makalenin ilk sürümü tamamlandıktan sonra çalışmayı derinleştirmek için eklenmiştir; bu yüzden Bölüm 8.3'teki uyarlamalı tasarım çekincesine tabidir.

SentiTurca'nın kendi betiği (`run_sentiturca.py`) bu görevde girdiyi (başlık, entry) cümle çifti olarak verir. Biz de aynı sırayla çift girdi kullandık (`common.TASKS["hate"]`, `train.encode`). Her split'in ilk 3.000 örneğinde çiftlerin yaklaşık %19–21'i 128 alt sözcüğü, %6–8'i 256 alt sözcüğü aşar.

Sınıf dağılımı çok dengesizdir. Test kümesinde 3.729 nötr (%74,6), 1.025 hakaret, 145 nefret ve 101 medeni örnek vardır; eğitim kümesinde medeni sınıfı 947 örnekle %2,2'dir. Her örneğe "nötr" diyen çoğunluk sınıfı tabanı testte %74,58 doğruluk ve %21,36 makro F1 alır (validation'da %74,14). Bu yüzden bu görevde birincil ölçüt makro F1'dir. Test kümesindeki başlıkların tamamı eğitim kümesinde de geçer; split başlık düzeyinde ayrılmadığı için sonuçlar görülmemiş konulara genellemeyi ölçmez.

## 4. Yöntem

Bu bölüm `experiments/src` altındaki kodu birebir izler ve her alt başlıkta ilgili dosya ile işleve atıf yapar. Tüm ince ayar koşuları `train.py` ile tek bir kod yolundan geçer. Yapılandırmalar yalnızca komut satırı bayraklarıyla ayrışır.

### 4.1 Temel model ve B0 ince ayarı

Temel model `dbmdz/bert-base-turkish-cased` (BERTurk) \[5\] üzerine eklenen bir dizi sınıflandırma başlığıdır (`AutoModelForSequenceClassification`). Başlık, havuzlanmış \[CLS\] gösterimi üzerindeki doğrusal katmandır. Eğitim Hugging Face Transformers \[6\] `Trainer` sınıfının bir alt sınıfıyla yapılır. TrSST-2 ve ürün görevleri iki, e-ticaret görevi beş sınıflıdır.

B0 ayarları TrGLUE `run_repro.sh` betiğinin sst2 dalını izler: öğrenme oranı 3e-5, toplu boyut 128, 3 epoch ve en fazla 128 alt sözcük. Öğrenme oranı doğrusal azalır; ısınma ve ağırlık azalımı sıfırdır. Değerlendirme toplu boyutu 256'dır ve dolgu her toplu iş içinde dinamik yapılır. B0R aynı kod yolunu README okumasıyla (öğrenme oranı 2e-5) koşar.

Eğitim sırasında ara değerlendirme, ara kayıt ve erken durdurma yoktur. Raporlanan model, son epoch sonundaki modeldir. CUDA varsa karma duyarlık (fp16) açıktır. Kayıp ise logitler float32'ye çevrilerek hesaplanır.

`set_seed(seed)` ve `data_seed=seed` birlikte ayarlanır; böylece seed hem başlık başlatmasını hem de veri sırasını belirler. Seed kümesi, TrGLUE deposunun README dosyasında bildirilen {1, 4, 21, 40, 124} kümesidir. Her koşu, validation, test ve varsa ek değerlendirme kümelerinin softmax olasılıklarını diske yazar.

TrSST-2 B0 koşuları modeli kaydeder ve ürün test kümesini ek değerlendirme olarak tahmin eder (`--save_model --extra_eval prod`). Bu tahmin sıfır atışlı aktarımı verir; kaydedilen modeller ise FSFILM'in başlangıç noktasıdır (Bölüm 4.10).

### 4.2 Morfolojik önişleme (KOK, KOKEK)

`morph.Morph.transform`, metni `\w+|[^\w\s]+` ifadesiyle sözcük ve noktalama birimlerine ayırır. Yalnız harften oluşan birimler Türkçeye uygun küçük harfe çevrilir (I→ı, İ→i) ve zeyrek \[8\] ile çözümlenir. Sayı ve noktalama gibi diğer birimler değiştirilmeden korunur.

Çözümlemede zeyrek'in döndürdüğü ilk ayrıştırma kullanılır; bağlamsal belirsizlik giderme yapılmaz. Biçimlendirilmiş çözümleme dizesinden yüzeyi olan biçimbirimler sırayla alınır. Örneğin "insan:Noun+A3sg+ın:Gen" dizesinde yüzeysiz "A3sg" atlanır ve \["insan", "ın"\] elde edilir.

Yüzeylerin birleşimi küçük harfli sözcüğe eşitse bölme kabul edilir. Eşit değilse ya da çözümleme hata verirse sözcük tek parça bırakılır. Bu denetim, ses olaylarının ve bilinmeyen sözcüklerin bozuk bölütlemeye yol açmasını önler.

Kesme işaretli özel adlarda ek, çözümleyiciden önce birim ayırıcıyla ayrılır. Örneğin "Türkiye'de" önce "Türkiye", "'" ve "de" birimlerine bölünür. Dönüşüm tüm birimleri tek boşlukla yeniden birleştirdiği için noktalama da sözcüklerden ayrılmış olur. BERTurk belirteçleyicisi noktalamayı zaten ayırdığından bu yan etkinin alt sözcük dizisine etkisi küçük olmalıdır.

Kök, özgün sözcüğün ilk karakterlerinden kesilir; böylece özgün büyük/küçük harf yazımı korunur. BERTurk büyük/küçük harfe duyarlı olduğu için bu önemlidir. `kok` modunda her sözcük yalnızca yüzey köküyle değiştirilir ("beğenmedim" → "beğen"). `kokek` modunda ekler "+" önekiyle ayrı birimler olarak yazılır ("evlerimizden" → "ev +ler +imiz +den").

"Beğenmedim" örneği, KOK varyantının duygu analizi için yapısal bir riskini gösterir: olumsuzluk eki "-mA" kökün dışında kaldığı için kutup bilgisi silinir. KOK'un olası bir yararı bu yüzden sözcük dağarıcığı seyrekliğini azaltmaktan gelmeli ve bu kaybı telafi etmelidir. KOKEK'te ise olumsuzluk, zaman ve kişi ekleri ayrı birimler olarak görünür kalır.

Çözümlemeler `experiments/data/morph_cache.json` dosyasında önbelleğe alınır. Dönüşüm eğitim, validation, test ve ek değerlendirme kümelerinin tümüne aynı biçimde uygulanır. Dönüşümden sonra BERTurk belirteçleyicisi, sözlüğü ve 128 alt sözcük kesmesi değişmeden çalışır.

### 4.3 Puan izi silme (STRIP)

`common.strip_rating`, `RATING_RE` eşleşmelerini boşlukla değiştirir, ardışık boşlukları teke indirir ve metnin başındaki ve sonundaki boşlukları kırpar. STRIP yapılandırmasında bu işlem eğitim, validation ve testin tümüne uygulanır. Temiz altküme bayrağı ham metinden hesaplandığı için STRIP'ten bağımsızdır (Bölüm 3.5).

STRIP ile temiz altküme farklı soruları yanıtlar. Temiz altküme, puan izi olmayan örneklerde bir modelin başarımını ölçer. STRIP ise modelin eğitimde puan izine hiç erişmediği durumu ölçer.

STRIP bir iyileştirme bileşeni değildir; AS5 için tasarlanmış ve değerlendirme metnini de değiştiren bir tanı deneyidir. Bu nedenle ORK birleşimine aday değildir (Bölüm 4.7).

### 4.4 Türkçe LLM ile etiket tutarlılığı oylaması

`llm_vote.py`, `ytu-ce-cosmos/Turkish-Llama-8b-Instruct-v0.1` modelini \[9\] sıfır atışlı bir yargıç olarak kullanır. Model bitsandbytes ile 4 bit NF4 nicemlenir ve bfloat16 hesaplama türüyle yüklenir \[11\]. Her istem, modelin sohbet şablonuna tek bir kullanıcı mesajı olarak yerleştirilir ve yanıt başlangıcı eklenir. Metin üretilmez; yalnızca yanıtın ilk konumundaki belirteç dağılımı okunur.

**Tablo 4.** LLM oylamasında kullanılan üç istem şablonu ve sınıf sözcükleri (`llm_vote.TEMPLATES`).

| Şablon | İstem (özet) | Olumsuz sözcükler | Olumlu sözcükler |
| --- | --- | --- | --- |
| t1 | Duyguyu sınıflandır; tek sözcükle 'pozitif' ya da 'negatif' yanıt ver | negatif, Negatif | pozitif, Pozitif |
| t2 | Yorumu yazan kişi memnun mu; 'evet' ya da 'hayır' yanıt ver | hayır, Hayır | evet, Evet |
| t3 | Olumsuzsa 0, olumluysa 1 yaz; yalnızca rakamı yaz | 0 | 1 |

Her sınıf sözcüğünün ilk belirteç kimliği alınır ve kimlikler küme olarak tekilleştirilir. t1 ve t2'de küçük ve büyük harfli iki yazım, t3'te tek bir rakam vardır. İki sınıfın kimlik kümeleri kesişirse kod bir doğrulama (assert) hatasıyla durur. Metin, isteme konmadan önce ilk 1.500 karakterle sınırlanır.

k şablonu için x örneğinin olumlu olasılığı aşağıdaki gibi tanımlanır.

```latex
s_c^{(k)}(x) = \sum_{v \in V_c^{(k)}} p_{\mathrm{LLM}}\big(v \mid \mathrm{istem}_k(x)\big), \qquad c \in \{\mathrm{poz}, \mathrm{neg}\}

P_k(\mathrm{olumlu} \mid x) = \frac{s_{\mathrm{poz}}^{(k)}(x)}{s_{\mathrm{poz}}^{(k)}(x) + s_{\mathrm{neg}}^{(k)}(x) + 10^{-12}}
```

Burada V\_c^{(k)}, k şablonunda c sınıfına ait sözcüklerin ilk belirteç kimlikleri kümesidir. Olasılıklar, son konumdaki logitlerin tüm sözlük üzerindeki softmax'ından alınır. Örnekler sola dolgulu, 16'lık toplu işlerle ve benzer uzunluklar aynı toplu işe düşecek biçimde sıralanarak işlenir.

Oylama TrSST-2'nin eğitim, validation ve test kümelerinde ve ürün yorumlarının test kümesinde çalıştırılır. Çıktılar `experiments/data/llm/<görev>_<split>_<şablon>.npy` dosyalarına yazılır. Eğitim olasılıkları bayrak kuralında kullanılır (Bölüm 4.5). Validation ve test olasılıkları ise füzyonda, LLM'in tek başına raporlandığı satırda ve `llm_vote_stats.md` istatistiklerinde kullanılır. E-ticaret görevi beş sınıflı olduğundan oylamanın dışındadır.

LLM, `load_task` ile yüklenen özgün metni, yani puan izleri silinmemiş metni görür. Puan izi taşıyan bir örnekte yargıç etiketi metindeki puandan da okuyabilir. Bu yüzden `llm_vote_stats.md`, eğitim kümesindeki bayrak oranını puan izli ve izsiz örnekler için ayrı raporlar.

### 4.5 Bayrak kuralı ve bayraklı örneklerin ele alınması (LDROP, LWEIGHT)

`common.llm_flags`, bir eğitim örneğini iki koşul birlikte sağlanırsa bayraklar. Birinci koşul, üç şablonun da 0,5 eşiğinde altın etikete karşı oy vermesidir. İkinci koşul, üç şablonun ortalama karşıt sınıf olasılığının en az 0,9 olmasıdır.

```latex
\hat{y}_k(x_i) = \mathbb{1}\big[P_k(\mathrm{olumlu} \mid x_i) \geq 0{,}5\big], \qquad \bar{P}_i = \frac{1}{3} \sum_{k=1}^{3} P_k(\mathrm{olumlu} \mid x_i)

\bar{q}_i = \begin{cases} 1 - \bar{P}_i & y_i = 1 \\ \bar{P}_i & y_i = 0 \end{cases}

\mathrm{bayrak}(x_i) = \Big[ \bigwedge_{k=1}^{3} \hat{y}_k(x_i) \neq y_i \Big] \wedge \big[ \bar{q}_i \geq 0{,}9 \big]
```

Üç şablonun oybirliği, tek bir istem biçimine özgü yanlılığı sınırlar. 0,9 eşiği yalnızca yüksek güvenli çelişkileri hedefler. Kural, güvenli öğrenmedeki \[12\] altın etiket ile model güveni karşılaştırmasının dış yargıçlı ve basitleştirilmiş bir biçimidir.

LDROP yapılandırmasında bayraklı örnekler eğitim kümesinden çıkarılır. LWEIGHT yapılandırmasında bayraklı örnekler tutulur ve 0,3 kayıp ağırlığı alır. Validation ve test kümelerine hiçbir bayrak uygulanmaz.

`train.WeightedTrainer`, örnek ağırlıklı çapraz entropiyi toplu iş içinde ağırlık toplamına bölerek normalleştirir. Bayrak kullanılmayan koşularda tüm ağırlıklar 1'dir ve kayıp sıradan ortalama çapraz entropiye eşit olur.

```latex
w_i = \begin{cases} 0{,}3 & \mathrm{bayrak}(x_i) \\ 1 & \text{aksi halde} \end{cases}, \qquad \mathcal{L}_B = \frac{\sum_{i \in B} w_i \, \mathrm{CE}\big(f_\theta(x_i), y_i\big)}{\sum_{i \in B} w_i}
```

### 4.6 Hiperparametre araması (G\_\*, HBEST)

TrSST-2 ızgarası; öğrenme oranı {2e-5, 3e-5, 5e-5}, toplu boyut {32, 128} ve epoch {3, 4} değerlerinin çarpımıdır ve 12 ayar içerir. E-ticaret ızgarası aynı öğrenme oranı ve toplu boyut değerlerini 3 epoch ile tarar ve 6 ayar içerir. Izgaranın tüm koşuları seed 42 ile yapılır; bu seed değerlendirme seed'lerinden ayrıdır. Koşu adı `G_lr<oran>_bs<boyut>_ep<epoch>` biçimindedir. README okuması bu adla `G_lr2e-5_bs128_ep3` hücresine karşılık gelir ve ayrıca B0R olarak beş seed ile koşulur.

`run_queue.best_hp`, validation doğruluğu en yüksek ayarı seçer. Karşılaştırma (doğruluk, sıra) çiftleri üzerinden yapıldığı için eşitlikte ızgara listesinde sonra gelen ayar seçilir. HBEST, seçilen ayarın beş değerlendirme seed'iyle yeniden koşulmasıdır. Tek seed'e dayalı seçim seed gürültüsünden etkilenebilir \[22\]; HBEST'in beş seed'li değerlendirmesi seçimin genellenip genellenmediğini sınar.

### 4.7 Orkestra birleşimi (ORK)

`run_queue.ork_spec`, ORK yapılandırmasını yalnızca validation ortalamalarından kurar. Hiperparametreler her zaman ızgarada seçilen ayardır. Diğer bileşenler, beş seed'lik validation doğruluk ortalaması B0'ınkini aşarsa eklenir.

```latex
\bar{a}_{\mathrm{val}}(C) = \frac{1}{|S|} \sum_{s \in S} \mathrm{Acc}_{\mathrm{val}}(C, s), \qquad S = \{1, 4, 21, 40, 124\}

m^{*} = \arg\max_{m \in \{\mathrm{KOK}, \mathrm{KOKEK}\}} \bar{a}_{\mathrm{val}}^{\mathrm{TrSST\text{-}2}}(m), \qquad \ell^{*} = \arg\max_{\ell \in \{\mathrm{LDROP}, \mathrm{LWEIGHT}\}} \bar{a}_{\mathrm{val}}^{\mathrm{TrSST\text{-}2}}(\ell)

\mathrm{ORK} = \mathrm{HBEST} \oplus \big[m^{*} \text{ eğer } \bar{a}_{\mathrm{val}}(m^{*}) > \bar{a}_{\mathrm{val}}(\mathrm{B0})\big] \oplus \big[\ell^{*} \text{ eğer } \bar{a}_{\mathrm{val}}(\ell^{*}) > \bar{a}_{\mathrm{val}}(\mathrm{B0})\big]
```

Morfoloji varyantı m\* her iki görev için TrSST-2 validation kümesinde seçilir. E-ticarette m\*, e-ticaret verisine bakılmadan MORPH yapılandırması olarak ayrıca koşulur. MORPH'un ORK'a girip girmeyeceği ise e-ticaretin kendi B0'ına karşı belirlenir. LLM temizliği yalnızca TrSST-2 ORK'ına adaydır. TrSST-2 ORK'ı ürün testinde de değerlendirilir. Seçim `<görev>_ORK_choice.json` dosyasına kaydedilir.

Bileşen kararları HBEST'e değil, B0'a, yani TrGLUE ayarlarına göre verilir. Bileşenler B0 hiperparametreleriyle ölçülür ve HBEST ayarlarıyla birleştirilir. Bu, bileşen etkilerinin hiperparametrelerle etkileşmediği varsayımına dayanır. Varsayım doğrulanmamıştır; ORK sonucu bu varsayımın bir sınaması olarak okunmalıdır.

### 4.8 Bağlam uzunluğu ve alternatif kodlayıcılar (LEN256, B128K, ELECTRA, XLMRL)

Dört ek yapılandırma, rakip protokolün dışına çıkan ama düşük riskli değişiklikleri ölçer; LEN256 ve B128K'nın olumlu sonuçlarından sonra üç birleşim (L256H, B128KL, KOKEK256) eklenmiştir (Tablo 5'in son üç satırı). B128KL iki adımlı gradyan biriktirmeyle (cihaz başına 64, etkin 128) koşulmuştur. İlk üç denemesi 1,0 dakikada rc=1 ile durmuştur. Bellek yetersizliği bildirilmiştir, ancak günlükler üzerine yazıldığı için neden dosyadan doğrulanamamaktadır (Bölüm 8.3). LEN256, B128K, ELECTRA ve XLMRL'nin ayarları ızgarasız, önceden sabitlenmiştir (`run_queue.MODEL_CONFIGS`). Tümü aynı `train.py` yolundan geçer; KOKEK256 dışındaki TrSST-2 yapılandırmaları ürün testini de tahmin eder. L256H TrSST-2 ızgarasında seçilen ayarı kullanır; L256H, B128KL ve KOKEK256 run\_queue.jobs içinde ayrıca tanımlanır.

**Tablo 5.** Ek kodlayıcı ve bağlam yapılandırmaları (öğrenme oranı / toplu boyut / epoch).

| Yapılandırma | Model | Ayar | Seed | Gerekçe |
| --- | --- | --- | --- | --- |
| LEN256 | BERTurk, en fazla 256 alt sözcük | 3e-5 / 128 / 3 | 5 | TrSST-2 örneklerinin %12,1-12,4'ü 128 sınırını aşar (Tablo 1); yalnız TrSST-2 |
| B128K | `dbmdz/bert-base-turkish-128k-cased` | 3e-5 / 128 / 3 | 5 | Daha geniş sözlük, biçimbirimlere daha uygun alt sözcükler \[15\] |
| ELECTRA | `dbmdz/electra-base-turkish-cased-discriminator` | 5e-5 / 32 / 3 | 5 | Aynı boyutta ayırt edici ön eğitim \[26\] |
| XLMRL | `FacebookAI/xlm-roberta-large` | 1e-5 / 32 / 3 | 3 (1, 4, 21) | Büyük çok dilli kodlayıcı \[27\] |
| L256H | BERTurk, en fazla 256 alt sözcük | TrSST-2 ızgarasında seçilen (5e-5 / 128 / 3) | 5 | Uzun bağlam ile seçilen hiperparametrenin birleşimi; yalnız TrSST-2 |
| B128KL | BERTurk 128k sözlük, en fazla 256 alt sözcük | 3e-5 / 128 / 3 (gradyan biriktirme 2 × 64) | 3 (1, 4, 21) | Validation'da olumlu iki değişikliğin birleşimi; yalnız TrSST-2 |
| KOKEK256 | BERTurk, kök + ek varyantı, en fazla 256 alt sözcük | 3e-5 / 128 / 3 | 3 (1, 4, 21) | Kök + ek kaybının kesmeden mi kaynaklandığını ayırmak (Tablo 3: %29,7 kesilme) |

ELECTRA ve XLM-R için öğrenme oranları, kendi makalelerindeki GLUE ince ayarına yakın değerlerden seçilmiştir. Bu yapılandırmalar plana kuyruk çalışırken eklenmiştir: LEN256, B128K, ELECTRA ve XLMRL ikinci, L256H ve KOKEK256 üçüncü, B128KL dördüncü geçişte koşulmuştur. ORK seçimine girmez, yalnızca ORK2 topluluğunun adaylarıdır.

### 4.9 Topluluklar (seed, ORK2) ve BERTurk + LLM füzyonu

`ensemble.py`, adı `<görev>_<BÜYÜK HARF/RAKAM>_s<seed>` kalıbına uyan, test olasılık dosyası bulunan ve en az iki seed'i olan her yapılandırma için bir topluluk kurar \[24\]. Topluluk, seed'lerin sınıf olasılıklarının ortalamasıdır ve tahmin ortalama olasılığı en büyük sınıftır. Tek seed'li ve kalıp dışı olan G\_\* ızgara koşuları topluluğa girmez.

```latex
\bar{p}_C(y \mid x) = \frac{1}{|S_C|} \sum_{s \in S_C} p_{C,s}(y \mid x), \qquad \hat{y}_C(x) = \arg\max_y \bar{p}_C(y \mid x)
```

ORK2, bu yapılandırma toplulukları arasından açgözlü ileri seçimle kurulan bir model topluluğudur \[28\]. Her adımda, seçilmiş listeye eklendiğinde validation doğruluğunu en çok artıran topluluk eklenir; aynı topluluk birden çok kez seçilebilir. Doğruluk artmazsa ya da 10 adıma ulaşılırsa seçim durur. ORK2 tahmini, seçilen listenin olasılık ortalamasıdır.

```latex
L_{t} = L_{t-1} \uplus \{C_t\}, \qquad C_t = \arg\max_{C} \mathrm{Acc}_{\mathrm{val}}\Big(\arg\max_y \frac{1}{|L_{t-1}| + 1}\big(\textstyle\sum_{C' \in L_{t-1}} \bar{p}_{C'} + \bar{p}_C\big)\Big)
```

Füzyon, TrSST-2'de B0, HBEST, LEN256, L256H, ORK ve ORK2 topluluklarının olumlu sınıf olasılığını LLM'in üç şablon ortalamasıyla doğrusal olarak karıştırır. Ağırlık w, {0; 0,05; …; 1} ızgarasındaki 21 değer arasından yalnızca TrSST-2 validation doğruluğuyla seçilir. Eşitlikte en küçük w seçilir.

```latex
p_{\mathrm{f\ddot{u}z}}(x) = (1 - w)\, \bar{p}_C(\mathrm{olumlu} \mid x) + w\, \bar{P}(x), \qquad \hat{y}_w(x) = \mathbb{1}\big[p_{\mathrm{f\ddot{u}z}}(x) \geq 0{,}5\big]

w^{*} = \arg\max_{w \in \{0;\, 0{,}05;\, \ldots;\, 1\}} \mathrm{Acc}_{\mathrm{val}}\big(\hat{y}_w\big)
```

Seçilen w\* TrSST-2 testinde (tam ve temiz altküme) değiştirilmeden kullanılır. Ürün testinde füzyon yalnızca ürün test olasılığı olan topluluklar için hesaplanır. HBEST koşuları ürün testini değerlendirmez. ORK = HBEST olduğundan (Bölüm 7.1), Tablo 11 ve 20'deki "HBEST (= ORK)" ürün değerleri ORK ve ORK + LLM topluluklarından alınmıştır. Ürün tarafında w yeniden ayarlanmaz; bu, film validation kümesinde seçilmiş bir ağırlığın sıfır atışlı aktarımıdır.

LLM'in tek başına başarımı da w = 1 satırı olarak, yani üç şablon ortalamasının 0,5 eşiğiyle, TrSST-2 ve ürün testlerinde raporlanır.

### 4.10 Film → ürün aktarımı (sıfır atış, FSBASE, FSFILM, FULL)

Sıfır atışlı aktarım, TrSST-2 B0 modellerinin ürün verisi hiç görmeden ürün test kümesinde yaptığı tahmindir (Bölüm 4.1). İki görevde de 0 olumsuz, 1 olumlu sınıftır; bu yüzden sınıflandırma başlığı etiket eşlemesi olmadan doğrudan kullanılır.

FSBASE, ürün eğitim kümesinden etiket oranı korunarak çekilen 1.000 örnekle, BERTurk'ten başlayarak eğitilir (`common.stratified_subset`). FSFILM, aynı 1.000 örnekle ve aynı seed'in TrSST-2 B0 modelinden başlatılarak eğitilir; sınıflandırma başlığı da devralınır. Alt küme seed'e göre çekildiği için aynı seed'deki FSBASE ve FSFILM aynı örnekleri görür. Bu da eşleştirilmiş karşılaştırmayı mümkün kılar.

Az örnekli koşularda öğrenme oranı 3e-5, toplu boyut 16 ve 5 epoch kullanılır. FULL, 188.132 örneklik ürün eğitim kümesinin tamamıyla TrGLUE ayarlarında eğitilir ve üst sınır işlevi görür. Maliyet nedeniyle FULL yalnızca ilk üç seed (1, 4, 21) ile koşulur.

### 4.11 Sınıf dengesizliği (yalnızca hate)

Hate görevinde medeni sınıfı neredeyse hiç tahmin edilmediği için (Bölüm 6.6) iki yöntem sonradan eklendi. İkisi de B0 ayarını (2e-5 / 128 / 3) değiştirmez.

Sınıf ağırlıklı kayıpta her eğitim örneğinin çapraz entropisi, etiketinin sınıf ağırlığıyla ağırlıklandırılır ve toplu iş içinde ağırlıklı ortalama alınır: Σ w ℓ / Σ w. Ağırlıkların mutlak ölçeği bu yüzden sadeleşir (`train.py`, `--class_weight`). WCE dengeli ağırlığı, WCES onun karekökünü kullanır:

```latex
w_c^{\mathrm{WCE}} = \frac{n}{K \, n_c}, \qquad w_c^{\mathrm{WCES}} = \sqrt{\frac{n}{K \, n_c}}
```

Burada n eğitim örneği sayısı, n\_c sınıf c'nin eğitim örneği sayısı ve K = 4'tür. Hakaret, nefret, nötr ve medeni için ağırlıklar WCE'de 1,20 / 8,44 / 0,34 / 11,13, WCES'te 1,09 / 2,90 / 0,58 / 3,34'tür.

Eşik ayarı eğitimi değiştirmez, yalnızca karar kuralını değiştirir (`hate_thresh.py`). Tahmin, log p\_c + b\_c değerini en büyük yapan sınıftır. Nötr sınıfının kaydırması 0'a sabittir. Diğer üç kaydırma {−2; −1,9; …; 4} ızgarasında, validation makro F1'ini en çok artıran koordinat yükselişiyle (3 tur) seçilir. Test etiketleri hiçbir seçimde kullanılmaz. Seed düzeyinde her koşu kendi validation tahminleriyle, toplulukta ise seed olasılık ortalaması üzerinde ayarlanır. Izgara ve nötr sabitlemesi önceden kayıtlı değildir; başka bir ağırlık ya da eşik yöntemi denenmedi.

## 5. Deney Kurulumu

### 5.1 Deney matrisi

`run_queue.py`, tüm planı sabit bir sırayla ve kaldığı yerden yürütür. Koşu adı `<görev>_<yapılandırma>_s<seed>` biçimindedir ve `results.json` dosyası olan koşular atlanır. Seçime bağlı işler (HBEST, L256H, MORPH, ORK) ancak sıraları geldiğinde, gerekli validation sonuçları oluştuktan sonra tanımlanır.

**Tablo 6.** Deney matrisi: yapılandırmalar, eğitim ayarları, seed'ler ve ince ayar koşu sayısı (`run_queue.jobs`). Hiperparametre sütunu öğrenme oranı / toplu boyut / epoch biçimindedir.

| Görev | Yapılandırma | Fark (B0'a göre) | Hiperparametre | Seed | Koşu |
| --- | --- | --- | --- | --- | --: |
| TrSST-2 | B0 | rakip protokol (betik); model kaydı, ürün testi | 3e-5 / 128 / 3 | 1, 4, 21, 40, 124 | 5 |
| TrSST-2 | B0R | rakip protokol (README okuması) | 2e-5 / 128 / 3 | 5 seed | 5 |
| TrSST-2 | LDROP | bayraklı örnekler çıkarılır | 3e-5 / 128 / 3 | 5 seed | 5 |
| TrSST-2 | LWEIGHT | bayraklı örneklere 0,3 ağırlık | 3e-5 / 128 / 3 | 5 seed | 5 |
| TrSST-2 | KOK | kök varyantı | 3e-5 / 128 / 3 | 5 seed | 5 |
| TrSST-2 | KOKEK | kök + ek varyantı | 3e-5 / 128 / 3 | 5 seed | 5 |
| TrSST-2 | STRIP | puan izleri silinir | 3e-5 / 128 / 3 | 5 seed | 5 |
| TrSST-2 | G\_\* | ızgara | {2e-5, 3e-5, 5e-5} / {32, 128} / {3, 4} | 42 | 12 |
| TrSST-2 | HBEST | seçilen ızgara ayarı | seçilen | 5 seed | 5 |
| TrSST-2 | ORK | HBEST + yardımcı bileşenler; ürün testi | seçilen | 5 seed | 5 |
| TrSST-2 | LEN256 | en fazla 256 alt sözcük; ürün testi | 3e-5 / 128 / 3 | 5 seed | 5 |
| TrSST-2 | B128K | BERTurk 128k sözlük; ürün testi | 3e-5 / 128 / 3 | 5 seed | 5 |
| TrSST-2 | ELECTRA | Türkçe ELECTRA-base; ürün testi | 5e-5 / 32 / 3 | 5 seed | 5 |
| TrSST-2 | XLMRL | XLM-R large; ürün testi | 1e-5 / 32 / 3 | 1, 4, 21 | 3 |
| TrSST-2 | L256H | 256 alt sözcük + seçilen ızgara ayarı; ürün testi | seçilen (5e-5 / 128 / 3) | 5 seed | 5 |
| TrSST-2 | B128KL | 128k sözlük + 256 alt sözcük; ürün testi | 3e-5 / 128 (2 × 64) / 3 | 1, 4, 21 | 3 |
| TrSST-2 | KOKEK256 | kök + ek varyantı, 256 alt sözcük | 3e-5 / 128 / 3 | 1, 4, 21 | 3 |
| E-ticaret | B0 | rakip protokol (betik) | 3e-5 / 128 / 3 | 5 seed | 5 |
| E-ticaret | B0R | rakip protokol (README okuması) | 2e-5 / 128 / 3 | 5 seed | 5 |
| E-ticaret | G\_\* | ızgara | {2e-5, 3e-5, 5e-5} / {32, 128} / 3 | 42 | 6 |
| E-ticaret | HBEST | seçilen ızgara ayarı | seçilen | 5 seed | 5 |
| E-ticaret | MORPH | TrSST-2'de seçilen varyant | 3e-5 / 128 / 3 | 5 seed | 5 |
| E-ticaret | ORK | HBEST (+ MORPH yardımcıysa) | seçilen | 5 seed | 5 |
| E-ticaret | B128K | BERTurk 128k sözlük | 3e-5 / 128 / 3 | 5 seed | 5 |
| E-ticaret | ELECTRA | Türkçe ELECTRA-base | 5e-5 / 32 / 3 | 5 seed | 5 |
| E-ticaret | XLMRL | XLM-R large | 1e-5 / 32 / 3 | 1, 4, 21 | 3 |
| Hate | B0 | rakip protokol (SentiTurca run\_all.sh); (başlık, entry) çifti | 2e-5 / 128 / 3 | 5 seed | 5 |
| Hate | B0E5 | rakip README hate komutu (5 epoch) | 2e-5 / 128 / 5 | 5 seed | 5 |
| Hate | LEN256 | en fazla 256 alt sözcük | 2e-5 / 128 / 3 | 5 seed | 5 |
| Hate | B128K | BERTurk 128k sözlük | 2e-5 / 128 / 3 | 5 seed | 5 |
| Hate | B128KL | 128k sözlük + 256 alt sözcük | 2e-5 / 128 (2 × 64) / 3 | 1, 4, 21 | 3 |
| Hate | XLMRL | XLM-R large | 1e-5 / 32 / 3 | 1, 4, 21 | 3 |
| Hate | WCE | B0 + sınıf ağırlıklı kayıp, n / (K n\_c) | 2e-5 / 128 / 3 | 5 seed | 5 |
| Hate | WCES | B0 + sınıf ağırlıklı kayıp, karekök n / (K n\_c) | 2e-5 / 128 / 3 | 5 seed | 5 |
| Ürün | FSBASE | 1.000 örnek, BERTurk'ten | 3e-5 / 16 / 5 | 5 seed | 5 |
| Ürün | FSFILM | 1.000 örnek, TrSST-2 B0'dan | 3e-5 / 16 / 5 | 5 seed | 5 |
| Ürün | FULL | tüm eğitim kümesi (üst sınır) | 3e-5 / 128 / 3 | 1, 4, 21 | 3 |

Toplam ince ayar koşusu 179'dur (86 TrSST-2, 44 e-ticaret, 36 hate, 13 ürün; ilk 143 koşu experiments/logs/analiz\_notlari.md'de). Hate görevinin 26 koşusu beşinci geçişte, 10 sınıf ağırlıklı kayıp koşusu (WCE, WCES) altıncı geçişte, makalenin ilk sürümünden sonra eklenmiştir (Bölüm 3.7). L256H, B128KL ve KOKEK256, kuyruğun sonraki geçişlerinde eklenmiştir; ilk 128 alt sözcüklük sonuçlar görüldükten sonra plana alındıkları için Bölüm 8'de uyarlamalı deney tasarımı olarak tartışılır. Bunlara iki LLM oylama işi eklenir: TrSST-2'de 3 şablon × 78.250 örnek ve ürün testinde 3 şablon × 23.517 örnek (türetilmiş).

Kuyruğun ilk geçişi şu sırayla ilerler: morfoloji önbelleği ve veri istatistikleri, TrSST-2 ve e-ticaret B0, LLM oylaması ve istatistikleri, LDROP/LWEIGHT, KOK/KOKEK, STRIP, ızgaralar ve HBEST, e-ticaret MORPH, aktarım koşuları ve en son ORK. B0R ile LEN256, B128K, ELECTRA ve XLMRL plana kuyruk çalışırken eklendi ve ikinci geçişte yürütülür; bunlar ORK seçimine girmez. Her geçişin sonunda `ensemble.py` ve `aggregate.py` çalışır ve makaledeki tüm sonuç tabloları yalnızca `experiments/logs/aggregate_latest.md` dosyasından alınır.

### 5.2 Seçim kuralları ve test yalıtımı

Koddaki beş seçim noktasının tümü validation kümesini kullanır. Bunlar ızgara ayarı (`best_hp`), morfoloji varyantı (`best_morph`), LLM temizlik kipi ve ORK'a giriş (`ork_spec`), ORK2 topluluğunun açgözlü seçimi ve füzyon ağırlığıdır (`ensemble.py`). Test etiketleri bu seçimlerin hiçbirinde okunmaz; yalnızca raporlamada kullanılır. Ancak sonradan eklenen yapılandırmalar, test sütunlarını da içeren tablolar görüldükten sonra aday havuzuna alınmıştır; bu yüzden yalıtım yapılandırma düzeyinde tam değildir (Bölüm 8.3). LLM bayrakları yalnızca eğitim kümesinden hesaplanır.

Morfoloji ve STRIP dönüşümleri tüm split'lere aynı kuralla uygulanır. Ürün bölmesi tek bir tohumla bir kez sabitlenip diske yazılmıştır. Böylece tüm ürün yapılandırmaları aynı test kümesinde değerlendirilir.

Validation kümesi seçim için kullanıldığından, seçilmiş yapılandırmaların validation skorları iyimser yanlıdır. Bu nedenle bileşen karşılaştırmaları ve rakip karşılaştırması yalnızca test üzerinden yorumlanmıştır. Izgara seçiminin tek seed'e (42) dayanması ayrı bir seçim gürültüsü kaynağıdır.

### 5.3 Metrikler

`common.metrics`, doğruluk, F1 ve makro F1'i yüzde olarak hesaplar. İkili görevlerde F1 olumlu sınıfın F1'idir; beş sınıflı e-ticarette ve dört sınıflı hate'te F1 makro F1'dir. Makro F1 her görev için ayrıca kaydedilir.

TrGLUE Tablo 7 başlığına göre özgün GLUE, SST-2 için yalnızca doğruluk raporlar; TrGLUE F1'i sınıf dengesizliği nedeniyle eklemiştir \[1\]\[2\]. Ancak F1'in ortalama türü kaynakta açıkça belirtilmez. Olumlu sınıf F1'ini seçtik, çünkü TrSST-2'de olumlu sınıf çoğunluktadır ve rakibin F1'inin doğruluğunun üzerinde olması bu tanımla tutarlıdır. Rakip F1 ile karşılaştırma bu varsayım altında yapılmıştır.

Rakibin e-ticaret değerleri 0-1 ölçeğinde verilmiştir (0,66 / 0,64) \[1\]. Tablolarımızda bunlar yüzde ölçeğine (66,0 / 64,0) çevrilir; bu bir ölçek dönüşümüdür, yeni bir ölçüm değildir.

TrSST-2 koşularında validation ve test temiz altkümede de değerlendirilir (`validation_clean`, `test_clean`). Ürün testinde dengesizlik nedeniyle makro F1 birincil metriktir (Bölüm 3.4). Her koşunun sınıf olasılıkları `probs_<split>.npy` dosyasına float16 olarak yazılır.

### 5.4 İstatistiksel çıkarım

Seed düzeyindeki sonuçlar ortalama ± örneklem standart sapması (ddof = 1) olarak verilir. Bir yapılandırmanın B0'a farkı, iki yapılandırmanın ortak seed'leri üzerinden eşleştirilmiş t ile %95 güven aralığıyla raporlanır. Aynı yöntem ürün testinde FSFILM ile FSBASE arasındaki makro F1 farkına da uygulanır.

```latex
d_s = M(C, s) - M(\mathrm{B0}, s), \qquad \bar{d} \pm t_{0{,}975,\, n-1} \, \frac{\mathrm{ss}(d)}{\sqrt{n}}, \qquad n = |S_C \cap S_{\mathrm{B0}}|
```

Eşleştirme seed düzeyindedir ve kısmi bir ortak rastgelelik varsayımına dayanır. Aynı seed, başlık başlatmasını ve veri karıştırma tohumunu sabitler. Ancak eğitim kümesi boyutunun, toplu boyutun ya da kodlayıcının değiştiği yapılandırmalarda veri sırası ve başlatma B0 ile birebir aynı değildir.

Beş seed ile serbestlik derecesi 4, kritik değer t₀,₉₇₅;₄ ≈ 2,776'dır; üç seed'li XLMRL, B128KL ve KOKEK256'da serbestlik derecesi 2, kritik değer t₀,₉₇₅;₂ ≈ 4,30'dur. Bu küçük örneklem güven aralıklarını geniş tutar. Aralık sıfırı içeren farklar "iyileştirme" olarak nitelenmez.

Seed ve model toplulukları (ORK2 dahil) ile füzyon sistemleri tek bir tahmin vektörü ürettiği için seed düzeyinde sınanamaz. Bunlar test örnekleri üzerinde eşleştirilmiş bootstrap ile aynı görevin B0 topluluğuna karşı sınanır \[23\]. Her sistem için 1.000 yeniden örnekleme yapılır (tohum 0) ve %2,5 ile %97,5 yüzdelikleri güven aralığı olarak alınır. M doğruluk ya da F1'dir (TrSST-2'de olumlu sınıf, e-ticaret ve hate'te makro F1).

```latex
\Delta^{(b)} = M\big(\hat{y}_{C}[I_b], y[I_b]\big) - M\big(\hat{y}_{\mathrm{B0}}[I_b], y[I_b]\big), \qquad I_b \sim \mathrm{U}\{1, \ldots, N\}^{N}, \quad b = 1, \ldots, 1000

\mathrm{GA}_{\%95} = \big[\, Q_{0{,}025}(\Delta),\; Q_{0{,}975}(\Delta) \,\big]
```

Bootstrap yalnızca B0 topluluğu bulunan görevlerde, yani TrSST-2, e-ticaret ve hate'te yapılır ve tam test kümesini kullanır. Ürün görevinde B0 yapılandırması olmadığından FSBASE, FSFILM ve FULL toplulukları bootstrap'e girmez; FSFILM − FSBASE farkı yalnızca eşleştirilmiş t ile sınanır.

Bootstrap aralığı test örneklemindeki belirsizliği yansıtır, seed varyansını yansıtmaz. Eşleştirilmiş t ise seed varyansını yakalar, test örneklemi belirsizliğini içermez. İki çıkarım birbirinin yerine kullanılmaz ve ayrı tablolarda raporlanır. Birden çok karşılaştırma için düzeltme uygulanmaz; bu durum bir sınırlılık olarak belirtilir.

Rakibin yayımlanmış değeri (87,4 / 91,3) için seed düzeyinde veri ya da tahmin vektörü yoktur. Bu yüzden rakibe karşı doğrudan bir anlamlılık sınaması yapılamaz. "Rakibi aştık" iddiası yalnızca B0'a karşı farkı da anlamlı olan yapılandırmalar için kurulur (Bölüm 6.5, 8.2, 9).

### 5.5 Donanım ve yazılım

Proje planı Colab T4 GPU'yu öngörüyordu. Deneyler gerçekte yerel bir NVIDIA RTX A4000 (16 GB) üzerinde, Windows 11 altında yürütülmüştür. Yazılım yığını Python 3.11, torch 2.5.1+cu118 ve transformers 4.57'dir.

Karma duyarlık (fp16) her iki GPU sınıfında da desteklendiği için ince ayar ayarları planla aynıdır. Toplu boyut 128, 128 alt sözcüklü koşularda tek GPU'ya gradyan biriktirme olmadan sığmıştır; B128KL (128k sözlük, 256 alt sözcük) ise 2 × 64 gradyan biriktirmeyle koşulmuştur (Bölüm 4.8). Veri yükleyici ek işçi süreci kullanmaz. Kayıp her 50 adımda günlüğe alınır ve günlük eğitim bittikten sonra `train_log.txt` dosyasına yazılır. LLM oylaması aynı GPU'da yürütülür; morfolojik çözümleme CPU'da bir kez yapılıp önbellekten okunur.

Her koşunun süresi `results.json` içindeki `minutes` alanında da kaydedilir; aşağıdaki süreler ise model yüklemeyi de içeren kuyruk duvar saatidir (queue\_status.txt) ve minutes değerlerinden biraz yüksektir. TrSST-2 B0 koşusu ortalama 9,9 dk, e-ticaret B0 10,1 dk, XLM-R large 40,8–41,6 dk sürer; LLM oylaması TrSST-2 testinde 27,2 dk, ürün testinde 54,8 dk sürer. İlk dört geçiş 2.137,1 dk (yaklaşık 35,6 saat) duvar saati tutmuştur (hate geçişleriyle toplam 2.526,5 dk, Bölüm 6): 1.800,8 dk başarılı eğitim, 82,0 dk başarılı LLM işi, 229,1 dk sıfırdan farklı çıkış kodlu iş ve 25,2 dk diğer işler (morfoloji önbelleği, istatistik, topluluk, toplama). Başarısız sürenin 225,5 dk'sı, TrSST-2 eğitim ve validation skorlarını yazıp test kümesinde rc=1 ile duran ilk LLM işidir (nedeni, günlük üzerine yazıldığı için dosyadan doğrulanamıyor) (queue\_status.txt). Kuyruk durumu `experiments/logs/queue_status.txt` dosyasına zaman damgasıyla yazılır. Sıfırdan farklı çıkış kodlu işler `aggregate_latest.md` içinde "Başarısız işler" başlığında listelenir. Veri istatistikleri işi kuyrukta ilk denemede değer hatasıyla (ValueError) başarısız olmuş, data\_stats.md kuyruk dışında üretilmiştir; bu kayıt aggregate\_latest.md içindeki "Başarısız işler" listesinde görünür.

### 5.6 Tasarımdan kaynaklanan geçerlilik tehditleri

KOKEK, validation örneklerinin %29,7'sinde metin sonunu keser (Tablo 3). Bu varyantın zararı ya da yararı, morfoloji ile bağlam kaybının toplamı olarak okunmalıdır. Sınırı 256 alt sözcüğe çıkaran bir denetim koşusu (KOKEK256, 3 seed) sonradan eklenmiştir (Bölüm 7.2); "+" işaretini kaldıran bir denetim koşusu yoktur.

Morfolojik çözümleme ilk ayrıştırmayı kullanır ve bağlamsal belirsizliği gidermez. Kök varyantı olumsuzluk ekini ("-mA") düşürdüğü için duygu yönünü taşıyan bilgiyi silebilir. Bu risk KOK sonuçlarının yorumunda dikkate alınmıştır (Bölüm 7.2).

LLM bayrakları gerçek etiket hatası ile LLM hatasını ayırt edemez. Bayraklı örneklerin elle denetimi planda yoktur; yalnızca rastgele 10 bayraklı örnek nitel olarak listelenir (`llm_vote_stats.md`). LLM puan izi silinmemiş metni gördüğü için izli örneklerdeki bayraklar puandan etkilenebilir. Ayrıca LLM'in ön eğitim verisinin bu yorumları içerip içermediği bilinmemektedir.

Ek kodlayıcıların (Bölüm 4.8) ayarları ızgarasız sabitlenmiştir; daha iyi ayarların var olması mümkündür. ORK2'nin açgözlü seçimi aynı validation kümesine dayandığı için validation skoru iyimserdir; başarımı yalnızca testte yorumlanır.

Puan izi düzenli ifadesi yazıyla verilen puanları ve yıldız simgelerini yakalamaz (Bölüm 3.5). Bu yüzden temiz altküme ve STRIP, puan bilgisinin yalnızca açık sayısal biçimini dışarıda bırakır.

Füzyon ağırlığı TrSST-2'de seçilip ürün testine taşınır. Ürün alanında ayarlanmamış bu ağırlık, ürün füzyon sonucunu temkinli okunması gereken bir aktarım ölçümüne dönüştürür. LLM'in ürün verisinde yalnızca test kümesinde oylaması da bu tasarımın gereğidir.

## 6. Sonuçlar

Bu bölümdeki tüm sayılar `experiments/logs/aggregate_latest.md` dosyasından alınmıştır. İlk 143 koşunun sayıları, süreleri ve koşu düzeyindeki denetimleri `experiments/logs/analiz_notlari.md` dosyasından gelir; hate koşularının süreleri experiments/logs/queue\_status.txt, sınıf bazlı F1 değerleri experiments/logs/hate\_thresh.md dosyasındadır. Tek model satırları seed ortalaması ± ss'dir; Δ, ortak seed'lerde B0'a karşı eşleştirilmiş t farkı ve %95 GA'dır (Bölüm 5.4).

Topluluk ve füzyon sistemleri, test örnekleri üzerinde B0 topluluğuna karşı eşleştirilmiş bootstrap ile sınanır \[23\]. "Türetilmiş" etiketi, sonuç dosyasında doğrudan bulunmayan ve oradaki sayılardan hesapladığımız farkları gösterir; bunların GA'sı yoktur. Rakip değerleri ss'siz tek sayılar olduğundan rakibe karşı anlamlılık sınaması yapılamaz.

Kuyruk altı geçişte 179 başarılı ince ayar koşusu üretti: TrSST-2'de 86, e-ticarette 44, hate görevinde 36, ürün görevinde 13. Bunlara 12 LLM skorlama geçişi (4 split × 3 şablon, 305.301 örnek-şablon skoru) eklenir. İlk dört geçişin kuyruk duvar saati tek RTX A4000 üzerinde 2.137,1 dakikadır (35,6 saat); model yükleme ve 229,1 dakikalık başarısız işler buna dahildir. Hate geçişi 315,2 dakika, sınıf dengesizliği geçişi 74,2 dakika daha sürdü (bu geçişte yarıda kesilen bir koşu baştan yeniden çalıştırıldı); toplam 2.526,5 dakikadır (42,1 saat).

Tablo 6'daki B0R, LEN256, B128K, ELECTRA ve XLMRL ikinci geçişte, L256H ve KOKEK256 üçüncü, B128KL dördüncü geçişte, ilk planda olmadan eklenmiştir. Bu uyarlamalı büyüme Bölüm 8.3'te bir geçerlilik tehdidi olarak ele alınır. Son geçişlerde eklenen üç yapılandırma (Tablo 5'in son üç satırı) aşağıda kısaca tanımlanır.

- **L256H:** BERTurk, en fazla 256 alt sözcük ve ızgarada validation'da seçilen ayar (lr 5e-5, bs 128, 3 epoch); 5 seed.
- **B128KL:** 128k sözlüklü BERTurk (`dbmdz/bert-base-turkish-128k-cased`), en fazla 256 alt sözcük, TrGLUE ayarı (3e-5 / 128 / 3); 3 seed (1, 4, 21). Bellek nedeniyle cihaz başına 64 örnek ve `grad_accum` 2 kullanılmış, etkin toplu boyut 128 korunmuştur.
- **KOKEK256:** kök-ek ayrımı varyantı, en fazla 256 alt sözcük, TrGLUE ayarı; 3 seed (1, 4, 21). Amaç, KOKEK'teki kesme karıştırıcısını morfoloji etkisinden ayırmaktır (Bölüm 3.6).

### 6.1 Rakip protokolün yeniden üretimi (AS1)

**Tablo 7.** Rakip protokolün yeniden üretimi. Rakip satırları yayımlanmış tek değerlerdir \[1\]; e-ticaret değerleri 0-1 ölçeğinden yüzdeye çevrilmiştir. F1, TrSST-2'de olumlu sınıf F1'i, e-ticarette makro F1'dir.

| Görev | Sistem | lr / bs / epoch | Seed | Val acc | Test acc | Test F1 | Δ acc \[%95 GA\] | Δ F1 \[%95 GA\] |
| --- | --- | --- | --: | --: | --: | --: | --- | --- |
| TrSST-2 | Rakip (TrGLUE Tablo 7) | – | – | – | 87,4 | 91,3 | – | – |
| TrSST-2 | B0 | 3e-5 / 128 / 3 | 5 | 87,87 ± 0,20 | 87,75 ± 0,12 | 91,54 ± 0,09 | referans | referans |
| TrSST-2 | B0R | 2e-5 / 128 / 3 | 5 | 87,80 ± 0,13 | 87,66 ± 0,10 | 91,49 ± 0,06 | −0,09 \[−0,31; +0,14\] | −0,05 \[−0,20; +0,10\] |
| TrSST-2 | HBEST (= ORK) | 5e-5 / 128 / 3 | 5 | 87,85 ± 0,12 | 87,83 ± 0,17 | 91,58 ± 0,12 | +0,09 \[−0,26; +0,44\] | +0,04 \[−0,19; +0,28\] |
| E-ticaret | Rakip (TrGLUE Tablo 11) | – | – | – | 66,0 | 64,0 | – | – |
| E-ticaret | B0 | 3e-5 / 128 / 3 | 5 | 69,04 ± 0,11 | 68,65 ± 0,15 | 66,31 ± 0,23 | referans | referans |
| E-ticaret | B0R | 2e-5 / 128 / 3 | 5 | 69,08 ± 0,19 | 68,70 ± 0,12 | 66,27 ± 0,14 | +0,05 \[−0,04; +0,15\] | −0,04 \[−0,24; +0,15\] |
| E-ticaret | HBEST (= ORK) | 2e-5 / 32 / 3 | 5 | 68,95 ± 0,16 | 68,63 ± 0,10 | 66,28 ± 0,16 | −0,01 \[−0,12; +0,09\] | −0,03 \[−0,19; +0,12\] |

TrSST-2'de B0, yayımlanmış değeri doğrulukta +0,35, F1'de +0,24 puan aşar (türetilmiş). Beş seed'in her biri hem 87,4'ün hem 91,3'ün üzerindedir. Rakip ss'siz tek bir sayı olduğu için bu fark üzerinde anlamlılık sınaması yapılamaz (Bölüm 5.4).

Protokolün iki okuması arasında fark bulunmaz: B0R'ın iki GA'sı da sıfırı içerir. Izgarada seçilen HBEST de B0'dan ayırt edilemez. Dolayısıyla AS1'in yeniden üretim kısmı olumlu, hiperparametre araması kısmı olumsuzdur.

E-ticarette TrSST-2 için yayımlanan `run_repro.sh` ayarını uyguladık. Kaynak SentiTurca için öğrenme oranı, epoch, seed sayısı ve en büyük dizi uzunluğu vermediğinden, bu ayarın rakibin e-ticaret protokolüyle aynı olduğu doğrulanamaz \[1\]. Kaynak yalnızca HF GLUE betiğinin küçük değişikliklerle kullanıldığını belirtir \[1\].

B0 e-ticarette yayımlanmış değerin +2,65 / +2,31 puan üzerindedir (türetilmiş) ve beş seed'in hepsi üstte kalır. Yuvarlama en elverişsiz yönde alınsa bile (0,66 en çok 66,5; 0,64 en çok 64,5) fark en az +2,15 / +1,81 puandır (türetilmiş). Uzunluk kesmesi de olası bir açıklama değildir, çünkü e-ticaret testinde 128 alt sözcüğü aşan örnek oranı %0,4'tür (Tablo 1).

Split boyutlarımız (73.920 / 15.000 / 15.000) kaynaktaki 73K / 15K / 15K ile yuvarlama düzeyinde uyumludur \[1\]. Kodda bariz bir hata bulunmadı (test n = 15.000, 5 sınıf, makro F1). Sürüm, ayar ya da metrik hesabı farkı olasıdır, ancak hiçbiri sınanmamıştır ve nedeni belirleyemiyoruz.

Yayımlanmış sayının bu görevde zayıf bir eşik olduğu başka bir gözlemle de görülür. En büyük kaybı veren MORPH bile doğrulukta rakibi geçer (66,78 > 66,0); makro F1'de ise hemen altındadır (63,99 < 64,0). Bu yüzden aşağıda yöntem katkısı her zaman B0'a göre verilir; rakibe göre fark ayrıca ve türetilmiş olarak belirtilir.

### 6.2 Tek modeller: uzunluk, sözlük ve ek kodlayıcılar

**Tablo 8.** TrSST-2 tek model sonuçları. Δ, ortak seed'lerde B0'a karşı eşleştirilmiş t'dir; 3 seed'li satırlar B0'ın 1, 4 ve 21 seed'leriyle eşleştirilir. Temiz test n = 8.259. Morfoloji, puan izi ve LLM satırları Bölüm 7'dedir.

| Yapılandırma | Fark (B0'a göre) | Seed | Val acc | Test acc | Test F1 | Δ acc \[%95 GA\] | Δ F1 \[%95 GA\] | Temiz test acc | Temiz test F1 |
| --- | --- | --: | --: | --: | --: | --- | --- | --: | --: |
| B0 | – | 5 | 87,87 ± 0,20 | 87,75 ± 0,12 | 91,54 ± 0,09 | referans | referans | 87,47 ± 0,13 | 91,38 ± 0,10 |
| LEN256 | 256 alt sözcük | 5 | 88,44 ± 0,21 | 88,33 ± 0,22 | 91,95 ± 0,14 | +0,59 \[+0,30; +0,88\] | +0,42 \[+0,22; +0,61\] | 87,93 ± 0,25 | 91,70 ± 0,16 |
| L256H | 256 alt sözcük + 5e-5 / 128 / 3 | 5 | 88,20 ± 0,22 | 88,18 ± 0,07 | 91,83 ± 0,04 | +0,43 \[+0,22; +0,64\] | +0,30 \[+0,17; +0,43\] | 87,77 ± 0,06 | 91,58 ± 0,03 |
| B128K | 128k sözlük | 5 | 88,27 ± 0,10 | 88,18 ± 0,12 | 91,83 ± 0,09 | +0,43 \[+0,24; +0,62\] | +0,29 \[+0,16; +0,43\] | 87,85 ± 0,13 | 91,63 ± 0,10 |
| B128KL | 128k sözlük + 256 alt sözcük | 3 | 88,53 ± 0,27 | 88,50 ± 0,02 | 92,05 ± 0,01 | +0,79 \[+0,45; +1,12\] | +0,56 \[+0,33; +0,78\] | 88,07 ± 0,03 | 91,78 ± 0,02 |
| ELECTRA | Türkçe ELECTRA-base, 5e-5 / 32 | 5 | 87,66 ± 0,10 | 87,52 ± 0,10 | 91,35 ± 0,04 | −0,22 \[−0,43; −0,02\] | −0,19 \[−0,29; −0,09\] | 87,19 ± 0,10 | 91,14 ± 0,03 |
| XLMRL | XLM-R large, 1e-5 / 32 | 3 | 88,16 ± 0,14 | 88,12 ± 0,21 | 91,80 ± 0,14 | +0,41 \[−0,19; +1,01\] | +0,30 \[−0,12; +0,73\] | 87,84 ± 0,20 | 91,63 ± 0,14 |

En iyi tek model B128KL'dir: 88,50 ± 0,02 doğruluk, 92,05 ± 0,01 F1 ve B0'a göre +0,79 \[+0,45; +1,12\]. Bu satır yalnızca 3 seed'e dayanır; n = 3 ile kestirilen 0,02'lik ss güvenilir bir değişkenlik ölçüsü değildir. Beş seed'le desteklenen en güçlü tek model bulgusu LEN256'dır: +0,59 \[+0,30; +0,88\].

Uzunluk etkisi veri istatistikleriyle tutarlıdır: TrSST-2 test örneklerinin %12,1'i 128 alt sözcüğü aşar (Tablo 1). 128k sözlük de tek başına +0,43 \[+0,24; +0,62\] kazandırır; bu, belirteçleyicinin aşağı akış başarımını etkilediği bulgularıyla uyumludur \[15\]\[16\]. 128k modeli ayrı bir ön eğitim koşusu olduğundan bu etki saf sözlük etkisi olarak okunmamalıdır.

L256H, LEN256'nın gerisinde kalır (88,18'e karşı 88,33; eşleştirilmemiş fark −0,15, türetilmiş). 128 alt sözcükte tek seed'le seçilen ayar, 256 alt sözcüğe taşındığında yarar sağlamamıştır. Bu fark ss düzeyindedir ve ayrıca sınanmamıştır.

XLM-R large \[27\] TrSST-2'de B0'dan ayırt edilemez: +0,41 \[−0,19; +1,01\]. ELECTRA \[26\] küçük ama GA'sı sıfırı dışlayan bir kayıp verir: −0,22 \[−0,43; −0,02\]. Üst sınır sıfıra çok yakındır ve çoklu karşılaştırma düzeltmesi altında korunmayabilir (Bölüm 8.3).

**Tablo 9.** SentiTurca e-ticaret tek model sonuçları (F1 = makro F1). MORPH satırı Bölüm 7.2'de verilir. LEN256 bu görevde koşulmamıştır.

| Yapılandırma | Seed | Val acc | Test acc | Test makro F1 | Δ acc \[%95 GA\] | Δ makro F1 \[%95 GA\] |
| --- | --: | --: | --: | --: | --- | --- |
| B0 | 5 | 69,04 ± 0,11 | 68,65 ± 0,15 | 66,31 ± 0,23 | referans | referans |
| B128K | 5 | 69,00 ± 0,20 | 69,07 ± 0,07 | 66,77 ± 0,13 | +0,42 \[+0,20; +0,64\] | +0,45 \[+0,15; +0,75\] |
| ELECTRA | 5 | 68,83 ± 0,28 | 68,43 ± 0,37 | 66,17 ± 0,38 | −0,22 \[−0,79; +0,36\] | −0,14 \[−0,87; +0,58\] |
| XLMRL | 3 | 69,84 ± 0,13 | 69,45 ± 0,08 | 67,30 ± 0,10 | +0,72 \[+0,43; +1,01\] | +0,87 \[+0,52; +1,23\] |

E-ticarette en iyi tek model XLMRL'dir: 69,45 ± 0,08 doğruluk ve 67,30 ± 0,10 makro F1. B0'a göre fark +0,72 \[+0,43; +1,01\] doğruluk ve +0,87 \[+0,52; +1,23\] makro F1'dir. B128K de iki metrikte GA'sı sıfırı dışlayan kazanç verir; ELECTRA ise B0'dan ayırt edilemez.

XLM-R large'ın GA'sı TrSST-2'de sıfırı içerir, e-ticarette dışlar. E-ticarette kesme neredeyse olmadığından (%0,4) bu kazanç uzunluktan gelmez. XLMRL farklı bir ayar da (1e-5 / 32) kullandığı için kodlayıcı etkisi hiperparametre etkisinden ayrılamaz.

128k sözlük iki görevde de aynı yönde ve benzer büyüklükte kazanç verir (+0,43 ve +0,42). LEN256 e-ticarette koşulmamıştır; yorumların yalnızca %0,4'ü sınırı aştığı için etkisinin küçük olması beklenir, ancak bu sınanmamıştır.

### 6.3 Seed toplulukları ve validation güdümlü topluluk seçimi (ORK2)

ORK2, her yapılandırmanın seed topluluğunu bir aday sayır (Bölüm 4.9). Her adımda validation doğruluğunu en çok artıran aday eklenir (yerine koymalı, en çok 10 adım) \[28\]. Artış durunca seçim biter; her seçim adımı olasılık ortalamasında eşit ağırlık alır, yani iki kez seçilen bir topluluğun ağırlığı iki katıdır ve test etiketleri seçimde kullanılmaz (`ensemble.py`).

TrSST-2'de seçim B128KL, XLMRL ve LEN256'yı bu sırayla ekler; validation doğruluğu 88,84 → 89,16 → 89,28 izler. Bu topluluk 11 modelden oluşur (3 + 3 + 5) ve üç XLM-R large modeli içerir. E-ticarette seçim XLMRL ve B0'ı ekler (3 + 5 = 8 model).

**Tablo 10.** Seed toplulukları (olasılık ortalaması) ve ORK2, test. Δ, B0 topluluğuna karşı eşleştirilmiş bootstrap farkıdır (1.000 örnekleme, %95 yüzdelik GA) ve seed varyansını içermez. "Model" topluluktaki ince ayarlı model sayısıdır; TrSST-2'de F1 olumlu sınıf, e-ticarette makro F1'dir.

| Görev | Sistem | Model | Val acc | Test acc | Test F1 | Δ acc \[%95 GA\] | Δ F1 \[%95 GA\] | Temiz acc / F1 |
| --- | --- | --: | --: | --: | --: | --- | --- | --- |
| TrSST-2 | B0 topluluğu | 5 | 88,05 | 87,87 | 91,63 | referans | referans | 87,53 / 91,43 |
| TrSST-2 | B0R | 5 | 87,92 | 87,79 | 91,58 | −0,08 \[−0,32; +0,16\] | −0,05 \[−0,22; +0,11\] | 87,53 / 91,43 |
| TrSST-2 | HBEST (= ORK) | 5 | 88,10 | 88,12 | 91,80 | +0,26 \[−0,04; +0,56\] | +0,17 \[−0,04; +0,38\] | 87,84 / 91,63 |
| TrSST-2 | LEN256 | 5 | 88,66 | 88,59 | 92,13 | +0,73 \[+0,43; +1,04\] | +0,50 \[+0,29; +0,72\] | 88,20 / 91,88 |
| TrSST-2 | L256H | 5 | 88,62 | 88,62 | 92,15 | +0,75 \[+0,39; +1,10\] | +0,52 \[+0,28; +0,75\] | 88,21 / 91,90 |
| TrSST-2 | B128K | 5 | 88,57 | 88,52 | 92,07 | +0,65 \[+0,17; +1,09\] | +0,44 \[+0,11; +0,74\] | 88,16 / 91,85 |
| TrSST-2 | B128KL | 3 | 88,84 | 88,75 | 92,23 | +0,88 \[+0,45; +1,37\] | +0,60 \[+0,30; +0,92\] | 88,34 / 91,97 |
| TrSST-2 | ELECTRA | 5 | 88,42 | 87,82 | 91,56 | −0,04 \[−0,51; +0,45\] | −0,07 \[−0,40; +0,28\] | 87,52 / 91,37 |
| TrSST-2 | XLMRL | 3 | 88,39 | 88,42 | 92,01 | +0,55 \[+0,04; +1,06\] | +0,38 \[+0,03; +0,73\] | 88,09 / 91,81 |
| TrSST-2 | KOK | 5 | 87,28 | 86,93 | 90,99 | −0,94 \[−1,48; −0,43\] | −0,64 \[−1,00; −0,28\] | 86,57 / 90,78 |
| TrSST-2 | KOKEK | 5 | 86,64 | 86,83 | 90,94 | −1,04 \[−1,54; −0,53\] | −0,69 \[−1,04; −0,33\] | 86,69 / 90,89 |
| TrSST-2 | KOKEK256 | 3 | 87,67 | 87,86 | 91,60 | −0,01 \[−0,45; +0,40\] | −0,03 \[−0,34; +0,26\] | 87,57 / 91,43 |
| TrSST-2 | STRIP | 5 | 87,74 | 87,63 | 91,47 | −0,24 \[−0,48; +0,04\] | −0,16 \[−0,32; +0,03\] | 87,63 / 91,50 |
| TrSST-2 | LDROP | 5 | 86,91 | 86,81 | 90,55 | −1,05 \[−1,57; −0,55\] | −1,08 \[−1,45; −0,72\] | 86,56 / 90,39 |
| TrSST-2 | LWEIGHT | 5 | 87,78 | 87,79 | 91,41 | −0,08 \[−0,45; +0,29\] | −0,22 \[−0,48; +0,04\] | 87,53 / 91,25 |
| TrSST-2 | ORK2: B128KL + XLMRL + LEN256 | 11 | 89,28 | 89,20 | 92,56 | +1,33 \[+0,96; +1,72\] | +0,93 \[+0,67; +1,19\] | 88,81 / 92,32 |
| E-ticaret | B0 topluluğu | 5 | 69,25 | 69,09 | 66,78 | referans | referans | – |
| E-ticaret | B0R | 5 | 69,25 | 68,93 | 66,53 | −0,16 \[−0,42; +0,11\] | −0,25 \[−0,55; +0,06\] | – |
| E-ticaret | HBEST (= ORK) | 5 | 69,63 | 69,03 | 66,71 | −0,06 \[−0,35; +0,19\] | −0,07 \[−0,40; +0,22\] | – |
| E-ticaret | B128K | 5 | 69,36 | 69,50 | 67,22 | +0,41 \[−0,06; +0,93\] | +0,44 \[−0,10; +1,01\] | – |
| E-ticaret | ELECTRA | 5 | 69,79 | 69,43 | 67,21 | +0,34 \[−0,18; +0,83\] | +0,43 \[−0,19; +0,97\] | – |
| E-ticaret | XLMRL | 3 | 70,32 | 69,76 | 67,70 | +0,67 \[+0,15; +1,19\] | +0,92 \[+0,30; +1,51\] | – |
| E-ticaret | MORPH (kök) | 5 | 66,69 | 67,21 | 64,59 | −1,89 \[−2,43; −1,37\] | −2,18 \[−2,85; −1,56\] | – |
| E-ticaret | ORK2: XLMRL + B0 | 8 | 70,67 | 69,89 | 67,72 | +0,80 \[+0,41; +1,19\] | +0,94 \[+0,49; +1,40\] | – |

Seed topluluğu tek başına sınırlı kazanç verir. TrSST-2'de B0 topluluğu seed ortalamasını +0,12, e-ticarette +0,44 puan artırır (türetilmiş: 87,87 − 87,75 ve 69,09 − 68,65). HBEST topluluğunun B0 topluluğuna farkı anlamlı değildir: +0,26 \[−0,04; +0,56\].

ORK2 çalışmanın en büyük farkını üretir: TrSST-2'de +1,33 \[+0,96; +1,72\] doğruluk ve +0,93 \[+0,67; +1,19\] F1. Ancak karşılaştırma eşit hesapla yapılmaz: ORK2 11 model, B0 topluluğu 5 BERTurk-base modeli kullanır. Koşu başına ortalama sürelerle ORK2'nin eğitimi yaklaşık 275, B0 topluluğunun yaklaşık 50 dakikadır (türetilmiş: 3 × 19,0 + 3 × 41,6 + 5 × 18,6 ve 5 × 9,9; yaklaşık 5,6 kat).

ORK2'nin en iyi tek yapılandırma topluluğuna (B128KL, 88,75) üstünlüğü +0,45 puandır (türetilmiş, sınanmamış). Temiz altkümede de ORK2 en yüksek sistemdir (88,81 / 92,32). E-ticarette ORK2 +0,80 \[+0,41; +1,19\] verir, ama XLMRL topluluğu tek başına +0,67'dir; B0'ın eklenmesi testte yalnızca +0,13 doğruluk ve +0,02 makro F1 katar (türetilmiş).

ORK2'nin bileşimi kararlı değildir. İkinci kuyruk geçişinde, aday havuzu daha darken, seçim LEN256 + XLMRL + B128K idi ve test sonucu 88,96 / 92,40'tı. Havuz test sütunlarını da içeren tablolar görüldükten sonra genişletildiği için ORK2 sayısı iyimser yanlı olabilir (Bölüm 8.3).

Rakibe göre ORK2, TrSST-2'de +1,80 / +1,26, e-ticarette +3,89 / +3,72 puan öndedir (türetilmiş). Bu farkın TrSST-2'de +0,35 / +0,24'lük, e-ticarette +2,65 / +2,31'lik kısmı tek model B0'da zaten vardır.

### 6.4 BERTurk + LLM füzyonu (AS3, ikinci yarı)

**Tablo 11.** BERTurk toplulukları ile Turkish-Llama-8b-Instruct \[9\] füzyonu (TrSST-2). w yalnızca TrSST-2 validation doğruluğuyla seçilmiştir ve ürün testinde yeniden ayarlanmaz. "Kendi topluluğuna göre" sütunu bootstrap'sız, türetilmiş farktır.

| Sistem | w | Test acc | Test F1 | Kendi topluluğuna göre Δ acc (türetilmiş) | B0 topluluğuna göre Δ acc \[%95 GA\] | B0 topluluğuna göre Δ F1 \[%95 GA\] | Temiz test acc / F1 | Ürün testi acc / makro F1 |
| --- | --: | --: | --: | --: | --- | --- | --- | --- |
| LLM (3 şablon ortalaması) | 1,0 | 78,41 | 82,97 | – | – | – | – | 88,94 / 72,17 |
| B0 + LLM | 0,30 | 88,19 | 91,71 | +0,32 | +0,32 \[−0,04; +0,67\] | +0,08 \[−0,18; +0,32\] | 87,87 / 91,50 | 93,77 / 78,72 |
| HBEST + LLM (= ORK + LLM) | 0,20 | 88,40 | 91,92 | +0,28 | +0,54 \[+0,21; +0,90\] | +0,29 \[+0,06; +0,53\] | 88,09 / 91,72 | 93,70 / 78,02 |
| LEN256 + LLM | 0,15 | 88,66 | 92,13 | +0,07 | +0,79 \[+0,47; +1,11\] | +0,50 \[+0,27; +0,71\] | 88,24 / 91,86 | 93,67 / 77,78 |
| L256H + LLM | 0,10 | 88,68 | 92,16 | +0,07 | +0,82 \[+0,48; +1,16\] | +0,53 \[+0,30; +0,77\] | 88,26 / 91,89 | 93,86 / 77,65 |
| ORK2 + LLM | 0,20 | 88,93 | 92,28 | −0,27 | +1,06 \[+0,69; +1,47\] | +0,65 \[+0,39; +0,93\] | 88,52 / 92,02 | 94,83 / 80,94 |

LLM tek başına 78,41 doğruluk ve 82,97 F1 alır. Bu, çoğunluk tabanının (%71,0) üzerinde, B0 topluluğunun ise yaklaşık 9,5 puan altındadır (türetilmiş). Füzyonun B0 topluluğuna katkısı anlamlı değildir: +0,32 \[−0,04; +0,67\] doğruluk, +0,08 \[−0,18; +0,32\] F1.

Diğer füzyon satırlarının B0'a karşı anlamlı farkları büyük ölçüde altlarındaki topluluktan gelir; örneğin LEN256 + LLM'in +0,79'unun +0,73'ü LEN256 topluluğuna aittir (Tablo 10). Kendi topluluklarına göre katkı taban güçlendikçe küçülür: +0,32, +0,28, +0,07, +0,07 ve ORK2'de −0,27 (türetilmiş). ORK2 + LLM (88,93 / 92,28) ORK2'nin (89,20 / 92,56) gerisinde kalır; validation'da seçilen w = 0,20 teste genellenmemiştir.

Ürün testinde tablo farklıdır: füzyon her toplulukta makro F1'i artırır. Artış L256H'de +0,65 ile HBEST'te (= ORK) +1,44 arasındadır; B0'da +1,38, ORK2'de +1,26'dır (türetilmiş: örneğin 77,33 → 78,72 ve 79,68 → 80,94). Doğruluk ise B0'da −0,17, ORK2'de −0,08 puan düşer (türetilmiş).

Bu ürün farkları bootstrap ile sınanmamıştır. w film validation'ında seçildiği için bunlar sıfır atışlı bir aktarım ölçümüdür. ORK = HBEST olduğundan (Bölüm 7.1) ORK + LLM satırı TrSST-2'de HBEST + LLM ile birebir aynıdır ve tabloda tek satırda verilmiştir.

### 6.5 Rakip ve B0 karşısında özet

Rakibin yayımlanmış değerleri iki görevde de yalnızca yeniden üretimle aşılmaktadır. Bu nedenle rakibe göre fark, yöntem katkısının ölçüsü değildir. Yöntem katkısının ölçüsü, aynı protokol ve aynı seed'lerdeki B0'dır.

B0'a karşı GA'sı sıfırı dışlayan tek model kazançları uzunluk, sözlük ve kodlayıcı değişikliklerinden gelir. TrSST-2'de bunlar B128KL (+0,79), LEN256 (+0,59), L256H (+0,43) ve B128K'dir (+0,43). E-ticarette XLMRL (+0,72) ve B128K (+0,42) bu gruptadır.

Morfoloji, LLM ile etiket temizliği, puan izi silme ve hiperparametre araması B0'ı geçmez (Bölüm 7). Topluluk düzeyinde ORK2 en yüksek sayıyı verir, ama eşit olmayan hesap ve uyarlamalı aday havuzu koşullarında. Bootstrap seed varyansını içermediği için ORK2 kazancı tek model kazançlarıyla aynı kanıt düzeyinde değildir.

### 6.6 Üçüncü görev: SentiTurca hate

**Tablo 12.** SentiTurca hate test sonuçları (%; F1 = makro F1; kaynak `experiments/logs/aggregate_latest.md`). Tek model satırları seed ortalaması ± ss'dir ve Δ, aynı seed'lerde B0'a göre eşleştirilmiş t ile %95 GA'dır. Topluluk satırları seed olasılık ortalamasıdır ve Δ, B0 topluluğuna karşı eşleştirilmiş bootstraptır (1.000 örnekleme). ORK2 seçimi yerine koymalıdır: B128KL iki kez seçildiği için olasılık ortalamasında ağırlığı 1/2, XLMRL ve LEN256'nınki 1/4'tür (toplam 11 ayrı model).

| Sistem | Seed | Val acc | Test acc | Test makro F1 | Δ acc | Δ makro F1 |
| --- | --: | --: | --: | --: | --- | --- |
| Rakip (SentiTurca, yayımlanmış) | – | – | 61,00 | 58,00 |  |  |
| Çoğunluk sınıfı (hep nötr) | – | 74,14 | 74,58 | 21,36 |  |  |
| B0 (run\_all.sh: 2e-5 / 128 / 3) | 5 | 84,06 ± 0,27 | 83,78 ± 0,16 | 59,83 ± 0,28 | – | – |
| B0E5 (aynı ayar, 5 epoch) | 5 | 83,84 ± 0,40 | 83,84 ± 0,15 | 60,65 ± 0,26 | +0,06 \[−0,31; +0,42\] | **+0,83 \[+0,31; +1,34\]** |
| LEN256 | 5 | 84,43 ± 0,36 | 84,05 ± 0,19 | 60,46 ± 0,43 | +0,27 \[−0,07; +0,61\] | +0,64 \[−0,00; +1,28\] |
| B128K | 5 | 84,23 ± 0,20 | 83,64 ± 0,10 | 59,49 ± 0,45 | −0,14 \[−0,41; +0,12\] | −0,33 \[−1,09; +0,42\] |
| B128KL | 3 | 84,65 ± 0,09 | 83,66 ± 0,14 | 60,27 ± 0,54 | −0,15 \[−0,31; −0,00\] | +0,50 \[−0,35; +1,34\] |
| XLMRL | 3 | 83,86 ± 0,28 | 83,38 ± 0,41 | 59,28 ± 0,21 | −0,43 \[−1,27; +0,40\] | −0,49 \[−1,54; +0,55\] |
| WCE (sınıf ağırlıklı kayıp, dengeli) | 5 | 70,88 ± 0,58 | 71,04 ± 0,64 | 60,55 ± 0,49 | −12,74 \[−13,72; −11,75\] | +0,72 \[−0,09; +1,53\] |
| WCES (sınıf ağırlıklı kayıp, karekök) | 5 | 79,40 ± 0,26 | 79,97 ± 0,37 | 65,87 ± 0,49 | −3,81 \[−4,44; −3,18\] | **+6,04 \[+5,32; +6,76\]** |
| B0 topluluğu | 5 | 84,14 | 83,96 | 60,07 | – | – |
| B0E5 topluluğu | 5 | 84,50 | 84,34 | 60,83 | +0,38 \[−0,08; +0,84\] | +0,76 \[−0,25; +2,08\] |
| LEN256 topluluğu | 5 | 84,52 | 84,22 | 60,61 | +0,26 \[−0,14; +0,72\] | +0,54 \[−0,12; +1,21\] |
| ORK2 (B128KL+XLMRL+LEN256+B128KL) | 4 adım (11 model) | 85,16 | 84,36 | 60,45 | +0,40 \[−0,16; +1,00\] | +0,38 \[−0,28; +1,01\] |

Rakibin yayımlanmış makro F1'i (58,00) B0 tekrarımız dahil her yapılandırmada aşılır; B0'ın beş seed'inin en düşüğü bile 59,53'tür. Doğruluk karşılaştırması ise anlamlı değildir. Rakibin 61,00'ı çoğunluk sınıfı tabanının (74,58) 13,6 puan altındadır ve B0'ımız onu 22,8 puan aşar.

Bu doğruluk farkı yöntemden değil, büyük olasılıkla veri sürümü ya da değerlendirme düzeni farkından gelir; nedenini belirleyemedik. Rakip betiği varsayılan olarak validation kümesinde değerlendirir, ama orada da çoğunluk tabanı %74,14'tür. Bu çekince makro F1 için de geçerlidir: yayımlanmış değerlerin bizimkiyle aynı veri ve değerlendirme düzeninde ölçüldüğü doğrulanamaz. Ayrıca rakip tek bir sayı verir, biz seed ortalaması raporlarız. Bu yüzden rakiple makro F1 karşılaştırması betimseldir; yöntem katkısını yalnızca B0'a göre raporluyoruz.

Sınıf dengesizliği yöntemleri (WCE, WCES; Bölüm 6.7) dışında B0'a karşı seed düzeyinde makro F1 güven aralığı sıfırı dışlayan tek değişiklik eğitimi 5 epoch'a uzatmaktır (B0E5, makro F1 +0,83). Bu ayar rakibin kendi README'sindeki hate komutudur; dolayısıyla yöntemimizin değil, protokol okumasının etkisidir. LEN256'nın makro F1 farkı sınırdadır (alt sınır −0,00). B128KL'nin doğruluk farkı da sınırdadır (−0,15 \[−0,31; −0,00\], 3 seed) ve küçük bir kayıp yönündedir. TrSST-2'de kazandıran 128k sözlük ve e-ticarette kazandıran XLM-R large bu görevde B0'ı geçemez.

Topluluk düzeyinde bootstrap güven aralıklarının hiçbiri sıfırı dışlamaz. ORK2 validation doğruluğunda en yüksek değeri (85,16) verir, ama test makro F1'i (60,45) B0E5 topluluğunun (60,83) altında kalır. ORK2'nin seçim ölçütü doğruluk olduğundan, makro F1'in birincil olduğu bu görevde seçim ve değerlendirme ölçütleri uyuşmaz. Ölçütü sonuçları gördükten sonra değiştirmedik.

Sınıf bazında F1, makro F1'in tavanını açıklar. B0 topluluğunda hakaret 62,6, nefret 87,6, nötr 90,1 ve medeni 0,0 F1 alır; B0E5'te medeni 1,9'a çıkar, LEN256'da yine 0,0'dır. Medeni sınıfı neredeyse hiç tahmin edilmediğinden makro F1 en fazla 75'e ulaşabilir. Bu görevde kazanç, mimari ya da topluluk değişikliğinden çok sınıf dengesizliğine yönelik yöntemlerden (ağırlıklı kayıp, eşik ayarı) beklenir; Bölüm 6.7 bu iki yöntemi dener.

### 6.7 Sınıf dengesizliği: ağırlıklı kayıp ve eşik ayarı

Eşik ayarı, ağırlıksız eğitilen altı yapılandırmanın topluluklarında makro F1'i aynı topluluğun ayarsız hâline göre 5,8–9,1 puan artırır. Ağırlıklı kayıpla eğitilen modellerde katkısı küçüktür: WCE'de 3,7, WCES'te 1,0 puan (türetilmiş). Validation makro F1'ine göre seçilen sistem eşik ayarlı B128KL topluluğudur: testte 69,08 makro F1, ayarsız B0 topluluğuna göre +9,02 \[+7,05; +10,84\]. Bu farkın büyük kısmı eşik ayarından gelir; eşik ayarlı B0 topluluğu +7,28 \[+5,23; +9,18\] alır. Bu analiz, test topluluğunda medeni sınıfının F1'inin 0 olduğu görüldükten sonra eklendi; yöntem ailesi ve kaydırma ızgarası bu gözleme yanıt olarak belirlendi (Bölüm 8.3). Kaydırmalar yalnızca validation makro F1'iyle seçildi, ama bulgular keşifseldir.

**Tablo 13.** SentiTurca hate seed toplulukları, sınıf dengesizliği yöntemleri (%; kaynak `experiments/logs/hate_thresh.md` ve `experiments/logs/aggregate_latest.md`). Δ, ayarsız B0 topluluğuna karşı eşleştirilmiş bootstraptır (1.000 örnekleme). Eşik ayarlı diğer topluluklar (B0E5, B128K, LEN256, XLMRL) 65,61–66,60 makro F1 alır. Medeni F1, 101 test örneğine dayanır; sınıf düzeyinde GA hesaplanmadı.

| Sistem | Eşik ayarı | Test acc | Test makro F1 | Medeni F1 | Δ acc | Δ makro F1 |
| --- | --- | --: | --: | --: | --- | --- |
| B0 topluluğu | yok | 83,96 | 60,07 | 0,0 | – | – |
| WCE topluluğu | yok | 71,50 | 60,85 | 21,8 | −12,46 \[−13,62; −11,22\] | +0,78 \[−0,89; +2,25\] |
| WCES topluluğu | yok | 80,32 | 66,31 | 28,8 | −3,64 \[−4,64; −2,84\] | **+6,24 \[+4,45; +7,92\]** |
| B0 topluluğu | var | 82,14 | 67,35 | 28,9 | −1,82 \[−2,52; −1,16\] | **+7,28 \[+5,23; +9,18\]** |
| WCE topluluğu | var | 77,86 | 64,57 | 27,3 | −6,10 \[−7,14; −5,16\] | **+4,50 \[+2,56; +6,08\]** |
| WCES topluluğu | var | 81,62 | 67,31 | 28,8 | −2,34 \[−3,22; −1,54\] | **+7,24 \[+5,17; +9,16\]** |
| B128KL topluluğu | var | 82,26 | 69,08 | 38,3 | −1,70 \[−2,62; −0,86\] | **+9,02 \[+7,05; +10,84\]** |

Dengeli ağırlık (WCE) işe yaramaz. Seed düzeyinde makro F1 farkı +0,72 \[−0,09; +1,53\] ile anlamlı değildir ve doğruluk 12,74 puan düşer (Tablo 12). Medeni ağırlığı 11,13 olunca model azınlık sınıflarını aşırı tahmin ediyor görünür; toplulukta nötr F1'i 90,1'den 80,2'ye iner.

Karekök ağırlık (WCES) daha ılımlıdır. Seed düzeyinde makro F1'i +6,04 \[+5,32; +6,76\] artırır ve doğruluğu 3,81 puan düşürür. Toplulukta medeni F1'i 0'dan 28,8'e çıkar.

Seed düzeyinde eşik ayarlı yapılandırmalar ayarsız B0'ı makro F1'de +4,56 ile +8,95 puan aşar; B0'ın kendisinde eşik ayarının katkısı +6,21 \[+4,84; +7,59\]'dur. Üst uçtaki B128KL aralığı (+8,95 \[+8,83; +9,07\]) üç seed'den kestirilmiştir ve güvenilir bir değişkenlik ölçüsü değildir. Toplulukta medeni F1'i 24,7–38,3'e çıkar. Bedeli doğruluktadır: ağırlıksız eşik ayarlı topluluklar ayarsız B0 topluluğundan 0,9–2,0 puan düşük doğruluk alır, ama çoğunluk tabanının (%74,58) en az 7,3 puan üzerinde kalır.

Bu deneyde iki yöntemin birleşimi ek kazanç sağlamadı. Eşik ayarlı WCES topluluğu (67,31) ile eşik ayarlı B0 topluluğu (67,35) arasında fark gözlenmedi (türetilmiş, sınanmamış); eşik ayarı WCES'e toplulukta 1,0, seed düzeyinde 0,4 puan ekler. Bu tek karşılaştırma, iki yöntemin bu düzende büyük ölçüde aynı etkiyi yakaladığını düşündürür. WCE'de seçilen kaydırmalar nefret ve medeni için negatiftir (−2,0 ve −1,8); bunu ağırlıklı kaybın aşırı düzeltmesinin geri alınması olarak yorumluyoruz. Nefret kaydırması ızgaranın alt sınırındadır; WCE'nin ayarlı sonucu (64,57) bu yüzden ızgarayla sınırlı olabilir. Yeniden eğitim gerektirmediği için eşik ayarı daha ucuz yoldur.

Validation makro F1'ine göre en iyi yapılandırma eşik ayarlı B128KL topluluğudur (69,14; diğerleri 64,62–68,98). Bu değer kaydırmaların seçildiği validation kümesinde ölçülür, seçim tablo okunarak elle yapıldı ve LEN256'ya fark yalnızca 0,16 puandır. B128KL topluluğu, diğer adayların beşi yerine üç modelden oluşur. Testte 69,08 makro F1 ve 38,3 medeni F1 alır, ama eşik ayarlı B0 topluluğuna üstünlüğü (+1,73, türetilmiş) sınanmamıştır; testteki sıralama doğrulayıcı değildir. Rakibin yayımlanmış makro F1'iyle (58,00) karşılaştırma Bölüm 6.6'daki çekinceye tabidir ve betimseldir. Medeni sınıfı yine de en zayıf sınıftır; kalan makro F1 açığının büyük kısmı bu sınıftan gelir.

## 7. Ablasyon, Dayanıklılık ve Alanlar Arası Aktarım

Bu bölüm her bileşeni B0'a karşı tek tek değerlendirir ve AS2-AS5'i yanıtlar. Buradaki karşıtlıkların bir kısmı türetilmiştir. GA'sı olmayan bir fark iyileştirme ya da kötüleşme diye nitelenmez.

### 7.1 Hiperparametre ızgarası, HBEST ve ORK

**Tablo 14.** Tek seed'li (42) ızgaralarda validation doğruluğu. TrSST-2'de seçilen hücre 5e-5 / 128 / 3, e-ticarette 2e-5 / 32 / 3'tür.

| lr / bs | TrSST-2, 3 epoch | TrSST-2, 4 epoch | E-ticaret, 3 epoch |
| --- | --: | --: | --: |
| 2e-5 / 128 | 87,75 | 87,73 | 69,09 |
| 2e-5 / 32 | 87,79 | 87,49 | **69,28** |
| 3e-5 / 128 | 87,68 | 87,61 | 68,91 |
| 3e-5 / 32 | 87,88 | 87,37 | 69,15 |
| 5e-5 / 128 | **87,95** | 87,67 | 68,87 |
| 5e-5 / 32 | 87,30 | 87,27 | 68,25 |

TrSST-2 ızgarasındaki 12 ayar 87,27-87,95 aralığında, yani 0,68 puanlık bir bantta toplanır (türetilmiş). B0 ayarı (3e-5 / 128 / 3) ızgarada 87,68, beş seed'de ise 87,87 ± 0,20 alır. Aynı ayarın yalnızca seed değişimiyle 0,19 puan oynaması, ızgara farklarının seed gürültüsü düzeyinde olduğunu gösterir \[22\].

Seçilen ayarlar beş seed'le yeniden koşulduğunda ikisi de B0'dan ayırt edilemez: TrSST-2'de +0,09 \[−0,26; +0,44\], e-ticarette −0,01 \[−0,12; +0,09\]. Seçimin validation üstünlüğü beş seed'de kaybolur (HBEST 87,85 ± 0,12, B0 87,87 ± 0,20). Rakibin ayarı bu ızgarada zaten iyi bir noktadır ve arama ek kazanç getirmemiştir.

ORK, validation'da B0'ı geçen bileşenleri HBEST'e eklemek üzere tasarlanmıştı (Bölüm 4.7). Hiçbir bileşen bu eşiği geçmedi: TrSST-2'de KOK 86,90 ve LWEIGHT 87,55, B0'ın 87,87'sinin altındadır. E-ticarette MORPH 66,38 ile B0'ın 69,04'ünün altında kalır.

Bu yüzden ORK ve HBEST aynı ayardır ve sonuçları da birebir aynıdır. On seed çiftinin hepsinde validation ve test olasılık dosyaları arasındaki fark sıfırdır ve eğitim kayıpları örtüşür. ORK ayrı bir yöntem sonucu olarak sunulmaz; tasarlanan orkestra birleşimi kazanç üretmemiştir.

### 7.2 Morfolojik önişleme ve uzunluk-sözlük etkileşimi (AS2)

Kök-ek ayrımı metni uzatır: validation'da ortalama alt sözcük sayısı 75,5'ten 132,6'ya çıkar. 128 sınırını aşan örnek oranı da %12,3'ten %29,7'ye yükselir (Tablo 3). Bu yüzden KOKEK'in kaybı iki etkinin toplamıdır; Tablo 15 bunları uzunluk ve sözlük karşıtlıklarıyla birlikte verir.

**Tablo 15.** Girdi temsilinin faktör karşıtlıkları (test doğruluğu). "Eşleştirilmiş" satırlar B0'a karşı eşleştirilmiş t'dir (Tablo 8 ve 9). "Türetilmiş" satırlar seed ortalamaları arasındaki basit farklardır; seed kümeleri farklı olabilir ve GA yoktur.

| Karşıtlık | Sabit tutulan | Değişen | Fark (acc) | Tür |
| --- | --- | --- | --- | --- |
| LEN256 − B0 | 32k sözlük, özgün metin | 128 → 256 alt sözcük | +0,59 \[+0,30; +0,88\] | eşleştirilmiş, 5 seed |
| B128K − B0 | 128 alt sözcük, özgün metin | 32k → 128k sözlük | +0,43 \[+0,24; +0,62\] | eşleştirilmiş, 5 seed |
| B128KL − B0 | özgün metin | sözlük ve uzunluk birlikte | +0,79 \[+0,45; +1,12\] | eşleştirilmiş, 3 seed |
| (LEN256 − B0) + (B128K − B0) | – | toplanabilirlik beklentisi | +1,02 | türetilmiş |
| KOK − B0 | 32k, 128 | özgün → kök | −1,20 \[−1,45; −0,94\] | eşleştirilmiş, 5 seed |
| KOKEK − B0 | 32k, 128 | özgün → kök-ek | −1,20 \[−1,49; −0,92\] | eşleştirilmiş, 5 seed |
| KOKEK256 − B0 | 32k | özgün/128 → kök-ek/256 | −0,19 \[−0,95; +0,58\] | eşleştirilmiş, 3 seed |
| KOKEK256 − KOKEK | kök-ek girdisi | 128 → 256 alt sözcük | +0,98 | türetilmiş (3 ve 5 seed) |
| KOKEK256 − LEN256 | 256 alt sözcük | özgün → kök-ek | −0,81 | türetilmiş (3 ve 5 seed) |
| MORPH − B0 (e-ticaret) | 32k, 128 | özgün → kök | −1,87 \[−2,08; −1,65\] | eşleştirilmiş, 5 seed |

128 alt sözcük sınırında iki varyant da B0'ın 1,20 puan gerisindedir ve GA'lar sıfırı açıkça dışlar. F1'de kayıplar −0,81 \[−1,01; −0,62\] (KOK) ve −0,80 \[−1,00; −0,60\]'tır (KOKEK). Topluluk düzeyinde de iki varyant B0 topluluğunun gerisindedir: −0,94 \[−1,48; −0,43\] ve −1,04 \[−1,54; −0,53\] (Tablo 10).

E-ticarette, TrSST-2 validation'ında seçilen kök varyantı (KOK 86,90 > KOKEK 86,26) daha büyük kayıp verir: −1,87 doğruluk, −2,32 \[−2,60; −2,04\] makro F1. Makro F1 kaybının doğruluk kaybından büyük olması, bitişik yıldız sınıflarını ayıran ek düzeyi ayrımların silinmesiyle tutarlıdır. Sınıf bazında hata dökümü olmadığından bu bir hipotezdir.

KOK'un kaybı, kökleme ile olumsuzluk ekinin ("-mA") silinmesi riskiyle uyumludur (Bölüm 4.2). KOK metni kısalttığı (ortalama 68,5 alt sözcük, %9,9 kesme) için bu kayıp kesmeden gelmez. Ancak mekanizma ek türüne göre bir ablasyonla doğrulanmamıştır; validation'da KOK'u seçtiren 0,64 puanlık üstünlük de testte kaybolur (86,55'e karşı 86,54).

KOKEK256, kök-ek kaybının çoğunun kesmeden geldiğini gösterir. Uzunluk 256'ya çıkınca fark −1,20'den −0,19 \[−0,95; +0,58\] değerine iner ve anlamsızlaşır. Topluluk düzeyinde de KOKEK256 B0 topluluğundan ayırt edilemez (−0,01 \[−0,45; +0,40\]).

Ne var ki adil uzunluk denetimi B0 değil, LEN256'dır. KOKEK256 LEN256'nın 0,81 puan gerisindedir (türetilmiş; eşleştirilmemiş, seed sayıları 3 ve 5). Eşit uzunlukta bile açık biçimbirim bölütlemesi bilgi eklememiş görünmektedir; bu fark ayrıca sınanmamıştır.

Olası bir mekanizma gösterim uyumsuzluğudur. Kök-ek girdisinde ekler "+" ile ayrılmış birimler olarak gelir; BERTurk ise ön eğitimde ekleri çoğunlukla sözcük içi "##" parçaları olarak görmüştür. 256 sınırının kök-ek metinlerinde ne kadar kesme bıraktığı da ölçülmemiştir.

AS2'nin yanıtı bu nedenle olumsuzdur. İnce ayar girdisinde verilen kök ve kök-ek bilgisi, BERTurk'ün WordPiece gösterimine yarar eklememiştir. Bu bulgu, morfolojinin ön eğitimde belirteçleyiciye katıldığı düzenleri \[15\] kapsamaz.

Uzunluk ve sözlük etkileri tam toplanır değildir: iki etkinin toplamı +1,02 iken birleşik B128KL +0,79 verir (türetilmiş karşılaştırma). Daha büyük sözlük metni daha az alt sözcüğe bölerek kesmeyi azaltıyor olabilir; bu oran 128k belirteçleyicisi için ölçülmemiştir. Farklı seed kümeleri nedeniyle toplanabilirlik sınanmamıştır.

### 7.3 LLM ile etiket tutarlılığı oylaması ve temizlik (AS3, birinci yarı)

**Tablo 16.** Turkish-Llama-8b-Instruct-v0.1 \[9\] (4 bit NF4) sıfır atışlı başarımı. F1 olumlu sınıf F1'idir.

| Split | Şablon | Acc | F1 | Makro F1 | n |
| --- | --- | --: | --: | --: | --: |
| TrSST-2 test | t1 | 75,96 | 80,61 | 74,49 | 8.934 |
| TrSST-2 test | t2 | 76,06 | 80,63 | 74,65 | 8.934 |
| TrSST-2 test | t3 | 82,04 | 86,44 | 79,92 | 8.934 |
| TrSST-2 test | çoğunluk oyu | 78,22 | 82,78 | 76,57 | 8.934 |
| TrSST-2 test | olasılık ortalaması | 78,41 | 82,97 | 76,74 | 8.934 |
| TrSST-2 eğitim | olasılık ortalaması | 79,71 | 84,52 | 77,54 | 60.411 |
| Ürün test | t1 | 88,57 | 93,55 | 71,67 | 23.517 |
| Ürün test | t2 | 85,41 | 91,61 | 67,93 | 23.517 |
| Ürün test | t3 | 91,59 | 95,35 | 75,56 | 23.517 |
| Ürün test | olasılık ortalaması | 88,94 | 93,77 | 72,17 | 23.517 |

Şablonlar arasında belirgin fark vardır: rakamla yanıt isteyen t3, TrSST-2 testinde t1'i 6,08 puan geçer (türetilmiş). Şablonlar arası ortalama ikili uyum TrSST-2 testinde %91,00, ürün testinde %93,94'tür. Ürün testinde LLM doğruluğu (88,94), her şeye "olumlu" diyen tabanın (%93,7) altındadır.

Bayrak kuralı eğitim kümesinin 6.051 / 60.411 örneğini (%10,02) işaretler. Bunların 5.175'i altın olumlu, 876'sı altın olumsuzdur; bayrakların %85,5'i olumlu sınıftandır (türetilmiş). Sınıf içi bayrak oranı olumlularda %11,8, olumsuzlarda %5,3'tür (türetilmiş: 5.175 / 43.838 ve 876 / 16.573).

Rastgele 10 bayraklı örnek nitel olarak karışıktır. Bazıları gerçek etiket-metin çelişkisine benzer: "Tavsiye etmem" ile biten bir yorum olumlu, "Filmi çok başarılı buldu..." içeren bir yorum olumsuz etiketlidir. Diğerleri karışık duygulu ya da yalnızca spoiler uyarısıdır; bayrakların kesinliği elle ölçülmemiştir.

**Tablo 17.** LLM bayraklarına dayalı eğitim kümesi müdahaleleri (TrSST-2, 5 seed). Topluluk düzeyindeki bootstrap farkları Tablo 10'dadır.

| Yapılandırma | Val acc | Test acc | Test F1 | Δ acc \[%95 GA\] | Δ F1 \[%95 GA\] | Temiz test acc | Temiz test F1 |
| --- | --: | --: | --: | --- | --- | --: | --: |
| LDROP (çıkarma) | 86,78 ± 0,13 | 86,55 ± 0,14 | 90,36 ± 0,10 | −1,20 \[−1,49; −0,91\] | −1,17 \[−1,40; −0,94\] | 86,27 ± 0,13 | 90,19 ± 0,09 |
| LWEIGHT (0,3 ağırlık) | 87,55 ± 0,11 | 87,41 ± 0,13 | 91,13 ± 0,11 | −0,34 \[−0,56; −0,11\] | −0,41 \[−0,56; −0,26\] | 87,13 ± 0,13 | 90,95 ± 0,11 |

Bayraklı örnekleri çıkarmak (LDROP) açık bir kayıp verir: −1,20 \[−1,49; −0,91\] doğruluk ve −1,17 \[−1,40; −0,94\] F1. Ağırlığı 0,3'e indirmek (LWEIGHT) kaybı küçültür ama kaldırmaz: −0,34 \[−0,56; −0,11\]. LWEIGHT'in topluluk düzeyindeki bootstrap farkı ise anlamsızdır (−0,08 \[−0,45; +0,29\]); iki çıkarım burada ayrışır.

Kaybın nedeni sınanmamıştır; bir hipotez öne sürüyoruz. TrSST-2 etiketi puandan türetildiği ve test etiketleri de aynı kuraldan geldiği için metin-etiket çelişkisi görevin parçası olabilir \[1\]. Bu örnekleri eğitimden silmek dağılım uyumsuzluğu yaratabilir.

Sınıf oranı kayması küçüktür: LDROP sonrası olumlu payı %72,6'dan %71,1'e iner (türetilmiş: 38.663 / 54.360). Kaybın sınıf dengesizliğiyle açıklanması bu yüzden güçtür. Ayrıca yargıç LLM'in eğitimdeki doğruluğu (79,71), ince ayarlı modelin çok altındadır; uyuşmazlıklar etiket gürültüsünden çok yargıcın kendi hatalarını yansıtabilir.

AS3'ün temizlik kısmının yanıtı olumsuzdur: LLM bayrakları ince ayarı iyileştirmemiş, kötüleştirmiştir. Bu sonuç bu LLM, bu bayrak kuralı ve puandan türetilmiş etiketler için geçerlidir. Genel bir etiket temizliği yargısı değildir \[12\]\[13\].

### 7.4 Puan izleri ve temiz altküme (AS5)

Temiz test altkümesi 8.259 örnektir; puan izi taşıyan 675 test örneği (%7,6) dışarıda kalır (türetilmiş: 8.934 − 8.259). İz altkümesindeki doğruluk doğrudan raporlanmamıştır. Bunu tam ve temiz test ortalamalarından aşağıdaki gibi türetiyoruz.

```latex
\mathrm{Acc}_{\mathrm{iz}} = \frac{8934 \cdot \mathrm{Acc}_{\mathrm{tam}} - 8259 \cdot \mathrm{Acc}_{\mathrm{temiz}}}{675}
```

Seed ortalamaları doğrusal olduğu için formül seed ortalamalarına doğrudan uygulanabilir. Değerler iki ondalıkla verildiğinden türetmede en çok ±0,13 puanlık yuvarlama belirsizliği vardır. 675 örnekte binom standart hatası da yaklaşık 1,1 puandır (türetilmiş, p ≈ 0,91 için); bu yüzden iz altkümesi değerleri yalnızca kaba büyüklük olarak okunmalıdır.

**Tablo 18.** TrSST-2 tek model doğruluğu: tam test, temiz altküme, aradaki fark, iz altkümesi doğruluğu ve sıralar. Fark, iz altkümesi ve sıralar türetilmiştir; eşit değerler sıra aralığıyla gösterilir.

| Sistem | Alt sözcük | Tam acc (n = 8.934) | Temiz acc (n = 8.259) | Temiz − tam | İz altkümesi acc (≈, n = 675) | Sıra (tam) | Sıra (temiz) |
| --- | --: | --: | --: | --: | --: | --: | --: |
| B128KL | 256 | 88,50 | 88,07 | −0,43 | 93,8 | 1 | 1 |
| LEN256 | 256 | 88,33 | 87,93 | −0,40 | 93,2 | 2 | 2 |
| L256H | 256 | 88,18 | 87,77 | −0,41 | 93,2 | 3–4 | 5 |
| B128K | 128 | 88,18 | 87,85 | −0,33 | 92,2 | 3–4 | 3 |
| XLMRL | 128 | 88,12 | 87,84 | −0,28 | 91,5 | 5 | 4 |
| HBEST | 128 | 87,83 | 87,53 | −0,30 | 91,5 | 6 | 6 |
| B0 | 128 | 87,75 | 87,47 | −0,28 | 91,2 | 7 | 7 |
| B0R | 128 | 87,66 | 87,38 | −0,28 | 91,1 | 8 | 9 |
| ELECTRA | 128 | 87,52 | 87,19 | −0,33 | 91,6 | 9–10 | 11 |
| KOKEK256 | 256 | 87,52 | 87,22 | −0,30 | 91,2 | 9–10 | 10 |
| LWEIGHT | 128 | 87,41 | 87,13 | −0,28 | 90,8 | 11 | 12 |
| STRIP | 128 | 87,38 | 87,39 | +0,01 | 87,3 | 12 | 8 |
| KOK | 128 | 86,55 | 86,16 | −0,39 | 91,3 | 13–14 | 15 |
| LDROP | 128 | 86,55 | 86,27 | −0,28 | 90,0 | 13–14 | 14 |
| KOKEK | 128 | 86,54 | 86,40 | −0,14 | 88,3 | 15 | 13 |

STRIP tam testte B0'ın −0,37 \[−0,64; −0,09\] gerisindedir. Temiz altkümede fark yalnızca −0,08'dir (87,39'a karşı 87,47; türetilmiş, sınanmamış). İz altkümesinde ise doğruluk yaklaşık 91,2'den 87,3'e iner; STRIP'in kaybı neredeyse tamamen izli örneklerdedir.

Topluluk düzeyinde STRIP temiz altkümede B0 topluluğunu geçer: 87,63'e karşı 87,53 (türetilmiş, Tablo 10). Bu, B0'ın izli örneklerde puan izini kullandığını düşündürür (fark 0,10 puandır ve sınanmamıştır); açıklama artefaktlarının tanımladığı örüntüyle uyumludur \[21\]. Yine de iz etiketi tam belirlemez: B0'ın iz altkümesi doğruluğu 100'e değil, yaklaşık 91'e yakındır (Bölüm 3.5).

Puan izinin skoru şişirme etkisi küçüktür. Çoğu sistemde tam test ile temiz altküme farkı 0,28-0,43 puandır; istisnalar STRIP (+0,01) ve KOKEK'tir (−0,14). B0'ın temiz altküme değeri (87,47) bile rakibin tam test değerine (87,4) denk düşer.

Sıralamanın üst kısmı değişmez: B128KL ve LEN256 temiz altkümede de ilk iki sıradır. Yer değiştirmeler ss düzeyindedir: L256H üçüncülükten beşinciliğe iner, STRIP on ikincilikten sekizinciliğe çıkar, KOKEK ise KOK'u geçer. Topluluk düzeyinde ORK2 temiz altkümede de en yüksek sistemdir (88,81 / 92,32).

Özgün metinle ve izli eğitilen 256 alt sözcüklü modeller (LEN256, L256H, B128KL) iz altkümesinde yaklaşık 93,2-93,8 verir. Tam eğitim kümesiyle eğitilen 128 alt sözcüklü modeller (B0, B0R, HBEST, B128K, ELECTRA, XLMRL) yaklaşık 91,1-92,2 verir (türetilmiş). Olası açıklama, uzun yorumların sonundaki puan izinin 128 sınırında kesilmesidir; izlerin konumu ölçülmediği için bu bir hipotezdir.

KOKEK'in iz altkümesindeki düşük değeri (yaklaşık 88,3) aynı hipotezle uyumludur, çünkü kök-ek girdisi metni uzatır. 256'da (KOKEK256) değer yaklaşık 91,2'ye çıkar; metni kısaltan KOK ise B0 düzeyindedir (yaklaşık 91,3). Bu değerlerin binom belirsizliği (≈1,1 puan) farkların büyüklüğüne yakındır.

Uzunluk kazancının bir kısmı izli örneklerden gelir. LEN256 iz altkümesinde B0'dan yaklaşık 2,0, temiz altkümede 0,46 puan yüksektir (türetilmiş). Kabaca, tam test kazancının (+0,58, eşleştirilmemiş) yaklaşık 0,15 puanı izli örneklerdendir (türetilmiş: 675 / 8.934 × 2,04).

Temiz altkümedeki +0,46'lık kazanç kalır; B128KL'de bu değer +0,60'tır (türetilmiş). Uzunluk etkisi yalnızca bir iz artefaktı değildir. Bu ayrıştırma GA'sız, kaba bir kestirimdir.

LLM bayrak oranı izli eğitim örneklerinde %5,43, izsizlerde %10,44'tür. LLM puanı okuyarak etikete uyuyor olabilir ya da izli yorumlar daha açık bir dil kullanıyor olabilir. Bu iki açıklama mevcut verilerle ayrıştırılamaz.

### 7.5 Seed dayanıklılığı ve iki çıkarım yönteminin uyumu

Eğitim koşularında seed ortalamasından en büyük sapma test doğruluğunda 0,45 puandır (e-ticaret ELECTRA). Çöken koşu yoktur: en düşük koşular TrSST-2'de 86,34 (LDROP, seed 124), e-ticarette 66,75'tir (MORPH, seed 21). İkisi de çoğunluk tabanlarının (%70,96 ve %30,27) çok üzerindedir.

Eğitim deterministiktir: ORK ve HBEST'in on seed çifti bit düzeyinde aynı olasılıkları üretir (Bölüm 7.1). XLMRL'nin seed 4 ve 21 koşuları aynı test doğruluğu ve F1'ini verir (88,001 / 91,712). Koşular kopya değildir: test tahmin uyumu %95,95, validation doğrulukları 88,007 ve 88,186'dır.

Topluluk kazancı yapılandırmaya göre değişir. TrSST-2'de topluluk ile tek model ortalaması arasındaki fark B0'da +0,12, B128K'de +0,34, ELECTRA'da +0,30'dur (türetilmiş). E-ticaret ELECTRA (ss 0,37) toplulukta 68,43'ten 69,43'e çıkar (+1,00), ama yine B0 topluluğundan ayrışmaz.

Seed düzeyi t ile topluluk bootstrap'i doğrulukta beş durumda farklı sonuç verir: TrSST-2'de XLMRL, ELECTRA, STRIP ve LWEIGHT, e-ticarette B128K. ELECTRA (−0,22 \[−0,43; −0,02\] ve −0,04 \[−0,51; +0,45\]), STRIP (−0,37 \[−0,64; −0,09\] ve −0,24 \[−0,48; +0,04\]) ve LWEIGHT (−0,34 \[−0,56; −0,11\] ve −0,08 \[−0,45; +0,29\]) seed düzeyinde anlamlı, toplulukta anlamsızdır. Aşağıdaki iki durumda ise ayrışma iki yönde de görülür. TrSST-2 XLMRL seed düzeyinde anlamsız (+0,41 \[−0,19; +1,01\]), toplulukta anlamlıdır (+0,55 \[+0,04; +1,06\]). E-ticaret B128K ise seed düzeyinde anlamlı (+0,42 \[+0,20; +0,64\]), toplulukta anlamsızdır (+0,41 \[−0,06; +0,93\]).

Bu ayrışma beklenen bir durumdur: seed düzeyi t eğitim rastgeleliğini, bootstrap ise test örneklemini ölçer (Bölüm 5.4). Alt sınırı sıfıra yakın bu iki bulguyu kırılgan sayıyoruz. Ürün aktarımındaki tek aykırı seed FSBASE'tedir (Bölüm 7.6).

### 7.6 Film → ürün aktarımı (AS4)

**Tablo 19.** Ürün testi (n = 23.517). Tek model sütunları seed ortalaması ± ss, son sütun seed olasılık ortalamasıyla kurulan topluluktur. Çoğunluk sınıfı satırı veri istatistiğinden türetilmiştir.

| Sistem | Açıklama | Seed | Acc | F1 (olumlu) | Makro F1 | Topluluk acc / makro F1 |
| --- | --- | --: | --: | --: | --: | --- |
| Çoğunluk sınıfı (türetilmiş) | her şeye "olumlu" | – | 93,67 | 96,73 | 48,37 | – |
| Sıfır atış | TrSST-2 B0, ürün verisi görmeden | 5 | 93,62 ± 0,26 | 96,56 ± 0,15 | 76,60 ± 0,37 | 93,94 / 77,33 |
| FSBASE | 1.000 ürün örneği, BERTurk'ten | 5 | 95,14 ± 0,42 | 97,43 ± 0,22 | 76,68 ± 2,43 | 95,67 / 78,48 |
| FSFILM | 1.000 ürün örneği, TrSST-2 B0'dan | 5 | 95,70 ± 0,19 | 97,72 ± 0,10 | 80,15 ± 0,90 | 96,17 / 81,75 |
| FULL | 188.132 örnek, TrGLUE ayarı | 3 | 96,92 ± 0,04 | 98,36 ± 0,02 | 86,00 ± 0,18 | 97,02 / 86,47 |
| LLM | 3 şablon ortalaması, sıfır atış | – | 88,94 | 93,77 | 72,17 | – |

Sıfır atışlı aktarımda doğruluk (93,62) çoğunluk tabanının (93,67) altındadır; doğruluk burada bilgisizdir. Makro F1 ise 76,60'tır ve tabanın (48,37) çok üzerindedir. Film modeli olumsuz ürün yorumlarını kısmen ayırt eder (olumsuz sınıf F1'i yaklaşık 56,6; türetilmiş: 2 × 76,60 − 96,56). Doğruluğun tabanın altında kalması, modelin bir kısım olumlu yorumu olumsuz sınıfa atadığını düşündürür.

1.000 ürün örneğiyle BERTurk'ten eğitilen FSBASE, makro F1'de sıfır atıştan ayırt edilemez (76,68 ± 2,43'e karşı 76,60 ± 0,37). Aynı örneklerle film modelinden başlayan FSFILM 80,15 ± 0,90'a çıkar. Eşleştirilmiş fark +3,47 \[+0,44; +6,50\]'dir ve GA'nın alt sınırı sıfıra yakındır.

Bu fark kırılgandır. Seed başına farklar +3,77, +3,05, +3,59, +0,05 ve +6,90'dır; sonuncusu FSBASE'in seed 124'teki aykırı değerinden (72,55) gelir. Bu seed çıkarıldığında ortalama fark +2,62'ye iner (türetilmiş); bu durum için GA hesaplanmamıştır.

Tam ürün eğitimi (FULL) 86,00 ± 0,18 makro F1 ile üst sınırı verir. FSFILM, sıfır atış ile FULL arasındaki 9,40 puanlık açığın yaklaşık %38'ini kapatır (türetilmiş: 3,55 / 9,40). Film bilgisi aktarılır, ama 1.000 örnek tam veri setinin yerini tutmaz \[19\]\[20\].

**Tablo 20.** TrSST-2 topluluklarının ürün testinde sıfır atışlı başarımı. Son sütun B0 topluluğuna göre makro F1 farkıdır (türetilmiş, bootstrap yok).

| Kaynak (TrSST-2) | Model | TrSST-2 test acc | Ürün acc | Ürün makro F1 | Makro F1 − B0 |
| --- | --: | --: | --: | --: | --: |
| B0 | 5 | 87,87 | 93,94 | 77,33 | – |
| HBEST (= ORK) | 5 | 88,12 | 93,54 | 76,59 | −0,75 |
| LEN256 | 5 | 88,59 | 93,59 | 76,75 | −0,58 |
| L256H | 5 | 88,62 | 93,75 | 77,01 | −0,33 |
| B128K | 5 | 88,52 | 94,21 | 77,32 | −0,01 |
| B128KL | 3 | 88,75 | 93,87 | 77,25 | −0,08 |
| ELECTRA | 5 | 87,82 | 93,92 | 77,17 | −0,17 |
| XLMRL | 3 | 88,42 | 95,25 | 80,13 | +2,80 |
| ORK2 | 11 | 89,20 | 94,91 | 79,68 | +2,34 |
| B0 + LLM | 5 + LLM | 88,19 | 93,77 | 78,72 | +1,38 |
| ORK2 + LLM | 11 + LLM | 88,93 | 94,83 | 80,94 | +3,61 |

TrSST-2'deki tek model kazançları ürün tarafına taşınmaz: uzunluk ve sözlük kazançları ürün testinde B0 topluluğunun makro F1'ini geçmez. Ürün yorumları kısa olduğu için (p95 = 85 alt sözcük, %1,4'ü 128'i aşar) bağlam uzunluğunun burada rol oynaması beklenmez. Film içi sıralama ürün sıralamasını öngörmez; bu, alan kayması bulgularıyla uyumludur \[19\].

İstisna XLMRL topluluğudur: 80,13, yani 1.000 örnekli FSFILM ortalamasıyla (80,15) aynı düzey. XLM-R'ın çok dilli ön eğitimi alan kaymasına daha dayanıklı olabilir \[27\]; ancak bu, 3 seed'li tek bir gözleme dayanır ve sınanmamıştır. ORK2 XLMRL'yi içerdiği için 79,68'e ulaşır; LLM tek başına 72,17 ile en zayıf sıfır atışlı sistemdir.

AS4'ün yanıtı kısmen olumludur: aktarım vardır ve 1.000 örnekli ince ayarda film başlangıcı yarar sağlar. Ancak etkinin büyüklüğü tek bir aykırı seed'e duyarlıdır. Üst sınırla aradaki boşluk da belirgindir.

## 8. Tartışma ve Sınırlılıklar

### 8.1 Kazançlar nereden geliyor?

B0'a karşı sağlam kazançların ortak paydası, modelin gördüğü bilgiyi artıran değişikliklerdir. Uzun bağlam (LEN256), büyük sözlük (B128K) ve e-ticarette büyük kodlayıcı (XLMRL) bu gruptadır. Girdiyi dönüştüren ya da eğitim verisini azaltan müdahaleler (KOK, KOKEK, MORPH, STRIP, LDROP) ise tam testte B0'ın altında kalır.

Bu tablo Türkçe morfolojinin önemsiz olduğu anlamına gelmez. BERTurk'ün WordPiece sözlüğü ek bilgisinin önemli kısmını zaten kodluyor olabilir. Önişleme ise buna çözümleme hatası, olumsuzluk ekinin kaybı ve uzunluk artışı ekler.

Topluluk seçimi en büyük sayıyı verir, ama hesap maliyetiyle birlikte okunmalıdır. ORK2'nin B0 topluluğuna +1,33'lük farkı, yaklaşık 5,6 kat eğitim süresi ve 11 modelli çıkarım karşılığında gelir (türetilmiş). Eşit hesaplı bir taban, örneğin 11 BERTurk modelinden oluşan bir topluluk, sınanmamıştır \[24\]\[28\].

LLM'in rolü bağlama bağlıdır. Sıfır atışlı 8B yargıç, ince ayarlı BERTurk topluluğunun yaklaşık 9,5 puan gerisindedir ve etiket temizliğinde zarar verir. Füzyonda ise zayıf tabanlarda ve alan dışı ürün verisinde yalnızca küçük sayısal artışlar görülür; bunlar B0 topluluğuna karşı anlamlı değildir (+0,32 \[−0,04; +0,67\]) ya da sınanmamıştır; Gilardi vd.'nin olumlu bulgusu \[14\] farklı bir model ve görev düzenine aittir.

Puan izleri gerçek bir artefaktır, ancak ana karşılaştırmaları bozmaz. İzli örneklerde B0 yaklaşık 91,2, izsiz eğitilen STRIP yaklaşık 87,3 alır (türetilmiş, n = 675). Bu örnekler testin %7,6'sı olduğundan sıralamanın üst kısmı temiz altkümede korunur; gelecekteki TrSST-2 raporlarının temiz altküme sonucunu da vermesini öneriyoruz.

### 8.2 Rakip karşılaştırmasının anlamı

Rakip değerler tek sayıdır; seed sayısı, ss ve tahmin vektörü yayımlanmamıştır \[1\]. Bu yüzden rakibe karşı her fark betimseldir. B0'ımız TrSST-2'de yayımlanmış değeri küçük bir payla, e-ticarette 2,65 puanla geçer.

TrSST-2'deki küçük pay, ince ayar sonuçlarının olağan değişkenliği içinde kalır \[22\]. E-ticaretteki pay ise açıklanamamıştır, çünkü kaynakta SentiTurca ince ayar ayarları verilmez \[1\]. Bu nedenle e-ticarette "rakibi aştık" ifadesi yöntem katkısı olarak kurulamaz; kurulabilen iddia "B0'ı aştık" iddiasıdır.

Sonradan eklenen SentiTurca hate görevinde tablo farklıdır. Rakibin makro F1'i (58,00) B0 tekrarımızla zaten aşılır (59,83), ama mimari ve topluluk bileşenlerinin hiçbiri B0'a karşı topluluk düzeyinde anlamlı kazanç vermez. Mimari ve protokol değişiklikleri arasında seed düzeyinde anlamlı tek kazanç rakibin kendi README ayarından (5 epoch) gelir. Rakibin %61 doğruluğu çoğunluk sınıfı tabanının altında olduğundan bu görevde yayımlanmış doğruluk karşılaştırma için kullanılamaz. Bu sonuç, TrSST-2 ve e-ticaretteki kazancın dengesiz ve dört sınıflı bir göreve taşınmadığını gösteren olumsuz bir bulgu olarak raporlanır. Bu görevde kazancı sınıf dengesizliğine yönelik yöntemler sağlar: validation'da seçilen eşik ayarıyla seçilen sistem 69,08 makro F1 alır, ama kazancın büyük kısmı mimariden değil eşik ayarından gelir (Bölüm 6.7). Bu ek analiz sonuçlar görüldükten sonra planlandığı için başka bir dengesiz veri setinde doğrulanmalıdır.

### 8.3 Sınırlılıklar ve geçerlilik tehditleri

- **Uyarlamalı tasarım.** Kuyruk altı geçişte büyüdü. B0R, LEN256, B128K, ELECTRA ve XLMRL ikinci geçişte, L256H, B128KL ve KOKEK256 sonraki geçişlerde, test sütunlarını da içeren tablolar görüldükten sonra eklendi. B128KL "iki olumlu değişikliğin birleşimi" gerekçesiyle eklendi (`run_queue.py`); seçimler validation'da yapılsa da aday havuzu önceden kayıtlı değildir. Hate görevi beşinci, sınıf dengesizliği analizi (WCE, WCES, eşik ayarı) altıncı geçişte, medeni sınıfının F1'inin 0 olduğu görüldükten sonra eklendi. Bu gözlem test topluluğundan geldi; kaydırmalar validation'da seçilse de yöntem ailesi ve ızgara bu gözleme yanıt olarak belirlendi. Bu nedenle Bölüm 6.7'deki kazançlar keşifseldir. Hate split'leri başlık düzeyinde ayrılmadığı için (Bölüm 3.7) hate sonuçları görülmemiş konulara genellemeyi ölçmez.
- **İyimser yan.** Bu nedenle B128KL ve ORK2 sayıları iyimser yanlı olabilir ve doğrulayıcı değil keşifsel bulgulardır. ORK2'nin bileşimi geçişten geçişe değişmiştir (ikinci geçişte LEN256 + XLMRL + B128K, 88,96 / 92,40). Hiç dokunulmamış bir ek test kümesi yoktur.
- **Tek seed'li ızgara.** Izgaralar yalnızca seed 42 ile koşuldu. TrSST-2'de ayarlar arası fark (0,68 puan) seed gürültüsü düzeyindedir; HBEST'in başarısızlığı aramanın değil, tek seed'le seçimin güvenilmezliğini gösterir \[22\].
- **Eşit olmayan seed sayısı.** XLMRL, B128KL, KOKEK256 ve FULL 3 seed (1, 4, 21), diğerleri 5 seed (1, 4, 21, 40, 124) ile koşuldu. n = 3'te t kritik değeri yaklaşık 4,30'dur ve ss kestirimi kararsızdır; B128KL'nin 0,02'lik ss'si bu nedenle temkinle okunmalıdır.
- **İki çıkarımın kapsamı.** Bootstrap yalnızca test örneklemi belirsizliğini, eşleştirilmiş t yalnızca seed belirsizliğini yansıtır. Topluluk farkları, ORK2 dahil, seed varyansına karşı sınanmamıştır; ürün testinde bootstrap yapılmamıştır. TrSST-2'de XLMRL, ELECTRA, LWEIGHT ve STRIP'te, e-ticarette B128K'de, hate'te B0E5'te (makro F1) iki çıkarım farklı sonuç verir.
- **Çoklu karşılaştırma.** Bootstrap tablosu 38 satır içerir; ORK'nin iki satırı ve ORK + LLM, HBEST satırlarının kopyası olduğundan 35 ayrık karşılaştırma kalır (türetilmiş sayım); hate eşik ayarı tablosu 8 bootstrap ve 8 t karşılaştırması daha ekler. Eşleştirilmiş t'deki 30 satırın 28'i ayrıktır ve iki metrikle raporlanmıştır; düzeltme uygulanmadı.
- **Beklenen yanlış pozitifler.** Tüm gerçek farklar sıfır olsaydı, 35 bootstrap karşılaştırmasında %5 düzeyinde yaklaşık 1,75 yanlış pozitif (eşik ayarı karşılaştırmalarıyla 43 karşılaştırmada yaklaşık 2,15) beklenirdi (türetilmiş). En kırılgan bulgular XLMRL TrSST-2 bootstrap'i (alt sınır +0,04), ELECTRA TrSST-2 (üst sınır −0,02) ve FSFILM − FSBASE'tür (alt sınır +0,44).
- **Hesap eşitsizliği.** ORK2 11, B0 topluluğu 5 model kullanır; XLM-R large koşusu ortalama 41,6 dakika, B0 koşusu 9,9 dakikadır. Kazanç birim hesap başına raporlanmamıştır.
- **Başarısız işler.** B128KL'nin üç ilk denemesi 1,0 dakikada rc=1 ile düştü. Bellek yetersizliği bildirildi, ama günlükler yeniden koşuyla üzerine yazıldığı için neden dosyadan doğrulanamıyor. llm\_vote\_trsst2'nin ilk denemesi 225,5 dakika sonra düştü; ikinci deneme önbellekteki skorları kullanıp yalnız testi skorladı (27,2 dakika). hate\_WCES\_s40'ın ilk denemesi 2026-10-15'te kuyruk süreci hata kaydı bırakmadan sonlandığı için yarım kaldı (günlüğün son yazımı başlangıçtan yaklaşık 1 dakika sonra) ve ertesi gün baştan koşuldu.
- **Kod değişikliği.** `grad_accum` parametresi `train.py`'ye deney ortasında eklendi; diğer 173 koşuda varsayılan 1 geçerlidir. B128KL'de 64 × 2 ile etkin toplu boyut 128 korundu, ama fp16 altında biriktirme tek toplu işle sayısal olarak özdeş değildir. Bölüm 5.5'teki "toplu boyut 128 gradyan biriktirmeden sığar" ifadesi yalnızca 128 alt sözcüklü koşular için geçerlidir. Sınıf ağırlığı parametresi (--class\_weight) altıncı geçiş için eklendi; varsayılan değeri (none) önceki koşuları etkilemez.
- **Yardımcı iş.** data\_stats işi kuyrukta ValueError ile düştü ve `data_stats.md` kuyruk dışında üretildi.
- **Metrik varsayımı.** Rakip F1'inin ortalama türü kaynakta belirtilmez \[1\]. Olumlu sınıf F1 varsayımı yanlışsa F1 karşılaştırması geçersiz olur (Bölüm 5.3).
- **E-ticaret protokolü.** Kaynak SentiTurca ayarlarını vermediği için e-ticarette TrSST-2 ayarı uygulandı \[1\]. B0'ın yayımlanmış değerden +2,65 puan yüksek olmasının nedeni belirlenemedi.
- **Puan izi tanımı.** `RATING_RE` elle doğrulanmadı ve yazıyla verilen puanları yakalamaz. İz altkümesi değerleri 675 örnekten türetilmiş, GA'sız kaba kestirimlerdir; izlerin metin içindeki konumu ölçülmemiştir.
- **LLM bayrakları ve sızıntı.** Bayraklar elle denetlenmedi; gerçek etiket hatası ile LLM hatası ayrılamaz. LLM'in ön eğitim verisinde bu yorumların bulunup bulunmadığı bilinmiyor; eğitimdeki 79,71 ile testteki 78,41 arasındaki küçük fark sızıntı lehine ya da aleyhine kanıt değildir.
- **Alan ve yargıç kapsamı.** Film (TrSST-2) ve ürün etiketleri farklı kurallarla üretilmiştir; sıfır atış farkının bir kısmı etiket tanımı uyumsuzluğundan gelebilir. Bayraklar tek bir LLM'den (8B, 4 bit) gelir ve başka bir yargıçla tekrarlanmamıştır.
- **Ürün verisi.** Ürün bölmesi bize aittir ve bu veri için yayımlanmış rakip sonuç yoktur. Veri kartı kaynak siteyi belirtmez \[25\].
- **Donanım.** Tüm deneyler tek RTX A4000 üzerinde, Windows 11 altında koşuldu. Belirlenimcilik başka donanımda sınanmadı.

### 8.4 Öneriler

Bu sınırlılıklar üç somut adım önerir. Aday havuzu ve seçim kuralı önceden kaydedilmeli, hiç dokunulmamış bir test kümesi ayrılmalıdır. Topluluk farkları, seed ve örneklem belirsizliğini birlikte içeren bir şemayla ve eşit hesaplı bir tabana karşı sınanmalıdır.

KOKEK için "+" işaretsiz ve LEN256 ile eşit seed'li bir denetim, morfoloji etkisini gösterim biçiminden ayırmalıdır. Puan izlerinin metin içindeki konumu ölçülerek uzunluk kazancının iz payı doğrudan kestirilebilir. Bayraklı örneklerin elle denetimi ise LLM bayraklarının kesinliğini belirlemek için gereklidir.

## 9. Sonuç

Bu çalışma TrGLUE'nun BERTurk sonuçlarını beş seed'le yeniden üretti ve bileşenleri aynı protokol ve seed'lerde B0'a karşı ölçtü. Deneyler 179 ince ayar koşusu (36'sı sonradan eklenen hate görevinde) ve 12 LLM skorlama geçişi içerdi; toplam kuyruk duvar saati tek RTX A4000 üzerinde 2.526,5 dakikadır. Araştırma sorularına yanıtlarımız aşağıdadır.

- **AS1.** TrGLUE Tablo 7 değerlerine ulaşılıyor ve aşılıyor: B0 TrSST-2'de 87,75 ± 0,12 / 91,54 ± 0,09 alır ve beş seed'in her biri 87,4 / 91,3'ün üzerindedir. Hiperparametre araması B0'ı geçmez (+0,09 \[−0,26; +0,44\]). Seed topluluğu tek başına yalnızca +0,12 ekler (türetilmiş) ve HBEST topluluğu B0 topluluğundan ayırt edilemez (+0,26 \[−0,04; +0,56\]). Daha güçlü kodlayıcı ve uzunluk seçenekleri ile topluluklar ise aşar: B128KL 88,50 / 92,05 (+0,79 \[+0,45; +1,12\]), ORK2 89,20 / 92,56 (B0 topluluğuna göre +1,33 \[+0,96; +1,72\]).
- **AS2.** Hayır. KOK ve KOKEK B0'ın 1,20 puan, e-ticarette MORPH 1,87 puan gerisindedir. KOKEK256 kaybın çoğunun kesmeden geldiğini gösterir (−0,19 \[−0,95; +0,58\]), ama eşit uzunlukta da LEN256'nın 0,81 puan gerisinde kalır (türetilmiş).
- **AS3.** Temizlik için hayır, füzyon için sınırlı. LDROP −1,20, LWEIGHT −0,34 kayıp verir. Füzyon B0 topluluğuna anlamlı katkı yapmaz (+0,32 \[−0,04; +0,67\]) ve en iyi sistemi kötüleştirir (88,93 < 89,20); yalnız ürün testinde makro F1'i +0,65 ile +1,44 arasında artırır (türetilmiş, sınanmamış).
- **AS4.** Kısmen. Sıfır atışta makro F1 76,60'tır, doğruluk ise çoğunluk tabanının altındadır. 1.000 örnekte film başlangıcı makro F1'i +3,47 \[+0,44; +6,50\] artırır; etki tek bir aykırı seed'e duyarlıdır ve tam veriyle eğitilen modelin (86,00) belirgin biçimde gerisinde kalınır.
- **AS5.** Etki küçüktür ve üst sıralama değişmez. B0'da tam ve temiz test farkı 0,28 puandır (türetilmiş). STRIP'in −0,37'lik kaybı neredeyse tamamen izli örneklerden gelir; temiz altkümede B128KL ve LEN256 ilk iki sırada kalır.

**Başlıktaki "aşılması" iddiası.** Rakibin yayımlanmış değerlerine karşı iddia destekleniyor. TrSST-2'de en iyi tek model B128KL 88,50 / 92,05, ORK2 89,20 / 92,56; e-ticarette XLMRL 69,45 / 67,30, ORK2 69,89 / 67,72 alır. Ancak bu üstünlüğün bir kısmı (TrSST-2'de +0,35, e-ticarette +2,65 doğruluk) yeniden üretilen B0'da zaten vardır.

Kendi B0'ımıza karşı da iddia GA'larla destekleniyor, ama daha dar bir kapsamda. TrSST-2'de B128KL +0,79 \[+0,45; +1,12\] (3 seed), LEN256 +0,59 \[+0,30; +0,88\] (5 seed), ORK2 topluluğu bootstrap'te +1,33 \[+0,96; +1,72\] verir. E-ticarette XLMRL +0,72 \[+0,43; +1,01\], ORK2 +0,80 \[+0,41; +1,19\] verir.

Üç çekince bu iddianın kapsamını daraltır. Tasarlanan orkestra birleşimi (ORK) hiçbir bileşen seçmedi ve HBEST ile aynı kaldı; kazançlar uzunluk, sözlük, kodlayıcı ve sonradan eklenen topluluk seçiminden geldi. ORK2 eşit olmayan hesapla ve uyarlamalı bir aday havuzuyla elde edildi; ürün yorumu verisinde ise yayımlanmış bir rakip yoktur.

Bu nedenle başlıktaki "aşılması" ifadesi iki ölçüte göre okunmalıdır: rakibin yayımlanmış değerleri her iki görevde tek modelle de aşılır; eşleştirilmiş B0'ın aşılması ise uzunluk, sözlük, kodlayıcı ve topluluk seçiminden gelir, morfoloji ya da LLM temizliğinden gelmez.

Uygulayıcılar için ilk denenecek müdahaleler, alt sözcük sınırını metin uzunluğuna göre artırmak ve daha büyük sözlüklü BERTurk kullanmaktır. Morfolojik önişleme ve LLM ile eğitim etiketi ayıklama, en azından burada sınanan biçimleriyle, önerilmez. TrSST-2 sonuçları temiz altkümeyle birlikte raporlanmalıdır.

Sonradan eklenen SentiTurca hate görevi bu tabloyu sınırlar. Rakibin makro F1'i B0 tekrarıyla aşılır, ama uzunluk, sözlük, kodlayıcı ve topluluk seçimi B0'a karşı anlamlı kazanç vermez. Bu görevde makro F1'i en çok azınlık sınıfları, özellikle neredeyse hiç tahmin edilmeyen medeni sınıfı sınırlar; sınıf dengesizliğine yönelik yöntemler bu sınırı kısmen aşar. Validation'a göre seçilen eşik ayarlı B128KL topluluğu 69,08 makro F1 ve 38,3 medeni F1 alır; karekök sınıf ağırlığı benzer bir kazanç verir, ama bu deneyde ikisinin birleşimi ek kazanç sağlamadı. Bu kazançların bedeli yaklaşık 1–4 puanlık doğruluk kaybıdır ve sonuçlar keşifseldir.

## Kaynaklar

1. Altinok, D. (2025). Introducing TrGLUE and SentiTurca: A Comprehensive Benchmark for Turkish General Language Understanding and Sentiment Analysis. arXiv:2512.22100. [arXiv](https://arxiv.org/abs/2512.22100)
2. Wang, A., Singh, A., Michael, J., Hill, F., Levy, O., Bowman, S. R. (2018). GLUE: A multi-task benchmark and analysis platform for natural language understanding. EMNLP BlackboxNLP Workshop.
3. Socher, R. vd. (2013). Recursive deep models for semantic compositionality over a sentiment treebank. EMNLP.
4. Devlin, J., Chang, M.-W., Lee, K., Toutanova, K. (2019). BERT: Pre-training of deep bidirectional transformers for language understanding. NAACL.
5. Schweter, S. (2020). BERTurk: BERT models for Turkish. Zenodo. [model kartı](https://huggingface.co/dbmdz/bert-base-turkish-cased)
6. Wolf, T. vd. (2020). Transformers: State-of-the-art natural language processing. EMNLP System Demonstrations.
7. Akın, A. A., Akın, M. D. (2007). Zemberek, an open source NLP framework for Turkic languages.
8. zeyrek: Zemberek'in Python uyarlaması. [GitHub](https://github.com/obulat/zeyrek)
9. Kesgin, H. T. vd. (2024). ytu-ce-cosmos/Turkish-Llama-8b-Instruct-v0.1 model kartı. [Hugging Face](https://huggingface.co/ytu-ce-cosmos/Turkish-Llama-8b-Instruct-v0.1)
10. Grattafiori, A. vd. (2024). The Llama 3 herd of models. arXiv:2407.21783.
11. Dettmers, T., Pagnoni, A., Holtzman, A., Zettlemoyer, L. (2023). QLoRA: Efficient finetuning of quantized LLMs. NeurIPS.
12. Northcutt, C., Jiang, L., Chuang, I. (2021). Confident learning: Estimating uncertainty in dataset labels. JAIR.
13. Frénay, B., Verleysen, M. (2014). Classification in the presence of label noise: A survey. IEEE TNNLS.
14. Gilardi, F., Alizadeh, M., Kubli, M. (2023). ChatGPT outperforms crowd workers for text-annotation tasks. PNAS.
15. Toraman, C., Yilmaz, E. H., Şahinüç, F., Ozcelik, O. (2023). Impact of tokenization on language models: An analysis for Turkish. ACM TALLIP.
16. Rust, P., Pfeiffer, J., Vulić, I., Ruder, S., Gurevych, I. (2021). How good is your tokenizer? On the monolingual performance of multilingual language models. ACL.
17. Safaya, A., Kurfalı, E., Yuret, D. (2022). Mukayese: Turkish NLP strikes back. Findings of ACL.
18. Demirtas, E., Pechenizkiy, M. (2013). Cross-lingual polarity detection with machine translation. WISDOM.
19. Blitzer, J., Dredze, M., Pereira, F. (2007). Biographies, Bollywood, boom-boxes and blenders: Domain adaptation for sentiment classification. ACL.
20. Gururangan, S. vd. (2020). Don't stop pretraining: Adapt language models to domains and tasks. ACL.
21. Gururangan, S. vd. (2018). Annotation artifacts in natural language inference data. NAACL.
22. Dodge, J. vd. (2020). Fine-tuning pretrained language models: Weight initializations, data orders, and early stopping. arXiv:2002.06305.
23. Berg-Kirkpatrick, T., Burkett, D., Klein, D. (2012). An empirical investigation of statistical significance in NLP. EMNLP-CoNLL.
24. Lakshminarayanan, B., Pritzel, A., Blundell, C. (2017). Simple and scalable predictive uncertainty estimation using deep ensembles. NeurIPS.
25. fthbrmnby/turkish\_product\_reviews veri kartı. [Hugging Face](https://huggingface.co/datasets/fthbrmnby/turkish_product_reviews)
26. Clark, K., Luong, M.-T., Le, Q. V., Manning, C. D. (2020). ELECTRA: Pre-training text encoders as discriminators rather than generators. ICLR.
27. Conneau, A. vd. (2020). Unsupervised cross-lingual representation learning at scale. ACL.
28. Caruana, R., Niculescu-Mizil, A., Crew, G., Ksikes, A. (2004). Ensemble selection from libraries of models. ICML.

Veri setleri: [TrGLUE](https://huggingface.co/datasets/turkish-nlp-suite/TrGLUE), [SentiTurca](https://huggingface.co/datasets/turkish-nlp-suite/SentiTurca). Rakip protokol: [TrGLUE deposu](https://github.com/turkish-nlp-suite/TrGLUE).
