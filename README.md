<img src="https://veyosis.com/assets/img/logo-dark.png" width="200"/>

# VEYOSIS API DOCS

## API Hakkında

VEYOSİS API ile VEYOSİS Web arayüzünden gerçekleştirdiğiniz hemem hemen tüm işlemleri web arayüzüdne oturum açmadan gerçekleştirebilirsiniz.

  - API hizmetimizin `BASE_URL`'i `https://api.veyosis.com` şeklindedir.
  
Tüm metodların HTTP istek ve cevap mesajlarında `JSON` söz dizimi standardı kullanılmıştır.Güvenli API‘ lerin erişiminde HTTP Authorization protokolü takip edilmektedir.Bu doğrultuda Kimlik Yönetimi metotlarıyla alınan erişim jetonu(“access token”) kullanılarak İzin Yönetim metotlarıyla güvenli veri alışverişi sağlanmaktadır.

# TANIMLAR
VEYOSİS API'sinde kullanılacak olan teknik terimler ve açıklamalar bu bölümde yer alacaktır. Güncellemeler oldukça bu bölümde ekleme ve  - Alıcı (`recipient`): Vatandaşın sistemde kayıtlı telefon numarası veya e-posta bilgisidir. Kişisel bilgiler (ad, soyad, adres vs.) bir izinde yer almaz.
- İzin Tipi (`type`): Vatandaşın izin verdiği iletişim kanalıdır. İYS üzerinde şu an için sadece `ARAMA`, `MESAJ` ve `EPOSTA` kanalları için izinler saklanmaktadır.
- Alıcı Tipi (`recipientType`): İznin tacir veya bireysel amaçla alındığını ifade eder.
- İzin Kaynağı (`source`): Vatandaşın izin durumu belirlediği kaynaktır. Alıcı tipi `TACIR` ise eklenmesi zorunlu değildir.
- İzin Durumu (`status`): Vatandaşın izin durumunu gösterir. ONAY veya RET olabilir.
- İzin Tarihi (`consentDate`): İznin, vatandaştan alındığı tarihtir. Alıcı tipi TACIR ise eklenmesi zorunlu değildir.
- İzinlerin tarih formatı `YYYY-MM-DD HH:mm:ss`, saat dilimi `Türkiye saati` kabul edilir.


## İzin Kaynakları

|Değer| Açıklama|
|------|--------|
|`HS_2015`|Alıcı, 1 Mayıs 2015 tarihi öncesinde onaylı olarak kaydedilmiştir.|
|`HS_KARAR`|İzin durumu, hizmet sağlayıcının kendi isteğiyle RET olarak belirlenmiştir. Hizmet sağlayıcı, ilk izin ekleme işleminde HS_KARAR kaynak(source) değerini kullanamamaktadır.|
|`HS_FIZIKSEL_ORTAM`|İzin, hizmet sağlayıcı tarafından fiziksel ortamda alınmıştır.|
|`HS_ISLAK_IMZA`|İzin, alıcının bir formu veya anketi imzalaması üzerine alınmıştır.|
|`HS_ETKINLIK`|İzin, hizmet sağlayıcının düzenlediği bir etkinlikte alınmıştır.||
|`HS_ATM`| İzin, hizmet sağlayıcıya ait yerleşik ATM cihazıyla alınmıştır.|
|`HS_EORTAM`| İzin, hizmet sağlayıcıya ait bir elektronik ortamda alınmıştır.|
|`HS_WEB`| İzin, hizmet sağlayıcının web sitesi üzerinde yapılan bir işlemle alınmıştır.|
|`HS_MOBIL`| İzin, hizmet sağlayıcıya ait mobil uygulama üzerinden alınmıştır.|
|`HS_MESAJ`| İzin, hizmet sağlayıcıya ait kısa mesaj numarası üzerinden alınmıştır.|
|`HS_EPOSTA`| İzin, hizmet sağlayıcıya ait e-posta vasıtasıyla alınmıştır.|
|`HS_CAGRI_MERKEZI`| İzin, hizmet sağlayıcıya bağlı bir çağrı merkezinde sesle veya numara tuşlamayla alınmıştır.|
|`HS_SOSYAL_MEDYA`| İzin, hizmet sağlayıcıya ait sosyal medya aracı üzerinden alınmıştır.|

## İzin Türleri

Hizmet sağlayıcının, ticari elektronik ileti gönderimi yaptığı iletişim kanalını (arama, mesaj, e-posta) ifade eder.
- İzin Türü (type)
  - Telefon (`+902121230000` gibi `[+][ülke kodu][alan kodu][telefon no]` şeklinde E164 uluslararası formata uygun verilmesi gerekmektedir. Maksimum uzunluk 15 olabilir.)
  
|Değer| Açıklama|
|----------|--------------|
|`ARAMA`| Alıcıların telefon numarasına ilişkin arama bazlı izinleri ifade eder.|
|`MESAJ`| Alıcıların telefon numarasına ilişkin mesaj bazlı izinleri ifade eder.|

- İzin Türü (type)\n  
  - E-posta   
  **Şartlar** 
    - Tüm e-posta adresleri [regEx](https://regex101.com/r/yEyjL3/27) örneğiyle uyumlu olmalıdır.
    - Türkçe harfler kullanılamaz.
    - Özel karakterler kullanılamaz. Örnek: `{[(!' ^ % & /()=?,”½²³°¶)}]` vb.
    - Geçerli bir e-posta, asgari [{1 karakter}@{2 karakter}.{2 karakter}] şablonunda olmalıdır.
    - Bir e-posta adresinin uzunluğu en az 6, en fazla 265 karakter olabilir.
    - {@} işaretinden önceki kısımda, e-posta servis sağlayıcıların etiketleme (`label`) yapmak için kabul ettiği özel karakter olarak artı işareti ( + ) kullanılabilir.
    - Bir e-posta adresi boşluk içeremez.
    
|Değer| Açıklama|
|-------------|---------------------|
|`EPOSTA`|Alıcıların elektronik posta adreslerine ilişkin e-posta bazlı izinleri ifade eder.|

## İzin Durumları
Alıcının ticari elektronik ileti gönderimine ilişkin izin durumunu (onay/ret) belirtir.

İzin Durumu(status) tablodaki değerleri alabilir.

|Değer| Açıklama|
|-------------|---------------------|
|`ONAY`|Alıcının, hizmet sağlayıcının ticari elektronik ileti göndermesini onayladığını ifade eder.|
|`RET`|Alıcının, hizmet sağlayıcının ticari elektronik ileti göndermesini onaylamadığını ifade eder.|

# KİMLİK DOĞRULAMA

## API Kodu Oluşturma
VEYOSIS API ile işlem gerçekleştirmek istiyorsanız öncelikle bir `API KOD`unuzun olması gerekmektedir.
Eğer API kodunuz yoksa;
 - https://panel.veyosis.com adresi üzerinde kaydınız olması gerekmektedir.
 - Kayıt işlemlerinin ardından oturum açmalısınız.
 - `API Erişimi` menüsünden Yeni API Kodu oluşturabilirsiniz.
 
## API Kodu Yetkileri
|İzin| Açıklama|
|----------------|------------------- |
| `Marka İşlemleri` |Yetkisi ile marka işlemleri izni verilir.|
| `İzin Yönetimi` | Yetkisi ile izin işlemlerinin tümü gerçekleştirilebilir.|
| `İzin Sorgulama` | Yetkisiyle izin sorgulama işlemlerine izin verilir.|
| `Ip Kısıtlama` | Özelliği aktif edildiğinde istenilen IP dışındaki adreslerin erişimi engellenir.|

## API Bağlantısı
Oluşturduğunuz API kodunu endpointlere ulaşmak için gönderdiğiniz sorgulara `Authorization` header'ı olarak `Bearer XYZXYZXYZ` şeklinde eklemeniz gerekiyor.
 
## Kullanıcı Bilgileri
Bu metot API Kodunun yetkili kullanıcısına ait bilgileri listeler.

> `API_URL` /me
### Başarılı Yanıt
Response Headers:
```
Method:       GET
Status:       200 OK
URL:          /me
Content-Type: application/json
```
Reponse Body:
```
{
  "data": {
    "user": "Örnek Kullanıcı",
    "username": "kullanici",
    "email": "mail@example.com",
    "company": "Örnek Firma Ltd.şti.",
    "title": "Örnek API",
    "permissions": [
      "brand",
      "consent",
      "report"
    ],
    "ip": false
  }
}
```
### Başarısız Yanıtlar
| Code | Description |
| ---- | ----------- |
| 401 | Unauthorized |
| 402 | Payment Required |
| 404 | Not Found |

# MARKA İŞLEMLERİ

Hizmet sağlayıcıya bağlı bulunan markaların listelemesi bu bölümde yapılmaktadır.

Panel Üzerinden pasifleştirdiğiniz markalarınızı görüntüleyemez ve işlem yapamazsınız.
## Marka Listeleme
Bu metot yetkili markaları listelemek için kullanılır. Bu metodu kullanabilmek için API kullanıcısına `Marka İşlemleri` yetkisi verilmiş olması gerekmektedir.

Markalarınızın kodu, isimleri ve izinlerinizin cevabı döner.

> `API_URL` /brand/get
### Başarılı Yanıt
Response Headers:
```
Method:       GET
Status:       200 OK
URL:          /brand/get
Content-Type: application/json
```
Reponse Body:
```
{
  "data": {
    "brands": [
        {
          "code": "00000",
          "title": "Organik Haberleşme Teknolojileri bilişim san.ltd.şti.",
          "permissions": {
            "ARAMA": "write",
            "MESAJ": "write",
            "EPOSTA": "write"
          },
          "consents": {
            "approval": 6479,
            "rejection": 1,
            "total": 6480
          }
        },
        {
          "code": "000001",
          "title": "Organik haberleşme teknolojileri bilişim sanayi ltd.şti.",
          "permissions": {
            "ARAMA": "write",
            "MESAJ": "write",
            "EPOSTA": "write"
          },
          "consents": {
            "approval": 0,
            "rejection": 0,
            "total": 0
          }
        }
    ]
  }
}
```
### Başarısız Yanıtlar

| Code | Description |
| ---- | ----------- |
| 401 | Unauthorized |
| 402 | Payment Required |
| 404 | Not Found |

# İZİN YÖNETİMİ

## Tekil İzin Ekleme

### /consent/single/{brand}

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| brand | path | Hizmet sağlayıcının markasına özel kod(code) bilgisidir. | Yes | integer |

|Değişken|Gereklilik|Tip|Açıklama|
|-|-|-|-|
|deliveries|Zorunlu|-|İstek gövdenizin ana elemanıdır. Tüm istekleriniz`data`etiketinin içinde olmalıdır.|

Tekil İzin Ekleme

##### Description:

Vatandaştan alınan tek **bir** izni kaydeder.



##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | İzin ekleme işlemi başarılı. |
| 400 | Bad Request. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable entity |
| 500 | Internal Server Error |

### /consent/async/{brand}

#### POST
##### Summary:

Çoklu İzin Ekleme

##### Description:

**Tek** markaya asenkron bir şekilde çoklu izin ekler.

Tek seferde en fazla 1.000 adet izin yüklenebilir.

Metotdan dönen `transaction` değeri ile [Çoklu İzin Ekleme Durumu](#operation/consentStatus) metodu ile kuyruk durumunu kontrol edilebilir.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| brand | path | Hizmet sağlayıcının markasına özel kod(code) bilgisidir. | Yes | integer |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | İzin ekleme işlemi başarılı. |
| 400 | Bad Request. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable entity |
| 500 | Internal Server Error |

### /consent/status/{transaction}

#### GET
##### Summary:

Çoklu İzin Ekleme Durumu

##### Description:

Çoklu İzin Ekleme metoduyla eklenen izinlerin durumu kontrol edilir.

Kontrol işlemi [Çoklu İzin Ekleme](#operation/consentAsync) metodundan dönen `transaction` değeri ile yapılır

Çoklu veri ekleme sonuçları sistemde **1**(bir) hafta tutulur.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| transaction | path | Çoklu izin ekleme metodundan dönen `transaction` kodu. | Yes | integer |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | İşlem Başarılı |
| 401 | Unauthorized |
| 402 | Payment Required |
| 404 | Not Found |

### /report/single/{brand}

#### POST
##### Summary:

Tekil İzin Sorgulama

##### Description:

Tek bir iznin durumunu kontrol eden metoddur.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| brand | path | Hizmet sağlayıcının markasına özel kod(code) bilgisidir. | Yes | integer |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | İşlemi başarılı. |
| 400 | Bad Request. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable entity |
| 500 | Internal Server Error |

### /report/async/{brand}

#### POST
##### Summary:

Çoklu İzin Sorgulama

##### Description:

Çoklu izin durumunu kontrol eden metoddur.

Tek seferde en fazla 1.000 adet izin yüklenebilir.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| brand | path | Hizmet sağlayıcının markasına özel kod(code) bilgisidir. | Yes | integer |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | İşlemi başarılı. |
| 400 | Bad Request. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable entity |
| 500 | Internal Server Error |

### /report/status/{transaction}

#### GET
##### Summary:

Çoklu İzin Sorgulama Durumu

##### Description:

Çoklu İzin Sorgulama metoduyla sorgulanan izinlerin durumu kontrol edilir.

Kontrol işlemi [Çoklu İzin Sorgulama](#operation/reportAsync) metodundan dönen `transaction` değeri ile yapılır

Çoklu veri ekleme sonuçları sistemde **1**(bir) hafta tutulur.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| transaction | path | Çoklu izin sorgulama metodundan dönen `transaction` kodu. | Yes | integer |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | İşlem Başarılı |
| 401 | Unauthorized |
| 402 | Payment Required |
| 404 | Not Found |

### Models


#### errorResponses

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer | Hata kodu. | No |
| message | String | Hata kodu ayrıntılı bilgisi. | No |

#### childItem

| Name | Type |  Description | Required |
| ---- | ---- | ----------- | -------- |
| ProductNumber | string |  | No |
| Length | integer |  | No |
