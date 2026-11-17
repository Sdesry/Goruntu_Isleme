# Klasik Görüntü İşleme ile Hücre Sayımı — Fluo-N2DH-GOWT1

Yüksek lisans ara sınav projesi: klasik görüntü işleme yöntemleriyle mikroskobik görüntülerde hücre segmentasyonu, sayımı ve zamansal analiz.

**Veri seti:** [Cell Tracking Challenge — 2D datasets](https://celltrackingchallenge.net/2d-datasets/)  
**Eğitim arşivi:** https://data.celltrackingchallenge.net/training-datasets/Fluo-N2DH-GOWT1.zip  
**Rastgele seçim seed:** `31040`

## Çalışma düzeni

- **Aşama 1:** `01` sekansından 10 geliştirme ve 5 doğrulama karesi; `02` sekansından üç ayrı beşli blok (15 test karesi). Baseline: ham gri TIFF + Otsu + bağlı bileşen analizi.
- **Aşama 2:** Klasik yoğunluk dönüşümü, filtre, eşikleme ve morfoloji deneyleri. 22 kontrollü konfigürasyondan 3 aday pipeline doğrulamada karşılaştırıldı.
- **Önceden seçilen aday E18:** CLAHE → Gaussian 3×3 → Otsu → Opening 3×3 → Closing 3×3 → bağlı bileşen analizi (min. alan 20). Aday seçimi test referanslarına bakılmadan yapıldı; doğrulama MAE: 6.0.
- **Aşama 3A:** Final parametreleri ve 15 test tahminini **test referansları okunmadan** üret; `final_pipeline.json`, `test_predictions_locked.csv` ve `test_lock_manifest.json` dosyalarını **ayrı bir GitHub commit'i** olarak kaydet.
- **Aşama 3B:** Yalnızca 3A kilitleme commit'i oluşturulduktan sonra gerçek test referanslarını oku; MAE/RMSE/Bias/exact-count accuracy ve zamansal grafikleri hesapla.

## Google Colab ile çalıştırma

1. Eğitim arşivini **bilgisayarında aç**. İçindeki `01`, `02`, `01_GT/TRA` ve `02_GT/TRA` klasörlerini Google Drive'a yükle.
2. Veri setini `MyDrive/Goruntu_Isleme_Projesi/Fluo-N2DH-GOWT1/` konumuna yerleştir.
3. Notebook'ları **Aşama 1 → Aşama 2 → Aşama 3A → (kilitleme commit'i) → Aşama 3B** sırasıyla çalıştır.
4. TIFF okuma için `tifffile` ve `imagecodecs` gerekir. **Colab içinde ZIP açma kodu kullanılmaz.**
5. Sonuçların kodlarla tutarlı ve tekrarlanabilir olduğunu doğrula; son kullanılan paket sürümlerini kaydet.

## Veri güvenliği / akademik kural

Orijinal mikroskop görüntülerini, referans etiket TIFF'lerini ve bunların kopyalarını bu depoya **yükleme**. Veri setini yeniden dağıtan veya orijinal mikroskop görüntülerini içine gömen notebook çıktıları da yüklenmez; görüntü örnekleri yalnızca özel ders teslim ortamında ve raporda bulunur.

**Durum:** Depo dokümantasyonu oluşturuldu. Gerçek 3A test tahminlerinin GitHub'a kilitlenmesi henüz doğrulanmadı. Bu adım tamamlanmadan 3B değerlendirmesine geçilmemelidir.
