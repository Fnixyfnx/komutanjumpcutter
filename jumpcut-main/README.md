# OpenJumpCuts  
Adobe Premiere Pro için ücretsiz, açık kaynaklı bir jump cut eklentisi.

Şu anda **alfa** aşamasında. Hatalar beklenmelidir. Çalışmalarınızın yedeğini alın.

## Hakkında
Bu eklentiyi, Adobe Premiere’de uzun ve tekrarlı eğitim içerikleri düzenleme deneyimime dayanarak yaptım. “Jump cut”, podcast’ler, röportajlar vb. içeriklerdeki sessizlikleri ve hataları kırpma işlemine verilen isimdir. Var olan birkaç jump cut eklentisi olsa da, bunların hiçbiri ücretsiz ve açık kaynaklı değildir.

Eklenti, verilen bir klibin ses dalga formunu analiz ederek belirli bir ses yüksekliği eşiğinin altındaki bölgeleri kaldırarak çalışır. Ses yüksekliği eşiği (cutoff), dolgu (padding) ve minimum sessizlik süresi gibi değerlerin tümü Premiere içindeki eklenti arayüzünden yapılandırılabilir.

### CEP ve UXP
Adobe, CEP eklentilerini aşamalı olarak bırakıp UXP eklentilerine geçiş üzerinde çalışıyor. Bu bir CEP eklentisidir ve bu nedenle sonunda UXP’ye taşınması gerekecektir. Aralık 2023 itibarıyla Premiere henüz UXP’yi desteklememektedir.

### Premiere AI araçları hakkında not
Kasım 2023 itibarıyla Premiere’in AI araçları hâlâ beta aşamasındadır. Dalga formu tabanlı jump cut otomasyonuna potansiyel bir alternatif olarak umut verici olsalar da, kendi iş akışımda kullanabileceğim seviyede değiller.

## Kurulum
Eklenti şu anda Adobe’nin dağıtım platformlarında yer almamaktadır, bu nedenle elle kurulmalıdır.

Depoyu, Adobe CEP eklentileri klasörünüze klonlayın.  
Windows’ta: `C:\Program Files (x86)\Common Files\Adobe\CEP\extensions`

Premiere API, ses dalga formuna dair verileri sunmadığından, eklenti jump cut hesaplamalarını gerçekleştirmek için harici bir çalıştırılabilir dosyaya ( `jumpcut.py` dosyasının `pyinstaller` ile derlenmiş hali) dayanır. Bu çalıştırılabilir dosya, farklı kodekleri okuyabilmek için [ffmpeg](https://ffmpeg.org/download.html)’e dayanır. `ffmpeg` sistem PATH’inizden erişilebilir olmalıdır.

`ffmpeg` ile ilgili sorunlar nedeniyle şu anda MacOS desteklenmemektedir.

Ayrıca, bu çalıştırılabilir dosyanın Premiere’de referans alınan orijinal medya kaynak dosyalarını okuyabilme iznine sahip olması gerektiği de unutulmamalıdır.

Eklenti henüz imzalı olmadığı için [debug mode](https://github.com/Adobe-CEP/Samples/tree/master/PProPanel) modunda çalıştırılmalıdır. Ayrıntılar için bölüm 2: “Loading of Unsigned Panels” kısmına bakın.

## Kılavuz

#### Önkoşullar
Şu anda çalışabilecek klip ve zaman çizelgesi yapılandırmaları için katı sınırlamalar bulunmaktadır.

Yalnızca o anda aktif olan sekans analiz edilir. Zaman çizelgesindeki iç içe (nested) sekanslar desteklenmez. Yalnızca Video 1 ve Audio 1 izlerindeki klipler analiz edilir. Video ve ses klipleri bağlantılı (linked) olmalıdır. Kliplerin zaman çizelgesindeki konumu ve kırpılması önemli değildir.

Tanımsız davranışlardan kaçınmak için, zaman çizelgesinde tek bir bağlantılı video/ses klip çifti yoksa eklenti şu anda çalışmaz. Zaman çizelgesinin yalnızca bir bölümünü jump cut yapmak istiyorsanız veya zaten kalabalık bir zaman çizelgesindeki içeriği jump cut etmek istiyorsanız, mevcut iş akışı; klipleri tek tek iç içe sekanslar (nested sequences) olarak kaydetmek ve jump cut işlemini bu iç içe sekanslar içinde çalıştırmaktır.

Gelecekteki geliştirme hedefleri, bu kısıtlamalar konusunda daha fazla esneklik sağlamayı ve birden fazla klip ile birden fazla iz için mantık eklemeyi içermektedir.

#### Seçenekler
Şu anda desteklenen dört jump cut parametresi vardır.

**Cutoff (Eşik):**  
Bir klibin sessiz kabul edilmesinin altında kalan değerdir. Dönüşümle ilgili zorluklar nedeniyle, şimdilik Premiere’de gösterilen gerçek dB değerlerini tam olarak yansıtmayabilir.

**Minimum Silence Length (Minimum Sessizlik Süresi):**  
Sessiz bir bölüm bu değerden kısaysa yok sayılır.

**Minimum Segment Length (Minimum Parça Süresi):**  
Sessiz olmayan bir bölüm bu değerden kısaysa sessiz kabul edilir.

**Padding (Dolgu):**  
Silinmiş sessiz kısımların etrafına bir miktar sessizlik tamponu ekler.

### Bilinen sorunlar
- Tüm klibiniz siliniyorsa, büyük ihtimalle sessizlik eşiği tüm klibi sessiz kabul edecek kadar yüksek ayarlanmıştır. Eşiği daha düşük bir değere çekmeyi deneyin.
- Cutoff dB değerleri Premiere’deki dB değerlerini tam olarak yansıtmayabilir.

## Hata raporu ve katkı
Düzeltmek istediğiniz bir hata veya eklemek istediğiniz bir özellik varsa lütfen bir pull request açın. Herhangi bir hatayı Issues bölümünde bildirin.
