# Caffeine
Caffeine, günlük Sass kodlaması için bir mixin ve fonksiyon kütüphanesidir. 

## Mixinler
Caffeine mixinleri CSS3 temelli olanlar ve genel olanlar şeklinde gruplanmıştır. CSS3 mixinlerinin içinde daha çok yapılmış tanımlara göre prefix olarak eklenen tanımlar bulunmaktadır.

### 1. Görünüm Mixini (_appearance_)
[Can I Use](http://caniuse.com/#search=appearance) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/-moz-appearance)

CSS `appearance` tanımı için prefix edilmiş çıktıları verir.

```scss
@include appearance(none);
// -webkit-appearance: none;
// -moz-appearance: none;
// appearance: none;
```

### 2. CSS Üçgen (_arrow_)
Border tanımlarını kullanarak CSS Üçgen oluşturmaya yarar.

Aldığı parametler;
* `$color`, görüntülenecek üçgenin rengini tanımlar. Bütün bir üçgen yerine çizgi ya da ok ucu görünümü istendiğinde Sass array formatında 2 renk de gönderilebilir. 
* `$size`, görüntülenecek üçgenin büyüklüğünü tanımlar. Bütün bir üçgen yerine çizgi ya da ok ucu görünümü istendiğinde Sass array formatında 2 büyüklük gönderilebilir. 
* `$direction`, üçgenin/okun bakacağı yönü belirtir. _default değeri 'top'_ şeklindedir. `top`, `right`, `bottom`, `left` değerleri verilebilir. 
* `$el`, üçgen gösteriminde hangi _pseudo_ elemanın kullanılacağını belirler. _default değeri ':after'. `:after` ya da `:before` değeri kullanılabilir. Çizgi ya da ok ucu görünümünde hem `after` hem de `before` bir arada kullanılmaktadır.

```scss
@include arrow(#ccc, 10px);
/*
  &:after {
		content: "";
		position: absolute;
		width: 0;
		height: 0;
		border: solid transparent;
		border-bottom-color: #ccc;
		border-width: 10px;
		pointer-events: none;
	}
 */
```
```scss
@include arrow(#ccc #ddd, 10px 8px, bottom);
/*
	&:before {
	  content: "";
	  position: absolute;
	  width: 0;
	  height: 0;
	  border: solid transparent;
	  border-top-color: #ddd;
	  border-width: 8px;
	  pointer-events: none;
	}
	
  &:after {
    content: "";
    position: absolute;
    width: 0;
    height: 0;
    border: solid transparent;
    border-top-color: #ccc;
    border-width: 10px;
    pointer-events: none;
	}
 */
```

### 3. Kırım Noktaları (_breakpoints_)
Media query tanımlarında kullanılan kırılım noktalarını belirler.

#### 3.1 `mobile-portrait` & `mobile-landscape`
Dik ve yatay konumda olan telefonların genişliğini hedef alır. Beklenen maksimum içerik genişliği `320px` dir. Yatay konum hesap için genişliğin `320px`den büyük olduğu kontrol edilir.

```scss
.example {
  @include mobile-portrait {
    display: block;
  }
  
  @include mobile-landscape {
    width: 50%;
  }
}

/*
@media screen and (max-width: 320px){
  .example {
    display: block;
  }
}

@media screen and (min-width: 321px){
  .example {
    width: 50%;
  }
}
*/
```

#### 3.2 `tablet-portrait` & `tablet-landscape`
Dik ve yatay konumda olan tabletlerin genişliğini hedef alır. Portrait için `768px` ile `1024px` arası bir ekran genişliğine sahip cihazın, `portrait` konumunda olduğu kontrol edilir. Landscape için ise, aynı genişliklerdeki cihazın `landscape` konumu kontrol edilir.

```scss
.example {
  @include tablet-portrait {
    width: 50%;
  }
  
  @include tablet-landscape {
    width: 75%;
  }
}

/*
@media only screen and (min-device-width: 768px) and (max-device-width: 1024px) and (orientation: portrait) {
  .example {
    width: 50%;
  }
}

@media only screen and (min-device-width: 768px) and (max-device-width: 1024px) and (orientation: landscape) {
  .example {
    width: 75%;
  }
}
*/
```

#### 3.3 `desktop`
`1224px` ve üzeri genişlikteki ekranlara özel yazılacak tanımları hazırlar.
```scss
.example {
  @include desktop {
    width: 50%;
  }
}

/*
@media only screen and (min-width: 1224px) {
  .example {
    width: 50%;
  }
}
*/
```

#### 3.4 `widescreen`
`1824px` ve üzeri genişlikteki ekranlara özel yazılacak tanımları hazırlar.
```scss
.example {
  @include desktop {
    width: 25%;
  }
}

/*
@media only screen and (min-width: 1824px) {
  .example {
    width: 25%;
  }
}
*/
```

### 4. Jenerik Mixinler (_generic_)
Diğer mixinler içinde kullanılacak mixinleri barındırır.

#### 4.1 `prefixer`
Tanımlara belirlenen vendor ön eklerini ekler.

Aldığı parametreler
* `$property`, tanımın ismi
* `$value`, tanımın değeri
* `$vendors`, eklenecek ön ekler, _default değeri webkit moz_

```scss
@include prefixer(box-shadow, none);
/*
  -webkit-box-shadow: none;
  -moz-box-shadow: none;
  box-shadow: none;
 */
```

#### 4.2 `value-prefixer`
Tanımlara belirlenen vendor ön eklerini ekler.

Aldığı parametreler
* `$property`, tanımın ismi
* `$value`, tanımın methodu
* `$content`, metodun değeri
* `$vendors`, eklenecek ön ekler, _default değeri webkit moz_

```scss
@include value-prefixer(display, flex);
/*
  display: -webkit-flex;
  display: -moz-flex;
  display: flex;
 */
```

### 5. Placeholder
Input elemanında `placeholder` tanımı ile gösterilen örnek içerik metninin görünümünü stillendirir.

Aldığı parametreler
* `$self`, tanımı classın kendisine mi yoksa içindeki elemanlara mı uygulanacağını belirler, _default değeri true_

```scss
.example {
    @include placeholder {
        color: #999;
        font-style: italic;
    }
}
/*
  .example {
    &::-webkit-input-placeholder {
      color: #999;
      font-style: italic;
    }
    
    &::-moz-placeholder {
      color: #999;
      font-style: italic;
    }
    
    &::-ms-input-placeholder {
      color: #999;
      font-style: italic;
    }
  }
 */
```
```scss
.example {
    @include placeholder(false) {
        color: #999;
        font-style: italic;
    }
}
/*
  .example {
    ::-webkit-input-placeholder {
      color: #999;
      font-style: italic;
    }
    
    ::-moz-placeholder {
      color: #999;
      font-style: italic;
    }
    
    ::-ms-input-placeholder {
      color: #999;
      font-style: italic;
    }
  }
 */
```

### 6. Pozisyon Mixinleri (_position_)
Elemanların bulundukları taşıyıcı ya da sayfa için ortalı pozisyonlamaları tanımları. Tanımlar içinde elemanın genişlik ve yüksekliğinin bilinmesine ihtiyaç duymayan `transform` ile ortalama metodu kullanılmaktadır. 

#### 6.1 `center`
Elemanı sayfaya ya da bulunduğu taşıyıcı içine hem yatayda hem de düşeyde ortalar.

```scss
.example {
	@include center;
}

/*
  .example {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translateY(-50%) translateX(-50%);
  }
 */
```

#### 6.2 `center-vertical`
Elemanı sayfaya ya da bulunduğu taşıyıcı içine düşeyde ortalar.

```scss
.example {
	@include center-vertical;
}

/*
  .example {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
  }
 */
```

#### 6.3 `center-horizontal`
Elemanı sayfaya ya da bulunduğu taşıyıcı içine yatayda ortalar. _transform_ ya da _margin_ metodlarından tercih ettiğiniz birini seçebilirsiniz.

Aldığı Parametreler
* `$type`, ortalamanın _transform_ ile mi yoksa _margin_ ile mi yapılacağını belirler. _default değeri `absolute`_, başka bir değer gönderildiğin _margin_ yöntemi kullanılır.

```scss
.example {
	@include center-horizontal;
}

/*
  .example {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
  }
 */
```
```scss
.example {
	@include center-horizontal(margin);
}

/*
  .example {
    margin: 0 auto;
  }
 */
```

### 7. Scrollbar Görünümü (_scrollbar_)
Webkit tarayıcılarında sayfanın ya da bir elemanın kenarında gözüken scrollbarın görünümünü stillendirir. Scrollbarların görünümlerin `overflow` ile tanımlanabildiği için ayrıca _scrollbar_ ın yatay mı olacağı yoksa düşey mi olacağı tanımına ihtiyaç duyulmaz.

Aldığı parametreler
* `$thickness`, düşey scrollbarın genişliğini ve yatay scrollbarın yüksekliğini belirler. Aynı zamanda scroll pozisyonunu gösteren `thumb` elemanını da genişlik ya da yüksekliğini belirler.
* `$thumbColor`, scrollbarın pozisyonunu gösteren `thumb` elemanının rengini belirler.
* `$trackColor`, scrollbarın rengini gelirler
* `$radius`, `scrollbar` ve `thumb` elemanının kenar yuvarlaklığını belirler. _default değeri `3px`_

```scss
.example {
  @include scrollbar(5px, #999, #222);
}

/*
  .example::-webkit-scrollbar {
    width: 5px;
    height: 5px;
  }

  .example::-webkit-scrollbar-thumb {
    background: #999;
    border-radius: 3px;
  }

  .example::-webkit-scrollbar-thumb:vertical {
    height: 5px;
  }

  .example::-webkit-scrollbar-track { background-color: #222; }
  .example::-webkit-scrollbar-track-piece { background-color: #222; }
 */
```

## Fonksiyonlar
Caffeine fonksiyonları, hem mixinler içinde hem de ihtiyaç durumunda geliştirme sırasında kullanılabilirler. Fonksiyonlar genel olarak bir CSS çıktısı üretmezler. Daha çok bir CSS tanımı içinde kullanılacak değer ya da hesaplama sonuçlarını üretirler.

### 1. Jenerik Fonksiyonlar (_generic_)
Jenerik fonksiyonlar genel kullanım amaçlıdır.

#### 1.1 `remove-unit`
Verilen bir değerdeki birimi kaldırır.
```scss
remove-unit(10px) // 10
remove-unit(100%) // 100
```

### 2. Metin Fonksiyonları (_string_)
Metinler (String) ile alakalı fonksiyonlardı.

#### 2.1 `str-replace`
Verilen bir metin içindeki harf/kelimeleri değiştirir ya da kaldırır.

Aldığı parametreler;
* `$string`, içinde arama yapılacak metin
* `$substr`, aranacak söz öbeği
* `$newsubstr`, orjinali ile değiştirilecek söz öbeği
* `$all` ilk bulunan söz öbeği mi yoksa hepsi mi değiştirilecek kontrolü

``` scss
str-replace('foo bar baz', 'bar', 'foo'); // foo foo baz
```

#### 2.2 `str-ireplace`
`str-replace` fonksiyonun büyük küçük harf ayırmayan versiyonu.
```scss
str-ireplace('for Bar bAz', 'bar', 'foo'); // foo foo bAz
```

### 3. Text/Boyut Fonksiyonları (_text_)
Font büyüklükleri ve `rem` - `px` dönüşümleri ile alakalı fonksiyonlardır.

#### 3.1 `rem-to-px`
`rem` olarak verilmiş bir font büyüklüğünü, sayfada tanımlanan ana font boyutuna göre (_default değeri 16px'dir_) dönüşümünü yapar.

```scss
rem-to-px(1rem) // 16px
rem-to-px(20px) // 20px
rem-to-px(1.5rem) // 24px
```

#### 3.2 `rem`
`px` olarak verilmiş bir font büyüklüğünü, sayfada tanımlanan ana font boyutuna göre (_default değeri 16px'dir_) dönüşümünü yapar.

```scss
rem(16px) // 1rem
rem(20px) // 1.25rem
rem(1.5rem) // 1.5rem
```
