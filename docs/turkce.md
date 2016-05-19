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

### 8. Boyut tanımları (_size_)
Elemanların boyutlandırmaları ile alakalı mixinleri bulundurur.

#### 8.1 `dims`
Elemanın kare ya da istenilen boyutları standart olarak verilebilmesini sağlar.

Aldığı parametreler
* `$width`, elemanın genişliğini belirler. Kare kullanımında, `$height` değeri verilmediğinde `$width` değeri kullanılır.
* `$height`, elemanının yüksekliğini belirler.

```scss
.example {
  @include dims(10px);
}

/*
  .example {
    width: 10px;
    height: 10px;
  }
 */
```
```scss
.example {
  @include dims(10px, 20px);
}

/*
  .example {
    width: 10px;
    height: 20px;
  }
 */
```

### 9. Metin tanımları (_text_)
Metin ve fontlar ile alakalı tanımları belirlerler.

#### 9.1 `font-face`
Sayfaya eklenecek sistem fontları dışındaki font tanımlarını kolayca hazırlamak için kullanılır. Font dosyası olarak, stillerin olduğu _assets_ klasörü içinde _fonts_ isimli bir klasörde fontların tutulmasını bekler. Mixin kendi başına kullanılır, her hangibir bir seçici (class, id, tag vs.) içinde tanımlanmaz.

Aldığı parametreler
* `$name`, stillerinizde font tanımı için kullanacağınız font ismi.
* `$file`, _fonts_ klasöründe bulunan font dosyanızın *uzantısı* olmayan dosya ismi.
* `$weight`, kullanılan fontun, font dosyasında tanımlı kalınlığı, _default değeri `normal`_, standart `font-weight` tanımlarını alır.
* `$style`, kullanılan fontun, font dosyasında tanımlı stili, _default değeri `normal`_, standart `font-style` tanımlarını alır.

_Ayrıca, stilleriniz için kullanacağınız genel ayarlar içinde `$support-for-ie8` ve `$support-for-woff2` parametreleri için vereceğiniz `true` ya da `false` değerlerine göre, font-face tanımı için IE8 destekli font tanımı ve Woff2 font tanımı eklenir._

```scss
@include font-face(OpenSans, opensans-regular);
@include font-face(OpenSans, opensans-bold, bold);
@include font-face(OpenSans, opensans-italic, normal, italic);

/*
  @font-face {
    font-family: "OpenSans";
    src: url("../fonts/opensans-regular.woff?caffeine") format("woff"),
         url("../fonts/opensans-regular.ttf?caffeine") format("truetype");
    font-weight: normal;
    font-style: normal;
  }
  
  @font-face {
    font-family: "OpenSans";
    src: url("../fonts/opensans-bold.woff?caffeine") format("woff"),
         url("../fonts/opensans-bold.ttf?caffeine") format("truetype");
    font-weight: bold;
    font-style: normal;
  }
  
  @font-face {
    font-family: "OpenSans";
    src: url("../fonts/opensans-italic.woff?caffeine") format("woff"),
         url("../fonts/opensans-italic.ttf?caffeine") format("truetype");
    font-weight: normal;
    font-style: italic;
  }
 */
```

#### 9.2 `font-icon`
Font-icon kullanımı için hazırlanmış fontların stillerinize eklenmesi için kullanılabilir. `font-face` tanımlarının yanısıra `:before` ile kullanılacak tavsiye edilen font-icon tanımını da içermektedi.

Aldığı parametreler
* `$name`, font iconun adı,
* `$file`, font icon dosyasının uzantısı olmadan dosya adı.

_Ayrıca, font tanımları için kullanılan stillerde ortak bir `.icon-` benzeri class öneki ve placeholder class tanımını da, stil ayarlarından yapabileceğini `$icon-prefix` ve `$icon-placeholder` değişkenleri de mixin içinde kullanılır. `icon-` değeri `$icon-prefix` için, `%font-icon` değeri de `$icon-placeholder` için default değerlerdir.

```scss
@include font-icon(my-icons, my-icon-fonts);

/*
  @font-face {
    font-family: "my-icons";
    src: url("../fonts/my-icons.woff?caffeine") format("woff"),
         url("../fonts/my-icons.ttf?caffeine") format("truetype");
    font-weight: normal;
    font-style: normal;
  }
  
  %font-icon,
  [class^="icon-"]:before, 
  [class*="icon-"]:before {
    font-family: my-icons;
    font-style: normal;
    font-variant: normal;
    font-weight: normal;
    line-height: 1;
    text-transform: none;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
 */
```

#### 9.3 `font-size`
CSS3 ile beraber kullanılması tavsiye edilen font-size tanımında `px` yerine `rem` kullanılmasını kolaylaştırmaktadır. `px` olarak verilen font değerinin `rem` karşılığı verilmektedir.

Aldığı parametreler
* `$font-size`, `px` ya da `rem` değerinde font boyutu.
_Ayrıca stillerinizde ortak kullanacağınız ayar değişkenleri içinde verilebilecek `$support-for-ie8` değeri `true` durumunda, fallback olarak `px` değeri de font-size olarak eklenir._

```scss
.example {
  @include font-size(16px);
}

/*
  .example {
    font-size: 16px; // sadece $suppor-for-ie8: true ise eklenir. diğer durumda eklenmez.
    font-size: 1em
  }
 */
```

#### 9.4 `disable-text-select`
[Can I Use](http://caniuse.com/#search=user-select) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/user-select)

Belirlediğiniz bir eleman içindeki metinlerin seçilmesi sırasında _highlight_ renginin olmamasını, metnin seçilmesinin görünümü engeller. Özellikle butonlara tıklanma sırasında buton metninin seçiminin önüne geçer.

```scss
.example {
  @include disable-text-select;
}

/*
  .example {
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }
 */
```

#### 9.5 `selection`
[Can I Use](http://caniuse.com/#search=selection) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/::selection)

Cursor ile seçilen metnin _highlight_ renginin ve görünümün değişmesini sağlar. Sadece *Firefox* için prefix ihtiyacı bulunmaktadır. Metin stillerinin sadece küçük kısmı uygulanabilir, bunlar: `color`, `background-color`, `cursor`, `outline`, `text-decoration`, `text-emphasis-color` and `text-shadow`.

```scss
.example {
  @include selection {
    background-color: pink;
    color: #fff;
    text-shadow: 0 1px 1px #ccc;
  }
}

/*
  .example::-moz-selection {
    background-color: pink;
    color: #fff;
    text-shadow: 0 1px 1px #ccc;
  }
  
  .example::selection {
    background-color: pink;
    color: #fff;
    text-shadow: 0 1px 1px #ccc;
  }
 */
```

## CSS3 Mixinleri
CSS3 mixinleri daha çok vendor-prefix ihtiyacı olan tanımlar için sonradan prefix eklemek için ek araçlara ihtiyaç olmaması açısından eklenmişlerdir. 

### 1. Animasyon tanımları
CSS3 ile beraber kullanılmaya başlanmış animasyon tanımlarını içerir.

#### 1.1 `animation`
[Can I Use](http://caniuse.com/#search=css3%20animation) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation)

Animasyon kısa tanımlarını kullanmaya yarar. 

```
.example {
  @include animation(3s ease-in 1s 2 reverse both paused slidein);
}

/*
  .example {
    -webkit-animation: 3s ease-in 1s 2 reverse both paused slidein;
    animation: 3s ease-in 1s 2 reverse both paused slidein;
  }
 */
```

#### 1.2 `animation-delay`
[Can I Use](http://caniuse.com/#search=css3%20animation) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-delay)

Animasyonun ne zaman başlayacağını belirler. 

```
.example {
  @include animation-delay(3s);
}

/*
  .example {
    -webkit-animation-delay: 3s;
    animation-delay: 3s;
  }
 */
```

#### 1.3 `animation-direction`
[Can I Use](http://caniuse.com/#search=css3%20animation) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-direction)

Animasyonun ileri, geri ya da alternate modda mı olacağını belirler. 

```
.example {
  @include animation-direction(reverse);
}

/*
  .example {
    -webkit-animation-direction: reverse;
    animation-direction: reverse;
  }
 */
```

#### 1.4 `animation-duration`
[Can I Use](http://caniuse.com/#search=css3%20animation) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-duration)

Animasyonun süresini belirler. 

```
.example {
  @include animation-duration(6s);
}

/*
  .example {
    -webkit-animation-duration: 6s;
    animation-duration: 6s;
  }
 */
```

#### 1.5 `animation-fill-mode`
[Can I Use](http://caniuse.com/#search=css3%20animation) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-fill-mode)

Animasyonun stili nasıl uygulayacağını belirler. Alabildiği değerler `none`, `forwards`, `backwards` ve `both`. _default değeri `none`dır._

```
.example {
  @include animation-fill-mode(forwards);
}

/*
  .example {
    -webkit-animation-fill-mode: forwards;
    animation-fill-mode: forwards;
  }
 */
```

#### 1.6 `animation-iteration-count`
[Can I Use](http://caniuse.com/#search=css3%20animation) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-iteration-count)

Animasyonun durmadan önce ne kadar tekrar edeceğini belirler. Sayı ya da `infinite` değerlerini alabilir.

```
.example {
  @include animation-iteration-count(12);
}

/*
  .example {
    -webkit-animation-iteration-count: 12;
    animation-iteration-count: 12;
  }
 */
```

#### 1.7 `animation-name`
[Can I Use](http://caniuse.com/#search=css3%20animation) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-name)

Animasyonun içinde kullanılıcak `@keyframes` ile tanımlanmış animasyonun adı.

```
.example {
  @include animation-name(my-animation);
}

/*
  .example {
    -webkit-animation-name: my-animation;
    animation-name: my-animation;
  }
 */
```

#### 1.8 `animation-play-state`
[Can I Use](http://caniuse.com/#search=css3%20animation) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-play-state)

Animasyonun çalışma durumunu belirler

```
.example {
  @include animation-play-state(paused);
}

/*
  .example {
    -webkit-animation-play-state: paused;
    animation-play-state: paused;
  }
 */
```

#### 1.9 `animation-timing-function`
[Can I Use](http://caniuse.com/#search=css3%20animation) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timing-function)

Animasyonun çalışırken kullanılacak _easing_ metodlarını belirler.

```
.example {
  @include animation-timing-function(ease, step-start, cubic-bezier(0.1, 0.7, 1.0, 0.1));
}

/*
  .example {
    -webkit-animation-timing-function: ease, step-start, cubic-bezier(0.1, 0.7, 1.0, 0.1);
    animation-timing-function: ease, step-start, cubic-bezier(0.1, 0.7, 1.0, 0.1);
  }
 */
```

#### 1.10 `keyframes`
[Can I Use](http://caniuse.com/#search=keyframes) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes)

Animasyonlar için kullanılacak frame geçişlerinin tanımlarını belirler.

```
@include keyframes(my-animation) {
  0% { top: 0; }
  50% { top: 30px; left: 20px; }
  50% { top: 10px; }
  100% { top: 0; }
}

/*
  @-webkit-keyframes my-animation {
    0% { top: 0; }
    50% { top: 30px; left: 20px; }
    50% { top: 10px; }
    100% { top: 0; }
  }
  
  @keyframes my-animation {
    0% { top: 0; }
    50% { top: 30px; left: 20px; }
    50% { top: 10px; }
    100% { top: 0; }
  }
 */
```

### 2. Kolonlar
Bir elemanın içindeki metinleri gazete gibi, kolonlu yapıda göstermek için kullanılabilecek tanımlardır.

#### 2.1 `columns`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/columns)

Kolon tanımların kısa kullanımıdır. Şuan Internet Explorer dışındaki tarayıcılarda prefix eklenmesi gerekmektedir.

```scss
.example {
  @include columns(100px 3);
}

/*
  .example {
    -webkit-columns: 100px 3;
    -moz-columns: 100px 3;
    columns: 100px 3;
  }
 */
```

#### 2.2 `column-count`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/column-count)

Eleman içindeki metinlerin kaç kolonda gözükeceğini tanımlar

```scss
.example {
  @include column-count(3);
}

/*
  .example {
    -webkit-column-count: 3;
    -moz-column-count: 3;
    column-count: 3;
  }
 */
```

#### 2.3 `column-width`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/column-count)

Eleman içindeki metinlerin kolon genişliğini tanımlar

```scss
.example {
  @include column-width(100px);
}

/*
  .example {
    -webkit-column-width: 100px;
    -moz-column-width: 100px;
    column-width: 100px;
  }
 */
```

#### 2.4 `column-gap`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/column-gap)

Eleman içindeki metinlerin kolon aralarındaki boşluğu tanımlar

```scss
.example {
  @include column-gap(10px);
}

/*
  .example {
    -webkit-column-gap: 10px;
    -moz-column-gap: 10px;
    column-gap: 10px;
  }
 */
```

#### 2.5 `column-rule`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/column-rule)

Eleman içindeki metinlerin kolonların kenarlarında gösterilecek çizgi/cetvel için kısa tanımdır.

```scss
.example {
  @include column-rule(thick inset blue);
}

/*
  .example {
    -webkit-column-rule: thick inset blue;
    -moz-column-rule: thick inset blue;
    column-rule: thick inset blue;
  }
 */
```

##### 2.5.1 `column-rule-color`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/column-rule-color)

Eleman içindeki metinlerin kolonların kenarlarında gösterilecek çizgi/cetvelin rengini tanımlar.

```scss
.example {
  @include column-rule-color(blue);
}

/*
  .example {
    -webkit-column-rule-color: blue;
    -moz-column-rule-color: blue;
    column-rule-color: blue;
  }
 */
```

##### 2.5.2 `column-rule-style`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/column-rule-style)

Eleman içindeki metinlerin kolonların kenarlarında gösterilecek çizgi/cetvelin stilini tanımlar. Alabileceği değerler `none`, `hidden`, `dotted`, `dashed, `solid`, `double`, `groove`, `ridge`, `inset`, `outset`

```scss
.example {
  @include column-rule-style(dotted);
}

/*
  .example {
    -webkit-column-rule-style: dotted;
    -moz-column-rule-style: dotted;
    column-rule-style: dotted;
  }
 */
```

##### 2.5.3 `column-rule-width`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/column-rule-width)

Eleman içindeki metinlerin kolonların kenarlarında gösterilecek çizgi/cetvelin genişliğini tanımlar.

```scss
.example {
  @include column-rule-width(2px);
}

/*
  .example {
    -webkit-column-rule-width: 2px;
    -moz-column-rule-width: 2px;
    column-rule-width: 2px;
  }
 */
```

#### 2.6 `column-span`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/column-span)

Eleman içindeki bazı elemanların kolonlarının tamamına genişlemesine sağlar.

```scss
.example {
  @include column-span(all);
}

/*
  .example {
    -webkit-column-span: all;
    -moz-column-span: all;
    column-span: all;
  }
 */
```

#### 2.7 `column-fill`
[Can I Use](http://caniuse.com/#search=column) | [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/column-fill)

Eleman içindeki kolonların, elemanın içine nasıl yayılacağını belirler.

```scss
.example {
  @include column-fill(balanced);
}

/*
  .example {
    -webkit-column-fill: balanced;
    column-fill: balanced;
  }
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
