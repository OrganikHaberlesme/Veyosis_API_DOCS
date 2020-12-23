<img src="https://veyosis.com/assets/img/logo-dark.png" width="200"/>

# VEYOSIS API DOCS

## API Hakkında

VEYOSİS API ile VEYOSİS Web arayüzünden gerçekleştirdiğiniz hemem hemen tüm işlemleri web arayüzüdne oturum açmadan gerçekleştirebilirsiniz.

  - API hizmetimizin `BASE_URL`'i **https://api.veyosis.com** şeklindedir.
  
Tüm metodların HTTP istek ve cevap mesajlarında `JSON` söz dizimi standardı kullanılmıştır.Güvenli API‘ lerin erişiminde HTTP Authorization protokolü takip edilmektedir.Bu doğrultuda Kimlik Yönetimi metotlarıyla alınan erişim jetonu(“access token”) kullanılarak İzin Yönetim metotlarıyla güvenli veri alışverişi sağlanmaktadır.

# TANIMLAR
VEYOSİS API'sinde kullanılacak olan teknik terimler ve açıklamalar bu bölümde yer alacaktır. Güncellemeler oldukça bu bölümde ekleme ve  - Alıcı (`recipient`): Vatandaşın sistemde kayıtlı telefon numarası veya e-posta bilgisidir. Kişisel bilgiler (ad, soyad, adres vs.) bir izinde yer almaz.
- İzin Tipi (`type`): Vatandaşın izin verdiği iletişim kanalıdır. İYS üzerinde şu an için sadece `ARAMA`, `MESAJ` ve `EPOSTA` kanalları için izinler saklanmaktadır.
- Alıcı Tipi (`recipientType`): İznin tacir veya bireysel amaçla alındığını ifade eder.
- İzin Kaynağı (`source`): Vatandaşın izin durumu belirlediği kaynaktır. Alıcı tipi `TACIR` ise eklenmesi zorunlu değildir.
- İzin Durumu (`status`): Vatandaşın izin durumunu gösterir. ONAY veya RET olabilir.
- İzin Tarihi (`consentDate`): İznin, vatandaştan alındığı tarihtir. Alıcı tipi TACIR ise eklenmesi zorunlu değildir.
- İzinlerin tarih formatı `YYYY-MM-DD HH:mm:ss`, saat dilimi `Türkiye saati` kabul edilir.


## Version: v1.0

> `API_URL` /me

Bu metot API Kodunun yetkili kullanıcısına ait bilgileri listeler.

##### Responses


| Code | Description |
| ---- | ----------- |
| 200 | İşlem Başarılı |
| 401 | Unauthorized |
| 402 | Payment Required |
| 404 | Not Found |

### /brand/get

#### GET
##### Summary:

Marka Listeleme

##### Description:

Bu metot yetkili markaları listelemek için kullanılır.
Bu metodu kullanabilmek için API kullanıcısına `Marka İşlemleri` yetkisi verilmiş olması gerekmektedir.

Markalarınızın kodu, isimleri ve izinlerinizin cevabı döner.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | İşlem Başarılı |
| 401 | Unauthorized |
| 402 | Payment Required |
| 404 | Not Found |

### /consent/single/{brand}

#### POST
##### Summary:

Tekil İzin Ekleme

##### Description:

Vatandaştan alınan tek **bir** izni kaydeder.

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
