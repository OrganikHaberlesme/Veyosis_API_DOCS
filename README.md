<img src="https://veyosis.com/assets/img/logo-api-docs.png" width="200"/>

# VEYOSIS API DOCS

API'ye gönderdiğiniz isteklerin ve aldığınız yanıtların tümü tek bir URL üzerinden çalışır. API farklı komutlar için farklı metodlarla çalışır. Genel olarak bu metodlar GET ve POST'tur. Hangi fonksiyonun, hangi metodla çalıştığı ilgili sayfalarda belirtilmiştir.
# VEYOSIS API
VEYOSİS API'yi kullanmak veya görüşlerinizi bizimle paylaşmak isterseniz lütfen bizimle info@veyosis.com adresi üzerinden iletişime geçiniz.

## Version: v1.0

> `API_URL` /me

Bu metot API Kodunun yetkili kullanıcısına ait bilgileri listeler.

##### Responses

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
