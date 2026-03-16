<div align="center">
  <img width="100%" alt="InfiniteMotion Studio Banner" src="InfiniteMotionLogo.png" />

  # InfiniteMotion Studio 2.1

  **Sahnede kamerayı ve nesneleri sanki gerçek bir fiziksel ortamdaymışsınız gibi hareket ettirebileceğiniz bir çalışma alanında video düzenlemenizi sağlayan profesyonel bir yazılım. Sonsuz çalışma alanı (infinite canvas), sanal kamera (virtual camera), deterministik donanım tabanlı işleme (Deterministic Hardware Rendering) ve gelişmiş motion graphics tasarım yetenekleri sunar.**

  [![React](https://img.shields.io/badge/React-19.2-blue.svg?style=for-the-badge&logo=react)](https://react.dev/)
  [![TypeScript](https://img.shields.io/badge/TypeScript-5.8-blue.svg?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)
  [![Vite](https://img.shields.io/badge/Vite-6.2-purple.svg?style=for-the-badge&logo=vite)](https://vitejs.dev/)
  [![Tailwind CSS](https://img.shields.io/badge/Tailwind-CSS-38B2AC.svg?style=for-the-badge&logo=tailwind-css)](https://tailwindcss.com/)
</div>

---

Tarayıcı üzerinden doğrudan video düzenleme imkanı sunan **InfiniteMotion Studio**'ya hoş geldiniz. Geleneksel çalışma alanlarının sınırlarını aşmak ve sinematik bir hareket deneyimi sunmak amacıyla profesyonel ve kapalı kaynaklı (closed-source) bir platform olarak geliştirildi. Kullanıcılar nesneleri dinamik olarak canlandırabilir (animate), sahneler arasında gezinmek için sanal bir kamerayı kontrol edebilir ve doğrudan render sürecine HTML/CSS kodları enjekte edebilir. Tüm bu işlemler yerel olarak (client-side) çalışır ve WebCodecs kullanılarak donanım hızlandırmalı (hardware-accelerated) özel bir pipeline üzerinden dışa aktarılır (export).

![Sistem Mimarisi ve Teknik İnovasyonlar](src/screenshot1.png)

## 🏗️ Sistem yöntemi ve Teknik İnovasyonlar

InfiniteMotion Studio özel (proprietary) bir kod tabanına sahip olduğu için, bu bölüm platformu ayakta tutan mühendislik kararlarına ve altyapıya genel bir bakış sunar.

### 1. Özel Canvas Render Motoru
Stüdyonun temelinde, yüksek performanslı grafik işleme ve render için optimize edilmiş, akıcı ve hızlı bir HTML5 Canvas mimarisi yatmaktadır.
*   **Matris Matematiği ile Konumlandırma:** Sahnedeki nesnelerin konumu, dönüşü (rotation) ve boyutu matris hesaplamalarına dayanır. Bu sayede kalite kaybı yaşanmadan sonsuz bir alan içinde pan ve zoom yapılabilir.
*   **Dinamik Arka Planlar:** Hareketli renk geçişleri (gradients) ve çeşitli ızgara (grid) desenleri gibi birbirinden güzel dinamik arka planlar oluşturabilirsiniz.
*   **DOM ve Canvas Entegrasyonu:** Video, resim ve metin gibi standart varlıklar (assets) doğrudan Canvas üzerine çizilirken, "kod" öğeleri (HTML, CSS, SVG) senkronize edilmiş bir DOM katmanı aracılığıyla Canvas'ın üstünde render edilir. Bu katman, sanal kameranın hareketlerini ve dönüşümlerini (transformations) birebir takip ederek CSS animasyonlarının video alanıyla kusursuz bir şekilde bütünleşmesini sağlar.

### 2. Dışa Aktarma (Export) ve Medya Senkronizasyonu

![Dışa Aktarma ve Medya Senkronizasyonu](src/screenshot2.png)

Tarayıcı üzerinden doğrudan yüksek kaliteli video çıktısı alma.
*   **WebCodecs Entegrasyonu:** Export pipeline'ı, kareleri (frames) doğrudan MP4 (H.264/AAC) veya WebM (VP9/Opus) dosyalarına yazmak için düşük seviyeli (low-level) `VideoEncoder` ve `AudioEncoder` arayüzlerini kullanır. Bu sayede video ve ses üzerinde yüksek kalite ve hassas kontrol sağlanır.
*   **Binary-Lock Kare Senkronizasyonu:** Render sırasında hiçbir karenin atlanmadığından (dropped frame) emin olmak için özel bir kilit (lock) mekanizması kullanıyoruz. Bu mekanizma, `HTMLVideoElement` öğelerinin `readyState` durumu hedeflenen kareyi belleğe tam olarak aldığını doğrulayana kadar render döngüsünü (render loop) bekletir.
*   **OfflineAudioContext ile Ses Miksajı:** Sesler sadece kaydedilmekle kalmaz; son video dosyasına (multiplexing) dahil edilmeden önce Web Audio API'nin `OfflineAudioContext` özelliği kullanılarak bellekte (Float32) tamamen mikslenir, dengelenir ve render edilir.

### 3. Akıllı State Yönetimi ve Kareler Arası Geçiş (Interpolation)
Proje state'i, performansı ve projeyi etkilemeden anında İleri/Geri (Undo/Redo) yapabilmeyi sağlayan katı ve değiştirilemez (immutable) bir History Stack üzerinden yönetilir.
*   **Otomatik Keyframe Oluşturma (Auto-Keyframing):** Motor, nesnelerin uzamsal özelliklerindeki (X, Y, Boyut, Dönüş) değişiklikleri algılar ve otomatik olarak yeni keyframe'ler oluşturur veya mevcut olanları günceller.
*   **Gelişmiş Easing (Yumuşatma) Fonksiyonları:** Keyframe'ler arası geçişler (interpolation), her kare için hassas bir şekilde hesaplanan ve `bounce-in`, `bounce-out`, `elastic` gibi gelişmiş easing efektlerini destekleyen özel matematiksel fonksiyonlarla işlenir.
*   **Kesişim Tetikleyicileri (Intersection Triggers):** Nesnelerin giriş ve çıkış animasyonları sadece zamana değil, sahneyle veya görünüm alanıyla kesişmelerine de bağlı olabilir. Bir nesnenin sanal kameranın görüş alanına ne zaman girdiği hesaplanır ve nesne durumu dinamik olarak (Mount/Unmount) ekrana yansıtılır.

## ✨ Temel Özellikler

*   🎥 **Uzamsal Düzenleme (Spatial Editing) ve Sanal Kamera:** Sonsuz bir çalışma alanı üzerinde tasarlayın. Sahne içinde gezinmek ve motion graphics tarzı sekanslar oluşturmak için sanal kamerayı hareket ettirin (Pan, Zoom, Rotate).
*   ⏱️ **Snapping Özellikli Profesyonel Timeline:** Klipler, keyframe'ler ve playhead için manyetik hizalama (snapping), kutu seçimi (Marquee Selection) ve sürükle-bırak ile kolay katman (layer) sıralama desteği sunan tam teşekküllü bir NLE (Non-Linear Editor) timeline'ı.
*   ⚡ **Hızlı Export:** WebCodecs API kullanarak sinematik sahnelerinizi doğrudan tarayıcıda oluşturun. Sesle tam senkronize, yüksek kaliteli MP4 (H.264/AAC) veya WebM (VP9/Opus) videolar dışa aktarın.
*   🎨 **Gelişmiş Stil ve Efektler:** Sahneye profesyonel dokunuşlar katmak için hareketli renk geçişlerini (gradients), buzlu cam (frosted glass) efektlerini, gölgeleri (drop shadow), dış çizgileri (stroke) ve gelişmiş maskeleme (masking) seçeneklerini kullanın.
*   💻 **Doğrudan Kod Enjeksiyonu:** Render sürecine doğrudan HTML, CSS ve SVG eklemek için "Kod" varlıklarını kullanın ve sınırsız görsel olasılığın kapılarını aralayın.

![Temel Özellikler](src/screenshot3.gif)

## 🌌 Arka Planlar, Izgaralar (Grid) ve Dokular (Texture)

InfiniteMotion Studio, projenizin görsel temeli üzerinde derinlemesine özelleştirme imkanı sunarak, sahneye hiçbir varlık eklemeden önce zengin ve detaylı kompozisyonlar oluşturmanıza olanak tanır.

*   **Dinamik Arka Plan Türleri:** Düz renkler, doğrusal (linear) geçişler, radyal geçişler, buzlu cam (glassmorphism) veya 8 farklı renge kadar harmanlanabilen, hız ve açı kontrolüne sahip gelişmiş **Animasyonlu Mesh Gradient** arasından seçim yapın.
*   **Editör Izgaraları (Editor Grids):** Uzamsal hizalamaya (spatial alignment) yardımcı olması için çalışma alanı ızgarasını aktif edin. *Çizgiler (Lines), Noktalar (Dots), Artılar (Crosses)* veya *İzometrik (Isometric)* stillerinden birini seçin. Izgara rengini özelleştirebilir ve son export edilen videoda görünüp görünmeyeceğine karar verebilirsiniz.
*   **Kaplamalar ve Dokular (Overlays & Textures):** Ayarlanabilir **Film Greni / Kumlanma (Film Grain / Noise)** ve *Mikro Noktalar, Teknik Izgaralar* veya *CRT Tarama Çizgileri (Scanlines)* gibi yapısal kaplamalarla sinematik bir dokunuş ekleyin. Klasik VHS efektlerinden şık ve modern teknolojik arayüzlere (HUD) kadar istediğiniz hissi yaratmak için bu desenlerin opaklığını ayarlayın.

![Arka Planlar, Izgaralar ve Dokular](src/screenshot4.png)

## 🔤 Tipografi ve Gelişmiş Dil Desteği (Arapça/RTL)

Dünyanın dört bir yanındaki içerik üreticileri için tasarlanan InfiniteMotion Studio, karmaşık metin şekillendirmeyi, Sağdan Sola (RTL) dilleri ve şık tipografiyi destekleyen güçlü bir metin render motoru içerir.

*   **Premium Standart Fontlar:** *Inter, Roboto, Open Sans, Lato, Montserrat, Poppins, Oswald, Playfair Display, Merriweather, Nunito* ve *Raleway* gibi özenle seçilmiş modern font koleksiyonuyla birlikte gelir.
*   **Doğal Arapça (RTL) Desteği:** Render motoru, bitişik Arapça metinleri ve RTL dilleri, harfler arasındaki bağları (ligatures) bozmadan kusursuz bir hassasiyetle işler.
*   **Özenle Seçilmiş RTL Fontlar:** Yüksek kaliteli motion graphics projeleri için tasarlanmış *Cairo, Almarai, Tajawal, Amiri, El Messiri* ve *Lateef* gibi en iyi Arapça fontlara anında erişim.
*   **Gelişmiş Metin Şekillendirme:** Kullanılan dil ne olursa olsun metin katmanlarına doğrudan doğrusal/radyal geçişler (gradients), özel dış çizgiler (strokes), iç boşluklar (padding), yuvarlatılmış arka planlar ve gölgeler (shadows) uygulayın.

![Tipografi ve Dil Desteği](src/screenshot5.png)

## ⌨️ Klavye Kısayolları

Profesyonel kurgucular için tasarlanan InfiniteMotion Studio, uzamsal düzenleme iş akışınızı hızlandırmak için kapsamlı bir klavye kısayolu seti sunar.

### Genel ve Oynatma Kısayolları (General & Playback)
| İşlem | Kısayol (Windows / Mac) |
| :--- | :--- |
| **Oynat / Duraklat (Play / Pause)** | `Space` |
| **Geri Al (Undo)** | `Ctrl` + `Z` / `Cmd` + `Z` |
| **Yinele (Redo)** | `Ctrl` + `Shift` + `Z` / `Cmd` + `Shift` + `Z` |
| **Seçileni Sil (Delete Selected)** | `Delete` veya `Backspace` |

### Çalışma Alanı Kısayolları (Workspace)
| İşlem | Kısayol (Windows / Mac) |
| :--- | :--- |
| **Çalışma Alanında Gezinme (Pan / Hand Tool)** | `Space` basılı tut + `Mouse ile Sürükle` |
| **Yakınlaştır / Uzaklaştır (Zoom In / Out)** | `Mouse Tekerleği` / `Trackpad Kaydırma` |
| **Sanal Kamerayı Döndür (Rotate Camera)** | `Alt` / `Option` basılı tut + `Mouse ile Sürükle` (Boş bir alanda) |

### Varlık (Asset) Yönetimi Kısayolları
| İşlem | Kısayol (Windows / Mac) |
| :--- | :--- |
| **Öğeyi Kaydır (Nudge - 1px)** | `Yön Tuşları` |
| **Hızlı Kaydır (Fast Nudge - 10px)** | `Shift` + `Yön Tuşları` |
| **Öğeyi Çoğalt (Duplicate)** | `Alt` / `Option` basılı tut + `Öğeyi Sürükle` |
| **Orantıyı Koru (Constrain Proportions)** | `Shift` basılı tut + `Boyutlandırma Tutamacını (Scale Handle) Sürükle` |
| **Sabit Açılarla Döndür (Snap Rotation - 15°)** | `Shift` basılı tut + `Döndürme Tutamacını (Rotate Handle) Sürükle` |

### Seçim Kısayolları (Selection)
| İşlem | Kısayol (Windows / Mac) |
| :--- | :--- |
| **Çoklu Seçim (Multi-Select)** | `Shift` basılı tut + `Tıkla` |
| **Kutu Seçimi (Marquee Selection)** | `Ctrl` / `Cmd` basılı tut + `Mouse ile Sürükle` (Çalışma alanı ve timeline'da çalışır) |

### Önizleme Modu Kısayolları (Preview Mode)
| İşlem | Kısayol (Windows / Mac) |
| :--- | :--- |
| **Oynat / Duraklat (Play / Pause)** | `Space` |
| **5 Saniye İleri / Geri Atla** | `Sağ Ok` / `Sol Ok` |
| **Tam Ekran Modu (Toggle Fullscreen)** | `F` |
| **Önizlemeden Çık** | `Esc` |

## 🛠️ Kullanılan Teknolojiler

**Frontend (Kullanıcı Arayüzü)**
*   [React 19](https://react.dev/) - UI Framework
*   [TypeScript](https://www.typescriptlang.org/) - Tip güvenliği (Type safety) ve iş mantığı
*   [Tailwind CSS](https://tailwindcss.com/) - Hızlı ve utility-first stil mimarisi
*   [Lucide React](https://lucide.dev/) - Temiz ve tutarlı ikon seti

**Render Motoru ve Medya İşleme**
*   **HTML5 Canvas API** - Temel render motoru
*   **WebCodecs API & OfflineAudioContext** - Yüksek performanslı kare kodlama (frame encoding) ve ses miksajı
*   [`mp4-muxer`](https://github.com/Vanilagy/mp4-muxer) & [`webm-muxer`](https://github.com/Vanilagy/webm-muxer) - Tüm medyayı tarayıcı içinde multiplex etme (birleştirme)

## 💡 İş Akışına Genel Bakış (Workflow Overview)

### Arayüz Elemanları
* **Varlık Merkezi (Sol Panel):** Projenize hızla yeni varlıklar (assets) ekleyin. Metin veya kod oluşturmak, resim/video yüklemek veya ses efektlerini yönetmek için ses paneline erişmek üzere ikonlara tıklayın.
* 
![Varlık Merkezi](src/screenshot10.png)

*   **Çalışma Alanı (Orta Panel):** Sonsuz çalışma alanında gezinmek için `Space + Mouse Sürükleme` kombinasyonunu kullanın. Yakınlaştırmak/uzaklaştırmak için scroll yapın. Varlıkları seçerek taşıyın, boyutlandırın veya döndürün.
*   
![Çalışma Alanı](src/screenshot6.png)

*   **Timeline (Alt Panel):** Animasyonunuzu önizlemek için playhead'i (oynatma başlığı) hareket ettirin. Manyetik hizalamayı (Snapping) açıp kapatmak için **Mıknatıs** ikonuna tıklayın. Çoklu klipleri kutuyla seçmek (marquee selection) için `Shift` tuşuna basılı tutarak sürükleyin.
*   
![Timeline](src/screenshot7.png)

*   **Inspector (Sağ Panel):** Seçili varlığın veya kameranın özelliklerini düzenleyin. Fontları, renkleri, giriş/çıkış animasyonlarını ve gölge efektlerini (drop shadow) ayarlayın.
*   
![Inspector](src/screenshot8.png)

### Animasyon Oluşturma (Keyframing)
1.  **Varlık Animasyonları (Asset Keyframing):** Çalışma alanından bir varlık seçin. Timeline'da playhead'i (oynatma başlığını) hareket ettirin. Varlığı çalışma alanında yeni bir konuma taşıyın; hareketiniz için otomatik olarak yeni bir keyframe oluşturulacaktır!
2.  **Kamera Animasyonları (Camera Keyframing):** Timeline başlığındaki `Camera KF` butonuna tıklayın. Playhead'i hareket ettirin ve dinamik kamera hareketleri oluşturmak için üst araç çubuğundan Zoom/Pan ayarlarını değiştirin.

### Projeyi Dışa Aktarma (Export)
1. Sağ üst köşedeki **Export** butonuna tıklayın.
2. İstediğiniz formatı (`MP4` veya `WebM`), kare hızını (60 FPS'ye kadar) ve çözünürlük ölçeğini seçin.
3. **Start Encoding** butonuna tıklayın. Yazılım, videoyu render etmek için yerel cihazınızın GPU'sunu kullanacaktır.

![Projeyi Dışa Aktarma](src/screenshot9.png)

## 🔒 Lisans

Tüm Hakları Saklıdır (All Rights Reserved). Bu proje ve kod tabanı tamamen kapalı kaynaklıdır ve mülkiyeti saklıdır. Telif hakkı sahibinin açık yazılı izni olmadan açık kaynak olarak dağıtılamaz, kopyalanamaz veya değiştirilemez.
