**Proje**

- **Ad:** Tenis takip ve kuş bakışı görselleştirme
- **Notebook:** `tenis.ipynb` (ana iş akışı: algılama, takip, segmentasyon, perspektif dönüşümü)

**Amaç**

- YOLOv8 modeli ve MobileSAM segmentasyonu ile oyuncu ve top tespiti yapmak.
- DeepSORT ile (özel olarak BoT‑SORT denenip yoksa DeepSORT kullanan bir sarmalayıcı) nesne takibi gerçekleştirmek.
- Perspektif dönüşümleri ve ölçeklemeyle gerçekçi kuş bakışı kort görselleştirmeleri üretmek (akıllı konumlandırma: yakındaki oyuncular için ayak, uzaktakiler için baş referansı).
- Örnek oluşturulan çıktı videoları: `nadal_ball_trail_only.mp4` (oluşturuldu), `nadal_full_realistic.mp4` (planlandı/üretilmesi hedefleniyor).

**Önemli Dosyalar / Yollar**

- Notebook: `tenis.ipynb`
- YOLO model ağırlığı: `kort_best.pt`
- MobileSAM kontrol noktası (örnek): `C:\Users\User\Desktop\MobileSAM\mobile_sam.pt`
- Çıktı videoları: `nadal_ball_trail_only.mp4`, `nadal_full_realistic.mp4`

**Çalışma Ortamı**

- Önerilen conda ortamı adı: `nasa` (geliştirme bu ortamda yapılmıştır)
- Gerekli (temel) Python paketleri (aktif ortamda kurun):

PowerShell komutları:

```powershell
& C:\Users\User\anaconda3\Scripts\activate; conda activate nasa
pip install ultralytics mobile-sam deep_sort_realtime opencv-python matplotlib pillow
# CPU-only PyTorch gerekiyorsa örnek:
pip install torch --index-url https://download.pytorch.org/whl/cpu
```

**Nasıl Çalıştırılır**

- `tenis.ipynb` dosyasını JupyterLab veya Jupyter Notebook ile açın ve hücreleri sırayla çalıştırın.
- Öne çıkan işleme hücreleri:
  - Tam gerçekçi kort işleme hücresi (çıkış: `nadal_full_realistic.mp4`)
  - Sadece top izi (ball‑trail) işleme hücresi (çıkış: `nadal_ball_trail_only.mp4`, bu çıktı oluşturuldu)
  - BoT‑SORT sarmalayıcı hücresi (BoT‑SORT yüklü değilse DeepSORT'a geri döner)

**Ana Parametreler (notebook içinde düzenlenebilir)**

- `BALL_THRESHOLD` — top algılama için güven eşiği (ör. `0.35`)
- YOLO `conf` değeri — model çağrısında kullanılan keşif eşik değeri (ör. `0.35`)
- Akıllı konumlandırma için `frame_mid_y` (çerçevenin yarısı) kullanılır
- Kort ölçeklemesi: gerçekçi kort hücresi yaklaşık `1 m ≈ 15 px` ve `court_width=1200`, `court_height=900` değerlerini kullanır

**Takip Seçenekleri**

- Varsayılan (çalışan): `deep_sort_realtime` (DeepSORT)
- İsteğe bağlı: BoT‑SORT — notebook içinde denenen bir sarmalayıcı var; gerçek anlamda BoT‑SORT kullanmak isterseniz ilgili BoT‑SORT paketini kurmamız gerekir (isteğiniz doğrultusunda kurulum komutunu eklerim).

**Sorun Giderme / Notlar**

- Eğer `NameError: name 'YOLO' is not defined` hatası alırsanız `ultralytics` paketinin yüklü olduğundan ve notebook başında `from ultralytics import YOLO` importunun bulunduğundan emin olun.
- MobileSAM GPU yoksa CPU üzerinde yüklenecektir; yükleme sırasında cihaz bilgisini gösteren mesajlar görebilirsiniz.
- Matplotlib ile ilgili font/glyph uyarıları genelde görsel olarak zararsızdır ve göz ardı edilebilir.

**İleri Adımlar / İyileştirme Önerileri**

- Gerçekten BoT‑SORT kullanmak isterseniz uygun BoT‑SORT paketini kurup notebook hücresini yeniden çalıştırabiliriz.
- Top yanlış pozitifleri görünmeye devam ediyorsa `BALL_THRESHOLD` ve YOLO `conf` değerleriyle kısa doğrulama çalıştırması yapıp eşikleri ayarlayabiliriz.
- İsterseniz README'yi İngilizceye çeviririm veya kurulum, katkı, lisans gibi bölümleri genişletebilirim.

**Yardım / İletişim**

- Hangi adımı yapmak istediğinizi söyleyin: `nadal_full_realistic.mp4` dosyasını doğrulamamı, BoT‑SORT'u kurup test etmemi, notebook içindeki dökümantasyon işlemini tamamlamamı veya İngilizce README eklememi ister misiniz?
