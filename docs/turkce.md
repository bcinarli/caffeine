# Caffeine
Caffeine, günlük Sass kodlaması için bir mixin ve fonksiyon kütüphanesidir. 

## Fonksiyonlar
Caffeine fonksiyonları, hem mixinler içinde hem de ihtiyaç durumunda geliştirme sırasında kullanılabilirler. Fonksiyonlar genel olarak bir CSS çıktısı üretmezler. Daha çok bir CSS tanımı içinde kullanılacak değer ya da hesaplama sonuçlarını üretirler.

### 1. Jenerik Fonksiyonlar
Jenerik fonksiyonlar genel kullanım amaçlıdır.

#### 1.1 `remove-unit`
Verilen bir değerdeki birimi kaldırır.
```scss
@include remove-unit(10px) // 10
@include remove-unit(100%) // 100
```

### 2. Metin Fonksiyonları
Metinler (String) ile alakalı fonksiyonlardı.

#### 2.1 `str-replace`
Verilen bir metin içindeki harf/kelimeleri değiştirir ya da kaldırır.

Aldığı parametreler;
* `$string`, içinde arama yapılacak metin
* `$substr`, aranacak söz öbeği
* `$newsubstr`, orjinali ile değiştirilecek söz öbeği
* `$all` ilk bulunan söz öbeği mi yoksa hepsi mi değiştirilecek kontrolü

``` scss
@include str-replace('foo bar baz', 'bar', 'foo'); // foo foo baz
```

#### 2.2 `str-ireplace`
`str-replace` fonksiyonun büyük küçük harf ayırmayan versiyonu.
```scss
@include str-ireplace('for Bar bAz', 'bar', 'foo'); // foo foo bAz
```

### 3. Text/Boyut Fonksiyonları
Font büyüklükleri ve `rem` - `px` dönüşümleri ile alakalı fonksiyonlardır.

#### 3.1 `rem-to-px`
`rem` olarak verilmiş bir font büyüklüğünü, sayfada tanımlanan ana font boyutuna göre (_default değeri 16px'dir_) dönüşümünü yapar.

```scss
@include rem-to-px(1rem) // 16px
@include rem-to-px(20px) // 20px
@include rem-to-px(1.5rem) // 24px
```

#### 3.2 `rem`
`px` olarak verilmiş bir font büyüklüğünü, sayfada tanımlanan ana font boyutuna göre (_default değeri 16px'dir_) dönüşümünü yapar.

```scss
@include rem(16px) // 1rem
@include rem(20px) // 1.25rem
@include rem(1.5rem) // 1.5rem
```
