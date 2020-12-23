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

Vatandaştan alınan tek **bir** izni kaydeder.

> `API_URL` /consent/single/{brand}

### Örnek İstek Gövdesi
Response Headers:
```
Method:       POST
Status:       200 OK
URL:          /consent/single/{brand}
Content-Type: application/json
```
Reponse Body:
```
{
  "type": "EPOSTA",
  "recipientType": "BIREYSEL",
  "recipient": "mail@example.com",
  "source": "HS_FIZIKSEL_ORTAM",
  "consentDate": "2020-12-18 07:07:09",
  "status": "ONAY"
}
```
##### Parameters

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| brand | path | Hizmet sağlayıcının markasına özel kod(code) bilgisidir. | Yes | integer |
| recipient | post | Vatandaşın sistemde kayıtlı telefon numarası veya e-posta bilgisidir. | Yes | string |
| type | post | Vatandaşın izin verdiği iletişim kanalıdır. | Yes | string |
| source | post | Vatandaşın izin durumu belirlediği kaynaktır. Alıcı tipi `TACIR` ise eklenmesi zorunlu değildir. | Yes | string |
| status | post | Vatandaşın izin durumunu gösterir. | Yes | string |
| consentDate | post | İznin, vatandaştan alındığı tarihtir. | Yes | string |
| recipientType | post | İzin kaydının tacir veya bireysel amaçla alındığını ifade eder. | Yes | string |

### Başarılı Yanıt
Response Headers:
```
Method:       POST
Status:       200 OK
URL:          /consent/single/{brand}
Content-Type: application/json
```
Reponse Body:
```
{
  "data": {
    "result": true
  }
}
```

##### Başarısız Yanıtlar

| Code | Description |
| ---- | ----------- |
| 400 | Bad Request. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable entity |
| 500 | Internal Server Error |


## Çoklu İzin Ekleme

**Tek** markaya asenkron bir şekilde çoklu izin ekler.

> `API_URL` /consent/async/{brand}

### Örnek İstek Gövdesi
Response Headers:
```
Method:       POST
Status:       200 OK
URL:          /consent/async/{brand}
Content-Type: application/json
```
Reponse Body:
```
[
  {
    "type": "MESAJ",
    "recipientType": "BIREYSEL",
    "recipient": "905000000001",
    "source": "HS_FIZIKSEL_ORTAM",
    "consentDate": "2020-12-10 10:00:00",
    "status": "RET"
  },
  {
    "type": "EPOSTA",
    "recipientType": "BIREYSEL",
    "recipient": "mail@example.com",
    "source": "HS_2015",
    "consentDate": "2015-05-01 00:00:00",
    "status": "ONAY"
  }
]
```
##### Parameters

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| brand | path | Hizmet sağlayıcının markasına özel kod(code) bilgisidir. | Yes | integer |
| recipient | post | Vatandaşın sistemde kayıtlı telefon numarası veya e-posta bilgisidir. | Yes | string |
| type | post | Vatandaşın izin verdiği iletişim kanalıdır. | Yes | string |
| source | post | Vatandaşın izin durumu belirlediği kaynaktır. Alıcı tipi `TACIR` ise eklenmesi zorunlu değildir. | Yes | string |
| status | post | Vatandaşın izin durumunu gösterir. | Yes | string |
| consentDate | post | İznin, vatandaştan alındığı tarihtir. | Yes | string |
| recipientType | post | İzin kaydının tacir veya bireysel amaçla alındığını ifade eder. | Yes | string |

### Başarılı Yanıt
Response Headers:
```
Method:       POST
Status:       200 OK
URL:          /consent/async/{brand}
Content-Type: application/json
```
Reponse Body:
```
{
  "data": {
    "transaction": 47
  }
}
```

##### Başarısız Yanıtlar

| Code | Description |
| ---- | ----------- |
| 400 | Bad Request. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable entity |
| 500 | Internal Server Error |

## Çoklu İzin Ekleme Durumu

Çoklu İzin Ekleme metoduyla eklenen izinlerin durumu kontrol edilir.

Kontrol işlemi [Çoklu İzin Ekleme](#çoklu-i̇zin-ekleme) metodundan dönen `transaction` değeri ile yapılır

Çoklu veri ekleme sonuçları sistemde **1**(bir) hafta tutulur.


> `API_URL` /consent/status/{transaction}

##### Parameters

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| transaction | path | Çoklu izin ekleme metodundan dönen `transaction` kodu. | Yes | integer |

### Başarılı Yanıt
Response Headers:
```
Method:       GET
Status:       200 OK
URL:          /consent/status/{transaction}
Content-Type: application/json
```
Reponse Body:
```
{
  "data": [
    {
      "type": "MESAJ",
      "recipientType": "BIREYSEL",
      "recipient": "+905000000001",
      "source": "HS_FIZIKSEL_ORTAM",
      "consentDate": "2020-12-10 10:00:00",
      "status": "RET",
      "result": "success"
    },
    {
      "type": "EPOSTA",
      "recipientType": "BIREYSEL",
      "recipient": "mail@example.com",
      "source": "HS_2015",
      "consentDate": "2015-05-01 00:00:00",
      "status": "ONAY",
      "result": "failure",
      "error": {
        "message": "Sistemdeki iznin tarihinden önceki tarihli izinlerle güncelleme yapılamaz."
      }
    }
  ]
}
```
### Başarısız Yanıtlar

| Code | Description |
| ---- | ----------- |
| 401 | Unauthorized |
| 402 | Payment Required |
| 404 | Not Found |

# İZİN SORGULAMA

## Tekil İzin Sorgulama

Tek bir iznin durumunu kontrol eden metoddur.

> `API_URL` /report/single/{brand}

### Örnek İstek Gövdesi
Response Headers:
```
Method:       POST
Status:       200 OK
URL:          /report/single/{brand}
Content-Type: application/json
```
Reponse Body:
```
{
  "type": "EPOSTA",
  "recipientType": "BIREYSEL",
  "recipient": "mail@example.com"
}
```
##### Parameters

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| brand | path | Hizmet sağlayıcının markasına özel kod(code) bilgisidir. | Yes | integer |
| type | post | Vatandaşın izin verdiği iletişim kanalıdır. | Yes | string |
| recipientType | post | İzin kaydının tacir veya bireysel amaçla alındığını ifade eder. | Yes | string |
| recipient | post | Vatandaşın sistemde kayıtlı telefon numarası veya e-posta bilgisidir. | Yes | string |

### Başarılı Yanıt
Response Headers:
```
Method:       POST
Status:       200 OK
URL:          /report/single/{brand}
Content-Type: application/json
```
Reponse Body:
```
{
  "recipient": "mail@example.com",
  "type": "EPOSTA",
  "source": "HS_FIZIKSEL_ORTAM",
  "status": "ONAY",
  "consentDate": "2020-12-18 07:07:09",
  "recipientType": "BIREYSEL"
}
```

##### Başarısız Yanıtlar

| Code | Description |
| ---- | ----------- |
| 400 | Bad Request. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable entity |
| 500 | Internal Server Error |


# Çoklu İzin Sorgulama

Çoklu izin durumunu kontrol eden metoddur.

Tek seferde en fazla 1.000 adet izin yüklenebilir.

> `API_URL` /report/async/{brand}

### Örnek İstek Gövdesi
Response Headers:
```
Method:       POST
Status:       200 OK
URL:          /report/async/{brand}
Content-Type: application/json
```
Reponse Body:
```
{
  "type": "MESAJ",
  "recipientType": "BIREYSEL",
  "recipients": [
    "905000000001",
    "905000000002"
  ]
}
```

##### Parameters

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| brand | path | Hizmet sağlayıcının markasına özel kod(code) bilgisidir. | Yes | integer |
| type | post | Vatandaşın izin verdiği iletişim kanalıdır. | Yes | string |
| recipientType | post | İzin kaydının tacir veya bireysel amaçla alındığını ifade eder. | Yes | string |
| recipient | post | Vatandaşın sistemde kayıtlı telefon numarası veya e-posta bilgisidir. | Yes | string |

### Başarılı Yanıt
Response Headers:
```
Method:       POST
Status:       200 OK
URL:          /report/async/{brand}
Content-Type: application/json
```
Reponse Body:
```
{
  "data": {
    "transaction": 47
  }
}
```

##### Başarısız Yanıtlar

| Code | Description |
| ---- | ----------- |
| 400 | Bad Request. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable entity |
| 500 | Internal Server Error |



# Çoklu İzin Sorgulama Durumu


Çoklu İzin Sorgulama metoduyla sorgulanan izinlerin durumu kontrol edilir.

Kontrol işlemi [Çoklu İzin Sorgulama](#operation/reportAsync) metodundan dönen `transaction` değeri ile yapılır

Çoklu veri ekleme sonuçları sistemde **1**(bir) hafta tutulur.

> `API_URL` /report/status/{transaction}

##### Parameters

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| transaction | path | Çoklu izin sorgulama metodundan dönen `transaction` kodu. | Yes | integer |

### Başarılı Yanıt
Response Headers:
```
Method:       GET
Status:       200 OK
URL:          /report/status/{transaction}
Content-Type: application/json
```
Reponse Body:
```
{
  "data": [
    {
      "type": "MESAJ",
      "recipientType": "BIREYSEL",
      "recipient": "905000000001",
      "source": "HS_FIZIKSEL_ORTAM",
      "consentDate": "2020-12-14 16:46:00",
      "status": "ONAY"
    },
    {
      "recipient": "905555555555"
    },
    {
      "recipient": "90555",
      "error": {
        "message": "Alıcı (recipient) için E164 uluslararası([+][country code][area code][local phone number]) formatına uygun bir telefon numarası girilmelidir. (örn. `+905992000000`)"
      }
    }
  ]
}
```
### Başarısız Yanıtlar

| Code | Description |
| ---- | ----------- |
| 401 | Unauthorized |
| 402 | Payment Required |
| 404 | Not Found |

# HATA KODLARI

## VEYOSI API Hata Kodları

API isteklerinde cevap olarak dönülen hataların kodlarına ve hata mesajlarına bu bölümde ulaşabilirsiniz.

| Hata Kodu | Durum Kodu | Açıklama|
|-----------|------------|---------------------------------------------------------------------------------------|
|V014|400|Gönderilen istek JSON formatına uygun olmalıdır.|
|V015|500|Beklenmedik bir hata oluştu.|
|V082|400|Enum listeleme işlemi başarısız oldu.|
|V083|429|Saniyede kabul edilen istek limitinine ulaşıldı. Lütfen daha sonra tekrar deneyiniz.|
|V085|400|İzin ekleme isteği geçerli olmalıdır.|
|V090|400|Bayi ekleme isteği geçerli olmalıdır.|
|V091|422|İzin ekleme sayısı en fazla {{BATCH_CONSENT_COUNT}} olmalıdır.|
|V092|400|Çoklu izin ekleme isteği geçerli olmalıdır.|
|V093|422|{{transactionId}} bulunamadı.|
|V095|400|İşlem (transaction) isteği geçerli olmalıdır.|
|V097|400|İzin sorgulama isteği geçerli olmalıdır.|
|V098|400|Günlük izin değişim sorgulama isteği geçerli olmalıdır.|
|V100|500|Sunucuya kurtarma verisi kaydedilemedi.|
|V101|500|Sunucuya esas veri kaydedilemedi.|
|V102|500|Şifreleme işlemleri yapılamadı.|
|V103|500|Sunucuda arama yapılamadı.|
|V105|500|Şifre çözme işlemi başarısız oldu.|
|V106|500|Durum raporu oluşturulamadı.|
|V107|500|İzinler listelenemedi.|
|V110|422|Durum (status) bulunamadı.|
|V111|422|İzin tipi (type) bulunamadı.|
|V112|422|İzin tarihi (consentDate) bulunamadı.|
|V113|422|İzin kaynağı (source) bulunamadı.|
|V114|422|Alıcı (recipient) bulunamadı.|
|V115|422|Durum (status) için uygun değerler: ONAY, RET|
|V116|422|Alıcı tipi (recipientType) için uygun değerler: BIREYSEL, TACIR|
|V117|422|İzin tipi (type) için uygun değerler: ARAMA, MESAJ, EPOSTA|
|V118|422|{{unacceptableField}} kabul edilemedi. İstek gövdesinde bulunabilecek değerler: recipientType, retailerAccess, recipient, retailerCode, source, type, consentDate, status olmalıdır.|
|V119|422|İzin kaynağı (source) için uygun değerler: HS_FIZIKSEL_ORTAM, HS_ISLAK_IMZA, HS_WEB, HS_CAGRI_MERKEZI, HS_SOSYAL_MEDYA, HS_EPOSTA, HS_MESAJ, HS_MOBIL, HS_EORTAM, HS_ETKINLIK, HS_2015, HS_ATM, HS_KARAR|
|V120|422|Alıcı (recipient) kabul edilemedi. Geçerli bir eposta adresi giriniz.|
|V121|422|Alıcı (recipient) için E164 uluslararası([+][country code][area code][local phone number]) formatına uygun bir telefon numarası girilmelidir. (örn. +905992000000)|
|V122|422|Alıcı (recipient) için E164 uluslararası ([+][country code][area code][local phone number]) formatına uygun bir telefon numarası (örn. +905992000000) ya da geçerli bir e-posta adresi girilmelidir.|
|V123|400|Ulaşmaya çalıştığınız servis şu anda meşguldür. Lütfen daha sonra tekrar deneyiniz.|
|V124|429|Saniyede kabul edilen istek limitine ulaşıldı. Lütfen daha sonra tekrar deneyiniz.|
|V125|422|Tek seferde eklenecek izin sayısı en fazla 1000 olmalıdır.|
|V126|422|İzin erişimi olan bayilerin (retailer) liste uzunluğu en fazla {{100|
|V127|422|Alıcı listesi (recipients) uzunluğu en fazla 100 olmalıdır.|
|V129|404|Entegratör:{{iysCode]} bulunamadı.|
|V155|422|İzin kaynağı “1 Mayıs 2015 öncesi” olan izinler için, izin tarihi sadece 2015-05-01 00:00:00 olabilir.|
|V156|422|İzinler için 1 Mayıs 2015’den önceki tarihler kabul edilmemektedir.|
|V157|422|Geçerli bir tarih girilmelidir.|
|V158|422|İzin tarihi yyyy-mm-dd hh:mm:ss formatında olmalıdır.|
|V160|422|Alıcı için telefon numaraları 15 karakterden uzun olmamalıdır…|
|V162|422|Mevcut tarih ve saatten ileri bir tarih ve saat girilmemelidir.|
|V163|422|Bayiler (retailer) kabul edilemedi. Lütfen boş liste eklemeyiniz.|
|V164|422|Alıcı listesi (recipients) kabul edilemedi. Liste boş olmamalıdır.|
|V166|422|Alıcı (recipient) kabul edilemedi. Telefon numaraları 15 karakterden uzun olamaz.|
|V168|422|Alıcı listesi (recipients) bulunamadı.|
|V169|422|İzin erişimi olan bayiler (retailerAccess) bulunamadı.|
|V170|422|Alıcı tipi (recipientType) bulunamadı.|
|V171|400|Bayi izin erişimi vermek için geçerli bir istek gönderilmelidir.|
|V172|400|Bayi izin erişimini silmek için geçerli bir istek gönderilmelidir.|
|V173|400|Bayi izin erişimi sorgulamak için geçerli bir istek gönderilmelidir.|
|V174|400|İzin durumu (status) güncellemesi için farklı bir durum girilmelidir. İzin durumu: {{ONAY \ RET}}|
|V175|400|İlk defa kaydedilen bir iznin durum (status) bilgisi RET olmamalıdır.|
|V176|400|Sadece admin hesabıyla işlem yapılabilir.|
|V178|400|Sistemdeki iznin tarihinden önceki tarihli izinlerle güncelleme yapılamaz.|
|V179|500|İzin tarihi karşılaştırması yapılamadı.|
|V180|403|İstek gövdesinde yer alan bayi İYS numarasıyla (retailerCode) herhangi bir bayi bulunamadı.|
|V181|500|Bayi bilgisi listelenemedi.|
|V182|422|offset değeri geçerli olmalıdır.|
|V183|422|limit değeri geçerli olmalıdır.|
|V184|422|all değeri geçerli olmalıdır.|
|V185|422|short değeri geçerli olmalıdır.|
|V186|422|Sayfalama (pagination) değeri en fazla 100 olabilir.|
|V187|422|Limit değerleri sıfır olmamalıdır.|
|V190|422|iysCode integer bir değer olmalıdır.|
|V191|422|brandCode integer bir değer olmalıdır.|
|V192|503|İstek zaman aşımına uğradı.|
|V193|422|{{Bayi kod \ Alıcı}} ({{retailerCode \ recipient}}) değeri liste içinde tekrarlanamaz.|
|V194|422|İzin listede daha önce tanımlandı. Liste içinde izinler alıcı tipi (recipientType), tip (type) ve alıcı (recipient) değerleri için tekrarlanamaz.|
|V195|403|Marka bilgisi için sunucuda arama yapılamadı.|
|V196|403|Sunucudan iysCode bilgisi elde edilemedi.|
|V197|422|Arama için kullanılacak karakter öbeği (text) bulunamadı.|
|V198|422|requestId 36 karakterden daha uzun olamaz.|
|V199|422|requestId sadece harflerden meydana gelemez.|
|V241|400|Enum getirme işlemi başarısız oldu.|
|V245|400|Verilen iysCode entegratör grubuna ait değildir!|
|V250|400|Kullanıcı adı veya şifre eksik!|
|V251|401|Kullanıcı kimlik bilgileri geçersiz!|
|V252|400|Kimlik doğrulaması yapılamadı. Kimlik bilgileri eksik!|
|V266|400|{{iysCode}} değeri istek üzerinde bulunamadı.|
|V267|400|{{iysCode}} değeri boş olmamalıdır.|
|V268|404|Verilen iysCode’una bağlı entegratör kullanıcısı bulunamadı!|
|V351|401|Eksik veya geçersiz jeton!|
|V353|403|{field}, kullanıcı izinleriyle eşleştirilemiyor|
|V355|404|Kullanıcı da {type} yetkisi bulunmamaktadır!|
|V363|400|Bu servisi kullanabilmek için yetkiniz bulunmamaktadır.|
|V400|422|requestId sadece sayılardan meydana gelemez.|
|V401|400|Bayi eklenmesi beklenen izin sistemde bulunmamaktadır.|
|V402|400|Yetkili marka sorgulama isteği geçersizdir.|
|V403|400|İzin durumu sorgulama isteği geçerli olmalıdır.|
|V404|422|text en az bir karakter içermelidir.|
|V405|400|Marka bilgisi sorgulama isteği geçerli olmalıdır.|
|V408|422|HS_KARAR kaynağından iletilen izinler için durum bilgisi RET olmalıdır.|
|V412|500|Beklenmedik bir hata oluştu.|
|V450|422|İzin tipi (type) değeri geçerli olmalıdır.|
|V452|422|İzin tipi (type) değeri [ARAMA, MESAJ] için beklenen izin kaynağı [IYS_CM, IYS_KISAMESAJ] olmalıdır.|
|V453|422|Alıcı (recipient) {{recipient}} için gün içerisinde yapılan işlem sayısı 2'dir, daha fazla işlem yapılamamaktadır.|
|V454|422|İzin kaydetme isteği için verilen istek id (requestId) geçerli olmalıdır.|
|V455|422|İzin kaydetme isteği için verilen doğrulama kodu (verificationCode) geçerli olmalıdır.|
|V456|422|İzin kaydetme isteği için verilen istek id (requestId) ile ilgili işlem bulunamadı.|
|V459|422|Alıcı (recipiet) +905320000000 için beklenen izin tipi (type) değerleri [MESAJ, ARAMA] olmalıdır|
|V460|422|Girilen veriler doğrulamadı.|
|V462|400|TACIR izinleri güncellenirken izin tarihi (consentDate) verilmelidir|
|V463|400|TACIR izinleri güncellenirken izin kaynağı (source) verilmelidir.|
|V464|422|İzin tipi (type) MESAJ için, alıcı (recipient) telefon formatında olmalıdır.|
|V465|422|Alıcı (recipient) {{recipient}} için 3 dk içinde yeni bir süreç başlatılamamaktadır|
|V470|500|Beklenmedik bir hata oluştu.|
||400|Kullanıcı kimlik bilgileri geçersiz.|
||400|Giriş yöntemi belirtilmemiş.|
||400|Bilinmeyen giriş yöntemi.|
||400|Giriş yöntemi boş olamaz.|
||400|Desteklenmeyen giriş yöntemi.|
||400|Yenileme jetonu verilmemiş.|
||400|Desteklenmeyen yenileme jetonu türü.|
||400|Yenileme jetonu boş olamaz.|
||400|Şifre null olamaz.|
||400|Desteklenmeyen şifre türü.|
||400|Şifre boş bırakılamaz.|
||400|Kullanıcı adı null olamaz.|
||400|Desteklenmeyen kullanıcı adı türü.|
||400|Kullanıcı adı boş bırakılamaz.|
||400|Beklenmeyen kullanıcı rolü: admin.|

## İsteğin Tekrar Gönderilmesi Gereken Hata Kodları

|Vata Kodu | Durum Kodu | Açıklama|
|-----------|------------|---------------------------------------------------------------------------------------|
|V015| 500| Beklenmedik bir hata oluştu.|
|V101|500| Sunucuya esas veri kaydedilemedi.|
|V102| 500| Şifreleme işlemleri yapılamadı.|
|V103|500| Sunucuda arama yapılamadı.|
|V123|400| Ulaşmaya çalıştığınız servis şu anda meşguldür. Lütfen daha sonra tekrar deneyiniz. |
|V124| 429| Saniyede kabul edilen istek limitine ulaşıldı. Lütfen daha sonra tekrar deneyiniz.  |
|V179| 500| İzin tarihi karşılaştırması yapılamadı.|
|V192|503| İstek zaman aşımına uğradı.|
|V412|500| Beklenmedik bir hataoluştu.|
|V470|500| Beklenmedik bir hata oluştu.|
