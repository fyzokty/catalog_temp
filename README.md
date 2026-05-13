# Catalog App — Proje Yapısı ve Mimari Rehberi

> Bu belge, projenin klasörleme mantığını, mimari kararlarını ve kodlama stilini açıklar.
> Kod örnekleri içermez; yalnızca yapı ve tasarım kararlarına odaklanır.

---

## İçindekiler

1. [Genel Bakış](#1-genel-bakış)
2. [Dizin Ağacı](#2-dizin-ağacı)
3. [Mimari Yaklaşım](#3-mimari-yaklaşım)
4. [Feature Modülleri](#4-feature-modülleri)
5. [Project Katmanı](#5-project-katmanı)
6. [Bağımsız Module Paketleri](#6-bağımsız-module-paketleri)
7. [State Management](#7-state-management)
8. [Navigasyon](#8-navigasyon)
9. [Network ve Servis Katmanı](#9-network-ve-servis-katmanı)
10. [Veri Modelleri](#10-veri-modelleri)
11. [Yerel Depolama ve Önbellek](#11-yerel-depolama-ve-önbellek)
12. [Tema ve Stil Sistemi](#12-tema-ve-stil-sistemi)
13. [Çok Dil Desteği](#13-çok-dil-desteği)
14. [Dependency Injection](#14-dependency-injection)
15. [Uygulama Başlangıç Sırası](#15-uygulama-başlangıç-sırası)
16. [Yardımcı Katman (Utility)](#16-yardımcı-katman-utility)
17. [Yeniden Kullanılabilir Widget Kütüphanesi](#17-yeniden-kullanılabilir-widget-kütüphanesi)
18. [Logger Yapısı](#18-logger-yapısı)
19. [Ortam Yönetimi (Environment)](#19-ortam-yönetimi-environment)
20. [Test Yapısı](#20-test-yapısı)
21. [Temel Bağımlılıklar](#21-temel-bağımlılıklar)

---

## 1. Genel Bakış

Catalog, Flutter ile geliştirilmiş bir e-ticaret / ürün katalogu uygulamasıdır.

| Özellik | Değer |
|---|---|
| Flutter SDK | ^3.11.0 |
| Uygulama Sürümü | 0.0.1+4 |
| State Management | BLoC / Cubit |
| Navigasyon | Auto Route |
| HTTP İstemcisi | Dio |
| Yerel Depolama | Hive + SharedPreferences |
| Dil Desteği | TR, EN, RU, AR, FA, DE |
| Tema | Light / Dark / System |

---

## 2. Dizin Ağacı

### Kök Yapısı

```
catalog_app/
├── lib/                    # Uygulama kaynak kodu
│   ├── feature/            # Ekrana özel modüller
│   ├── project/            # Projeye ait altyapı ve paylaşılan kod
│   ├── app.dart            # Uygulama widget'ı (MaterialApp konfigürasyonu)
│   ├── main.dart           # Giriş noktası
│   ├── firebase_options.dart
│   └── development_helper.dart
│
├── module/                 # Bağımsız Dart paketleri
│   ├── core/               # Önbellek yönetimi (Hive)
│   ├── common/             # Ortak yardımcılar (URL, resim)
│   └── widget/             # Duyarlı (responsive) framework
│
├── asset/                  # Statik kaynaklar
│   ├── lang/               # Çeviri JSON dosyaları
│   ├── font/               # Özel yazı tipleri
│   ├── image/              # Uygulama görselleri
│   └── lottie/             # Animasyon dosyaları
│
├── android/
├── ios/
├── test/
└── pubspec.yaml
```

### `lib/feature/` — Özellik Modülleri

```
feature/
├── auth/
│   └── view/
│       ├── phone_view.dart
│       ├── otp_view.dart
│       ├── register_view.dart
│       ├── mixin/
│       └── widget/
│   └── view_model/
│       ├── auth_view_model.dart
│       └── state/
│
├── home/
│   └── view/
│       ├── home_page.dart
│       ├── mixin/
│       └── widget/
│   └── view_model/
│       ├── home_view_model.dart
│       └── state/
│
├── product/
│   └── view/ (product_list, product_detail, similar_products)
│   └── view_model/
│
├── category/
├── blog/
├── favorites/
├── feedback/
├── profile/
├── profile_edit/
├── app_settings/
├── main/          ← Alt navigasyon container'ı
└── splash/
```

### `lib/project/` — Proje Altyapısı

```
project/
├── cache/
│   └── project_cache.dart
│
├── init/
│   ├── app_initialization.dart
│   ├── system_chrome_init.dart
│   ├── package_info_init.dart
│   └── theme/
│       ├── custom_light_theme.dart
│       ├── custom_dark_theme.dart
│       ├── color_scheme.dart
│       └── project_themes/
│           ├── text_theme.dart
│           └── app_bar_theme.dart
│
├── model/
│   └── api/
│       ├── api_response.dart
│       ├── abstract/
│       ├── auth/
│       ├── banner/
│       ├── blog/
│       ├── product/
│       ├── profile/
│       ├── feedback/
│       └── general_data/
│
├── navigation/
│   ├── app_router.dart
│   └── app_router.gr.dart    ← Otomatik üretilen dosya
│
├── service/
│   ├── network_manager/
│   │   ├── app_network_manager.dart
│   │   ├── api_paths.dart
│   │   ├── network_error_manager.dart
│   │   └── request_type.dart
│   ├── auth_service/
│   ├── blog_service/
│   ├── product_service/
│   ├── data_service/
│   ├── feedback_service/
│   ├── logger/
│   └── sms_retriever/
│
├── state/
│   ├── project_view_model/   ← Tema, dil, font boyutu
│   └── session_view_model/   ← Kullanıcı, favoriler
│
├── utility/
│   ├── constants/
│   ├── enum/
│   ├── extension/
│   └── helper/
│
└── widget/
    ├── app_bar/
    ├── banner/
    ├── bottom_nav_bar/
    ├── button/
    ├── bottom_sheet/
    ├── dialog/
    ├── error_empty/
    ├── gallery/
    ├── html/
    ├── image/
    ├── indicator/
    ├── input/
    ├── list_item/
    ├── scroll_view/
    ├── selection_widget/
    ├── switcher/
    └── text/
```

---

## 3. Mimari Yaklaşım

Proje **Feature-Driven Clean Architecture** kullanır. Her özellik kendi içinde kapalı bir modül olarak tasarlanmıştır; dışa bağımlılık yalnızca `project/` katmanı üzerinden sağlanır.

```
┌─────────────────────────────────┐
│         feature/                │   ← Ekrana özgü UI + iş mantığı
│   view/ ←→ view_model/          │
└─────────────┬───────────────────┘
              │ service çağrısı
┌─────────────▼───────────────────┐
│         project/service/        │   ← Ağ, auth, loglama
└─────────────┬───────────────────┘
              │ veri modeli
┌─────────────▼───────────────────┐
│         project/model/          │   ← API response modelleri
└─────────────────────────────────┘
```

### Temel Prensipler

- **Tek yönlü veri akışı:** Kullanıcı etkileşimi → ViewModel → State → UI
- **İmmutable state:** Her state değişikliği `copyWith()` ile yeni bir nesne üretir
- **Separation of Concerns:** View yalnızca render eder; iş mantığı mixin ve ViewModel'de yaşar
- **Yeniden kullanılabilirlik:** Tüm ortak bileşenler `project/widget/` altında merkezi olarak bulunur

---

## 4. Feature Modülleri

Her feature modülü aynı iç yapıya sahiptir:

```
feature/[özellik_adı]/
├── view/
│   ├── [özellik]_page.dart       ← BlocProvider + BlocBuilder içerir
│   ├── mixin/
│   │   └── [özellik]_mixin.dart  ← State init, event handler'lar
│   └── widget/
│       └── ...                   ← Yalnızca bu ekrana özel alt widget'lar
└── view_model/
    ├── [özellik]_view_model.dart  ← Cubit; service çağrıları burada
    └── state/
        └── [özellik]_state.dart   ← Equatable; copyWith destekli
```

### Katman Görevleri

| Katman | Dosya | Sorumluluk |
|---|---|---|
| View | `_page.dart` | Widget tree, BLoC bağlantısı |
| Mixin | `_mixin.dart` | Lifecycle, event yönetimi, controller'lar |
| Widget | `widget/` | Yalnızca bu sayfada kullanılan alt bileşenler |
| ViewModel | `_view_model.dart` | Servis çağrısı, state yönetimi |
| State | `_state.dart` | İmmutable durum verisi |

### Mevcut Feature'lar

| Modül | Amaç |
|---|---|
| `splash` | Açılış ekranı, yönlendirme |
| `auth` | Telefon doğrulama, OTP, kayıt |
| `main` | Alt navigasyon bar container'ı |
| `home` | Ana sayfa; banner, blog, kategoriler |
| `category` | Ürün kategorileri |
| `product` | Ürün listeleme ve detay |
| `blog` | Blog listeleme ve içerik |
| `favorites` | Kullanıcının favori ürünleri |
| `feedback` | Ürün ve genel geri bildirim |
| `profile` | Kullanıcı profili görüntüleme |
| `profile_edit` | Profil düzenleme |
| `app_settings` | Tema, dil, font boyutu ayarları |

---

## 5. Project Katmanı

`lib/project/` tüm feature'ların ortak kullandığı altyapıyı barındırır. Hiçbir feature, başka bir feature'a doğrudan bağımlı değildir; bağımlılıklar yalnızca bu katman üzerinden kurulur.

### Alt Klasörler ve Görevleri

| Klasör | İçerik |
|---|---|
| `cache/` | Hive önbellek başlatma ve yönetimi |
| `init/` | Uygulama başlangıç prosedürleri ve tema tanımları |
| `model/api/` | Tüm API istek/yanıt modelleri |
| `navigation/` | Auto Route konfigürasyonu |
| `service/` | Ağ yöneticisi ve domain servis sınıfları |
| `state/` | Global state (tema, dil, oturum) |
| `utility/` | Sabitler, enumlar, extension'lar, yardımcılar |
| `widget/` | Paylaşılan UI bileşen kütüphanesi |

---

## 6. Bağımsız Module Paketleri

`module/` dizini, ana uygulamadan bağımsız üç Dart paketi içerir. Bu paketler kendi `pubspec.yaml` dosyalarına sahiptir ve teorik olarak başka projelerde de kullanılabilir.

```
module/
├── core/       Hive önbellek yöneticisi ve soyut arayüzler
├── common/     URL başlatma, ağ üzerinden resim önbellekleme
└── widget/     Duyarlı (responsive) yardımcı framework
```

Bu ayrım, büyük çaplı değişikliklerde bağımlılık yönetimini kolaylaştırır ve test edilebilirliği artırır.

---

## 7. State Management

### Teknoloji: BLoC + Cubit

Proje `flutter_bloc` paketini kullanır. Daha az event tanımı gerektiren ekranlar için `Cubit` tercih edilir.

### Temel Sınıf Hiyerarşisi

```
Cubit<S>  (flutter_bloc)
    └── BaseCubit<S>         ← Kapalı Cubit'e emit koruması
            └── [Feature]ViewModel

Equatable  (equatable)
    └── BaseState<T>         ← copyWith() destekli immutable state
            └── [Feature]State
```

### State Akışı

```
Kullanıcı etkileşimi
        ↓
View (mixin üzerinden)
        ↓
ViewModel.someMethod()
        ↓
Service çağrısı (async)
        ↓
emit(state.copyWith(...))
        ↓
BlocBuilder → UI yenilenir
```

### Global State

Uygulama genelinde iki global Cubit vardır:

- **ProjectViewModel** — Tema modu, dil, font boyutu (kalıcı tercihler)
- **SessionViewModel** — Oturum açmış kullanıcı bilgisi, favoriler (oturum bazlı)

Bu iki Cubit, `main.dart` içinde `BlocProvider` ile uygulama ağacının en üstüne enjekte edilir.

---

## 8. Navigasyon

### Teknoloji: Auto Route

Deklaratif ve tip güvenli rota yönetimi sağlar. `@AutoRouterConfig` anotasyonu ile rota tanımları kod üreteci tarafından işlenerek `app_router.gr.dart` dosyası otomatik üretilir.

### Rota Hiyerarşisi

```
/ (root)
├── /splash              ← Başlangıç rotası
├── /phone               ← Auth akışı
├── /otp
├── /register
├── /main                ← Alt navigasyon shell'i
│   ├── home
│   ├── category
│   ├── favorites
│   └── profile
├── /blog/:blogID
├── /blog_category/:categoryID
├── /products/:productID
├── /feedback
├── /product_feedback
├── /profile_edit
└── /app_settings
```

### Tasarım Kararları

- Parametereli rotalar (`:productID`, `:blogID`) tip güvenli argüman iletimi sağlar
- Ana ekranlar `nested route` yapısıyla `MainView` altında tanımlanır; alt sekme geçişlerinde stack temizlenmez
- Otomatik üretilen dosya (`app_router.gr.dart`) elle düzenlenmez

---

## 9. Network ve Servis Katmanı

### Yapı

```
project/service/
├── network_manager/
│   ├── app_network_manager.dart   ← Dio instance + interceptor'lar
│   ├── api_paths.dart             ← Tüm endpoint sabitleri
│   ├── network_error_manager.dart ← Hata dönüşüm mantığı
│   └── request_type.dart          ← GET, POST, PUT, DELETE enum
│
├── auth_service/
├── blog_service/
├── product_service/
├── data_service/
├── feedback_service/
└── sms_retriever/
```

### AppNetworkManager

Dio istemcisini yapılandırır:

- Temel URL ortam değişkeninden okunur (`AppEnvironment`)
- Bağlantı, alma ve gönderme zaman aşımları 15 saniye
- İçerik türü: `application/x-www-form-urlencoded`
- `PrettyDioLogger` ile istek/yanıt loglama
- Token yenileme için interceptor

### Domain Servis Sınıfları

Her servis sınıfı şu kurala uyar:

- Yalnızca `AppNetworkManager` ile iletişim kurar
- `ApiResponse<T>` generic tipi döndürür
- Auth token'ı `AuthStorage`'dan alır
- Hata yönetimini `NetworkErrorManager` üzerinden yapar
- Loglama için `apiLogger` kullanır

---

## 10. Veri Modelleri

### Konum

```
project/model/api/
├── api_response.dart        ← Generic wrapper; success/error ayrımı
├── api_data.dart
├── api_error.dart
├── abstract/
├── auth/                    ← Login, OTP, register response'ları
├── banner/
├── blog/                    ← BlogItem, BlogDetail, BlogPagination
├── product/                 ← 7 alt model (detail, category, filter, vb.)
├── profile/
├── feedback/
└── general_data/            ← Countries, Cities, Languages, UserTypes
```

### Kod Stili

Tüm modeller aynı kalıbı izler:

- `Equatable` extends ederek değer karşılaştırması yapılır
- `fromMap(Map<String, dynamic>)` constructor ile JSON ayrıştırma
- `copyWith()` ile kısmi güncelleme desteği
- İç içe (nested) modeller ayrı dosyalarda tanımlanır
- Sayfalama için `Pagination` wrapper modelleri mevcuttur

---

## 11. Yerel Depolama ve Önbellek

### Katmanlar

| Teknoloji | Kullanım Amacı |
|---|---|
| **Hive** | Yapısal veri önbellekleme (NoSQL, hızlı okuma) |
| **SharedPreferences** | Kullanıcı tercihleri (tema, dil, font boyutu) |
| **Auth Storage** | JWT token ve oturum bilgisi |

### Hive Organizasyonu

`module/core/` paketi içinde `HiveCacheManager` soyut arayüzü ve somut implementasyonu bulunur. `ProjectCache`, uygulama başlangıcında Hive'ı başlatır ve gerekli `adapter`'ları kaydeder.

---

## 12. Tema ve Stil Sistemi

### Tema Dosyaları

```
project/init/theme/
├── custom_light_theme.dart   ← Aydınlık tema tanımı
├── custom_dark_theme.dart    ← Karanlık tema tanımı
├── custom_theme.dart         ← Ortak tema yardımcıları
├── color_scheme.dart         ← Material 3 renk şeması
└── project_themes/
    ├── text_theme.dart       ← Tipografi sistemi
    └── app_bar_theme.dart
```

### Renk Sabitleri

`ProjectColor` sınıfında 60'tan fazla renk sabiti tanımlıdır. Renkler anlam taşıyan isimlerle gruplandırılmıştır:
`primaryRed`, `darkGrey`, `dividerGrey`, `barrierColor` gibi.

### Yazı Tipleri

Uygulama üç özel yazı tipi ailesi kullanır:

| Aile | Ağırlıklar | Kullanım |
|---|---|---|
| **NotoSans** | 100–900 | Genel metin |
| **DM Sans** | 100–900 | UI etiketleri |
| **Playfair Display** | 400–900 | Başlıklar / vurgular |

### Tema Değişimi

`ProjectViewModel` üç kullanıcı tercihini yönetir:

- `setThemeMode(AppThemeMode)` — Light / Dark / System
- `setAppLocale(AppLocale)` — Dil değişikliği
- `setAppFontSize(AppFontSize)` — Font boyutu

Değişiklikler anında `SharedPreferences`'a yazılır ve uygulama yeniden başlatıldığında geri yüklenir.

---

## 13. Çok Dil Desteği

### Teknoloji: Easy Localization

```
asset/lang/
├── tr.json   (~23 KB)   ← Varsayılan dil
├── en.json   (~18 KB)
├── ru.json   (~25 KB)
├── ar.json   (~23 KB)
├── fa.json   (~23 KB)
└── de.json   (~19 KB)
```

### Yapılandırma

- Desteklenen locale'ler: TR, EN, RU, AR, FA, DE
- Yedek locale: `AppLocale.tr`
- `AppLocale` enum'u: `name` property API çağrılarında dil parametresi olarak kullanılır
- Dil değişikliği `ProjectViewModel.setAppLocale()` üzerinden yapılır; kalıcı olarak saklanır

---

## 14. Dependency Injection

### Teknoloji: GetIt

`ProjectStateContainer.setup()` metodu uygulama başlangıcında tüm bağımlılıkları kaydeder.

### Kayıt Stratejileri

| Strateji | Nesneler |
|---|---|
| **Singleton** | ProjectCache, SharedPreferences, AppNetworkManager |
| **Lazy Singleton** | ProjectViewModel, SessionViewModel |

### Erişim

`ProjectStateItems` sınıfı static getter'lar aracılığıyla kayıtlı nesnelere erişim sağlar:

```
ProjectStateItems.projectViewModel
ProjectStateItems.sessionViewModel
ProjectStateItems.networkManager
ProjectStateItems.projectCache
```

---

## 15. Uygulama Başlangıç Sırası

`BeforeRunAppInit.run()` metodu sıralı olarak çağrılır:

```
1.  WidgetsFlutterBinding.ensureInitialized()
2.  Firebase.initializeApp()
3.  EasyLocalization.ensureInitialized()
4.  NotificationService.initialize()
5.  SystemChromeInit.init()        ← Durum çubuğu, yön kilidi
6.  AppEnvironment.general()       ← Ortam değişkenleri
7.  ProjectStateContainer.setup()  ← GetIt kayıtları
8.  ProjectCache.initialize()      ← Hive başlatma
9.  AppSettingStorage.initialize() ← SharedPreferences başlatma
10. PackageInfoInit.init()          ← Uygulama versiyonu
11. DeviceInfoInit.init()           ← Cihaz bilgisi
12. FlutterError.onError            ← Global hata yakalayıcı
```

---

## 16. Yardımcı Katman (Utility)

```
project/utility/
├── constants/
│   ├── project_color.dart          ← Renk sabitleri
│   ├── project_duration.dart       ← Zaman sabitleri (API timeout, animasyon)
│   ├── project_padding.dart        ← EdgeInsets sabitleri
│   ├── project_radius.dart         ← BorderRadius sabitleri
│   ├── project_form_border.dart    ← Form kenarlık stilleri
│   ├── project_input_decoration.dart ← TextField dekorasyonları
│   └── project_scroll_behavior.dart  ← Özel kaydırma davranışı
│
├── enum/
│   ├── screen_state.dart   ← Initial, FirstLoading, Loading, Loaded, LastPage, Error
│   ├── app_locale.dart     ← TR, EN, RU, AR, FA, DE
│   ├── theme_mode.dart     ← Light, Dark, System
│   ├── font_size.dart      ← Kullanıcı font boyutu seçenekleri
│   ├── api_navigation_type.dart  ← Blog, Product, ExternalUrl
│   └── form_screen_state.dart    ← Form durumları
│
├── extension/
│   ├── screen_helper.dart      ← width, height, screenHeight, screenWidth
│   ├── theme_helper.dart       ← colorScheme, textTheme erişimi
│   ├── string_join.dart        ← String birleştirme
│   ├── phone_obscure.dart      ← "555 55** **** 56" maskeleme
│   ├── show_context_helper.dart ← showDialog, showBottomSheet
│   ├── toast_extension.dart    ← Context üzerinden toast
│   ├── locale.dart             ← Locale dönüşümleri
│   └── future_delay.dart       ← Future.delayed kısayolu
│
└── helper/
    ├── toast_helper.dart       ← Önceden tanımlı toast mesajları
    └── form_validators.dart    ← Email, şifre, telefon doğrulama
```

---

## 17. Yeniden Kullanılabilir Widget Kütüphanesi

`project/widget/` altında 34'ten fazla klasörde organize edilmiş paylaşılan UI bileşenleri bulunur:

| Klasör | İçerik |
|---|---|
| `app_bar/` | Özel AppBar varyantları |
| `banner/` | Banner görüntüleyici |
| `bottom_nav_bar/` | Sekme navigasyon çubuğu |
| `button/` | 8+ düğme türü (scale, text, colored, favorite, filter, back, iconed) |
| `bottom_sheet/` | Özel alt çekmeceler; filtre bottom sheet dahil |
| `dialog/` | Özel dialog'lar |
| `divider/` | Özel ayırıcılar |
| `error_empty/` | Hata ve boş durum ekranları |
| `gallery/` | Görsel galerisi |
| `html/` | HTML içerik render'layıcı |
| `image/` | Ağ görseli widget'ı (önbellekli) |
| `indicator/` | Yükleme göstergeleri, RefreshIndicator |
| `input/` | Text field'lar ve özel giriş bileşenleri |
| `list_item/` | Özel liste elemanları |
| `scroll_view/` | AnimatedGridView, AnimatedListView, HorizontalProductListView |
| `selection_widget/` | Çoklu seçim; ekran ve alt çekmece varyantları |
| `switcher/` | State'e göre animasyonlu UI geçişi |
| `text/` | Özel metin widget'ları |

---

## 18. Logger Yapısı

```
project/service/logger/
├── loggers.dart        ← Dışa açık instance'lar
├── api_logger.dart     ← Ağ istekleri ve yanıtları
├── cache_logger.dart   ← Hive okuma/yazma işlemleri
└── ui_logger.dart      ← UI olayları
```

Üç ayrı logger örneği farklı katmanlar için özelleştirilmiştir. `PrettyDioLogger` ağ katmanında ek HTTP loglama sağlar.

---

## 19. Ortam Yönetimi (Environment)

### Dosyalar

```
lib/project/init/environment/
├── environment.dart           ← Soyut temel sınıf
├── dev_env.dart               ← Geliştirme ortamı
├── dev_env.g.dart             ← Otomatik üretilen (envied)
├── prod_env.dart              ← Üretim ortamı
├── prod_env.g.dart            ← Otomatik üretilen (envied)
└── app_configuration.dart     ← Aktif ortam seçimi
```

### Envied Paketi

`.env` dosyasından ortam değişkenlerini okur ve kod üreteci aracılığıyla Dart sabitine dönüştürür. API anahtarları ve temel URL'ler kaynak kodda düz metin olarak bulunmaz.

---

## 20. Test Yapısı

```
test/
├── widget_test.dart     ← Temel widget testi
└── network/             ← Ağ katmanı testleri
```

Proje büyük ölçüde manuel test odaklıdır. Otomatik test altyapısı temel düzeydedir; `build.yaml` konfigürasyonu kod üreteci için yapılandırılmıştır.

---

## 21. Temel Bağımlılıklar

| Kategori | Paket | Amaç |
|---|---|---|
| State | `flutter_bloc ^9.1.1` | BLoC + Cubit pattern |
| Navigasyon | `auto_route ^11.1.0` | Deklaratif, tip güvenli routing |
| HTTP | `dio ^5.9.1` | HTTP istemcisi |
| HTTP Log | `pretty_dio_logger ^1.4.0` | İstek/yanıt loglama |
| Önbellek | `hive_ce ^2.19.3` | NoSQL yerel veritabanı |
| Tercihler | `shared_preferences ^2.5.4` | Anahtar-değer depolama |
| Lokalizasyon | `easy_localization ^3.0.8` | Çok dil desteği |
| Firebase | `firebase_core ^4.4.0` | Firebase altyapısı |
| Bildirim | `firebase_messaging ^16.1.1` | Push notification |
| DI | `get_it ^9.2.0` | Servis bulucu / DI container |
| Eşitlik | `equatable ^2.0.8` | Değer tabanlı karşılaştırma |
| Animasyon | `lottie ^3.3.2` | JSON animasyonları |
| SVG | `flutter_svg ^2.2.3` | SVG render |
| Loading | `skeletonizer ^2.1.3` | İskelet yükleme efekti |
| OTP | `pinput ^6.0.2` | PIN/OTP giriş alanı |
| Telefon | `intl_phone_number_input ^0.7.5` | Uluslararası telefon girişi |
| HTML | `flutter_html ^3.0.0` | HTML içerik render |
| Log | `logger ^2.6.2` | Yapılandırılmış loglama |
| Ortam | `envied ^1.3.3` | Güvenli ortam değişkenleri |
| Cihaz | `device_info_plus ^12.3.0` | Cihaz bilgisi |
| Paket | `package_info_plus ^9.0.0` | Uygulama sürüm bilgisi |
| Auth | `smart_auth ^3.2.0` | SMS OTP otomatik okuma |
| Toast | `fluttertoast ^9.0.0` | Toast mesajları |

---

*Bu belge proje yapısını tanımlar; kod değişiklikleriyle birlikte güncel tutulmalıdır.*
